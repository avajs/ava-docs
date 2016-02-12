___
**訳注**

これは[code-coverage.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/sindresorhus/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6)になります(このリンクをクリックして、`code-coverage.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# コードカバレッジ

AVAは[テストファイルの実行を隔離された環境で行う][isolated-env]ので、コードカバレッジに[`istanbul`]を使うことはできません。代わりに、基本的にはサブプロセスのサポートがある[`istanbul`]である[`nyc`]で代用できます。なので、まずこれをインストールする必要があります:

```
npm install nyc --save-dev
```

## 設置

まず、NYCを設置します:

```
$ npm install nyc --save-dev
```

続いて、`.nyc_output`と`coverage`ディレクトリを`.gitignore`に追加します。

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## ES5のカバレッジ

ES5をカバーするには、単純にテストスクリプトの先頭に`nyc`を追加します。このnpmスクリプトがコードカバレッジとテストを扱います:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

これで完了です。

もしHTMLのカバレッジレポートが欲しい場合や、カバレッジデータをCoverallsにアップロードしたい場合なら、下のセクションはスキップしてください。

## ES2015のカバレッジ

Babelで製造段階のコードをトランスパイルする場合はもう少しやります。いくつかのステップに分けておきました。

### Babelを設定する

まず、babelの設定が必要です。下は単なる例題です。後で各自の必要に合わせて修正をしてください。

`package.json`:
```json
{
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	}
}
```

上の例で２つの注目するべきであることがあります。

1. AVAがすでにテストをtranspileしているので、テストファイルは無視します。

2. 開発を用意にするため、`inline`ソースマップを設定します。これは適切なカバレッジを確保るために重要です。`env`を使うことで、productionの場合、source mapを生成することを防いでくれます。

### ビルドスクリプトの追加

`inline`ソースマップをせいぞ段階のコードには生成しないようにビルドスクリプトに環境変数を特定します。:

`package.json`
```json
{
	"scripts": {
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	}
}
```
> 注意: `BABEL_ENV=production` はWindowsでは使えないので、`set`を使わなきゃなりません。(`set BABEL_ENV=production`). クロスプラットフォームビルドが必要であれば、[`cross-env`]を確認してください。

これはAVAにはあまり影響しない部分であり、Babelの`env`からAVAに対応できるように設定を操作する例です。

### Babelのrequire hook

Babelのrequire hookを使うには、`package.json`のAVAの設定の`require`に`babel-core/register`を追加してください。

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

*参考*: コマンドラインからrequire hookを設定することもできます: `ava --require=babel-core/register`。しかし、`package.json`に入れると毎回フラグを追加しなくていいです。

### まとめて

上記のステップをまとめると`package.json`は次のようになります:

```json
{
	"scripts": {
		"test": "nyc ava",
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	},
	"babel": {
		"presets": ["es2015"],
		"plugins": ["transform-runtime"],
		"ignore": "test.js",
		"env": {
			"development": {
				"sourceMaps": "inline"
			}
		}
	},
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

## HTMLレポート

NYCは`json`カバレッジファイルをプロセス毎に`.nyc_ouput`ディレクトリに作ります。

読みやすいHTMLレポートにまとめるには次のようにします:

```
$ ./node_modules/.bin/nyc report --reporter=html
```

または、より少ないタイプで済ませるためにnpmスクリプトに変換してください:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

または、より少ないタイプで済ませるためにnpmスクリプトに変換してください:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

これで`coverage`ディレクトリにHTMLファイルを出力します。


## ホストされたカバレッジ

### Travis CIとCoveralls

最初に、coverallsでリポジトリを認証する必要があります。それが完了したら、開発時の依存モジュールとして[`coveralls`]を追加してください。

```
npm install coveralls --save-dev
```

そして`.travis.yml`に以下の内容を追加してください。

```
after_success:
	- './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

カバレッジレポートはCIサービスが完了した後すぐにcoverallsに表示されます。

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[isolated-env]: https://github.com/sindresorhus/ava#isolated-environment
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
