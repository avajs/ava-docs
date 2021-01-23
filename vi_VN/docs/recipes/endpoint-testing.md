# Test endpoint

Translations: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/endpoint-testing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/endpoint-testing.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/endpoint-testing.md)

AVA không có phương thức dựng sẵn để test endpoint, nhưng bạn có thể sử dụng bất kỳ thư việc xác nhận giá trị nào với nó. Hãy dùng [`supertest`](https://github.com/visionmedia/supertest).

Vì các test chạy đồng thời, tốt nhất là bạn nên tạo một server instanse cho mỗi test, bởi vì nếu chúng ta tham chiếu cùng một cá thể, điều này có thể dẫn đến sự đột biến giữa các test. Điều này có thể được thực hiện với một `test.beforeEach` và `t.context`, hoặc chỉ đơn giản là một factory function:

```js
function makeApp() {
	const app = express();
	app.use(bodyParser.json());
	app.post('/signup', signupHandler);
	return app;
}
```

Tiếp theo, chỉ cần inject server instance vào supertest. Cách duy nhất là sử dụng một promise hoặc cú pháp async/await thay vì phương thức `end` của supertest:

```js
test('signup:Success', async t => {
	t.plan(2);

	const res = await request(makeApp())
		.post('/signup')
		.send({email: 'ava@rocks.com', password: '123123'});

	t.is(res.status, 200);
	t.is(res.body.email, 'ava@rocks.com');
});
```
