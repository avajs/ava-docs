# Konfiguracja testowa

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/test-setup.md)

Testy można skonfigurować za pomocą hooka `beforeEach()`. Często można zamiast tego użyć zwykłej funkcji konfiguracji. Ten przepis pomaga zdecydować, co najlepiej pasuje do Twojego przypadku użycia.

# Hook `beforeEach()` versus funkcje konfiguracji

Hook `beforeEach()` ma pewne wady. Na przykład nie można go wyłączyć dla określonych testów ani nie można zastosować do określonych testów. Alternatywnie możesz użyć prostych funkcji. Umożliwia to korzystanie z wielu funkcji konfiguracji dla różnych wymagań konfiguracji i wywoływanie różnych części konfiguracji z różnych testów. Możesz nawet mieć funkcje konfiguracji z parametrami, aby testy mogły dostosować własną konfigurację.

Załóżmy, że masz funkcję współdziałającą z systemem plików. Być może uruchamiasz kilka testów przy użyciu `mock-fs`, a następnie kilka, które korzystają z prawdziwego systemu plików i katalogu tymczasowego. Lub masz funkcję instalacyjną uruchamianą z poprawnymi danymi dla niektórych testów i niepoprawnymi danymi dla innych testów, wszystkie w tym samym pliku testowym.

Możesz zrobić wszystkie te rzeczy za pomocą zwykłych funkcji konfiguracji, ale są pewne kompromisy:

|`beforeEach()`| Setup functions
|---|---
| ⛔️ &nbsp; used for all tests| ✅ &nbsp; can change or skip depending on test
| ⛔️ &nbsp; more overhead for beginners, "some magic"| ✅ &nbsp; easier for beginners, "no magic"
| ✅ &nbsp; supports callback mode, built-in support for observables| ⛔️ &nbsp; must use promises for asynchronous behavior
| ✅ &nbsp; failure has friendly output| ⛔️ &nbsp; errors are attributed to the test
| ✅ &nbsp; corresponding `afterEach` and `afterEach.always` for cleanup| ⛔️ &nbsp; cannot easily clean up

## Kompleksowa konfiguracja testu

W tym przykładzie mamy oba hooki `beforeEach()`, a następnie kolejne modyfikacje w ramach każdego testu.

```js
test.beforeEach(t => {
	setupConditionA(t);
	setupConditionB(t);
	setupConditionC(t);
});

test('first scenario', t => {
	tweakSomething(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

test('second scenario', t => {
	tweakSomethingElse(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

Jeśli dla każdego testu trzeba zmienić zbyt wiele zmiennych, rozważ pominięcie opcji hook `beforeEach()` i przeprowadzanie kroków konfiguracji w ramach samych testów.

```js
test('first scenario', t => {
	setupConditionA(t);
	setupConditionB(t, {/* options */});
	setupConditionC(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

// In this test, setupConditionB() is never called.
test('second scenario', t => {
	setupConditionA(t);
	setupConditionC(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

## Praktyczny przykład

```js
test.beforeEach(t => {
	t.context = {
		authenticator: new Authenticator(),
		credentials: new Credentials('admin', 's3cr3t')
	};
});

test('authenticating with valid credentials', async t => {
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.true(await isValid);
});

test('authenticating with an invalid username', async t => {
	t.context.credentials.username = 'bad_username';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});

test('authenticating with an invalid password', async t => {
	t.context.credentials.password = 'bad_password';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});
```

Te same testy, teraz korzystające z funkcji instalacyjnych, wyglądałyby następująco.

```js
function setup({username = 'admin', password = 's3cr3t'} = {}) {
	return {
		authenticator: new Authenticator(),
		credentials: new Credentials(username, password)
	};
}

test('authenticating with valid credentials', async t => {
	const {authenticator, credentials} = setup();
	const isValid = authenticator.authenticate(credentials);
	t.true(await isValid);
});

test('authenticating with an invalid username', async t => {
	const {authenticator, credentials} = setup({username: 'bad_username'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});

test('authenticating with an invalid password', async t => {
	const {authenticator, credentials} = setup({password: 'bad_password'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});
```

## Łączenie hooków i funkcji konfiguracji

Oczywiście `beforeEach()` i funkcje zwykłej konfiguracji mogą być używane razem:

```js
test.beforeEach(t => {
	t.context = setupAllTests();
});

test('first scenario', t => {
	firstSetup(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});
```
