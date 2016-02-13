___
**訳注**

これは[readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/sindresorhus/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9)になります(このリンクをクリックして、`readme.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> 未来型のテストランナー

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/sindresorhus/ava)

JavaScriptはシングルスレッドだけれども、Node.jsにおけるIOは、その非同期の性質によって並行で起こり得ます。AVAは、この点において利点があり、テストを同時に実行することができ、特に重いIOのテストに有益です。加えて、テストファイル群は独立したプロセスで並行して実行することで、より良いパフォーマンスと各テストファイル毎に隔離された環境を与えられます。PagresにおけるMochaからAVAへの[切り替え](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0)は、テストにかかる時間を31秒から11秒まで減らしました。並行で実行するテストは、原子のテストを記述することを強制します。それはテストが大域の状態や他のテストの状態から影響を受けないことを意味します。それは偉大なことです！

*Issueやプルリクエストなどで貢献したい場合は、[コントリビューションガイド](contributing.md)を読んでください。

アップデートの情報のために[AVAのTwitterアカウント](https://twitter.com/ava__js)をフォローしてください。

翻訳: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md)


## 目次

- [使い方](#使い方)
- [CLIの使い方](#cliの使い方)
- [設定](#設定)
- [ドキュメント](#ドキュメント)
- [API](#api)
- [アサーション](#アサーション)
- [FAQ](#faq)
- [レシピ](#レシピ)


## なぜAVAなのか?

- 軽量で速い
- シンプルな文法
- テストを並行で実行
- 分割できない単位のテストを記述することを強制
- 暗黙のグローバルを排除
- [テストファイル毎に隔離された環境](#隔離された環境)
- [ES2015でテストコードを記述](#es2015のサポート)
- [Promiseのサポート](#promiseのサポート)
- [generatorのサポート](#generator関数のサポート)
- [asyncのサポート](#async関数のサポート)
- [Observableのサポート](#observableのサポート)
- [強化されたassert](#強化されたassert)
- [オプション付きのTAP出力](#任意のtapの出力)
- [明快なスタックトレース](#明快なスタックトレース)


## テストの構文

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## 使い方

#### 初期設定

AVAを`$ npm install --global ava`でグローバルにインストールして、`$ ava --init`を実行してpackage.jsonにAVAを追加するか手動で作成します。

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.11.0"
	}
}
```

#### テストファイルの作成

`test.js`という名前のファイルをプロジェクトのルートディレクトリに作ってください。

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');

	t.is(await bar, 'bar');
});
```

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

#### 実行

```
$ npm test
```


## CLIの使い方

```
$ ava --help

  Usage
    ava [<file|folder|glob> ...]

  Options
    --init           Add AVA to your project
    --fail-fast      Stop after first test failure
    --serial, -s     Run tests serially
    --require, -r    Module to preload (Can be repeated)
    --tap, -t        Generate TAP output
    --verbose, -v    Enable verbose output
    --no-cache       Disable the transpiler cache

  Examples
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments:
  test.js test-*.js test/**/*.js
```

ディレクトリはデフォルトで再帰的です。`fixtures`や`helpers`と名付けられたディレクトリは無視され、`_`で始まるファイルも同様です。これはテストファイルと同じディレクトリにヘルパーを置くのに役立ちます。

`npm test`を実行するときに、引数を使って`npm test test2.js`のようにテストの位置を直接に渡すことができます。しかし、フラグは`npm test -- --verbose`のように渡されなきゃなりません。

*警告: 非標準の振る舞い:* AVA CLIは常にプロジェクトローカルにインストールされたAVAを使います。それは、グローバルの`ava`コマンドを実行しても変わりません。この非標準の振る舞いは重要な[issue](https://github.com/sindresorhus/ava/issues/157)を解決するもので、日常的に利用する際には影響がないはずです。

## 設定

CLIの全てのオプションは、`package.json`の`ava`のセクションで設定できます。これは、`ava`コマンドのデフォルトの振る舞いを変更することを許可しするので、繰り返して同じオプションをコマンドプロンプトでタイプする必要がなくなります。

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "failFast": true,
    "serial": true,
    "tap": true,
    "verbose": true,
    "require": [
      "babel-core/register"
    ]
  }
}
```

