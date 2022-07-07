# Khi nào sử dụng `t.plan()`

Các bản dịch: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/when-to-use-plan.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/when-to-use-plan.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/when-to-use-plan.md)

Một sự khác biệt lớn giữa AVA và [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) là hành vi của `t.plan()`. Trong AVA, `t.plan()` chỉ được sử dụng để khẳng định rằng số lượng các xác nhận dự kiến được gọi; nó không tự động kết thúc test.

## Những cách sử dụng sai của `t.plan()`

Rất nhiều người dùng chuyển từ `tap`/`tape` đã quen với việc sử dụng `t.plan()` trong tất cả các test. Tuy nhiên, trong AVA, chúng tôi không coi đó là một "thói quen tốt". Thay vào đó, chúng tôi tin rằng `t.plan()` chỉ nên được dùng trong các tính huống mà nó mang lại một số giá trị.

### Test đồng bộ không phân nhánh

`t.plan()` là không cần thiết trong hầu hết các test đồng bộ.

```js
test('simple sums', t => {
	// Không tốt: không có sự phân nhánh ở đây - t.plan() là vô nghĩa
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` không cung cấp bất kỳ giá trị nào ở đây và tạo thêm một công việc vặt nếu bạn quyết định thêm hoặc xóa các xác nhận giá trị.

### Các promise được mong đợi là sẽ resolve

```js
test('gives foo', t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

Thoạt nhìn, các bài kiểm tra này dường như sử dụng tốt `t.plan()` ví nó có một trình sử lý promise không đồng bộ. Tuy nhiên, có một số vấn đề với test:

1. `t.plan()` có lẽ được sử dụng ở đây để bảo vệ khả năng `somePromise()` có thể bị reject; Nhưng việc trả về một promise bị reject cũng sẽ làm test bị bất bại.

2. Sẽ tốt hơn nếu tận dụng lợi thế của `async`/`await`:

```js
test('gives foo', async t => {
	t.is(await somePromise(), 'foo');
});
```

### Các promise với một khối `.catch()`

```js
test('rejects with foo', t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello');
		t.is(reason.foo, 'bar');
	});
});
```

Ở đây, việc sử dụng `t.plan()` tìm cách đảm bảo rằng code bên trong khối `catch` được thực thi.
Thay vào đó, bạn nên tận dụng `t.throwsAsync` và `async`/`await`, vì điều này dẫn đến code rõ ràng và dễ hiểu hơn:

```js
test('rejects with foo', async t => {
	const reason = await t.throwsAsync(shouldRejectWithFoo());
	t.is(reason.message, 'Hello');
	t.is(reason.foo, 'bar');
});
```

### Đảm bảo lệnh catch xảy ra

```js
test('throws', t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello');
		t.is(err.foo, 'bar');
	}
});
```

Như đã nêu trong ví dụ trước, việc sử dụng xác nhận `t.throws()` với `async`/`await` là một lựa chọn tốt hơn.

## Cách sử dụng tốt của `t.plan()`

`t.plan()` cung cấp giá trị trong các trường hợp sau.

### Đảm bảo nhiều callback thực sự được gọi

```js
test.cb('invokes callbacks', t => {
	t.plan(2);

	const callbackA = () => {
		t.pass();
		t.end();
	};

	const callbackB = () => t.pass();

	bThenA(callbackA, callbackB);
});
```

Đoạn code trên đảm bảo rằng `callbackB` được gọi đầu tiên (và chỉ một lần), tiếp theo là `callbackA`. Bất kỳ trường hợp nào khác sẽ không làm thỏa mãn kế hoạch.

### Test với các lệnh phân nhánh

Trong hầu hết các trường hợp, bạn nên sử dụng bất kỳ sự phân nhánh phức tạp nào trong các test của mình. Một ngoại lệ đáng chú ý là cho các test được tự động (có lẽ từ một tài liệu JSON). Bên dưới `t.plan()` được sử dụng để đảm bảo tính chính xác của đầu vào JSON.

```js
const testData = require('./fixtures/test-definitions.json');

for (const testDefinition of testData) {
	test('foo or bar', t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition phải có một kỳ vọng cho `foo` hoặc `bar` nhưng không phải là cả 2
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
}
```

## Kết luận

`t.plan()` có nhiều cách sử dụng hợp lệ, nhưng nó không nên được sử dụng bừa bãi. Một nguyên tắc chung là nên sử dụng nó bất kỳ lúc nào *test* của bạn không đơn giản, dễ dàng giải thích, flow của code. Các test với xác nhận bên trong callback, lệnh `if`/`then`, `for`/`while`, và (trong một số trường hợp) các khối  `try`/`catch` , là tất cả các ứng cử viên tốt cho `t.plan()`.
