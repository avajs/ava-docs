# CLI

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/05-command-line.md)

```console
ava [<pattern>...]
ava debug [<pattern>...]
ava reset-cache

Commands:
  ava [<pattern>...]        Run tests                                  [default]
  ava debug [<pattern>...]  Activate Node.js inspector and run a single test
                            file
  ava reset-cache           Reset AVA's compilation cache and exit

Positionals:
  pattern  Glob patterns to select what test files to run. Leave empty if you
           want AVA to run all test files instead                       [string]

Options:
  --version               Show version number                          [boolean]
  --color                 Force color output                           [boolean]
  --config                Specific JavaScript file for AVA to read its config
                          from, instead of using package.json or ava.config.*
                          files
  --help                  Show help                                    [boolean]
  --concurrency, -c       Max number of test files running at the same time
                          (default: CPU cores)                          [number]
  --fail-fast             Stop after first test failure                [boolean]
  --match, -m             Only run tests with matching title (can be repeated)
                                                                        [string]
  --node-arguments        Additional Node.js arguments for launching worker
                          processes (specify as a single string)        [string]
  --serial, -s            Run tests serially                           [boolean]
  --tap, -t               Generate TAP output                          [boolean]
  --timeout, -T           Set global timeout (milliseconds or human-readable,
                          e.g. 10s, 2m)                                 [string]
  --update-snapshots, -u  Update snapshots                             [boolean]
  --verbose, -v           Enable verbose output                        [boolean]
  --watch, -w             Re-run tests when files change               [boolean]

Examples:
  ava
  ava test.js
```

*Pamiętaj, że CLI użyje lokalnej instalacji AVA, jeśli jest dostępna, nawet jeśli działa globalnie.*

AVA wyszukuje pliki testowe przy użyciu następujących wzorców:

* `test.js`
* `src/test.js`
* `source/test.js`
* `**/test-*.js`
* `**/*.spec.js`
* `**/*.test.js`
* `**/test/**/*.js`
* `**/tests/**/*.js`
* `**/__tests__/**/*.js`

	Pliki w środku `node_modules` są *zawsze* ignorowane. Pliki zaczynają się od `_` lub w katalogach zaczynających się od jednego `_`. Ponadto pliki pasujące do tych wzorców są domyślnie ignorowane, chyba że skonfigurowane zostaną różne wzorce:

* `**/__tests__/**/__helper__/**/*`
* `**/__tests__/**/__helpers__/**/*`
* `**/__tests__/**/__fixture__/**/*`
* `**/__tests__/**/__fixtures__/**/*`
* `**/test/**/helper/**/*`
* `**/test/**/helpers/**/*`
* `**/test/**/fixture/**/*`
* `**/test/**/fixtures/**/*`
* `**/tests/**/helper/**/*`
* `**/tests/**/helpers/**/*`
* `**/tests/**/fixture/**/*`
* `**/tests/**/fixtures/**/*`

Podczas używania `npm test`, możesz przekazać argumenty pozycyjne bezpośrednio `npm test test2.js`, ale flagi muszą być przekazywane jako `npm test -- --verbose`.

## Przeprowadzanie testów z dopasowywaniem tytułów

Flaga `--match` pozwala uruchomić tylko testy o pasującym tytule. Osiąga się to za pomocą prostych wzorów wieloznacznych. Wzory nie uwzględniają wielkości liter. Zobacz [`matcher`](https://github.com/sindresorhus/matcher) po więcej szczegółów.

Dopasowanie tytułów kończących się na `foo`:

```console
npx ava --match='*foo'
```

Dopasowanie tytułów zaczynających się na `foo`:

```console
npx ava --match='foo*'
```

Dopasowanie tytułów zawierających `foo`:

```console
npx ava --match='*foo*'
```

Dopasuj tytuły, które są *dokładnie* `foo` (choć wielkość liter jest nieistotna):

```console
npx ava --match='foo'
```

Dopasuj tytuły niezawierające `foo`:

```console
npx ava --match='!*foo*'
```

Dopasuj tytuły zaczynające się od `foo` i kończące z `bar`:

```console
npx ava --match='foo*bar'
```

Dopasuj tytuły zaczynające się od `foo` lub kończące się z `bar`:

```console
npx ava --match='foo*' --match='*bar'
```

Zauważ, że wzorzec dopasowania ma pierwszeństwo przed modyfikatorem `.only`. Dopasowywane są tylko testy z wyraźnym tytułem. Testy bez tytułów lub których tytuł pochodzi od funkcji implementacji zostaną pominięte, gdy `--match` jest używane.

Oto, co się stanie, gdy uruchomisz AVA z wzorcem dopasowania `*oo*` oraz następujące testy:

```js
test('foo will run', t => {
	t.pass();
});

test('moo will also run', t => {
	t.pass();
});

test.only('boo will run but not exclusively', t => {
	t.pass();
});

// Won't run, no title
test(function (t) {
	t.fail();
});

// Won't run, no explicit title
test(function foo(t) {
	t.fail();
});
```

## Resetowanie cache AVA

AVA może buforować niektóre pliki, szczególnie gdy korzystasz z naszego [`@ava/babel`](https://github.com/avajs/babel) provider. Jeśli wygląda na to, że AVA nie odbiera Twoich najnowszych zmian, możesz zresetować cache, uruchamiając:

```console
npx ava reset-cache
```

Spowoduje to usunięcie wszystkich plików w katalogu `node_modules/.cache/ava`.

## Zgłaszanie raportów

Domyślnie AVA używa minimalnego reportera:

<img src="../media/mini-reporter.gif" width="460">

Użycie flagi `--verbose` aby włączyć pełny reporter. Jest to zawsze używane w środowiskach CI, chyba że [TAP reporter](#tap-reporter) jest włączony.

<img src="../media/verbose-reporter.png" width="294">

### TAP reporter

AVA obsługuje format TAP, a zatem jest kompatybilny z [dowolnym TAP reporterem](https://github.com/sindresorhus/awesome-tap#reporters). Użyj flagi `--tap` aby włączyć output TAP.

```console
$ npx ava --tap | npx tap-nyan
```

<img src="../media/tap-reporter.png" width="420">

Należy pamiętać, że reporter TAP jest niedostępny podczas korzystania z [trybu watch](./recipes/watch-mode.md).

## Argumenty Node

Argument `--node-arguments` może być użyty do podania dodatkowych argumentów do uruchomienia procesów roboczych. Są one połączone z configuracją `nodeArguments` oraz wszelkie argumenty przekazane do binarki `node` binary podczas uruchamiania AVA.

**Przekaż tylko zaufane wartości.**

Podaj argumenty jako pojedynczy ciąg:

```console
npx ava --node-arguments="--throw-deprecation --zero-fill-buffers"
```

**Przekaż tylko zaufane wartości.**
