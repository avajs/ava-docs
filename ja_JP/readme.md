___
**訳注**

これは[readme.md](https://github.com/avajs/ava/blob/master/readme.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/avajs/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9)になります(このリンクをクリックして、`readme.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# ![AVA](https://github.com/avajs/ava/blob/master/media/header.png)

> 未来型のテストランナー

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/avajs/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/avajs/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/avajs/ava?branch=master) [![Gitter](https://img.shields.io/badge/Gitter-Join_the_AVA_chat_%E2%86%92-00d06f.svg)](https://gitter.im/avajs/ava)

JavaScript自体はシングルスレッドですが、Node.jsにおけるIOは、その非同期の性質によって並列で実行可能です。AVAは、この事が利点であり、テストを同時に実行することができ、特にIOが重いテストで効果があります。また、テストファイルは独立したプロセスで並列して実行することで、より良いパフォーマンスと各テストファイル毎に分離された環境にすることができます。PageresにおけるMochaからAVAへの[切り替え](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0)によって、テストにかかる時間を31秒から11秒まで減らしました。並列実行するテストにすると、大域の状態や他のテストの状態から影響を受けないことを意味するようになる粒度が極小のテストを書くようになります。これはすごい事ですよね!

*Issueやプルリクエストなどで貢献したい場合は、[コントリビューションガイド](contributing.md)を読んでください。

アップデートの情報のために[AVAのTwitterアカウント](https://twitter.com/ava__js)をフォローしてください。

翻訳: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md)


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

- 軽量で高速
- テストのシンプルな文法
- テストを並列で実行
- 粒度が極小のテストを書かせる
- 暗黙のグローバルを排除
- [テストファイル毎に分離された環境](#分離された環境)
- [ES2015でテストコードを記述可能](#es2015のサポート)
- [Promiseのサポート](#promiseのサポート)
- [Generatorのサポート](#generator関数のサポート)
- [Asyncのサポート](#async関数のサポート)
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

AVAを`$ npm install --global ava`でグローバルにインストールして、`$ ava --init`(他にどんなオプションでも付けられます)を実行してpackage.jsonにAVAを追加するか手動で作成します。

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

<img src="https://github.com/avajs/ava/blob/master/media/screenshot.png" width="150" align="right">

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

*CLIはグローバルで実行できる場合でも、実行可能であればローカルにインストールされたAVAを実行します。*

ディレクトリはデフォルトで再帰的です。`fixtures`や`helpers`と名付けられたディレクトリは無視され、`_`で始まるファイルも同様です。これはテストファイルと同じディレクトリにヘルパーを置くのに役立ちます。

`npm test`を実行するときに、引数を使って`npm test test2.js`のようにテストの位置を直接渡すことができます。しかし、フラグは`npm test -- --verbose`のように渡す必要があります。

## 設定

CLIの全てのオプションは、`package.json`の`ava`のセクションで設定できます。この設定で`ava`コマンドのデフォルトの動作を変更することができるので、同じオプションをコマンドプロンプトで繰り返してタイプする必要がなくなります。

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

テストは非同期で実行され、サポートされたasyncオブジェクト(promiseか[observable](https://github.com/zenparsing/zen-observable))で返す必要があります。私たちは、[async関数](#async関数のサポート)を使うことを*強く*推奨します; これはasyncのコードを簡潔で読みやすいものにして、暗黙のうちにpromiseを返すので、自分では明示的に返す必要はありません。

上記のサポート対象のasyncオブジェクトの中の1つも返さなければ、同期で即座に終了すると考えられます。

promiseかその他のサポート対象のasyncオブジェクトを使用できなければ、`test.cb([title], fn)`でテストを記述することで"コールバックモード"を有効にできます。この方法で宣言されるテストは、`t.end()`によって手動で終了**しなければなりません**。このモードは主にコールバックスタイルのAPIをテストするためのものです。

全てのテストは同期で定義しなければなりません。テストは`setTimeout`や`setImmediate`などの中に記述することはできません。

テストのファイルはそれらのカレントディレクトリで実行されるので、[`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd)は常に[`__dirname`](https://nodejs.org/api/globals.html#globals_dirname)と同じになります。`path.join(__dirname, 'relative/path')`とする代わりに相対パスを使用することができます。

### テストの解剖

テストをつくるには、AVAから`require`した`test`関数を呼び出して、任意のテスト名とテストの実行する関数を与えます。渡された関数は第1引数としてコンテキストを与えられますが、ここではAVAのメソッドと[アサーション](#アサーション)を呼び出すことが出来ます。


```js
test('name', t => {
	t.pass();
});
```

### 任意のテスト名

テストに名前をつけるのは任意ですが、1つ以上のテストがある場合には名前を使用することを推奨します。

```js
test(t => {
	t.pass();
});
```

また名前付き関数をテスト名の代わりに選ぶことも出来ます:

```js
test(function name(t) {
	t.pass();
});
```

### アサーションプラン

アサーションプランは特定数のアサーションを確保するのに使用されます。一番よくある状況としては、これでアサーションの期待された数の実行前に終了しなかったテストを確認するような状況です。コールバックやループの中でアサーションがある時に有用なのが、あまりに多くのアサーションが実行されたらテストが失敗することです。注意してほしいのは、AVAはnode-tapやtapeとは違い、計画されたアサーション数に到達しても自動終了は*しない*という点です。

これは成功するテストになります:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```


### 直列テスト

並列実行は素晴らしいものですが、一方で並列で処理できないものもあります。このようなまれなケースでは、並列のテストの前に直列でテストを強制的に実行する、`test.serial`を呼び出すことができます。

```js
test.serial(t => {
	t.pass();
});
```

### Only-tests

Only-testsはテストを限定的に実行することを強制します。開発時に幾つかのテストだけを実行したいときに役立ちます。

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Skip-tests

Skip-testsはスキップされたことを出力はしますが、絶対に実行しません。

```js
test.skip('will not be run', t => {
	t.fail();
});
```

### Before & after hooks

setupとteardownの両方または、いずれかが必要なとき、`test()`と同じように、`test.before()`と`test.after()`を利用できます。`test.before()`と`test.after()`に与えられたテスト関数は、全てのテストの前/後で呼び出されます。また各テストにsetup/teardownが必要なときは、`test.beforeEach()`と`test.afterEach()`が利用できます。必要なだけ追加してください。失敗したときに表示されるタイトルを指定することも必要に応じて出来ます。


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

どのフックでも、asyncを使うこと、asyncオブジェクトを返すこと、"コールバックモード"を有効にすることも、出来ます。

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

`beforeEach`と`afterEach`のフックは特定のテストでコンテキストを共有できます:

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

これはテストで、他の修飾子の振る舞いや情報を失わないように一時的に`skip`や`only`を使うときに、特に役立ちます。

### カスタムアサーションモジュール

AVAのデフォルトに加えて、または代わりとしてどのようなアサーションモジュールを利用することも可能ですが、[今はまだ](https://github.com/avajs/ava/issues/25)`.plan()`メソッドを使うことはできません。

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### ES2015のサポート

AVAは、[Babel 6](https://babeljs.io)を使ってES2015をサポートしています。単にES2015でテストを書いてください。他の環境整備は必要ありません。プロジェクト内でバージョンを問わずにBabelを利用することができます。AVAは、[`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) と[`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/)のプリセットをバンドルしたBabelを使用しています。

#### importしたモジュールのトランスパイル

現時点でのAVAは、実行するようにしたテストだけをトランスパイルします。*テストの外から```import```したモジュールはトランスパイルしません。*このアプローチを採用したのには妥当な理由がありますが、期待した挙動ではないかもしれません！

シンプルな代替策として、[Babel's require hook](https://babeljs.io/docs/usage/require/)を使ってインポートされるモジュールをリアルタイムにトランスパイルすることができます。AVAはES2015モジュールをサポートしてるのでrequireフックとして使用ができるからです:

```js
import test from 'ava';
import 'babel-core/register';
import foo from './foo'; // <-- ES2015 で書くことが出来ます！

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

[#111](https://github.com/avajs/ava/issues/111)が、将来性のある拡張として、議論されています。


### Promiseのサポート

テスト中でpromiseを返すと、promiseがresolveしたときに終了するので、明示的にテストを終了する必要はありません。

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### generator関数のサポート

AVAは[generator関数](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/function*)をビルトインサポートしています。

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### async関数のサポート

AVAは[async](https://tc39.github.io/ecmascript-asyncawait/) *(async/wait)*をビルトインサポートしています。

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

AVAは[Observable](https://github.com/zenparsing/es-observable)をビルトインサポートしています。
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

AVAはnodeスタイルのエラーファーストのコールバックAPIを使用したときに、最終コールバックとして`t.end`の使用をサポートしています。AVAは`t.end`に渡される第1引数がどんな真と判断される値もエラーと見なします。`t.end`は、`test.cb`チェーンを利用することで有効になる、"コールバックモード"が必要になるということを覚えておいてください。

```js
test.cb(t => {
	// t.endは自動的に第1引数としてエラーをチェックします。
	fs.readFile('data.txt', t.end);
});
```

### 任意のTAPの出力

AVAは`--tap`オプションの指定でどのような[TAPレポーター](https://github.com/sindresorhus/awesome-tap#reporters)を使用しても、TAPの出力をすることができます。

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/blob/master/media/tap-output.png" width="398">

### 明快なスタックトレース

エラーの箇所をより素早く見つけられるように、AVAはスタックトレースから自動的に無関係の行を取り除きます。

<img src="https://github.com/avajs/ava/raw/master/media/stack-traces.png" width="398">

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

テストのタイトルです。

#### body(context)

Type: `function`

実際にテストが含まれている必要があります。

##### context

テスト関数に渡され、個別のAVAのメソッドと[アサーション](#アサーション)が含まれます。

###### .plan(count)

テストの中でいくつのアサーションがあるかを計画します。計画されたアサーションと実際のアサーションの数が一致しない場合、テストは失敗します。

###### .end()

テストを終了します。`test.cb()`と一緒に使用しないと動きません。


## アサーション

アサーションはテストの[コンテキスト](#context)に合成されています:

```js
test(t => {
	t.ok('unicorn'); // アサーション
});
```

1つのテストの中で複数のアサーションの失敗が発生したとき、AVAは*最初*の1つだけを表示します。

### .pass([message])

アサーションを通します。

### .fail([message])

アサーションを失敗させます。

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

`value`が`expected`と同じ値ではないとします。

### .same(value, expected, [message])

`value`が`expected`と厳密に同じ値であるとします。

### .notSame(value, expected, [message])

`value`が`expected`と厳密に同じ値ではないとします。

### .throws(function|promise, [error, [message]])

エラーを投げる`function`か`promise`のrejectであるとします。

`error`は、コンストラクタ、正規表現、エラーメッセージ、バリデーション関数などでも大丈夫です。

### .notThrows(function|promise, [message])

`error`を投げない`function`か`promise`のresolveであるとします。

### .regex(contents, regex, [message])

`contents`が`regex`にマッチするとします。

### .ifError(error, [message])

`error`が偽と判断できる値とします。

## アサーションのスキップ

`skip`修飾子を使うことでどのアサーションもスキップできます。スキップされたアサーションはカウントされるので、計画したアサーションの数を変更する必要はありません。


```js
test(t => {
  t.plan(2);
  t.skip.is(foo(), 5); // スキップするときにplanの数を変更する必要はありません。
  t.is(1, 1);
});
```

## 強化されたassert

AVAには、より説明的なアサーションメッセージを与える、[`power-assert`](https://github.com/power-assert-js/power-assert)がビルトインとして付属してます。これにより、テストを読んでよりコード内部の情報を得ることができます。

以下のようなテストの場合:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

この場合、普通はほとんど手助けにならない出力になります:

```
false === true
```

強化されたassertでは以下のような結果が得られます:

```
t.ok(x === 'bar')
     |
     "foo"
```

実際はこのような場合には`t.is()`を使えますし、そうしたほうが良いですが、これはただの簡単な例です。

もっと高度な例を試しましょう:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

この結果は以下です:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

全てのassertのメソッドは強化されています。

楽しんでください!


## 分離された環境

それぞれのテストファイルは分離されたNode.jsのプロセスで実行されます。これには多くの利点があります。それぞれのテストファイルは互いに影響することはありません。グローバル環境でのテストファイルのモック使用、ビルトインの機能を上書きしたりなどなどです。しかし、主にパフォーマンスの理由によりこうしています。Node.jsは非同期IOを同時に実行できますが、テストがメインスレッドをブロックするような、重い同期処理の場合はあまり役に立ちません。テストを同時に実行して並列でテストすることで、モダンシステムの利点を最大限に活用できます。


## Tips

### 一時ファイル

テストを並列で行うことには色々な問題が伴いますが、IOがその1つです。一般的に、直列のテストではカレントディレクトリに一時的なディレクトリを作成して、終了時にそれを削除します。並列ではテストがお互いに競合するのでこれはできません。正しいやり方としては、それぞれのテストが新規に一時ディレクトリを利用することです。[`tempfile`](https://github.com/sindresorhus/tempfile)や[`temp-write`](https://github.com/sindresorhus/temp-write)といったモジュールが役立つでしょう。

### デバッグ

AVAはデフォルトで並列にテストを実行しますが、何かデバッグする必要があるときには最善ではありません。代わりに`--serial`オプションをつけてテストを直列で実行してください:

```
$ ava --serial
```

### コードカバレッジ

AVAが[実行するテストファイル](#分離された環境)のコードカバレッジに[`istanbul`](https://github.com/gotwarlost/istanbul)を使うことは出来ませんが、代わりにサブプロセスのサポートがある`istanbul`である[`nyc`](https://github.com/bcoe/nyc)を利用できます。

バージョン`5.0.0`では、トランスパイルに関係なく、実際のコードのカバレッジをレポートするのにソースマップを使います。テスト対象のコードがインラインのソースマップかソースマップファイルへの参照のいずれかを含んでいることを確認してください。`babel/register`を使っていれば、`.babelrc`で`sourceMaps`オプションを`inline`に設定できます。


## FAQ

### なぜ`mocha`、`tape`、`node-tap`ではダメなのか？

Mochaはデフォルトのインターフェース(多くの人々が利用している)で`describe`や`it`のような暗黙のグローバルを利用することを必須になっており、強い主張もなく、肥大化していて、デフォルトで同期で、プログラム的なAPIもなくて、直列でテストを実行して、そして遅いです。Tapeやnode-tapはかなり良いです。AVAはそれらのシンタックスに強く影響を受けています。ですが、これらはいずれも、テストを直列で実行し、[TAP](https://testanything.org/)を第1級オブジェクトとして作り上げ、私の見方ですが、これがコードベースを複雑で結合したものにしました。TAPの出力は読みづらいので、外部のレポーターを利用することになります。AVAは自己主張が強く並列です。デフォルトの簡単なレポーターがあり、CLIのフラグを通じてTAPもサポートしています。

### どうやってカスタムレポーターを使いますか？

[TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters)の中の何でも一つを[`--tap` フラグ](#任意のtapの出力)から使ってください。

### 名前はどのように書いて、どう発音するのか?

AVAで、Avaやavaではありません。発音は[`/ˈeɪvə/` ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true)です。

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
- [Gitter chat](https://gitter.im/avajs/ava)
- [Twitter](https://twitter.com/ava__js)


## 関連リンク

- [gulp-ava](https://github.com/avajs/gulp-ava) - gulpでテストを実行
- [grunt-ava](https://github.com/avajs/grunt-ava) - gruntでテストを実行
- [fly-ava](https://github.com/pine/fly-ava) - flyでテストを実行
- [start-ava](https://github.com/start-runner/ava) - startでテストを実行

## リンク

- [AVAステッカー購入](https://www.stickermule.com/user/1070705604/stickers)

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
	<img src="https://cdn.jsdelivr.net/gh/avajs/ava@fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
