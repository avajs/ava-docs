# Pokrycie kodu

Tłumaczenia: [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/code-coverage.md)

Użyj [`nyc`] aby obliczyć zasięg kodu twoich testów.

Najpierw zainstaluj [`nyc`]:

```
$ npm install --save-dev nyc
```

W najprostszym przypadku uruchom AVA [`nyc`]. W twoim pliku `package.json`:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Możesz wykluczyć katalogi `.nyc_output` i `coverage` z kontroli źródła. Zakładając, że używasz Git, dodaj następujące elementy do swojego pliku `.gitignore`:

```
.nyc_output
coverage
```

[`nyc`]: https://github.com/istanbuljs/nyc
