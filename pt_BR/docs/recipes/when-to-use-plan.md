___
**Nota do tradutor**

Esta é a tradução de [when-to-use-plan.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md). [Este link](https://github.com/sindresorhus/ava/compare/39982a5c12b90c31ef3e0dea8c0e3499c47a7bc9...master) compara a versão em que se baseou esta tradução com a última versão disponível no branch `master` do AVA. Se não houver mudanças em `when-to-use-plan.md`, então a tradução está atualizada.
___

# Quando usar `t.plan()`

Uma diferença importante entre AVA e [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) é o comportamento de `t.plan()`. No AVA, `t.plan()` só é usado para afirmar que o número esperado de asserções é chamado; e não auto-encerra o teste.

## Usos pobres de `t.plan()`

Muitos usuários fazendo transição de `tap`/`tape` estão acostumados a usar `t.plan()`prolificamente em cada teste. No entanto, no AVA, não consideramos que seja uma "melhor prática". Em vez disso, acreditamos que `t.plan()` só deve ser usado em situações onde ele fornece algum valor.

### Testes de Sync sem branching

`t.plan()` é desnecessária na maioria dos testes de sync.

```js
test(t => {
	// RUIM: não há nenhuma branching aqui - t.plan () é inútil
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` não fornece qualquer valor aqui, e cria uma tarefa extra se você decidir que quer adicionar ou remover afirmações.

### Promises esperadas para resolver

```js
test(t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

À primeira vista, esse teste parece fazer bom uso da `t.plan()` já que um manipulador de promise assíncrona está envolvido. No entanto, há vários problemas com o teste:

1. `t.plan()` resumivelmente é usado aqui para proteger contra a possibilidade de que `somePromise()` seja rejeitada. Mas retornar uma promise rejeitada falharia o teste de qualquer maneira.

2. Seria melhor tirar proveito de `async`/`await`:

```js
test(async t => {
	t.is(await somePromise(), 'foo');
});
```

## Bons usos de `t.plan()`

`t.plan()` tem muitos usos aceitáveis.

### Promises com um bloco `.catch()`

```js
test(t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello') // Prefira t.throws() se tudo o que importa é a mensagem
		t.is(reason.foo, 'bar');
	});
});
```

Aqui, `t.plan()` é utilizado para garantir o código dentro do bloco `catch` aconteça. Na maioria dos casos, você deveria preferir a afirmação `t.throws()`, mas esse é um uso aceitável já que `t.throws()` só permite que você faça asserção contra a propriedade `message` do erro.

### Garantir que uma instrução catch aconteça

```js
test(t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello') // Prefira t.throws() se tudo o que importa é a mensagem
		t.is(err.foo, 'bar');
	}
});
```

Como foi dito no exemplo `try`/`catch` acima, usar a asserção `t.throws()` é geralmente uma escolha melhor, mas só permite que você faça asserção contra a propriedade `message` do erro.

### Garantir que callbacks múltiplos são realmente chamados

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

O código acima garante que `callbackB` seja chamado primeiro (e apenas uma vez), seguido por `callbackA`. Qualquer outra combinação não satisfaria o plano.

### Testes com instruções de branching

Na maioria dos casos, é uma má idéia usar qualquer branching complexo em seus testes. Uma notável exceção é para testes que são gerados automaticamente (talvez a partir de um documento JSON). Abaixo, `t.plan()` é usada para garantir a exactidão da entrada JSON:

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
	test(t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition deve ter uma expectativa para `foo` ou `bar`, mas não ambos
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

## Conclusão

`t.plan()` tem muitos usos válidos, mas não deve ser usado indiscriminadamente. Uma boa regra é usá-lo sempre que seu *test* não tiver um fluxo de código simples que seja fácil raciocinar sobre. Testes com asserções dentro de callbacks, instruções `if`/`then`, laços `for`/`while`, e (em alguns casos) blocos `try`/`catch`, são todos bons candidatos para `t.plan()`.
