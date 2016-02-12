___
**訳注**

これは[endpoint-testing.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/sindresorhus/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-0c25d982e94d600cb6b8e438a0e67169)になります(このリンクをクリックして、`endpoint-testing.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# `t.plan()`はいつ使うのか

AVAと[`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape)の主な差は`t.plan()`の振る舞いです。AVAで`t.plan()`は何回アサーションが行われたのかを数えるときのみ利用されます。これによるテストの自動終了はしていません。

## `t.plan()`の良くない使い方

`tap`/`tape`になれた多くのユーザは`t.plan()`をすべてのテストごとに使う傾向があります。しかし、AVAではこれを「最先のやり方」だと考えていません。その代わりに、`t.plan()`がある意味を表す状況のみに使われるべきだと思います。

### 分岐せずに同期テストをする

`t.plan()`はほとんどの同期テストに要りません。

```js
test(t => {
  // BAD: there is no branching here - t.plan() is pointless
  t.plan(2);

  t.is(1 + 1, 2);
  t.is(2 + 2, 4);
});
```

この場合`t.plan()`は何の意味もなくて、無駄な作業を増やしています。

### Resolveされると思われるPromise

```js
test(t => {
  t.plan(1);

  return somePromise().then(result => {
    t.is(result, 'foo');
  });
});
```

ちらっと見ると、このテストには非同期のPromiseが使われているので`t.plan()`を使って良いように見えます。しかし、ここにはいくつかの問題があります：

1. `t.plan()`はたぶん`somePromise()`がrejectされる可能性を突き止めるために使われていると思います。しかし、rejectされたpromiseがリターンされるとどのみちテストは失敗するようになります。

2. `async`/`await`の長所を活かしたほうがいいはずです：

2. It would be better to take advantage of `async`/`await`:

```js
test(async t => {
  t.is(await somePromise(), 'foo');
});
```

## `t.plan()`の良い使い方

`t.plan()`が容認できる場合も多いです。

### `.catch()`を使うPromise

```js
test(t => {
  t.plan(2);

  return shouldRejectWithFoo().catch(reason => {
    t.is(reason.message, 'Hello') // Prefer t.throws() if all you care about is the message
    t.is(reason.foo, 'bar');
  });
});
```

ここでの`t.plan()`は`catch`の中のコードが実行されるのかを保証してくれます。殆どの場合、`t.throws()`を使ったほうがいいが、`t.throws()`はエラーの`message`プロパーティしか確認できないので有効な使い方です。

### catch部分がちゃんと起動されたのかを確認するとき

```js
test(t => {
  t.plan(2);

  try {
    shouldThrow();
  } catch (err) {
    t.is(err.message, 'Hello') // Prefer t.throws() if all you care about is the message
    t.is(err.foo, 'bar');
  }
});
```

上の`try`/`catch`の例のように、だいたい`t.throws()`がいい選択であります。しかし、これは`message`プロパーティしか確認できません。

### 複数のコールバックが実際に呼ばれたのかを確認するとき

```js
test.cb(t => {
  t.plan(2);

  const callbackA = () => {
    t.pass();
    t.end();
  };

  const callbackB = () => t.pass();

  bThenA(callbackA, callbackB);
});
```

上のコードは`callbackA`の次に来る`callbackB`が一番最初(そして、一回だけ)呼ばれたのかを確認します。それ以外の組み合わせは失敗します。

### 分岐するテスト

殆どの場合、複雑な分岐をテストに入れるのは良くないアイディアであります。主な例外は(JSON文書などから)自動生成されたテストぐらいです。下の`t.plan()`はJSON入力が正しいのかを確認するために使われています。

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
  test(t => {
    const result = functionUnderTest(testDefinition.input);

    // testDefinition should have an expectation for `foo` or `bar` but not both
    t.plan(1);

    if (testDefinition.foo) {
      t.is(result.foo, testDefinition.foo);
    }

    if (testDefinition.bar) {
      t.is(result.bar, testDefinition.foo);
    }
  });
});
```

## 結論

`t.plan()`は多くの有効な使い方がありますが、分別なく使うのはいけません。おおよそ、テストが直線的ではなくて、簡単に理由が把握できず、コードフローもない場合に使った方がいいです。コールバックの中のアサーション、`if`/`then`文、`for`/`while`文、(ある場合での)`try`/`catch`が`t.plan()`を使ったほうがいい候補となります。
