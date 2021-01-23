# Cài đặt Test 

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/test-setup.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/test-setup.md)

Các test có thể được thiết lập bằng cách sử dụng hook `beforeEach()`. Thông thường, bạn có thể sử dụng hàm thiết lập cơ bản. Công thức này giúp bạn quyết định xem cách nào là tốt nhất trong trường hợp sử dụng của riêng mình.

# Hook `beforeEach()`  với các hàm cài đặt 

Hook `beforeEach()` có một số nhược điểm. Ví dụ, bạn không thể tắt nó cho các test cụ thể, và bạn cũng không thể áp dụng nó cho các test cụ thể. Thay vào đó, bạn có thể sử dụng các chức năng đơn giản. Điều này cho phép bạn sử dụng nhiều chức năng thiết lập cho các yêu cầu thiết lập khác nhau và gọi các phần khác nhau của thiết lập từ các test khác nhau. Bạn thậm chí có thể thiết lập các chức năng với các tham số để test có thể tùy chỉnh thiết lập riêng của chúng.

Giả sử bạn có một hàm tương tác với hệ thống file. Có lẽ bạn chạy một vài test sử dụng `mock-fs`, và một số ít sử dụng hệ thông file thật và một thư mục tạm. Hoặc bạn có một hàm thiết lập mà bạn chạy với dữ liệu hợp lệ cho một số test và dữ liệu không hợp lệ cho các test khác, tất cả trong cùng một file test.

Bạn có thể làm tất cả những điều này bằng cách sử dụng các hàm thiết lập đơn giản, nhưng ở đây có sự cân bằng:

|`beforeEach()`| Các hàm thiết lập
|---|---
| ⛔️ &nbsp; được sử dụng cho tất cả các test| ✅ &nbsp; có thể thay đổi hoặc bỏ qua tùy vào test
| ⛔️ &nbsp; khó khăn hơn cho người mới bắt đầu, "some magic"| ✅ &nbsp; dễ cho người mới bắt đầu, "no magic"
| ✅ &nbsp; hỗ trợ chế độ callback, tích hợp hỗ trợ cho observables| ⛔️ &nbsp; phải dùng promise  cho các hành động bất đồng bộ
| ✅ &nbsp; thất bại có đầu ra thân thiện| ⛔️ &nbsp; lỗi được quy cho test
| ✅ &nbsp; tương ứng `afterEach` và `afterEach.always` để dọn dẹp| ⛔️ &nbsp; không thể dễ dàng dọn dẹp

## Thiết lập test phức tạp

Trong ví dụ này, chúng ta có một hook `beforeEach()`, và sau đó có nhiều sửa đổi hơn trong mỗi test.

```js
test.beforeEach(t => {
	setupConditionA(t);
	setupConditionB(t);
	setupConditionC(t);
});

test('first scenario', t => {
	tweakSomething(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

test('second scenario', t => {
	tweakSomethingElse(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

Nếu có quá nhiều biến cần phải thay đổi cho mỗi test, hãy xem xét bỏ qua hook `beforeEach()` và thực hiện các bước thiết lập trong chính các test.

```js
test('first scenario', t => {
	setupConditionA(t);
	setupConditionB(t, {/* options */});
	setupConditionC(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

// Trong test này, setupConditionB() không bao giờ được gọi
test('second scenario', t => {
	setupConditionA(t);
	setupConditionC(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

## Ví dụ thực tế

```js
test.beforeEach(t => {
	t.context = {
		authenticator: new Authenticator(),
		credentials: new Credentials('admin', 's3cr3t')
	};
});

test('authenticating with valid credentials', async t => {
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.true(await isValid);
});

test('authenticating with an invalid username', async t => {
	t.context.credentials.username = 'bad_username';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});

test('authenticating with an invalid password', async t => {
	t.context.credentials.password = 'bad_password';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});
```

Các test tương tự, hiện đang sử dụng các chức năng thiết lập, sẽ trông giống nhau.

```js
function setup({username = 'admin', password = 's3cr3t'} = {}) {
	return {
		authenticator: new Authenticator(),
		credentials: new Credentials(username, password)
	};
}

test('authenticating with valid credentials', async t => {
	const {authenticator, credentials} = setup();
	const isValid = authenticator.authenticate(credentials);
	t.true(await isValid);
});

test('authenticating with an invalid username', async t => {
	const {authenticator, credentials} = setup({username: 'bad_username'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});

test('authenticating with an invalid password', async t => {
	const {authenticator, credentials} = setup({password: 'bad_password'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});
```

## Kết hợp các hook và hàm thiết lập

Dĩ nhiên `beforeEach()` và hàm thiết lập đơn giản có thể được sử dụng cùng nhau:

```js
test.beforeEach(t => {
	t.context = setupAllTests();
});

test('first scenario', t => {
	firstSetup(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});
```
