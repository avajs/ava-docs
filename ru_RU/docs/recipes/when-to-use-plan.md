# Когда применять `t.plan()`

Переводы: [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md)

Одна большая разница между AVA и [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) это поведение `t.plan()`. В AVA, `t.plan()` используется для того, чтобы убедиться, что будет выполнено ожидаемое число assert'ов без автоматического завершения теста.

## Злоупотребление использованием `t.plan()`

Большинство пользователей, пришедших из `tap`/`tape` привыкли использовать `t.plan()` буквально в каждом тесте. Однако в AVA мы не считаем это "хорошей идеей". Вместо этого, мы уверены, что `t.plan()` должен использоваться только там, где он явно необходим.

### Синхронные тесты без вложенностей

`t.plan()` не нужен в большинстве синхронных тестов.

```js
test(t => {
	// ПЛОХО: нет вложенности - t.plan() бесполезен
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()` не имеет никакого смысла здесь и усложняет понимание кода, если Вы решите добавить или удалить assert'ы.

### Обещания, которые предполагают завершение без ошибок

```js
test(t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

На первый взгляд кажется, что этот тест - правильное использование `t.plan()`, так как задействован асинхронный обработчик. Однако с этим связано несколько проблем:

1. `t.plan()` используется для защиты от того, что `somePromise()` завершится с ошибкой; Но возврат отклоненного обещания все равно провалит тест.

2. Было бы лучше использовать `async`/`await`:

```js
test(async t => {
	t.is(await somePromise(), 'foo');
});
```

## Правильное использование `t.plan()`

`t.plan()` имеет много способов использования.

### Обещания с `.catch()` блоком

```js
test(t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello') // Предпочтительней t.throws(), если все что Вам нужно - проверить свойство message
		t.is(reason.foo, 'bar');
	});
});
```

Здесь `t.plan()` использован для того, чтобы убедиться в выполнении кода внутри блока `catch`. В большинстве случаях, Вы должны использовать `t.throws()` сравнение, тем не менее, код выше приемлимый, так как `t.throws()` лишь позволяет сделать проверку свойства `message` у исключения.

### Убеждаемся, что конструкция catch обрабатывается

```js
test(t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello') // Предпочтительней t.throws(), если все что Вам нужно - проверить свойство message
		t.is(err.foo, 'bar');
	}
});
```

Как указанов в примере `try`/`catch` выше, использование `t.throws()` является лучший выбором, но оно позволяет лишь проверить свойство `message` у исключения.

### Убеждаемся, что все callback'и были вызваны

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

В примере выше `callbackB` будет гарантированно вызван первым(единожды), после чего будет вызван `callbackA`. Любые другие комбинации не удовлетворят тестовый план.

### Тесты с вложенными конструкциями

В большинстве случаев - плохая идея использовать любые сложные вложенности внутри ваших тестов. Единственным исключением являются автоматически сгенерированные тесты(например из JSON документа). Ниже `t.plan()` используется для проверки корректного JSON ввода:

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
	test(t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition должен иметь свойства `foo` или `bar`, но не оба из них
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

## Заключение

`t.plan()` имеет много корректных применений, но он не должен использоваться без разбора. Хорошим применением является тестирование сложного, не всегда очевидного кода. Тесты с assert'ами внутри callback'ов, операторы `if`/`then`, `for`/`while` циклы, и (в некоторых случаях) `try`/`catch` блоки - все это хорошие кандидаты на `t.plan()`.