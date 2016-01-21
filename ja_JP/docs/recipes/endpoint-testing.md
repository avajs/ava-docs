# エンドポイントのテスト

AVAはエンドポイントのテストのためのビルトインメソッドを持っていませんが、どのようなアサーションライブラリでも一緒に利用できます。[`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised)を使ってみましょう。

テストを直列で実行するので、同じインスタンスを参照するとテスト間でお互いに打ち消しうるために、テスト毎に新たなサーバーインスタンスを作成するのがベストです。これは、`test.beforeEach`や`t.context`、もしくは単純にファクトリー関数で実現できます。

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

次に、supertestにサーバーインスタンスを挿入します。唯一の落とし穴は、supertestの`end`メソッドの代わりにpromiseかasync/awaitの構文を使用することです:

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