CLIに渡される引数は常に`package.json`の設定よりも優先されます。

## ドキュメント

テストは非同期で実行され、サポートされたasyncオブジェクト(promiseか[observable](https://github.com/zenparsing/zen-observable))で返すことを要求します。私たちは、[async関数](#async関数のサポート)を使うことを*強く*推奨します; それがasyncのコードを簡潔で読みやすいものにして、暗黙のうちにpromiseを返すので、自分でそうする必要はありません。

上述のサポート対象のasyncオブジェクトの中の1つを返さなければ、同期で即座に終了すると考えられます。

promiseかその他のサポート対象のasyncオブジェクトを使用できなければ、`test.cb([title], fn)`でテストを記述することで"コールバックモード"を有効にできます。この方法で宣言されるテストは、`t.end()`によって手動で終了**しなければなりません**。このモードは主にコールバックスタイルのAPIをテストするためのものです。

全てのテストは同期で定義しなければなりません。それらは`setTimeout`や`setImmediate`などの中に記述することはできません。

テストのファイルはそれらのカレントディレクトリで実行されるので、[`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd)は常に[`__dirname`](https://nodejs.org/api/globals.html#globals_dirname)と同じになります。`path.join(__dirname, 'relative/path')`とする代わりに相対パスを使用することができます。

### テストの解剖

テストをつくるには、AVAから`require`した`test`関数を呼び出して、任意のテスト名とテストの実行を含む関数を与えます。渡された関数は第1引数としてコンテキストを与えられますが、それはAVAのメソッドと[アサーション](#アサーション)を呼び出すことが出来ます。


```js
test('name', t => {
	t.pass();
});
```

### 任意のテスト名

テストに名前をつけるのは任意ですが、1つ以上のテストがある場合には名前を使用することが推奨されます。

```js
test(t => {
	t.pass();
});
```

名前付き関数を代わりに選ぶことも出来ます:

```js
test(function name(t) {
	t.pass();
});
```

### アサーションプラン

アサーションプランは特定の数のアサーションを確保するのに使用されます。もっともよくある状況としては、これでアサーションの期待された数の実行前に終了しなかったテストを確認します。コールバックやループの中でアサーションがある時に有用なのが、あまりに多くのアサーションが実行されたらテストが失敗することです。

これは成功するテストになります:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	setTimeout(() => {
		t.pass();
		t.end();
	}, 100);
});
```

#### 警告: 最近の互換性に影響のある変更。

AVAは`t.plan(...)`によって自動的に終了することをもはやサポートしていません。これはアサーションを追加したときに偽陽性を防ぎますが、planの数が増加していることを忘れてください。

```js
// これはもはや動きません

test('auto ending is dangerous', t => {
	t.plan(2);

	t.pass();
	t.pass();

	// 計画された2つのアサーションに到達した後で自動終了するので、この最後の1つは見逃されます。
	setTimeout(() => t.fail(), 10000);
});
```

これを動くようにするには、今は"コールバックモード"を使って明示的に`t.end()`を呼ばなければなりません。

```js
test.cb('explicitly end your tests', t => {
	t.plan(2);

	t.pass();
	t.pass();

	setTimeout(() => {
		// この失敗は確実に捕まえられます。
		t.fail();
		t.end();
	}, 1000);
});
```

### 直列テスト

並行は素晴らしいですが、一方で並行で処理できないものもあります。そのようなまれなケースでは、並行のテストの前に直列でテストを実行することを強制する、`test.serial`を呼び出せます。

```js
test.serial(t => {
	t.pass();
});
```

### Only-tests

Only-testsはテストを限定的に実行することを強制します。開発時に幾つかのテストだけを実行したいときに有用です。

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Skip-tests

Skip-testsはスキップされたことを出力しますが、決して実行しません。

```js
test.skip('will not be run', t => {
	t.fail();
});
```

### Before & after hooks

setup とteardownの両方かいずれかが必要なとき、`test()`と同じように、`test.before()`と`test.after()`を利用できます。`test.before()`と`test.after()`に与えられたテスト関数は、全てのテストの前/後に呼び出されます。それぞれのテストにsetup/teardownが必要なときは、`test.beforeEach()`と`test.afterEach()`が利用できます。必要なだけ追加してください。失敗したときに表示されるタイトルを特定することも必要に応じて出来ます。


```js
test.before(t => {
	// 全てのテストの前に実行します
});

test.before(t => {
	// テストの前に実行しますが、上記の処理の後です
});

test.after('cleanup', t => {
	// 全てのテストの後に実行します
});

test.beforeEach(t => {
	// それぞれのテストの前に実行します
});

test.afterEach(t => {
	// それぞれのテストの後に実行します
});

test(t => {
	// 通常のテスト
});
```

これらのいずれにおいても、asyncを使うことも、asyncオブジェクトを返すことも、"コールバックモード"を有効にすることも、出来ます。

```js
test.before(async t => {
	await promiseFn();
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});

test.after(t => {
   return new Promise(/* ... */);
});
```

`beforeEach`と`afterEach`はテストでコンテキストを共有できます:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

このコンテキストは、デフォルトでは1つのオブジェクトですが、直接アサインすることも出来ます。

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

### テスト修飾子のチェーン

テストの修飾子を以下のようにチェーンすることができます。

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

これはテストで、他の修飾子の振る舞いや情報を失わないで、一時的に`skip`や`only`を使うときに、特に有用です。

### カスタムのアサーションモジュール

AVAに加えて、もしくは、代わりにどのようなアサーションモジュールを利用することもできますが、[今のところ](https://github.com/sindresorhus/ava/issues/25)`.plan()`メソッドを使うことはできません。

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### ES2015のサポート

AVAは、[Babel 6](https://babeljs.io)によってES2015をサポートしています。ES2015でテストを書いてください。他の環境整備は必要ありません。プロジェクト内でバージョンを問わずにBabelを利用することができます。私たちは、[`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) と[`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/)のプリセットで独自にバンドルしたBabelを使用しています。

#### importしたモジュールのトランスパイル

AVAは現時点では、実行するように依頼したテストだけをトランスパイルします。*テストの外から```import```したモジュールはトランスパイルしません。*このアプローチには妥当な理由がありますが、期待した挙動ではないかもしれません！

シンプルな次善策として、続けてインポートされるモジュールのトランスパイルを行うために、[Babel's require hook](https://babeljs.io/docs/usage/require/)を利用できます。

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- ES2015 で書くことが出来ます！

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/sindresorhus/ava/issues/111)は、将来性のあるエンハンスメントとして、この問題を追いかけています。


### Promiseのサポート

テストの中でpromiseを返したら、テストはpromiseがresolveしたときに終了するので、明示的にテストを終了する必要はありません。

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### generator関数のサポート

AVAは[generator](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/function*)をサポートしています。

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### async関数のサポート

AVAは[async](https://tc39.github.io/ecmascript-asyncawait/) *(async/wait)*をサポートしています。

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// asyncのアロー関数
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Observableのサポート

AVAは[Observable](https://github.com/zenparsing/es-observable)をサポートしています。
テストからobservableを返すと、AVAはテストが終了する前にobservableを自動的に完了します。

*"コールバックモード"や`t.end()`は必要ありません*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// 偶数のみ
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### コールバックのサポート

AVAはnodeスタイルのエラーファーストのコールバックAPIを使用しているとき、最後のコールバックとして`t.end`の使用をサポートしています。AVAは`t.end`に渡される第1引数をどのような真と判断される値もエラーと見なします。`t.end`は、`test.cb`を利用することによって有効になる、"コールバックモード"を必要とすることを覚えておいてください。

```js
test.cb(t => {
	// t.endは自動的に第1引数としてエラーをチェックします。
	fs.readFile('data.txt', t.end);
});
```

### 任意のTAPの出力

AVAは[TAPレポーター](https://github.com/sindresorhus/awesome-tap#reporters)を使って`--tap`オプションでTAPの出力ができます。

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

### 明快なスタックトレース

エラーの箇所をより素早く見つけられるように、AVAはスタックトレースから自動的に無関係の行を取り除きます。

<img src="https://github.com/sindresorhus/ava/raw/master/media/stack-traces.png" width="398">

## API

### test([title], body)
### test.serial([title], body)
### test.cb([title], body)
### test.only([title], body)
### test.skip([title], body)
### test.before([title], body)
### test.after([title], body)
### test.beforeEach([title], body)
### test.afterEach([title], body)

#### title

Type: `string`

テストの見出しです。

#### body(context)

Type: `function`

実際のテストを含む必要があります。

##### context

テスト関数に渡され、別個のAVAのメソッドと[アサーション](#アサーション)を含みます。

###### .plan(count)

テストの中でいくつのアサーションがあるかを計画します。計算されたアサーションと実際のアサーションの数が一致しない場合、テストは失敗します。

###### .end()

テストを終了します。`test.cb()`と一緒でのみ利用できます。


## アサーション

アサーションはテストの[コンテキスト](#context)に混ぜられます:

```js
test(t => {
	t.ok('unicorn'); // アサーション
});
```

複数のアサーションの失敗が1つのテストの中で発生したとき、AVAは*最初*の1つだけを表示します。

### .pass([message])

アサーションを通します。

### .fail([message])

アサーションを失敗します。

### .ok(value, [message])

`value`が真と判断できる値とします。

### .notOk(value, [message])

`value`が偽と判断できる値とします。

### .true(value, [message])

`value`が`true`であるとします。

### .false(value, [message])

`value`が`false`であるとします。

### .is(value, expected, [message])

`value`が`expected`と同じ値であるとします。

### .not(value, expected, [message])

`value`が`expected`と同じ値でないとします。

### .same(value, expected, [message])

`value`が`expected`と厳密に同じ値であるとします。

### .notSame(value, expected, [message])

`value`が`expected`と厳密に同じ値でないとします。

### .throws(function|promise, [error, [message]])

エラーを投げる`関数`か`promise`のrejectであるとします。

`error`は、コンストラクタ、正規表現、エラーメッセージ、バリデーション関数などにも出来ます。

### .notThrows(function|promise, [message])

`error`を投げない`関数`か`promise`のresolveであるとします。

### .regex(contents, regex, [message])

`contents`が`regex`にマッチするとします。

### .ifError(error, [message])

`error`が偽と判断できる値とします。

## アサーションのスキップ

`skip`修飾子を使うことでどのようなアサーションもスキップできます。スキップされたアサーションはカウントされますので、計画したアサーションの数を変更する必要はありません。


```js
test(t => {
  t.plan(2);
  t.skip.is(foo(), 5); // スキップするときにplanの数を変更する必要はありません。
  t.is(1, 1);
});
```

## 強化されたassert

AVAには、より叙述的なアサーションメッセージを与える、[`power-assert`](https://github.com/power-assert-js/power-assert)がビルトインとして付属してます。それは、テストを読んで、コードからより多くの情報を推論しようとします。

以下のようなテストの場合:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

通常通り手助けにならない出力が得られます:

```
false === true
```

強化されたassertでは以下のような結果が得られます:

```
t.ok(x === 'bar')
     |
     "foo"
```

このような場合には`t.is()`を使えて、またそうすべきでしょうが、これはただの簡単な例です。

さらに高度な例を試しましょう:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

その結果は以下です:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

全てのassertのメソッドは強化されます。

楽しんでください!


## 隔離された環境

それぞれのテストファイルは分離されたNode.jsのプロセスで実行されます。これには多くの利点があります。それぞれのテストファイルはお互いに影響し合うことがありません。グローバル環境でモック、ビルトインを上書きしたり等。ですが、このようになっているのは、主にパフォーマンスが理由です。Node.jsは非同期IOを同時に実行できますが、テストがメインスレッドをブロックするような、同期処理で重い場合は助けになりません。テストを同時に実行して並行でテストすることで、現代システムの利点を最大限に活用します。


## Tips

### 一時ファイル

テストを並行で行うことは幾つかの課題が伴いますが、IOがその1つです。一般的に、直列のテストはテストのカレントディレクトリに一時的なディレクトリを作成して、終了時にそれを削除します。テストがお互いに競合するので並行でそれを行うことはできません。正しく行うには、それぞれが新しい一時的なディレクトリを利用することです。[`tempfile`](https://github.com/sindresorhus/tempfile)や[`temp-write`](https://github.com/sindresorhus/temp-write)といったモジュールが役立つでしょう。

### デバッグ

AVAはデフォルトで並列にテストを実行しますが、何かしらデバッグする必要があるときには次善策です。代わりに`--serial`オプションをつけてテストを直列で実行してください:

```
$ ava --serial
```

### コードカバレッジ

AVAが[実行するテストファイル](#隔離された環境)のコードカバレッジに[`istanbul`](https://github.com/gotwarlost/istanbul)を使うことは出来ませんが、代わりに基本的にサブプロセスのサポートがある`istanbul`である[`nyc`](https://github.com/bcoe/nyc)を利用できます。

バージョン`5.0.0`では、トランスパイルに関係なく、実際のコードのカバレッジをレポートするのにソースマップを使います。テスト対象のコードがインラインのソースマップかソースマップファイルへの参照のいずれかを含んでいることを確認してください。`babel/register`を使っていれば、`.babelrc`で`sourceMaps`オプションを`inline`に設定することができます。


## FAQ

### なぜ`mocha`、`tape`、`node-tap`などではないのか?

Mochaはデフォルトのインターフェース(多くの人々が利用している)で`describe`や`it`のような暗黙のグローバルを利用することを必須としていて、主張が無さすぎて、肥大化していて、デフォルトで同期で、プログラム的なAPIもなくて、直列でテストを実行して、そして遅いです。Tapeやnode-tapはかなり良いです。AVAはそれらのシンタックスに強く影響を受けています。ですが、それらはいずれも、テストを直列で実行し、[TAP](https://testanything.org/)を第1級オブジェクトとして作り上げ、私の見方ですが、それがそのコードベースを複雑で結合したものにしました。TAPの出力は読みづらいので、外部のレポーターを利用することになります。AVAは自己主張が強く並行です。デフォルトの簡単なレポーターがあり、CLIのフラグを通じてTAPもサポートしています。

### どうやってカスタムレポーターを使いますか？

[TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters)の中で一つを[`--tap` フラグ](#任意のtapの出力)から使ってください。

### 名前はどのように書いて、どう発音するのか?

AVAで、Avaやavaではありません。発音は[`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true)です。

### ヘッダーの背景は何?

[アンドロメダ銀河です。](https://ja.wikipedia.org/wiki/%E3%82%A2%E3%83%B3%E3%83%89%E3%83%AD%E3%83%A1%E3%83%80%E9%8A%80%E6%B2%B3)

### 並行 vs. 並列

[並行は並列ではありません。それは並列を可能にするものです。](http://stackoverflow.com/q/1050222)

## レシピ

- [コードカバレッジ](docs/recipes/code-coverage.md)
- [エンドポイントのテスト](docs/recipes/endpoint-testing.md)
- [`t.plan()`をいつ使うのか](docs/recipes/when-to-use-plan.md)

## サポート

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)

## その他

- [AVAのロゴステッカー](https://www.stickermule.com/user/1070705604/stickers)

## 関連リンク

- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - gulpでテストを実行
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - gruntでテストを実行
- [fly-ava](https://github.com/pine613/fly-ava) - flyでテストを実行
- [start-ava](https://github.com/start-runner/ava) - startでテストを実行

## 開発者

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage)
---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage)

#### 元メンバー

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
