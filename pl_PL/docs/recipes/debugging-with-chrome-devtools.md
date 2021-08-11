# Debuggowanie testów z Chrome DevTools

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/debugging-with-chrome-devtools.md)

Możesz debugować swoje testy za pomocą [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools).

Otwórz Chrome, a następnie przejdź do <chrome://inspect/>. Kliknij link *Otwórz dedykowane narzędzia DevTools dla Node* w obrębie sekcji *Urządzenia*.

W oknie *DevTools dla Node*, przejdź do *Źródła* i w lewej kolumnie wybierz *Filesystem*. Dodaj katalog projektu do obszaru roboczego. Upewnij się, że udzieliłeś pozwolenia.

Teraz uruchom konkretny plik testowy:

```console
npx ava debug test.js
```

DevTools powinien połączyć się automatycznie, a twoje testy zostaną uruchomione. Użyj DevTools, aby ustawić punkty przerwania lub użyj słowa kluczowego `debugger`.

Uruchom z opcją `--break` aby upewnić się, że DevTools osiągnęło punkt przerwania tuż przed załadowaniem pliku testowego:

```console
npx ava debug --break test.js
```

Domyślnie inspektor nasłuchuje na `127.0.0.1:9229`. Możesz dostosować hosta i port:

```console
npx ava debug --host 0.0.0.0 --port 9230 test.js
```

Musisz dodać połączenie dla tego portu w zakładce *Connection*.
