# Test endpoint với Mongoose

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing-with-mongoose.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/endpoint-testing-with-mongoose.md)

Công thức này chỉ bạn cách làm thế nào để test endpoint của bạn với AVA và Mongoose, giả sử bạn đang sử dụng Express làm framework của mình.

## Cài đặt

Công thức này sử dụng các thư viện sau:

1. [`mongod-memory-server`](https://github.com/nodkz/mongodb-memory-server) (Server in-memory MongoDB)
2. [SuperTest](https://github.com/visionmedia/supertest) (Thư viện test endpointy)
3. [Mongoose](http://mongoosejs.com)

Cài đặt hai thư viện đầu tiên bằng cách chạy đoạn code sau:

```console
$ npm install --save-dev mongodb-memory-server supertest
```

Bạn phải cài đặt Mongoose trước. Nếu không, hãy chạy đoạn code sau để cài đặt nó:

(Lưu ý: Bạn cần ít nhất là phiên bản v4.11.3)

```console
$ npm install mongoose
```

## Điều kiện tiên quyết

Bạn sẽ cần một file server và một model Mongoose. Xem các ví dụ [`server.js`](https://github.com/zellwk/ava-mdb-test/blob/master/server.js) và [`models/User.js`](https://github.com/zellwk/ava-mdb-test/blob/master/models/User.js).

Lưu ý rằng `server.js` không khởi động ứng dụng. Thay vào đó, điều này phải được thực hiện bởi SuperTest, để các endpoint của app có thể được test. Nếu bạn đang sử dụng Express cho ứng dụng của mình, hãy đảm bảo rằng bạn có một file khởi động có import `app` và gọi `app.listen()`.

## File test của bạn

Đầu tiên, include các thư việc mà bạn cần:

```js
// Các thư viện cần cho việc test
import test from 'ava'
import request from 'supertest'
import MongodbMemoryServer from 'mongodb-memory-server'
import mongoose from 'mongoose'

// Các model và server của bạn
import app from '../server'
import User from '../models/User'
```

Tiếp theo, khởi động MongoDB in-memory và kết nối với Mongoose:

```js
// Khởi động một instance của MongoDB
const mongod = new MongodbMemoryServer()

// Tạo kết nối đến Mongoose trước khi chạy test
test.before(async () => {
	const uri = await mongod.getConnectionString();
	await mongoose.connect(uri, {useMongoClient: true});
});
```

Khi bạn chạy test của mình lần đầu tiên, MongoDB sẽ tải xuống các file binary MongoDB mới nhất. Dung lượng tải xuống có thể là khoảng 70MB vì vậy quá trình này có thể mất một phút.

Bạn sẽ muốn sử dụng cơ sở dữ liệu của mình với dữ liệu giả. Đây là một ví dụ:

```js
test.beforeEach(async () => {
	const user = new User({
		email: 'one@example.com',
		name: 'One'
	});
	await user.save();
});
```

Dữ liệu giả nên được xóa sau mỗi lần test:

```js
test.afterEach.always(() => User.remove());
```

Bây giờ bạn có thể sử dụng SuperTest để gửi yêu cầu endpoint của app của bạn. Sử dụng AVA cho các xác nhận giá trị của bạn:

```js
// Lưu ý rằng các test sẽ được chạy tuần tự. Xem đoạn code dưới đây để biết lý do.

test.serial('litmus get user', async t => {
	const {app} = t.context;
	const res = await request(app)
		.get('/litmus')
		.send({email: 'one@example.com'});
	t.is(res.status, 200);
	t.is(res.body.name, 'One');
});

test.serial('litmus create user', async t => {
	const {app} = t.context;
	const res = await request(app)
		.post('/litmus')
		.send({
			email: 'new@example.com',
			name: 'New name'
		});

	t.is(res.status, 200);
	t.is(res.body.name, 'New name');

	// Xác minh rằng user đã được tạo trong cơ sở dữ liệu
	const newUser = await User.findOne({email: 'new@example.com'});
	t.is(newUser.name, 'New name');
});
```

Cuối cùng ngắt kết nối và dừng MongoDB khi tất cả các test được thực hiện:

```js
test.after.always(async () => {
	mongoose.disconnect()
	mongod.stop()
})

```

Và bạn đã hoàn tất!

## Sử dụng lại cấu hình trên các file

Bạn có thể chọn để extract code cho các hook `test.before`, `test.beforeEach`, `test.afterEach.always` và `test.after.always` vào các file riêng. Xem tại thêm ví dụ tại https://github.com/zellwk/ava-mdb-test.

## Sử dụng `test.serial` thay vì `test`

Các test của bạn có khả năng làm thay đổi cơ sở dữ liệu. Sử dụng `test()` có nghĩa là các test sẽ chạy đồng thời, điều này có thể làm cho một test ảnh hướng đến các test khác. Thay vì đó nếu bạn sử dụng `test.serial()` thì AVA chỉ chạy một test tại một thời điểm. Sau đó, bạn có thể dọn dẹp cơ sở dữ liệu giữa các lần test, làm cho các test có thể đoán trước được nhiều hơn.

Bạn có thể chạy test đồng thời nếu bạn tạo các kết nối Mongoose riêng biệt cho từng test.  Tuy nhiên, điều này khó thiết lập hơn. Bạn có thể tìm thêm thông tin [tại đây](https://github.com/nodkz/mongodb-memory-server#several-mongoose-connections-simultaneously).
