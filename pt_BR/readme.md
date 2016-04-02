___
**Nota do tradutor**

Esta é a tradução de [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). [Este link](https://github.com/sindresorhus/ava/compare/e697629a77b511e069eb0a2991c8ee72cd09034c...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9) compara a versão em que se baseou esta tradução com a última versão disponível no branch `master` do AVA. Se não houver mudanças em `readme.md`, então a tradução está atualizada.
___

# ![AVA](https://raw.githubusercontent.com/sindresorhus/ava/master/media/header.png)

> Executor de testes futurista

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join chat.svg)](https://gitter.im/sindresorhus/ava)

Apesar de o JavaScript ser single-threaded, as IO no Node.js podem acontecer em paralelo devido à sua natureza assíncrona. AVA aproveita-se disso e executa os testes simultaneamente, o que é especialmente favorável para testes pesados de IO. Além disso, arquivos de teste são executados em paralelo como processos separados, oferencendo ainda melhor desempenho e um ambiente isolado para cada arquivo de teste. [Migrar]( https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) de Mocha para AVA no Pageres reduziu o tempo de teste de 31 para 11 segundos. Testes executados simultaneamente forçam a escrever testes atômicos, ou seja, testes que não dependem do estado global ou do estado de outros testes, o que representa um grande diferencial!

*Leia nosso [guia para contribuir](contributing.md) se deseja contribuir (issues/PRs/etc).*

Siga o [AVA no Twitter](https://twitter.com/ava__js) para atualizações.

Traduções: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/readme.md)


## Índice

- [Utilização](#utilização)
- [Utilização do CLI](#cli)
- [Configuração](#configuração)
- [Documentação](#documentação)
- [API](#api)
- [Asserções](#asserções)
- [Perguntas Frequentes](#perguntas-frequentes)
- [Receitas](#receitas)


## Por que AVA?

- Mínimo e rápido
- Sintaxe de teste simples
- Executa testes simultaneamente
- Induz a escrever testes atômicos
- Sem globais implícitas
- [Ambiente isolado para cada arquivo de teste](#ambiente-isolado)
- [Escreva seus testes em ES2015](#suporte-para-es2015)
- [Suporte para promises](#suporte-para-promises)
- [Suporte para função geradora](#suporte-para-função-geradora)
- [Suporte para funções assíncronas](#suporte-para-funções-assíncronas)
- [Suporte para Observável](#suporte-para-observável)
- [Asserções melhoradas](#asserções-melhoradas)
- [Saída opcional TAP](#saída-opcional-tap)
- [Rastreamentos limpos de Pilha](#rastreamentos-limpos-de-pilha)


## Sintaxe de teste

```js
import test from 'ava';

test(t => {
	t.same([1, 2], [1, 2]);
});
```


## Utilização

#### Inicie

Instale o AVA globalmente `$ npm install --global ava` e execute `$ ava --init` (com quaisquer opções) para adicionar o AVA a seu package.json ou criar um.

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

#### Crie seu arquivo de teste

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

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

#### Execute

```
$ npm test
```


## CLI

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

*Note que o CLI usará a instalação local do AVA quando disponível, mesmo quando executado globalmente.*

Diretórios são recursivos por padrão. Os diretórios nomeados `fixtures` e `helpers` são ignorados, assim como arquivos que começam com `_`. Isso pode ser útil para ter helpers no mesmo diretório que seus arquivos de teste.

Ao utilizar `npm test`, você pode passar argumentos posicionais diretamente `npm test test2.js`, mas flags precisam ser passadas como em `npm test -- --verbose`.

## Configuração

Todas as opções do CLI podem ser configuradas na seção `ava` do seu `package.json`. Isso permite que você modifique o comportamento padrão do comando `ava` para que você não tenha que digitar repetidamente as mesmas opções no prompt de comando.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
    ]
  }
}
```

Argumentos passados para o CLI sempre terão precedência sobre a configuração em `package.json`.

## Documentação

Testes são executados de forma assíncrona e exigem que você retorne um objeto assíncrono suportado (uma promise, ou um [observável](https://github.com/zenparsing/zen-observable)). Nós *altamente* recomendamos o uso de [funções assíncronas](#suporte-para-funções-assíncronas); Elas produzem código assíncrono conciso e legível, e retornam implicitamente uma promise, assim você não precisa fazê-lo.

Se você não retornar um dos objetos assíncronos suportados, mencionados acima, o teste é considerado síncrono e encerrado imediatamente.

Se você é incapaz de usar promises ou outros objetos assíncronos suportados, você pode ativar o "callback mode", definindo seu teste com `test.cb([title], fn)`. Testes declarados desta forma **devem** deve ser manualmente encerrados com `t.end()`. Este modo destina-se principalmente para testar APIs do estilo callback.

Você deve definir todos os testes de forma síncrona. Eles não podem ser definidos dentro de `setTimeout`, `setImmediate`, etc.

Arquivos de teste são executados a partir de seu diretório atual, portanto, [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd) sempre é o mesmo que [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname). Você pode apenas usar caminhos relativos ao invés de fazer `path.join(__dirname, 'relative/path')`.

### Teste de anatomia

Para criar um teste, você chama a função `test` após o `require` do AVA e passa um nome de teste opcional e uma função que contém a execução do teste. A função fornecida recebe o contexto como primeiro argumento, no qual você pode chamar os distintos métodos e [asserções](#asserções) do AVA.

```js
test('name', t => {
	t.pass();
});
```

### Nome opcional de teste

Nomear um teste é opcional, mas recomendado se você tiver mais de um teste.

```js
test(t => {
	t.pass();
});
```

Você também pode optar por utilizar uma função nomeada:

```js
test(function name(t) {
	t.pass();
});
```

### Plano de asserção

Um plano de asserção pode ser usado para garantir que um número específico de asserções seja feito. No cenário mais comum, ele valida que o teste não saia antes de executar o número esperado de asserções. Ele também falha o teste se muitas asserções são executadas, o que pode ser útil se você tem afirmações dentro de callbacks ou loops. Esteja ciente de que, ao contrário de node-tap e tape, AVA *não* auto-encerra um teste quando a contagem de asserção planejada é atingida.

Isso resultará em um teste aprovado:

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

### Testes em série

Enquanto a simultaneidade é incrível, há algumas coisas que não podem ser feitas simultaneamente. Nesses raros casos, você pode chamar `test.serial`, que forçará esses testes a serem executados em série antes dos simultâneos.

```js
test.serial(t => {
	t.pass();
});
```

### Testes-only

Testes-only impõem apenas aqueles testes a serem executados. Isso pode ser útil para a execução de apenas alguns testes durante o desenvolvimento.

```js
test('will not be run', t => {
	t.fail();
})

test.only('will be run', t => {
	t.pass();
});
```

### Testes-skip

Testes-skip são mostrados na saída como ignorados, e nunca são executados. Testes-skip requerem uma função.

```js
test.skip('will not be run', t => {
	t.fail();
});
```
### Testes-todo

Testes-todo, assim como Testes-skip, são mostrados na saída, mas nunca são executados. Podem ser úteis para o planejamento de testes futuros. Testes-todo requerem apenas um título de teste.

```js
test.todo('vou pensar em escrever isto mais tarde');
```

### Hooks before e after

Quando instalação e/ou subdivisão é necessária, você pode usar `test.before()` e `test.after()`, da mesma maneira que `test()`. A função de teste fornecida para `test.before()` e `test.after()` é chamada antes ou após todos os testes. Você também pode usar `test.beforeEach()` e `test.afterEach()` se você precisa de configuração/subdivisão para cada teste. Hooks são executados serialmente no arquivo teste. Adicione quantos quiser. Opcionalmente, você pode especificar um título que é mostrado em caso de falha.

Se você precisa configurar um estado global entre testes usando `test.beforeEach()` e `test.afterEach()` (como espiar no `console.log` [por exemplo](https://github.com/sindresorhus/ava/issues/560)), você precisará certificar-se de que os testes são executados serialmente (usando [test.serial](#serial-tests) ou [`--serial`](#cli)).


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

test.beforeEach(t => {
	// isto é executado antes de cada teste
});

test.afterEach(t => {
	// isto é executado após cada teste
});

test(t => {
	// teste normal
});
```

Você pode usar funções assíncronas, retornar objetos assíncronos, ou ativar o "modo callback" em qualquer um dos hooks.

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

Os hooks `beforeEach` & `afterEach` podem compartilhar o contexto com o teste:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

O contexto é, por padrão, um objeto, mas também pode ser atribuído diretamente:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

### Encadeamento de modificadores de teste

Você pode encadear modificadores de teste das seguintes maneiras:

```js
test.before.skip([title], testFn);
test.skip.after(....);
test.serial.only(...);
test.only.serial(...);
```

Isso é especialmente útil temporariamente usando `skip` ou `only` em um teste, sem perder a informação e o comportamento que os outros modificadores fornecem.

### Módulo personalizado de asserção

Você pode usar qualquer módulo de asserção no lugar ou além do que vem com o AVA, mas você não será capaz de usar o método `.plan()`, [ainda](https://github.com/sindresorhus/ava/issues/25).

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### Suporte para ES2015

AVA vem com suporte embutido para ES2015 através do [Babel 6](https://babeljs.io). Basta escrever seus testes em ES2015. Nenhuma configuração extra é necessária. Você pode usar qualquer versão do Babel em seu projeto. Usamos nosso próprio Babel empacotado com o [`es2015`](http://babeljs.io/docs/plugins/preset-es2015/) e predefinições [`stage-2`](http://babeljs.io/docs/plugins/preset-stage-2/).

### TypeScript support

AVA inclui tipagens para TypeScript. Você mesmo tem que configurar a transpilação. Quando você define `module` para `commonjs` em seu arquivo `tsconfig.json`, TypeScript encontrará automaticamente as definições de tipo para AVA. Você deve definir `target` para `es2015` para usar Promises e funções assíncronas.

#### Transpiling de Módulos Importados

AVA atualmente só faz o transpiling de testes que você pede para serem executados. *Não fará o transpiling de módulos que você importa (```import```) de fora do teste.*  Embora existam razões válidas para esta abordagem, pode não ser o que você espera!

Como uma solução simples, você pode usar [Babel's require hook](https://babeljs.io/docs/usage/require/) a fim de fazer transpiling on-the-fly de módulos que são posteriormente importados. Como o AVA oferece suporte para a sintaxe de módulo ES2015, você pode usá-lo para importar o hook requerido em si:

```js
import test from 'ava';
import 'babel-register';
import foo from './foo'; // <-- foo pode ser escrito em ES2015!

test('foo bar', t => {
	t.same('baz', foo('bar'));
});
```

### Suporte para promises

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
	// t.end  verifica automaticamente se há erro como primeiro argumento
	fs.readFile('data.txt', t.end);
});
```

### Saída opcional TAP

AVA pode gerar saída TAP através da opção `--tap` para uso com qualquer [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

```
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">


### Rastreamentos limpos de Pilha

AVA automaticamente remove linhas independentes em rastreamentos de pilha, permitindo que você encontre a fonte de um erro muito mais rápido.

<img src="https://github.com/sindresorhus/ava/blob/master/media/stack-traces.png" width="300">


## API

### test([title], body)
### test.serial([title], body)
### test.cb([title], body)
### test.only([title], body)
### test.skip([title], body)
### test.todo(title)
### test.before([title], body)
### test.after([title], body)
### test.beforeEach([title], body)
### test.afterEach([title], body)

#### title

Type: `string`

Título do teste

#### body(context)

Type: `function`

Deve conter o teste real.

##### context

Passado para a função de teste e contém os diferentes métodos de AVA e [asserções](#assercoes).

###### .plan(count)

Planeja quantas asserções existem no teste. O teste falhará caso a contagem real de asserção não corresponda às asserções planejadas.

###### .end()

Encerra o teste. Funciona somente com `test.cb()`.


## Asserções

Asserções são misturadas no [context](#context) do teste:

```js
test(t => {
	t.ok('unicorn'); // asserção
});
```

Se várias falhas de asserção forem encontradas dentro de um único teste, AVA só exibirá a *primeira* delas.

### .pass([message])

Passar asserção.

### .fail([message])

Falha de asserção.

### .ok(value, [message])

Afirma que `value` é truthy.

### .notOk(value, [message])

Afirma que `value` é falsy.

### .true(value, [message])

Afirma que `value` é `true`.

### .false(value, [message])

Afirma que `value` é `false`.

### .is(value, expected, [message])

Afirma que `value` é igual a `expected`.

### .not(value, expected, [message])

Afirma que `value` não é igual a `expected`.

### .same(value, expected, [message])

Afirma que `value` é estritamente igual a `expected`.

### .notSame(value, expected, [message])

Afirma que `value` não é estritamente igual a `expected`.

### .throws(function|promise, [error, [message]])

Afirma que `function` gera um erro, ou rejeições de `promise` com um erro.

`error` pode ser construtor, regex, mensagem de erro ou função de validação.

Retorna o erro gerado pela `function` ou a razão da rejeição da `promise`.

### .notThrows(function|promise, [message])

Afirma que `function` não gera um `error` ou resoluções de `promise`.

### .regex(contents, regex, [message])

Afirma que `contents` corresponde a `regex`.

### .ifError(error, [message])

Afirma que `error` é falsy.

## Ignorando asserções

Qualquer asserção pode ser ignorada usando o modificador `skip`. Asserções ignoradas ainda são contadas, e, portanto, não há nenhuma necessidade de mudar a sua contagem de asserção planejada.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // não é necessário mudar sua contagem de plano ao ignorar
	t.is(1, 1);
});
```

## Asserções melhoradas

AVA vem com [`power-assert`](https://github.com/power-assert-js/power-assert) embutido, dando-lhe mensagens de asserção mais descritivas. Lê o seu teste e tenta inferir mais informações a partir do código.

O seguinte teste:

```js
test(t => {
	const x = 'foo';
	t.ok(x === 'bar');
});
```

Normalmente retornaria a seguinte saída inútil:

```
false === true
```

Com asserções melhoradas, você obterá:

```
t.ok(x === 'bar')
     |
     "foo"
```

É verdade, você pode usar `t.is()` neste caso, e provavelmente deveria, mas este é apenas um exemplo simples.

Vamos tentar um exemplo mais avançado:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.ok(a.test(b) || b === c);
});
```

E vamos lá:

```
t.ok(a.test(b) || b === c)
       |    |     |     |
       |    "bar" "bar" "baz"
       false
```

Todos os métodos de asserção são melhorados.

Divirta-se!


## Ambiente isolado

Cada arquivo de teste é executado em um processo separado do Node.js. Isso traz uma série de benefícios. Arquivos de teste diferentes já não podem afetar uns aos outros. Como arquivos de teste afetando o ambiente global, substituindo builtins, etc. No entanto, isso é feito principalmente por motivos de desempenho. Mesmo que o Node.js possa executar IO assíncronos simultaneamente, isso não ajuda muito quando testes são pesados em operações síncronas, o que bloqueia o thread principal.  Ao executar testes simultaneamente e arquivos de teste em paralelo, podemos tirar o máximo proveito de sistemas modernos.

## Dicas

### Arquivos temporários

Executar testes simultaneamente vem com alguns desafios, fazer IO é um deles. Normalmente, os testes de série apenas criam diretórios temporários no diretório de teste atual e o limpam no final. Isso não vai funcionar quando você executar testes simultaneamente, já que esses testes entrarão em conflito uns com os outros. A maneira correta de fazer isso é usar um novo diretório temporário para cada teste. Os módulos [`tempfile`](https://github.com/sindresorhus/tempfile) e [`temp-write`](https://github.com/sindresorhus/temp-write) podem ser úteis.

### Depuração

AVA executa testes simultaneamente por padrão, que é abaixo do ideal quando você precisa depurar algo. Em vez disso, execute os testes em série com a opção `--serial`:

```
$ ava --serial
```

### Cobertura de código

Você não pode usar [`istanbul`](https://github.com/gotwarlost/istanbul) para cobertura de código porque o AVA [gera os arquivos de teste](#isolated-environment), mas você pode usar [`nyc`](https://github.com/bcoe/nyc) em vez disso, que é basicamente `istanbul` com suporte para subprocessos.

A partir da versão `5.0.0` ele usa source maps para relatar cobertura para seu código real, independentemente da transpilation. Certifique-se de que o código que você está testando inclui source map inline ou faz referência a um arquivo source map. Se você usar `babel-register`, você pode definir a opção `sourceMaps` em seu `.babelrc` como `inline`.

## Perguntas Frequentes

### Por que não `mocha`, `tape`, `node-tap`?

Mocha requer a utilização de globais implícitas como `describe` e `it` com a interface padrão (que a maioria das pessoas usam), é muito não-opinativo, inchado, síncrono por padrão, com API programática inutilizável, execução de teste serial, e é lento. Tape e node-tap são muito bons. AVA é altamente inspirado em sua sintaxe. No entanto, ambos executam testes em série e fizeram do [TAP](https://testanything.org) um cidadão de primeira classe que, na minha opinião, tornou as suas bases de código um pouco complicadas e acopladas. A saída do TAP é difícil de ler, então você sempre acaba usando um reporter externo. AVA é altamente opinativo e simultâneo. Vem com um simples reporter padrão e suporta TAP através de um sinalizador CLI.

### Como posso utilizar reporters personalizados?

Use a [`--tap` flag](#optional-tap-output) com qualquer [TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters).

### Como o nome é escrito e pronunciado?

AVA, não Ava nem ava. Pronuncia-se [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true).

### O que é o plano de fundo do cabeçalho?

[Galáxia de Andrômeda.](https://pt.wikipedia.org/wiki/Gal%C3%A1xia_de_Andr%C3%B4meda)

### Simultaneidade vs. paralelismo

[Simultaneidade não é paralelismo. Ela permite o paralelismo.](http://stackoverflow.com/q/1050222)


## Receitas

- [Cobertura de código](docs/recipes/code-coverage.md)
- [Teste de Endpoint](docs/recipes/endpoint-testing.md)
- [Quando usar `t.plan()`](docs/recipes/when-to-use-plan.md)


## Suporte

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter chat](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)


## Relacionados

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - Snippets para testes AVA
- [atom-ava](https://github.com/sindresorhus/atom-ava) - Snippets para testes AVA
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets para testes AVA
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - Regras de lint para testes AVA
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - Execute testes com gulp
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - Execute testes com grunt
- [fly-ava](https://github.com/pine613/fly-ava) - Execute testes com fly
- [start-ava](https://github.com/start-runner/ava) - Execute testes com start

## Links

- [Compre adesivos do AVA](https://www.stickermule.com/user/1070705604/stickers)
- [Lista impressionante](https://github.com/sindresorhus/awesome-ava)


## Time

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net)
 ---|---|---|---|---
 [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net)

#### Anterior

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
