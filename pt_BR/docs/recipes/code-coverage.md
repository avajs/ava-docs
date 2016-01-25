# Cobertura de código

Traduções: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/code-coverage.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md)

Como o AVA [gera os arquivos de teste][isolated-env], você não pode usar [`istanbul`] para cobertura de código; mas você pode conseguir isso com [`nyc`], que é basicamente [`istanbul`] com suporte para subprocessos.

## Configuração

Primeiro, instale o NYC:

```
$ npm install nyc --save-dev
```

Em seguida, adicione os diretórios `.nyc_output` e `coverage` a seu arquivo `.gitignore`.

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## Cobertura ES5

Usar NYC para fornecer cobertura para o código de produção escrito em ES5 é simples. Basta preceder seu script de teste com `nyc`:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

É isso!

Se você deseja criar relatórios de cobertura HTML, ou carregar dados de cobertura para o Coveralls, você deve pular para as seções abaixo.

## ES2015 coverage

Usar Babel para transpilar seu código de produção é um pouco mais complicado. Aqui, dividimos essa tarefa em vários passos.

### Configure o Babel

Em primeiro lugar, precisamos de uma configuração do Babel. O que se segue é apenas um exemplo. Você terá que modificá-lo para atender às suas necessidades.

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

Há duas coisas importantes a serem observadas a partir do exemplo acima.

1. Nós ignoramos arquivos de teste porque o AVA já faz o transpiling de testes para você.

2. Nós especificamos source maps `inline` para o desenvolvimento. Isso é importante para gerar corretamente a cobertura. Usando a seção `env` da configuração do Babel nos permite desativar os source maps para compilações de produção.

### Criar um script de compilação

Como é pouco provável que você queira source maps `inline` em seu código de produção, você deve especificar uma variável de ambiente alternativa em seus scripts de compilação:

`package.json`

```json
{
	"scripts": {
		"build": "BABEL_ENV=production babel --out-dir=dist index.js"
	}
}
```

> ATENÇÃO: `BABEL_ENV=production` não funciona no Windows, você deve usar a palavra-chave `set` (`set BABEL_ENV=production`). Para compilações multiplataforma, confira [`cross-env`].

Observe que o script de compilação realmente tem muito pouco a ver com o AVA, e é apenas uma demonstração de como usar a configuração `env` do Babel para manipular sua configuração, de modo a troná-lo compatível com o AVA.

### Utilize o require hook do Babel

Para utilizar o hook require do Babel, adicione `babel-core/register` à seção `require` da configuração de seu AVA em `package.json`.

```json
{
	"ava": {
		"require": ["babel-core/register"]
	}
}
```

*Observação*: Você também pode definir o hook require pela linha de comando: `ava --require=babel-core/register`. No entanto, configurando-o em `package.json` evita que você precise digitar repetidamente essa flag.

### Colocando tudo junto

Combinando os passos acima, seu `package.json` completo deve ser algo parecido com isto:

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

## Relatórios HTML

NYC cria um arquivo de cobertura `json` para cada processo bifurcado no diretório `.nyc_ouput`.

Para combiná-los em um relatório HTML legível, faça o seguinte:

```
$ ./node_modules/.bin/nyc report --reporter=html
```

Ou, utilize um script npm para poupar digitação:

```json
{
	"scripts": {
		"report": "nyc report --reporter=html"
	}
}
```

Isto produzirá um arquivo HTML no diretório `coverage`.

## Cobertura hospedada

### Travis CI & Coveralls

Primeiro, você deve logar em [coveralls.io] e ativar seu repositório.

Depois disso, adicione [`coveralls`] como uma dependência de desenvolvimento:

```
$ npm install coveralls --save-dev
```

Em seguida, adicione o seguinte ao seu `.travis.yml`:

```yaml
after_success:
	- './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

Seu relatório de cobertura aparecerá, então, no coveralls pouco após o Travis ser concluído.

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[isolated-env]: https://github.com/sindresorhus/ava#isolated-environment
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
