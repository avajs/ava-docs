___
**Nota do tradutor**

Esta é a tradução de [readme.md](https://github.com/avajs/ava/blob/master/readme.md). [Este link](https://github.com/avajs/ava/compare/24a38acc5b1b3e3deb77af54d90dda4677bd7749...master) compara a versão em que se baseou esta tradução com a última versão disponível no branch `master` do AVA. Se não houver mudanças em `readme.md`, então a tradução está atualizada.
___

# [![AVA](https://raw.githubusercontent.com/avajs/ava/master/media/header.png)](https://ava.li)

> Executor de testes futurista

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![Dependency Status](https://dependencyci.com/github/avajs/ava/badge)](https://dependencyci.com/github/avajs/ava) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/avajs/ava)

Apesar de o JavaScript ser single-threaded, as IO no Node.js podem acontecer em paralelo devido à sua natureza assíncrona. AVA aproveita-se disso e executa os testes simultaneamente, o que é especialmente favorável para testes pesados de IO. Além disso, arquivos de teste são executados em paralelo como processos separados, oferencendo ainda melhor desempenho e um ambiente isolado para cada arquivo de teste. [Migrar]( https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha para AVA no Pageres reduziu o tempo de teste de 31 para 11 segundos. Testes executados simultaneamente forçam a escrever testes atômicos, ou seja, testes que não dependem do estado global ou do estado de outros testes, o que representa um grande diferencial!

![](https://raw.githubusercontent.com/avajs/ava/master/media/screenshot-mini-reporter.gif)

*Leia nosso [guia para contribuir](contributing.md) se deseja contribuir (issues/PRs/etc).*

Siga o [AVA no Twitter](https://twitter.com/ava__js) para atualizações.

Traduções: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md)


## Índice

- [Utilização](#utilização)
- [Utilização do CLI](#cli)
- [Reporters](#reporters)
- [Configuração](#configuração)
- [Documentação](#documentação)
- [API](#api)
- [Asserções](#asserções)
- [Dicas](#dicas)
- [Perguntas Frequentes](#perguntas-frequentes)
- [Receitas](#receitas)
- [Suporte](#suporte)
- [Relacionados](#relacionadas)
- [Links](#links)
- [Time](#time)

## Por que AVA?

- Mínimo e rápido
- Sintaxe de teste simples
- Executa testes simultaneamente
- Induz a escrever testes atômicos
- Sem globais implícitas
- [Ambiente isolado para cada arquivo de teste](#isolamento-de-processo)
- [Escreva seus testes em ES2015](#suporte-para-es2015)
- [Suporte para promises](#suporte-para-promises)
- [Suporte para função geradora](#suporte-para-função-geradora)
- [Suporte para funções assíncronas](#suporte-para-funções-assíncronas)
- [Suporte para Observável](#suporte-para-observável)
- [Mensagens de asserção melhoradas](#mensagens-de-asserção-melhoradas)
- [Saída opcional TAP](#saída-opcional-tap)
- [Rastreamentos limpos de Pilha](#rastreamentos-limpos-de-pilha)

## Sintaxe de teste

```js
import test from 'ava';

test(t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## Utilização

### Adicione AVA ao seu projeto

Instale o AVA globalmente e o execute com `--init` para adicionar o AVA a seu `package.json`:

```console
$ npm install --global ava
$ ava --init
```

O seu `package.json` então vai ficar assim:

```json
{
  "name": "awesome-package",
  "scripts": {
    "test": "ava"
  },
  "devDependencies": {
    "ava": "^0.15.0"
  }
}
```

Quaisquer argumentos passados após `--init` são adicionados como configurações ao `package.json`.

#### Manual de instalação

Você também pode instalar o AVA diretamente:

```console
$ npm install --save-dev ava
```

Você terá que configurar o script `test` em seu `package.json` para usar `ava` (veja acima).

### Crie seu arquivo de teste

Crie um arquivo chamado `test.js` no diretório raíz de seu projeto:

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

### Execute

```console
$ npm test
```

### Observe

```console
$ npm test -- --watch
```

AVA vem com um modo inteligente de observação. [Saiba mais na receita](docs/recipes/watch-mode.md).

## CLI

```console
$ ava --help

  Uso
    ava [<arquivo|diretório|glob> ...]

  Opções
    --init             Adiciona o AVA ao seu projeto
    --fail-fast        Para depois da primeira falha em um teste
    --serial, -s       Roda os testes em série
    --require, -r      Módulo para pré-carregar (pode conter repetidos)
    --tap, -t          Gera uma saída TAP
    --verbose, -v      Habilita a saída verborrágica
    --no-cache         Desabilita o cache do transpiler
    --no-power-assert  Desabilita o Power Assert
    --match, -m        Somente roda testes com que batam com o título informado (pode conter repetidos)
    --watch, -w        Roda novamente os testes quando os testes e os arquivos código-fonte mudarem
    --source, -S       Padrão para procurar nomes de arquivo, para que os testes possam ser rodados novamente (pode conter repetidos)
    --timeout, -T      Define um timeout global
    --concurrency, -c  Número máximo de arquivos-teste rodando ao mesmo tempo (EXPERIMENTAL)

  Exemplos
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Padrões de arquivo pré-definidos, quando executado sem argumetos:
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*Note que o CLI usará a instalação local do AVA quando disponível, mesmo quando executado globalmente.*

Diretórios são recursivos, com todos os arquivos `*.js` sendo tratados como arquivos de teste. Os diretórios nomeados `fixtures`, `helpers` e `node_modules` são *sempre* ignorados, assim como arquivos que começam com `_`, o que permite ter helpers no mesmo diretório que seus arquivos de teste.

Ao utilizar `npm test`, você pode passar argumentos posicionais diretamente `npm test test2.js`, mas flags precisam ser passadas como em `npm test -- --verbose`.

## Reporters

### Mini-reporter

O mini-reporter é o reporter padrão.

<img src="media/screenshot-mini-reporter.gif" width="460">

### Verbose reporter

Use a flag `--verbose` para ativar o reporter verborrágico. Ele é sempre usado em ambientes CI, exceto se o [TAP reporter](#tap-reporter) estiver ativo.

<img src="media/screenshot.png" width="150">

### TAP reporter

AVA suporta o padrão TAP, e, portanto, é compatível com [qualquer TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters). Use a flag `--tap` para habilitar a saída TAP.

```console
$ ava --tap | tap-nyan
```

<img src="media/tap-output.png" width="398">

Por-favor, note que o TAP reporter não está disponível no modo observador [watch mode](#watch-it).

### Stack traces limpos

AVA remove automaticamente linhas não relacionadas nos stack traces, permitindo que você encontre o código fonte de um erro muito mais rápido.

<img src="media/stack-traces.png" width="300">

## Configuração

Todas as opções do CLI podem ser configuradas na seção `ava` do seu `package.json`. Isso permite que você modifique o comportamento padrão do comando `ava` para que você não tenha que digitar repetidamente as mesmas opções no prompt de comando.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "source": [
      "**/*.{js,jsx}",
      "!dist/**/*"
    ],
    "match": [
      "*oo",
      "!foo"
    ],
    "concurrency": 5,
    "failFast": true,
    "tap": true,
    "powerAssert": false,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

Argumentos passados para o CLI sempre terão precedência sobre a configuração em `package.json`.

Veja a seção [Suporte para ES2015](#suporte-para-es2015) para obter detalhes sobre a opção `babel`.

## Documentação

Testes são executados simultaneamente. Você pode especificar testes síncronos e assíncronos. Testes são considerados síncronos, a menos que você retorne uma promise ou um [observável](https://github.com/zenparsing/zen-observable).

Nós *altamente* recomendamos o uso de [funções assíncronas](#suporte-para-funções-assíncronas); Elas deixam o código assíncrono conciso e legível, e retornam implicitamente uma promise, assim você não precisa fazê-lo.

Se você não pode usar promises ou observáveis, você pode ativar o "callback mode", definindo seu teste com `test.cb([title], fn)`. Testes declarados desta forma **devem** ser manualmente encerrados com `t.end()`. Este modo destina-se principalmente para testar APIs do estilo callback.

Você deve definir todos os testes de forma síncrona. Eles não podem ser definidos dentro de `setTimeout`, `setImmediate`, etc.

Arquivos de teste são executados a partir de seu diretório atual, portanto, [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) sempre é o mesmo que [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Você pode apenas usar caminhos relativos ao invés de fazer `path.join(__dirname, 'relative/path')`.

### Criando testes

Para criar um teste, você chama a função `test` que você importou do AVA. Forneça o título de teste opcional e uma função de implementação. A função será chamada quando seu teste for executado. Passa-se um [objeto de execução](#t) como seu primeiro argumento. Por convenção, este argumento é chamado `t`.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

#### Títulos

Títulos são opcionais, o que significa que você pode fazer:

```js
test(t => {
	t.pass();
});
```

É recomendado fornecer títulos de teste se você tiver mais de um teste.

Se você não forneceu um título de teste, mas a implementação é uma função nomeada, esse nome será usado como o título de teste:

```js
test(function name(t) {
	t.pass();
});
```

### Plano de asserção

Planos de asserção garantem que os testes passem somente quando um número específico de asserções for executado. Ajudam a evitar casos em que os testes se encerram muito cedo. Falham também os testes se muitas asserções forem executadas, o que pode ser útil se você tem asserções dentro de callbacks ou loops.

Esteja ciente de que, ao contrário de [`tap`](https://www.npmjs.com/package/tap) e [`tape`](https://www.npmjs.com/package/tape), AVA *não* encerra automaticamente um teste quando a contagem de asserção planejada é atingida.

Estes exemplos resultarão em um teste aprovado:

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

Estes não:

```js
test(t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // falha, 3 asserções são executadas, o que é muito

test(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // falha, o teste termina de forma síncrona antes de a asserção ser executada
```

### Testes em série

Por padrão, testes são executados simultaneamente, o que é incrível. Às vezes, porém, você tem que escrever testes que não podem ser executados simultaneamente.

Nesses raros casos, você pode usar o modificador `.serial`, que forçará esses testes a serem executados em série *antes* dos simultâneos.

```js
test.serial(t => {
	t.pass();
});
```

Note que isto só se aplica aos testes dentro de um arquivo de teste específico. O AVA ainda executará vários arquivos de testes ao mesmo tempo, a menos que você passe a [CLI flag `--serial`](#cli)

### Testes específicos

Durante o desenvolvimento, pode ser útil a execução de apenas alguns testes específicos. Isso pode ser feito usando o modificador `.only`:

```js
test('não será executado', t => {
	t.fail();
})

test.only('será executado', t => {
	t.pass();
});
```

`.only` se aplica a todos os arquivos de teste. Por isso, se você usá-lo em um arquivo, testes de outro arquivo não serão executados.

### Testes com títulos correspondentes

A flag `--match` permite executar apenas os testes que têm um título correspondente. Isso é feito com padrões simples de wildcard. Os padrões são insensíveis à caixa. Veja [`matcher`](https://github.com/sindresorhus/matcher) para mais detalhes.

Retornar títulos que terminem com `foo`:

```console
$ ava --match='*foo'
```

Retornar títulos que comecem com `foo`:

```console
$ ava --match='foo*'
```

Retornar títulos que contenham `foo`:

```console
$ ava --match='*foo*'
```

Retornar títulos que sejam *exatamente* `foo` (embora insensíveis à caixa):

```console
$ ava --match='foo'
```

Retornar títulos que não contenham `foo`:

```console
$ ava --match='!*foo*'
```

Retornar títulos que comecem com `foo` *e* terminem com `bar`:

```console
$ ava --match='foo*bar'
```

Retornar títulos que comecem com `foo` *ou* terminem com `bar`:

```console
$ ava --match='foo*' --match='*bar'
```

Observe que um padrão de correspondência tem precedência sobre o modificador `.only`. Somente testes com um título explícito podem ser correspondentes. Testes sem título ou cujo título é derivado da função de implementação serão ignorados quando o `--match` for usado.

Eis o que acontece ao executar o AVA com um padrão de correspondência `*oo*` e os seguintes testes:

```js
test('foo será executado', t => {
	t.pass();
});

test('moo também será executado', t => {
	t.pass();
});

test.only('boo será executado, mas não exclusivamente', t => {
	t.pass();
});

// não será executado, nenhum título
test(function (t) {
	t.fail();
});

// não será executado, nenhum título explícito
test(function foo(t) {
	t.fail();
});
```

### Ignorar testes

Às vezes, testes que falham podem ser difíceis de corrigir. Você pode dizer ao AVA para ignorar esses testes usando o modificador `.skip`. Eles ainda serão mostrados na saída (como tendo sido ignorados), mas nunca são executados.

```js
test.skip('não será executado', t => {
	t.fail();
});
```

Você deve especificar a função de implementação.

### Reservar espaços para testes ("todo")

Você pode usar o modificador `.todo` quando você está planejando escrever um teste. Assim como testes ignorados, esses espaços reservados são mostrados na saída. Eles só precisam de um título; não é possível especificar a função de implementação.

```js
test.todo('vou pensar em escrever isto mais tarde');
```

### Failing tests

You can use the `.failing` modifier to document issues with your code that need to be fixed. Failing tests are run just like normal ones, but they are expected to fail, and will not break your build when they do. If a test marked as failing actually passes, it will be reported as an error and fail the build with a helpful message instructing you to remove the `.failing` modifier.

This allows you to merge `.failing` tests before a fix is implemented without breaking CI. This is a great way to recognize good bug report PR's with a commit credit, even if the reporter is unable to actually fix the problem.

```js
// See: github.com/user/repo/issues/1234
test.failing('demonstrate some bug', t => {
	t.fail(); // test will count as passed
});
```

### Hooks before e after

AVA permite registrar hooks que são executados antes e depois de seus testes. Isso permite que você execute um código de instalação e/ou subdivisão.

`test.before()` registra um hook para ser executado antes do primeiro teste em seu arquivo de teste. Da mesma forma, `test.after()` registra um hook para ser executado após o último teste. Use `test.after.always()` to register a hook that will **always** run once your tests and other hooks complete. `.always()` hooks run regardless of whether there were earlier failures, so they are ideal for cleanup tasks. There are two exceptions to this however. If you use `--fail-fast` AVA will stop testing as soon as a failure occurs, and it won't run any hooks including the `.always()` hooks. Uncaught exceptions will crash your tests, possibly preventing `.always()` hooks from running.

`test.beforeEach()` registra um hook para ser executado antes de cada teste em seu arquivo de teste. Da mesma forma `test.afterEach()`, um hook para ser executado após cada teste. Use `test.afterEach.always()` to register an after hook that is called even if other test hooks, or the test itself, fail. `.always()` hooks are ideal for cleanup tasks.

**Note**: If the `--fail-fast` flag is specified, AVA will stop after the first test failure and the `.always` hook will **not** run.

Assim como `test()`, esses métodos terão um título opcional e uma função callback. O título é mostrado se o seu hook falha na execução. O callback é chamado com um [objeto de execução](#t).

Hooks `before` são executados antes dos `beforeEach`. `afterEach` são executados antes dos hooks `after`. Dentro de sua categoria, os hooks são executados na ordem em que foram definidos.

```js
test.before(t => {
	// isto é executado antes de todos os testes
});

test.before(t => {
	// isto é executado após o acima, mas antes dos testes
});

test.after('cleanup', t => {
	// isto é executado após todos os testes
});

test.after.always('guaranteed cleanup', t => {
	// this will always run, regardless of earlier failures
});

test.beforeEach(t => {
	// isto é executado antes de cada teste
});

test.afterEach(t => {
	// isto é executado após cada teste
});

test.afterEach.always(t => {
	// this runs after each test and other test hooks, even if they failed
});

test(t => {
	// teste normal
});
```

Hooks podem ser síncronos ou assíncronos, assim como os testes. Para fazer um hook assíncrono retornar uma promise ou um observável, use uma função assíncrona, ou ative o modo callback via `test.cb.before()`, `test.cb.beforeEach()`, etc.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Tenha em mente que os hooks `beforeEach` e `afterEach` se executam imediatamente antes e depois de um teste ser executado, e por isso, por padrão, testes são executados simultaneamente. Se você precisa configurar o estado global para cada teste (como espiar `console.log` [por exemplo](https://github.com/avajs/ava/issues/560)), você vai precisar se certificar que os testes são [executados em série](#testes-em-série).

Lembre-se que o AVA executa cada arquivo de teste em seu próprio processo. Pode ser que você não tenha que limpar o estado global em um hook `after` uma vez que só é chamado logo antes de o processo ser encerrado.

Os hooks `beforeEach` & `afterEach` podem compartilhar o contexto com o teste:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

O contexto é, por padrão, um objeto, mas você pode reatribuí-lo:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

Compartilhamento de contexto *não* está disponível para hooks `before` e `after`.

### Encadeamento de modificadores de teste

Você pode usar os modificadores `.serial`, `.only` e `.skip` em qualquer ordem, com `test`, `before`, `after`, `beforeEach` e `afterEach`. Por exemplo:

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

Isso significa que você pode temporariamente adicionar `.skip` ou` .only` no final de um teste ou definição de hook sem ter que fazer qualquer outra alteração.

### Test macros

Additional arguments passed to the test declaration will be passed to the test implementation. This is useful for creating reusable test macros.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 === 4', macro, '2 + 2', 4);
test('2 * 3 === 6', macro, '2 * 3', 6);
```

You can build the test title programmatically by attaching a `title` function to the macro:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle, input, expected) => `${providedTitle} ${input} === ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

The `providedTitle` argument defaults to an empty string if the user does not supply a string title. This allows for easy concatenation without having to worry about `null` / `undefined`. It is worth remembering that the empty string is considered a falsy value, so you can still use `if(providedTitle) {...}`.

You can also pass arrays of macro functions:

```js
const safeEval = require('safe-eval');

function evalMacro(t, input, expected) {
	t.is(eval(input), expected);
}

function safeEvalMacro(t, input, expected) {
	t.is(safeEval(input), expected);
}

test([evalMacro, safeEvalMacro], '2 + 2', 4);
test([evalMacro, safeEvalMacro], '2 * 3', 6);
```

We encourage you to use macros instead of building your own test generators ([here is an example](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) of code that should be replaced with a macro). Macros are designed to perform static analysis of your code, which can lead to better performance, IDE integration, and linter rules.

### Asserções personalizadas

Você pode usar qualquer biblioteca de asserção no lugar, ou além, da que vem embutida, desde que lance exceções quando a asserção falhar.

Isso não vai lhe oferecer uma experiência tão agradável quanto com as [asserções](#assercoes) embutidas, e você não poderá usar o [planejamento de asserção](#assertion-planning) ([ver #25](https://github.com/avajs/ava/issues/25)).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Suporte para ES2015

AVA vem com suporte embutido para ES2015 através do [Babel 6](https://babeljs.io). Basta escrever seus testes em ES2015. Nenhuma configuração extra é necessária. Você pode usar qualquer versão do Babel em seu projeto. Nós usamos nosso próprio Babel empacotado com o [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e predefinições [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/), assim como os plugins [`espower`](https://github.com/power-assert-js/babel-plugin-espower) e [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/).

Eis a configuração do Babel correspondente ao AVA:

```json
{
  "presets": [
    "es2015",
    "stage-2"
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

Você pode personalizar a forma como AVA transpila os arquivos de teste através da opção `babel` na [`configuração do package.json`](#configuração) do AVA. Por exemplo, para substituir as predefinições, você pode usar:

```json
{
	"ava": {
		 "babel": {
			 "presets": [
					"es2015",
					"stage-0",
					"react"
			 ]
		 }
	},
}
```

Você também pode usar a palavra-chave especial `"inherit"`. Isso faz com que o AVA use a configuração do Babel em seu [arquivo `.babelrc` ou `package.json`](https://babeljs.io/docs/usage/babelrc/). Desta forma, seus arquivos de teste serão transpilados usando a mesma configuração que os arquivos de origem, sem ter que repetí-la apenas para o AVA:

```json
{
	"babel": {
		"presets": [
			"es2015",
			"stage-0",
			"react"
		]
	},
	"ava": {
		"babel": "inherit"
	},
}
```

Veja a [`receita .babelrc`](docs/recipes/babelrc.md) do AVA para mais exemplos e uma explicação mais detalhada das opções de configuração.

Note que o AVA irá *sempre* aplicar [alguns plugins internos](docs/recipes/babelrc.md#notas), independentemente da configuração, mas eles não devem afetar o comportamento do seu código.

### Suporte para TypeScript

AVA inclui tipagens para TypeScript. Você mesmo tem que configurar a transpilação. Quando você define `module` para `commonjs` em seu arquivo `tsconfig.json`, TypeScript encontrará automaticamente as definições de tipo para AVA. Você deve definir `target` para `es2015` para usar Promises e funções assíncronas.

#### Transpiling de Módulos Importados

AVA atualmente só faz o transpiling de testes que você pede para serem executados. *Não fará o transpiling de módulos que você importa (`import`) de fora do teste.* Isso pode ser inesperado, mas existem soluções.

Se você usa o Babel, você pode pode usar o [hook require](https://babeljs.io/docs/usage/require/) a fim de fazer transpiling on-the-fly dos módulos importados. Execute o AVA com `--require babel-register` (veja [CLI](#cli)) ou [configure-o em seu `package.json`](#configuração).

Você também pode transpilar seus módulos em um processo separado e referir-se aos arquivos transpilados ao invés das fontes de seus testes.

### Suporte para Promises

Se você retornar uma promise no teste, você não precisa explicitamente encerrá-lo, já que se encerrará quando a promise for resolvida.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Suporte para função geradora

AVA vem com suporte embutido para [funções geradoras](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/function*).

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

###  Suporte para funções assíncronas

AVA vem com suporte embutido para [funções assíncronas](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// função arrow assíncrona
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Suporte para Observável

AVA vem com suporte embutido para [ observáveis](https://github.com/zenparsing/es-observable).
 Se você retornar um observável de um teste, AVA automaticamente irá consumí-lo até sua conclusão antes de encerrar o teste.

*Você não precisa usar o "modo callback" ou chamar `t.end()`.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// somente números pares
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Suporte para Callback

AVA suporta o uso de `t.end` como callback final ao usar APIs de callback node-style error-first. AVA irá considerar qualquer valor truthy passado como o primeiro argumento para o `t.end` como sendo um erro. Observe que `t.end` requer "modo callback", que pode ser ativado usando a cadeia `test.cb`.

```js
test.cb(t => {
	// t.end verifica automaticamente se há erro como primeiro argumento
	fs.readFile('data.txt', t.end);
});
```

### Tempo limite global

Um tempo limite global pode ser definido através da opção `--timeout`.
O tempo limite em AVA se comporta de forma diferente do que em outros frameworks de teste.
AVA redefine um timer após cada teste, forçando testes a se encerrar se não forem recebidos novos resultados dentro do tempo limite especificado.

Você pode definir o tempo limite de uma forma legível:

```
$ ava --timeout=10s # 10 segundos
$ ava --timeout=2m # 2 minutos
$ ava --timeout=100 # 100 milissegundos
```

## API

### `test([title], implementation)`
### `test.serial([title], implementation)`
### `test.cb([title], implementation)`
### `test.only([title], implementation)`
### `test.skip([title], implementation)`
### `test.todo(title)`
### `test.failing([title], implementation)`
### `test.before([title], implementation)`
### `test.after([title], implementation)`
### `test.beforeEach([title], implementation)`
### `test.afterEach([title], implementation)`

#### `title`

Type: `string`

Título do teste

#### `implementation(t)`

Type: `function`

Deve conter o teste real.

##### `t`

Type: `object`

O objecto de execução de um teste em particular. Cada implementação de teste recebe um objeto diferente. Contém as [asserções](#assercoes), bem como os métodos `.plan(count)` e `.end()`. `t.context` pode conter estado compartilhado dos hooks `beforeEach`.

###### `t.plan(count)`

Planeja quantas asserções existem no teste. O teste falhará caso a contagem real de asserção não corresponda às asserções planejadas. Veja [plano de asserções](#plano-de-assercoes).

###### `t.end()`

Encerra o teste. Funciona somente com `test.cb()`.

## Asserções

Asserções são misturadas no [objeto de execução](#t) fornecido em cada implementação de teste:

```js
test(t => {
	t.truthy('unicorn'); // asserção
});
```

Se várias falhas de asserção forem encontradas dentro de um único teste, AVA só exibirá a *primeira* delas.

### `.pass([message])`

Passa asserção.

### `.fail([message])`

Falha de asserção.

### `.truthy(value, [message])`

Afirma que `value` é truthy.

### `.falsy(value, [message])`

Afirma que `value` é falsy.

### `.true(value, [message])`

Afirma que `value` é `true`.

### `.false(value, [message])`

Afirma que `value` é `false`.

### `.is(value, expected, [message])`

Afirma que `value` é igual a `expected`.

### `.not(value, expected, [message])`

Afirma que `value` não é igual a `expected`.

### `.deepEqual(value, expected, [message])`

Afirma que `value` é estritamente igual a `expected`.

### `.notDeepEqual(value, expected, [message])`

Afirma que `value` não é estritamente igual a `expected`.

### `.throws(function|promise, [error, [message]])`

Afirma que `function` gera um erro, ou rejeições de `promise` com um erro.

`error` pode ser construtor, regex, mensagem de erro ou função de validação.

Retorna o erro gerado pela `function` ou a razão da rejeição da `promise`.

### `.notThrows(function|promise, [message])`

Afirma que `function` não gera um `error` ou resoluções de `promise`.

### `.regex(contents, regex, [message])`

Afirma que `contents` corresponde a `regex`.

### `.notRegex(contents, regex, [message])`

Afirma que `contents` não corresponde a `regex`.

### `.ifError(error, [message])`

Afirma que `error` é falsy.

### Ignorando asserções

Qualquer asserção pode ser ignorada usando o modificador `skip`. Asserções ignoradas ainda são contadas, e, portanto, não há nenhuma necessidade de mudar a sua contagem de asserções planejadas.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // não é necessário mudar seu plano de contagem ao ignorar
	t.is(1, 1);
});
```

### Mensagens de Asserções melhoradas

AVA vem com [`power-assert`](https://github.com/power-assert-js/power-assert) embutido, com mensagens de asserção mais descritivas. Lê o seu teste e tenta inferir mais informações a partir do código.

Vamos pegar este exemplo, usando a [biblioteca `assert`](https://nodejs.org/api/assert.html) padrão do Node:

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Se você colar isso em um REPL Node, ele retornará:

```
AssertionError: false == true
```

No AVA, no entanto, este teste:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

retornará:

```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```

## Isolar processo

Cada arquivo de teste é executado em um processo separado do Node.js. Isso permite que você altere o estado global ou substitua um built-in em um arquivo de teste, sem afetar o outro. Também é ótimo para o desempenho em modernos processadores multi-core, permitindo que vários arquivos de teste sejam executados em paralelo.

## Dicas

### Arquivos temporários

Executar testes simultaneamente vem com alguns desafios, fazer IO de arquivos é um deles.

Normalmente, os testes em série apenas criam diretórios temporários no diretório de teste atual e os limpam no final. Isso não vai funcionar quando você executar testes simultaneamente, já que esses testes entrarão em conflito uns com os outros. A maneira correta de fazer isso é usar um novo diretório temporário para cada teste. Os módulos [`tempfile`](https://github.com/sindresorhus/tempfile) e [`temp-write`](https://github.com/sindresorhus/temp-write) podem ser úteis.

### Depuração

AVA executa testes simultaneamente por padrão, o que é abaixo do ideal quando você precisa depurar algo. Em vez disso, execute os testes em série com a opção `--serial`:

```console
$ ava --serial
```

### Cobertura de código

Você não pode usar [`istanbul`](https://github.com/gotwarlost/istanbul) para cobertura de código porque o AVA [gera os arquivos de teste](#isolar-processo), mas você pode usar [`nyc`](https://github.com/bcoe/nyc) em vez disso, que é basicamente `istanbul` com suporte para subprocessos.

A partir da versão `5.0.0` ele usa source maps para relatar cobertura para seu código real, independentemente da transpilação. Certifique-se de que o código que você está testando inclui source map em linha ou faz referência a um arquivo source map. Se você usar `babel-register`, você pode definir a opção `sourceMaps` em seu `.babelrc` como `inline`.

### Common pitfalls

We have a growing list of [common pitfalls](docs/common-pitfalls.md) you may experience while using AVA. If you encounter any issues you think are common, comment in [this issue](https://github.com/avajs/ava/issues/404).

## Perguntas Frequentes

### Por que não `mocha`, `tape`, `tap`?

Mocha requer a utilização de globais implícitas como `describe` e `it` com a interface padrão (que a maioria das pessoas usam), não é muito opinativo e executa testes serialmente sem isolamento de processos, o que o deixa lento.

Tape e node-tap são muito bons. AVA é altamente inspirado em sua sintaxe. No entanto, ambos executam testes em série. Seu padrão de saída [TAP](https://testanything.org) não é muito fácil de usar, então você sempre acaba usando um reporter externo.

Em contrapartida, AVA é altamente opinativo e executa testes simultaneamente com um processo separado para cada arquivo. Vem com um simples reporter padrão fácil de usar e suporta TAP através de um CLI flag.

### Como o nome é escrito e pronunciado?

AVA, não Ava nem ava. Pronuncia-se [`/ˈeɪvə/` ay-və](https://github.com/avajs/ava/blob/master/media/pronunciation.m4a?raw=true).

### O que é o plano de fundo do cabeçalho?

É a [Galáxia de Andrômeda.](https://pt.wikipedia.org/wiki/Gal%C3%A1xia_de_Andr%C3%B4meda)

### Qual é a diferença entre simultaneidade e paralelismo?

[Simultaneidade não é paralelismo. Ela permite o paralelismo.](http://stackoverflow.com/q/1050222)

## Receitas

- [Cobertura de código](docs/recipes/code-coverage.md)
- [Modo de observação](docs/recipes/watch-mode.md)
- [Teste de Endpoint](docs/recipes/endpoint-testing.md)
- [Quando usar `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Browser testing](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Configurar Babel](docs/recipes/babelrc.md)
- [Testing React components](docs/recipes/react.md)
- [JSPM and SystemJS](docs/recipes/jspm-systemjs.md)
- [Debugging tests with WebStorm](docs/recipes/debugging-with-webstorm.md)

## Suporte

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/avajs/ava)
- [Twitter](https://twitter.com/ava__js)

## Relacionados

- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets para testes AVA
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets para testes AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets para testes AVA
- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Regras de lint para testes AVA
- [gulp-ava](https://github.com/avajs/gulp-ava) - Execute testes com gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Execute testes com grunt
- [fly-ava](https://github.com/pine/fly-ava) - Execute testes com fly
- [start-ava](https://github.com/start-runner/ava) - Execute testes com start

[Mais...](https://github.com/avajs/awesome-ava#packages)

## Links

- [Compre adesivos do AVA](https://www.stickermule.com/user/1070705604/stickers)
- [Lista impressionante](https://github.com/avajs/awesome-ava)
- [JavaScript Air podcast episode](http://jsair.io/ava)


## Time

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net) | [![Juan Soto](https://avatars.githubusercontent.com/u/8217766?s=130)](https://juansoto.me) | [![Jeroen Engels](https://avatars.githubusercontent.com/u/3869412?s=130)](https://github.com/jfmengels)
---|---|---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net) | [Juan Soto](http://juansoto.me) | [Jeroen Engels](https://github.com/jfmengels)

### Precursor

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
	<br>
	<br>
	<br>
	<a href="https://ava.li">
		<img src="https://cdn.rawgit.com/avajs/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>
