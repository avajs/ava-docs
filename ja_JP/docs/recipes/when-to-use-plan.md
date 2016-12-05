___
**訳注**

これは[when-to-use-plan.md](https://github.com/avajs/ava/blob/master/docs/recipes/when-to-use-plan.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/avajs/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-0c25d982e94d600cb6b8e438a0e67169)になります(このリンクをクリックして、`when-to-use-plan.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# `t.plan()`をいつ使うのか

AVAと[`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape)の主な違いは`t.plan()`の振る舞いです。AVAで`t.plan()`は予定されたアサーションが実行される回数と一致するかどうかを確認するときのみ利用されます。これによるテストの自動終了はしていません。

## `t.plan()`の良くない使い方

`tap`/`tape`になれた多くのユーザは`t.plan()`をすべてのテストごとに使う傾向があります。しかし、AVAではこれを「ベストプラクティス」だと考えていません。その代わりに、`t.plan()`が何かしらの値を意味するときだけに使われるべきと信じています。

### 分岐なしの同期テスト

`t.plan()`はほとんどの同期テストに必要ありません。

```js
test(t => {
  // 望ましくない: ここには分岐がない - t.plan()が無意味である
  t.plan(2);

  t.is(1 + 1, 2);
  t.is(2 + 2, 4);
});
```

この場合`t.plan()`は何の意味もなくて、無駄な作業を増やしています。

### resolveされると思われるPromise

```js
test(t => {
  t.plan(1);

  return somePromise().then(result => {
    t.is(result, 'foo');
  });
});
```

ちらっと見ると、このテストには非同期のPromiseが使われているので`t.plan()`を使って良いように見えます。しかし、ここにはいくつかの問題があります：

1. `t.plan()`はたぶん`somePromise()`がrejectされる可能性から守るために使われていると思います。しかし、rejectされたpromiseが返されるとどのみちテストは失敗するようになります。

2. `async`/`await`の長所を活かしたほうがいいはずです：

```js
test(async t => {
  t.is(await somePromise(), 'foo');
});
```

## `t.plan()`の良い使い方

`t.plan()`が好ましい場合も多いです。

### `.catch()`を使うPromise

```js
test(t => {
  t.plan(2);

  return shouldRejectWithFoo().catch(reason => {
    t.is(reason.message, 'Hello') // メッセージだけが気になるならt.throws()を使ってください
    t.is(reason.foo, 'bar');
  });
});
```

ここでの`t.plan()`は`catch`の中のコードが実行されるのかを保証してくれます。殆どの場合、`t.throws()`を使ったほうがいいが、`t.throws()`はエラーの`message`プロパティしか確認できないので有効な使い方です。

### catch句の発生の保証

```js
test(t => {
  t.plan(2);

  try {
    shouldThrow();
  } catch (err) {
    t.is(err.message, 'Hello') // メッセージだけが気になるならt.throws()を使ってください
    t.is(err.foo, 'bar');
  }
});
```

上の`try`/`catch`の例のように、だいたい`t.throws()`がいい選択です。しかし、これは`message`プロパティしか確認できません。

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

上のコードは`callbackA`の次に来る`callbackB`が一番最初に(そして、一回だけ)呼ばれたのかを確認します。それ以外の組み合わせは失敗します。

### 分岐するテスト

殆どの場合、複雑な分岐をテストに入れるのは良くないアイディアです。明らかな例外は(JSONなどから)自動生成されたテストぐらいです。下記の`t.plan()`はJSONが正しいのかを確認するために使われています。

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
  test(t => {
    const result = functionUnderTest(testDefinition.input);

    // testDefinitionは`foo`と`bar`の両方ではなく、いずれかが期待値であるべきです。

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

`t.plan()`は多くの有効な使い方がありますが、分別なく使うのはいけません。おおよそ、テストが直線的ではなくて、簡単に推論できず、コードフローもない場合に使った方がいいです。コールバックの中のアサーション、`if`/`then`文、`for`/`while`文、(場合によっては)`try`/`catch`が`t.plan()`を使ったほうがいい候補となります。
