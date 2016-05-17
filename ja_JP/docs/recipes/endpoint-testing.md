___
**訳注**

これは[endpoint-testing.md](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/avajs/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-aee54ab6a703c02779edb3ebbb35e96f)になります(このリンクをクリックして、`endpoint-testing.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

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
