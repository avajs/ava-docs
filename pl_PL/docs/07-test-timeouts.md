# Test timeoutów

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/07-test-timeouts.md)

Limity czasu w AVA zachowują się inaczej niż w innych frameworkach testowych. AVA resetuje licznik czasu po każdym teście, zmuszając testy do zakończenia, jeśli nie otrzymano nowych wyników testu w określonym czasie. Można to wykorzystać do obsługi zablokowanych testów.

Domyślny limit czasu to 10 sekund.

Możesz skonfigurować limity czasu za pomocą `--timeout` [opcji wiersza poleceń](./05-command-line.md), lub w [konfiguracji](./06-configuration.md). Można je ustawić w sposób czytelny dla człowieka:

```console
npx ava --timeout=10s # 10 seconds
npx ava --timeout=2m # 2 minutes
npx ava --timeout=100 # 100 milliseconds
```

Limity czasu można również ustawić indywidualnie dla każdego testu. Limity czasu są resetowane za każdym razem, gdy dochodzi do asercji.

```js
test('foo', t => {
	t.timeout(100); // 100 milliseconds
	// Write your assertions here
});
```
