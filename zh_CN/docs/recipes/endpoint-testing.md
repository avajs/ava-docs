___
**备注**

这是[endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/endpoint-testing.md)的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/master...zhaozhiming:master)用来查看本翻译与AVA的master分支是否有差别（如果你没有看到`endpoint-testing.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 端点测试

翻译: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/endpoint-testing.md)

AVA没有内嵌的方法可以来做端点测试，但你可以用其他断言库来做，让我们用[`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised)来看看。

因为测试是并发执行的，所以最好是为每个测试建立一个新的服务器实例，如果所有测试都引用同一个实例，那实例可能会被不同的测试改变状态。这可以在`test.beforeEach`和`t.context`里完成，或者简单的工厂方法：

```js
function makeApp() {
    const app = express();
    app.post('/signup', signupHandler);
    return app;
}
```

然后，将你的服务器注入到测试超类中，主要的点是用promise或async/await语法来代替测试超类的`end`方法：

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

