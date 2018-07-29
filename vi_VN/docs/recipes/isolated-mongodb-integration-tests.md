# Các test tích hợp MongoDB riêng biệt

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/isolated-mongodb-integration-tests.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/isolated-mongodb-integration-tests.md)

> Làm thế nào để chạy các cơ sở dữ liệu MongoDB dùng một lần trong các test AVA của bạn với sự cách lý cho mỗi test.

Để làm điều này bạn cần sử dụng [`MongoMem`](https://github.com/CImrie/mongomem), sẽ cho phép bạn nhanh chóng chạy một server cục bộ MongoDB tạm thời. Nó sử dụng kho lưu trữ file tạm, sẽ bị hủy khi server dừng.


## Cài đặt server MongoDB in-memory (MongoMem)

Trong thư mục gốc của ứng dụng, chạy lệnh:

```console
$ npm install --save-dev mongomem
```


## Sử dụng MongoMem

Trong file test của bạn, hãy import module và chạy server.

**Đảm bảo lệnh chạy server được đặt ở đầu file, bên ngoài tất cả các test case.**

```js
import test from 'ava';
import {MongoDBServer} from 'mongomem';

test.before('start server', async t => {
	await MongoDBServer.start();
})

test('some feature', async t => {
	const connectionString = await MongoDBServer.getConnectionString();

	// connectionString === 'mongodb://localhost:27017/3411fd12-b5d6-4860-854c-5bbdb011cb93'
	// Sử dụng `connectionString` để kết nối đến cơ sở dữ liệu với một client mà bạn chọn. Xem bên dưới để biết cách sử dụng với Mongoose.
});
```


## Dọn dẹp

Sau khi bạn đã chạy các test của mình, bạn nên thêm một phương thức `test.after.always()` để dọn dẹp server MongoDB. Điều sẽ sẽ xóa các file tạm mà server sử dụng khi chạy.

Việc này thường được dọn dẹp bởi hệ điều hành của bạn, nhưng tốt nhất là bạn nên tự làm nó.

```js
test.after.always('cleanup', t => {
	MongoDBServer.tearDown(); // Dọn dẹp kho lưu trữ file tạm
});
```


## Gỡ lỗi

Nếu server dường như không khởi động, bạn có thể đặt tùy chọn `MongoDBServer.debug=true;` trước khi bạn gọi `MongoDBServer.start()`. Điều này sẽ cho phép server MongoDB in lỗi kết nối hoặc lỗi quyền sử dụng file khi nó bắt đầu. Nó kiểm tra và chọn một port có sẵn để chạy máy chủ, vì vậy các lỗi có liên quan đến các quyền sử dụng file.


## Extra: Thiết lập và sử dụng trong Mongoose

[Mongoose](http://mongoosejs.com) là một Object-Document-Mapper (ODM) mạnh mẽ cho MongoDB. Tham khảo tài liệu của nó để bắt đầu với Mongoose.

Để sử dụng Mongoose một cách hiệu quả với AVA, hãy xem [Tài liệu tích hợp Mongoose](endpoint-testing-with-mongoose.md).
