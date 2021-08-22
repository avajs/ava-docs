# Tryb watch

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/watch-mode.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/watch-mode.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/watch-mode.md)

AVA jest wyposażony w inteligentny tryb watch. Sprawdza, czy pliki się zmieniają i uruchamia tylko te testy, których to dotyczy.


## Uruchamianie testów z włączonym trybem watch

Możesz włączyć tryb watch za pomocą flag `--watch` lub `-w`. Jeśli zainstalowałeś AVA globalnie:

```console
$ ava --watch
```

Jeśli skonfigurowałeś to w swoim `package.json` w ten sposób:

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

Możesz uruchomić:

```console
$ npm test -- --watch
```

Możesz także skonfigurować specjalny skrypt:

```json
{
	"scripts": {
		"test": "ava",
		"watch:test": "ava --watch"
	}
}
```

I wtedy użyć:

```console
$ npm run watch:test
```

Na koniec możesz skonfigurować AVA, aby *zawsze* działał w trybie watch, ustawiając klucz `watch` w [`ava` sekcji twojego `package.json`, lub pliku `ava.config.*`][config].

**`package.json`:**

```json
{
	"ava": {
		"watch": true
	}
}
```

Pamiętaj, że reporter TAP jest niedostępny podczas korzystania z trybu watch.

## Wymagania

AVA używa [`chokidar`] jako pliku watcher. Pamiętaj, że nawet jeśli zobaczysz ostrzeżenia o niepowodzeniu opcjonalnych zależności podczas instalacji, nadal będzie działać poprawnie. Proszę odnieść się do sekcji *[Install Troubleshooting]* z dokumentacji `chokidar` po to jak rozwiązać problemy z instalacją chokidar.

## Ignorowanie zmian

Domyślnie AVA sprawdza zmiany we wszystkich plikach, z wyjątkiem tych z rozszerzeniem `.snap.md`, `ava.config.*` i plików w [niektórych katalogach](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) zgodnie z pakietem [`ignore-by-default`].

Możesz skonfigurować dodatkowe wzorce, aby pliki były ignorowane w pliku [`ava` sekcji twojego `package.json`, lub pliku `ava.config.*`][config], używając klucza `ignoredByWatcher`.

Jeśli testy zapisują się na dysku, mogą spowodować, że watcher uruchomi ponownie testy. Konfigurowanie dodatkowych wzorców ignorowania pomaga tego uniknąć.

## Śledzenie zależności

AVA śledzi, od których plików źródłowych zależą twoje pliki testowe. Jeśli zmienisz taką zależność, zostanie ponownie uruchomiony tylko plik testowy, który jest od niej zależny. AVA uruchomi ponownie wszystkie testy, jeśli nie będzie w stanie ustalić, który plik testowy zależy od zmienionego pliku źródłowego.

Śledzenie zależności działa dla wymaganych modułów. Niestandardowe rozszerzenia i transpilery są obsługiwane, pod warunkiem [dodania ich do twojego pliku `package.json` lub `ava.config.*`][config], a nie z pliku testowego. Pliki dostępne za pomocą modułu `fs` nie są śledzone.

## Tryb watch i modyfikator `.only`

[Modyfikator `.only`] wyłącza algorytm śledzenia zależności trybu watch. Po wprowadzeniu zmiany wszystkie testy `.only` zostaną ponownie uruchomione, niezależnie od tego, czy test zależy od zmienionego pliku.

## Tryb watch oraz CI

Jeśli uruchomisz AVA w CI z trybem watch, wykonanie zakończy się z błędem (`Error : Watch mode is not available in CI, as it prevents AVA from terminating.`). AVA nie będzie działać z opcją `--watch` (`-w`) w CI, ponieważ procesy CI powinny zostać zakończone, wraz z opcją `--watch`, AVA nigdy się nie zakończy.

## Ręcznie uruchom ponownie wszystkie testy

Możesz szybko ponownie uruchomić wszystkie testy, wpisując <kbd>r</kbd> w konsoli, a następnie <kbd>Enter</kbd>.

## Aktualizacja snapshotów

Możesz zaktualizować nieudane migawki, wpisując <kbd>u</kbd> na konsoli, a następnie <kbd>Enter</kbd>.

## Debugowanie

Czasami tryb oglądania robi coś zaskakującego, jak ponowne uruchamianie wszystkich testów, gdy wydaje się, że zostanie uruchomiony tylko jeden test. Aby zobaczyć jego uzasadnienie, możesz włączyć tryb debugowania. Działa to najlepiej z pełnym reporterem:

```console
$ DEBUG=ava:watcher npm test -- --watch --verbose
```

W systemie Windows użyj:

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch --verbose
```

## Pomóż nam ulepszyć tryb watch

Tryb watch jest stosunkowo nowy i mogą występować pewne nierówne krawędzie. Proszę [zgłosić](https://github.com/avajs/ava/issues) wszelkie napotkane problemy. Dzięki!

[`chokidar`]: https://github.com/paulmillr/chokidar
[Install Troubleshooting]: https://github.com/paulmillr/chokidar#install-troubleshooting
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`.only` modifier]: ../01-writing-tests.md#running-specific-tests
[config]: ../06-configuration.md
