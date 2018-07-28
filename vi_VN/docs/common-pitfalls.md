# Những sai lầm phổ biến

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/common-pitfalls.md), [Tiếng Việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/common-pitfalls.md)

## ESLint plugin

Nếu bạn sử dụng [ESLint](http://eslint.org/), bạn có thể cài đặt [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava). Nó sẽ giúp bạn sử dụng AVA một cách chính xác và tránh một số hiểu lầm phổ biến.

## AVA trong Docker

Nếu bạn chạy AVA trong Docker như là một phần của CI, bạn cần sửa các biến môi trường thích hợp. Cụ thể, thêm `-e CI=true` vào lệnh `docker exec`. Xem [#751](https://github.com/avajs/ava/issues/751).

AVA sử dụng [is-ci](https://github.com/watson/is-ci) để quyết định xem nó có trong môi trường CI hay không, hoặc không sử dụng [các biến này](https://github.com/watson/ci-info/blob/master/index.js).

## AVA và các giới hạn kết nối client

Có thể bạn đang sử dụng một service có sự hạn chế, chỉ cho phép một số kết nối đồng thời. Ví dụ: nhiều doanh nghiệp cung cấp cơ sở dữ liệu cung cấp các gói miễn phí với sự giới hạn số lượng client có thể sử dụng cùng một lúc. AVA có thể đạt tới giới hạn khi nó chạy nhiều process, nhưng các service được viết tốt sẽ phát ra lỗi hoặc có sự điều tiết trong các trường hợp đó. Nếu dịch vụ bạn sử dụng không có những thứ đó, test của bạn sẽ bị treo.

Theo mặc định, AVA sẽ sử dụng số process theo số [nhân vật lý](https://superuser.com/questions/1105654/logical-vs-physical-cpu-performance) trên thiết bị của bạn. Số process được giới hạn là 2 trong môi trường CI.

Sử dụng tham số `concurrency` để giới hạn số lượng các process chạy. Ví dụ, nếu service của bạn chỉ cho phép 5 client, bạn nên chạy AVA với `concurrency=5` hoặc ít hơn.

## Các thao tác không đồng bộ

Có thể bạn đang chạy một hoạt động không đồng bộ bên trong một test và tự hỏi rằng tại sao nó không hoàn thành. Nếu hoạt động không đồng bộ của bạn sử dụng promise, bạn nên trả về promise:

```js
test('fetches foo', t => {
	return fetch().then(data => {
		t.is(data, 'foo');
	});
});
```

Còn tốt hơn nữa, nên dùng `async` / `await`:

```js
test('fetches foo', async t => {
	const data = await fetch();
	t.is(data, 'foo');
});
```

Nếu bạn đang sử dụng callback, dùng [`test.cb`](https://github.com/avajs/ava#callback-support):

```js
test.cb('fetches foo', t => {
	fetch((err, data) => {
		t.is(data, 'foo');
		t.end();
	});
});
```

Ngoài ra, hãy promisify hàm callback bằng cách sử dụng một cái gì đó như [`pify`](https://github.com/sindresorhus/pify):

```js
test('fetches foo', async t => {
	const data = await pify(fetch)();
	t.is(data, 'foo');
});
```

### Phân bổ các ngoại lệ chưa được kiểm tra cho các test

AVA [không thể theo dõi các ngoại lệ chưa được kiểm tra](https://github.com/avajs/ava/issues/214) trở lại test đã kích hoạt chúng. Hàm Callback-taking có thể dẫn đến các trường hợp ngoại lệ vô tình mà sau đó khó có thể gỡ lỗi. Xem xét promisifying và sử dụng `async`/`await`, như ví dụ ở trên. Điều này sẽ cho phép AVA bắt được ngoại lệ và gán nó vào test một cách chính xác.

### Tại sao các thông báo xác nhận giá trị nâng cao không được hiển thị?

Hãy đảm bảo rằng tham số đầu tiên được truyền vào test của bạn phải được đặt tên là `t`. Đây là sự yêu cầu của [`power-assert`](https://github.com/power-assert-js/power-assert), thư viện cung cấp các thông báo nâng cao.

```js
test('one is one', t => {
	t.is(1, 1);
});
```

### Các helper không được compile khi sử dụng thư mục test không mặc định

Đây là một [vấn đề đã biết](https://github.com/avajs/ava/issues/1319). Bạn nên đặt các test của bạn vào một folder tên là `test` hoặc `__tests__`.

---

Vấn đề của bạn không được liệt kê ở đây? Hãy gửi một pull request hoặc đăng nhận xét của mình vào [vấn đề này](https://github.com/avajs/ava/issues/404).
