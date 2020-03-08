# Pisanie testów

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/01-writing-tests.md)

Testy są uruchamiane jednocześnie. Możesz określić testy synchroniczne i asynchroniczne. Testy są uważane za synchroniczne, chyba że zwrócisz promise [observable](https://github.com/zenparsing/zen-observable), lub zadeklaruj jako test callback.

Musisz zdefiniować wszystkie testy synchronicznie. Nie można ich zdefiniować w środku `setTimeout`, `setImmediate`, etc.

AVA próbuje uruchomić pliki testowe z bieżącym katalogiem roboczym ustawionym na katalog zawierający Twój plik `package.json`.

## Proces izolacji

Każdy plik testowy jest uruchamiany w osobnym procesie Node.js. Umożliwia to zmianę stanu globalnego lub zastąpienie wbudowanego w jednym pliku testowym, bez wpływu na inny. Jest również doskonały do wydajności na nowoczesnych procesorach wielordzeniowych, umożliwiając równoległe wykonywanie wielu plików testowych.

AVA będzie ustawiał `process.env.NODE_ENV` dla `test`, dopóki środowisko `NODE_ENV` zmiennej zostanie ustawione. Jest to przydatne, jeśli testowany kod ma domyślne wartości testowe (na przykład podczas wybierania bazy danych, z którą należy się połączyć). Może to jednak powodować, że kod lub jego zależności będą zachowywać się inaczej. Zauważ, że `'NODE_ENV' in process.env` będzie zawsze `true`.

## Deklarowanie testów

Aby zadeklarować test, wywołaj funkcję `test` którą zaimportowałeś z AVA. Podaj wymagany tytuł i funkcję implementacyjną. Tytuły muszą być unikalne w każdym pliku testowym. Funkcja zostanie wywołana po uruchomieniu testu. Przekazano [obiekt wykonawczy](./02-execution-context.md) jako pierwszy argument.

**Uwaga:** W celu [ulepszonych komunikatów potwierdzających](./03-assertions.md#enhanced-assertion-messages) aby zachowywało się poprawnie, pierwszy argument **musi** być nazwany `t`.

```js
const test = require('ava');

test('my passing test', t => {
	t.pass();
});
```

## Uruchamianie testów szeregowo

Testy są domyślnie uruchamiane jednocześnie, jednak czasami trzeba napisać testy, które nie mogą być uruchomione jednocześnie. W tych rzadkich przypadkach możesz użyć modyfikatora `.serial`. Zmusi te testy do uruchomienia szeregowego *przed* tymi współbieżnymi.

```js
test.serial('passes serially', t => {
	t.pass();
});
```

Należy pamiętać, że dotyczy to tylko testów w obrębie określonego pliku testowego. AVA nadal będzie uruchamiać wiele plików testowych jednocześnie, chyba że przekażesz flagę [`--serial` CLI](./05-command-line.md).

Możesz użyć modyfikatora `.serial` ze wszystkimi testami, hooks i nawet `.todo()`, ale jest dostępny tylko w funkcji `test`.

## Wsparcie promise

Testy mogą zwracać promise. AVA będzie czekać na promise aby rozwiązać przed zakończeniem testu. Jeśli promise odrzuca test się nie powiedzie.

```js
test('resolves with unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

## Wsparcie funkcji Async

AVA ma wbudowaną obsługę [funkcji asynchronicznych](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// Async arrow function
test('promises the truth', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

## Wsparcie Observable

AVA ma wbudowaną obsługę [observables](https://github.com/zenparsing/es-observable). Jeśli zwrócisz observable z testu, AVA automatycznie zużyje go do końca przed zakończeniem testu.

*Nie musisz używać "callback mode" lub wywoływać `t.end()`.*

```js
test('handles observables', t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// Only even numbers
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

## Wsparcie Callback

AVA obsługuje używanie `t.end` jako końcowy callback podczas używania Node.js-style error-first callback APIs. AVA weźmie pod uwagę każdą prawdziwą wartość przekazaną jako pierwszy argument `t.end` dla wystąpienia błędu. Zauważ że `t.end` wymaga "callback mode", które można włączyć za pomocą łańcucha `test.cb`.

```js
test.cb('data.txt can be read', t => {
	// `t.end` automatically checks for error as first argument
	fs.readFile('data.txt', t.end);
});
```

## Przeprowadzanie określonych testów

Podczas programowania pomocne może być wykonanie tylko kilku konkretnych testów. Można to osiągnąć za pomocą modyfikatora `.only`:

```js
test('will not be run', t => {
	t.fail();
});

test.only('will be run', t => {
	t.pass();
});
```

Możesz użyć modyfikatora `.only` ze wszystkimi testami. Nie można go używać z hooks czy `.todo()`.

*Uwaga:* Modyfikator `.only` dotyczy pliku testowego, w którym jest zdefiniowany, więc jeśli uruchomisz wiele plików testowych, testy w innych plikach będą nadal działać. Jeśli chcesz tylko uruchomić test `test.only`, podaj tylko ten plik testowy do AVA.

## Pomijanie testów

Czasami nieudane testy mogą być trudne do naprawy. Możesz poprosić AVA o pominięcie tych testów za pomocą modyfikatora `.skip`. Nadal będą wyświetlane w danych wyjściowych (jako pomijane), ale nigdy nie są uruchamiane.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

Musisz określić funkcję implementacji. Możesz użyć modyfikatora `.skip` ze wszystkimi testami i hooks, ale nie z `.todo()`. Nie możesz zastosować dalszych modyfikatorów do `.skip`.

## Testowanie symboli zastępczych ("todo")

Możesz użyć modyfikatora `.todo` kiedy planujesz napisać test. Podobnie jak testy pominięte, te symbole zastępcze są wyświetlane w danych wyjściowych. Wymagają tylko tytułu; nie można określić funkcji implementacji.

```js
test.todo('will think about writing this later');
```

Możesz zasygnalizować, że musisz napisać serial test:

```js
test.serial.todo('will think about writing this later');
```

## Nieudane testy

Możesz użyć modyfikatora `.failing` do udokumentowania issues z Twoim kodem który potrzebuje naprawy. Testy zakończone niepowodzeniem są uruchamiane tak jak normalne, ale oczekuje się, że zakończą się niepowodzeniem i nie spowodują uszkodzenia kompilacji. Jeśli test oznaczony jako zakończony niepowodzeniem faktycznie przejdzie, zostanie zgłoszony jako błąd i nie powiedzie się kompilacja z pomocnym komunikatem z instrukcją usunięcia modyfikatora `.failing`.

To pozwala Ci mergować testy `.failing` przed wprowadzeniem poprawki bez zerwania CI. Jest to świetny sposób dla rozpoznania dobry raportów bugów PR'ów z commit credit, nawet jeśli reporter nie jest w stanie naprawić problemu.

```js
// See: github.com/user/repo/issues/1234
test.failing('demonstrate some bug', t => {
	t.fail(); // Test will count as passed
});
```

## Before & after hooks

AVA pozwala zarejestrować hooks, które są uruchamiane przed i po testach. Umożliwia to uruchomienie kodu instalacyjnego i / lub porzucenia.

`test.before()` rejestruje hook do uruchomienia przed pierwszym testem w pliku testowym. Podobnie `test.after()` rejestruje hook do uruchomienia po ostatnim teście. Użyj `test.after.always()` do rejestracji hook który będzie **zawsze** uruchamiał raz testy i inne kompletne hooks. `.always()` hooks działają niezależnie od tego, czy wystąpiły wcześniejsze awarie, dlatego idealnie nadają się do zadań czyszczenia. Pamiętaj jednak, że nieprzechwycone wyjątki, nieobsługiwane odrzucenia lub przekroczenia limitu czasu spowodują awarię testów, prawdopodobnie zapobiegając `.always()` hooks z działania.

`test.beforeEach()` rejestruje hook do użycie przed każdym testem w Twoim pliku testu. Podobnie `test.afterEach()` rejestruje hook do użycia po każdym teście. Użyj `test.afterEach.always()` aby zarejestrować po hook, które jest wywoływane, nawet jeśli inne testy hooks lub sam test zawiodą.

Jeśli test zostanie pominięty przy pomocy modyfikatora `.skip`, odpowiedni hook `.beforeEach()`, `.afterEach()` i `.afterEach.always()` nie będzie działał. Podobnie, jeśli wszystkie testy w pliku testowym zostaną pominięte `.before()`, `.after()` i `.after.always()` hooks dla pliku nie są uruchamiane.

Podobnie jak `test()` metody te przyjmują opcjonalny tytuł i funkcję implementacyjną. Tytuł jest wyświetlany, jeśli hook się nie uruchomi. Implementacja jest wywoływana z [execution object](./02-execution-context.md). Możesz używać asercji dla Twoich hooks. Możesz także przekazać [macro function](#reusing-test-logic-through-macros) i dodatkowe argumenty.

`.before()` hooks wykonuje przed `.beforeEach()` hooks. `.afterEach()` hooks wykonuje przed `.after()` hooks. W ramach swojej kategorii hooks wykonać w kolejności, w jakiej zostały zdefiniowane. Domyślnie hooks są wykonywane jednocześnie, ale możesz ich użyć `test.serial` aby upewnić się, że jednocześnie uruchamiany jest tylko jeden hook. W przeciwieństwie do testów, szeregowe hooks *nie* są uruchamiane przed innymi hooks:

```js
test.before(t => {
	// This runs before all tests
});

test.before(t => {
	// This runs concurrently with the above
});

test.serial.before(t => {
	// This runs after the above
});

test.serial.before(t => {
	// This too runs after the above, and before tests
});

test.after('cleanup', t => {
	// This runs after all tests
});

test.after.always('guaranteed cleanup', t => {
	// This will always run, regardless of earlier failures
});

test.beforeEach(t => {
	// This runs before each test
});

test.afterEach(t => {
	// This runs after each test
});

test.afterEach.always(t => {
	// This runs after each test and other test hooks, even if they failed
});

test('title', t => {
	// Regular test
});
```

Hooks mogą być synchroniczne lub asynchroniczne, dokładnie tak jak testy. Aby zrobić hook asynchroniczny zwróć promise lub observable, użyj funkcji async, lub włącz tryb callback przez `test.before.cb()`, `test.beforeEach.cb()` etc.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.beforeEach.cb(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Pamiętaj że `.beforeEach()` i `.afterEach()` hooks uruchamiane tuż przed uruchomieniem testu i po nim, a domyślnie testy są uruchamiane jednocześnie. Oznacza to każdą wielokrotność hooks `.beforeEach()` mogą działać jednocześnie. Użycie `test.serial.beforeEach()` nie zmienia tego. Jeśli musisz ustawić stan globalny dla każdego testu (np. szpiegowanie na `console.log` [na przykład](https://github.com/avajs/ava/issues/560)), musisz się upewnić, że same testy są [uruchamiane seryjnie](#running-tests-serially).

Pamiętaj, że AVA uruchamia każdy plik testowy we własnym procesie. Być może nie będziesz musiał wyczyścić stanu globalnego w `.after()`- hook, ponieważ jest to wywoływane tylko tuż przed zakończeniem procesu.

## Test kontekstu

Hooks może współdzielić kontekst z testem:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('context data is foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Kontekst utworzony w `.before()` hooks jest [klonowany](https://www.npmjs.com/package/lodash.clone) przed przekazaniem do `.beforeEach()` hooks i / lub testów. `.after()` oraz `.after.always()` hooks otrzymuje oryginalną wartość kontekstu.

Dla `.beforeEach()`, `.afterEach()` i `.afterEach.always()` hooks kontekst *nie* jest współdzielony między różnymi testami, co pozwala skonfigurować dane w taki sposób, aby nie przenikały do innych testów.

Domyślnie `t.context` jest obiektem, ale możesz go ponownie przypisać:

```js
test.before(t => {
	t.context = 'unicorn';
});

test('context is unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

## Pobieranie test meta data

Pliki pomocnicze mogą określić nazwę pliku uruchamianego testu, czytając `test.meta.file`.  Eliminuje to konieczność przekazywania `__filename` z testu do helpers.

```js
const test = require('ava');

console.log('Test currently being run: ', test.meta.file);
```

## Ponowne użycie logiki testowej w makrach

Dodatkowe argumenty przekazane do deklaracji testu zostaną przekazane do implementacji testu. Jest to przydatne do tworzenia makr testowych wielokrotnego użytku.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

Możesz zbudować tytuł testu programowo, dołączając funkcję `title` do makra:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle = '', input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Argument `providedTitle` domyślnie dla `undefined` jeśli użytkownik nie poda tytułu ciągu. Oznacza to, że możesz użyć przypisania parametru do ustawienia wartości domyślnej. W powyższym przykładzie domyślny jest pusty ciąg znaków.

Możesz także przekazywać tablice funkcji makro:

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

Zachęcamy do korzystania z makr zamiast budowania własnych generatorów testowych ([tutaj jest przykład](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) kodu, który należy zastąpić makrem). Makra są zaprojektowane do wykonywania statycznej analizy kodu, co może prowadzić do lepszej wydajności, integracji IDE i reguł lintera.
