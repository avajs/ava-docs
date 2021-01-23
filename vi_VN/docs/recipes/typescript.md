# TypeScript

Các bản dịch: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/typescript.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/typescript.md)

AVA đi kèm với một file định nghĩa TypeScript. Điều này cho phép các nhà phát triển tận dụng TypeScript để viết các bài kiểm tra.

Hướng dẫn này giả định là bạn đã thiết lập TypeScript cho dự án của mình. Lưu ý rằng định nghĩa của AVA đã được thử nghiệm với phiên bản 2.8.3.

## Cấu hình AVA để biên dịch các file TypeScript đang di chuyển

Bạn có thể cấu hình AVA để nhận dạng các file TypeScript. Sau đó, với `ts-node` được cài đặt, bạn có thể biên dịch chúng một cách nhanh chóng:

```json
{
	"ava": {
		"compileEnhancements": false,
		"extensions": [
			"ts"
		],
		"require": [
			"ts-node/register"
		]
	}
}
```

Cần lưu ý rằng với các cấu hình này test sẽ thất bại nếu có lỗi build từ TypeScript. Nếu bạn muốn bỏ qua các lỗi này trong khi test, bạn có thể dùng `ts-node/register/transpile-only` thay vì `ts-node/register`.

## Biên dịch các file TypeScript trước khi chạy AVA

Thêm một script `test` vào file `package.json`. Nó sẽ biên dịch dự án trước và sau đó chạy AVA.

```json
{
	"scripts": {
		"test": "tsc && ava"
	}
}
```

Đảm bảo rằng AVA chạy các file TypeScript đã được build của bạn.

## Viết các test

Tạo một file `test.ts`.

```ts
import test from 'ava';

const fn = async () => Promise.resolve('foo');

test(async (t) => {
	t.is(await fn(), 'foo');
});
```

## Sử dụng [macros](https://github.com/avajs/ava#test-macros)

Để có thể gán thuộc tính `title` cho macro, bạn cần nhập hàm:

```ts
import test, {Macro} from 'ava';

const macro: Macro = (t, input: string, expected: number) => {
	t.is(eval(input), expected);
};
macro.title = (providedTitle: string, input: string, expected: number) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Bạn sẽ cần một loại khác nếu bạn mong muốn macro của mình được sử dụng với callback test:

```ts
import test, {CbMacro} from 'ava';

const macro: CbMacro = t => {
	t.pass();
	setTimeout(t.end, 100);
};

test.cb(macro);
```

## Typing [`t.context`](https://github.com/avajs/ava#test-context)

Theo mặc định, kiểu `t.context` sẽ là đối tượng rỗng (`{}`). AVA chìa ra một interface `TestInterface<Context>` mà bạn có thể sử dụng để áp dụng kiểu riêng của mình cho `t.context`. Điều này có thể giúp bạn nắm bắt lỗi tại thời gian biên dịch:

```ts
import anyTest, {TestInterface} from 'ava';

const test = anyTest as TestInterface<{foo: string}>;

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // lỗi: Kiểu '123' không thể gán vào kiểu 'string'
});

test.serial.cb.failing('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // lỗi: Thuộc tính 'fooo' không tồn tại trên kiểu ''
});

test('an actual test', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // lỗi: Thuộc tính 'map' không tồn tại trên kiểu 'string'
});
```

Bạn cũng có thể nhập ngữ cảnh khi tạo macro:

```ts
import anyTest, {Macro, TestInterface} from 'ava';

interface Context {
	foo: string
}

const test = anyTest as TestInterface<Context>;

const macro: Macro<Context> = (t, expected: string) => {
	t.is(t.context.foo, expected);
};

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test('foo is bar', macro, 'bar');
```

Lưu ý rằng, mặc dù kiểu được truyền ở trên, khi thực thi `t.context` là một đối tượng rỗng trừ khi nó được gán.
