___
**訳注**

これは[code-coverage.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/code-coverage.md)の日本語訳です。こちらがAVAのmasterブランチとの差分の[リンク](https://github.com/sindresorhus/ava/compare/66aeefb5a2b90d90a82f91b32b6c202f2b2567a2...master#diff-cb1a0a79c5c751cd6b2568e604d23237ff8eb85f)になります(このリンクをクリックして、`code-coverage.md`に変更点が見当たらなければ、この翻訳が最新であることを意味します)。
___

# コードカバレッジ

AVAは[テストファイルの実行を隔離された環境で行う][isolated-env]ので、コードカバレッジに[`istanbul`]を使うことはできません。代わりに、基本的にはサブプロセスのサポートがある[`istanbul`]である[`nyc`]で代用できます。なので、まずこれをインストールする必要があります:

```
npm install nyc --save-dev
```

ES2015とES5の両方の環境で、`.gitignore`に`.nyc_output`と`coverage`を追加することを忘れないでください。


## ES5のカバレッジ

ES5をカバーするには、単純にテストスクリプトの先頭に`nyc`を追加します。このnpmスクリプトがコードカバレッジとテストを扱います:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```


## ES2015のカバレッジ

まず、babelの設定が必要です。これは開発者によって異なりますが、この`package.json`のbabelの設定を出発点として使うことができます。

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

開発段階では、コードをトランスパイルする際にソースマップを特定する必要があり、製造段階ではそれが必要無くなります。なので、プロダクションのスクリプトでは、development以外の環境を使用してください。例えば:

```json
{
	"scripts": {
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	}
}
```

ES6をカバーするには、単純にテストスクリプトの先頭に`nyc`と`--babel`フラグを追加します。このnpmスクリプトがコードカバレッジとテストを扱います。

```json
{
	"scripts": {
		"test": "nyc --babel --reporter=text ava"
	}
}
```


## HTMLレポート

私たちが筋書きを書いた、ES6かES5のいずれかのカバレッジの戦略をHTMLレポートを確認するには、以下のようにしてください:

```
nyc report --reporter=html
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
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[isolated-env]: https://github.com/sindresorhus/ava#isolated-environment
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
