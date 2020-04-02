# Debugowanie testów z WebStorm

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-webstorm.md)

**Ten przepis jest nieaktualny.**

---

Począwszy od wersji 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) i innych IDE od JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional, i RubyMine z zainstalowaną wtyczką Node.js) pozwalają debugować testy AVA.


## Skonfiguruj za pomocą Node.js

Dodaj nowe *Node.js Run/Debug configuration*: wybierz `Edit Configurations...` z listy rozwijanej w prawym górnym rogu, a następnie kliknij `+` i wybierz *Node.js*.

W polu `JavaScript file` podaj ścieżkę do AVA w folderze projektu `node_modules`: `node_modules/.bin/ava` na macOS i Linux lub `node_modules/.bin/ava.cmd` na Windows.

W `Application parameters` przekaż na przykład flagi CLI i pliki testowe, które chcesz debugować, na przykład `--verbose test.js`.

W `Node parameters`, przekaż flagę `--inspect-brk` aby włączyć inspektora Node'a.

Zapisz konfigurację.

## Skonfiguruj za pomocą npm

Wykonaj `npx @ava/init` w katalogu projektu, aby dodać AVA do swojego `package.json`.

Twój `package.json` powinien wyglądać mniej więcej tak:

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^1.0.0"
	}
}
```

Dodaj nowe *npm Run/Debug configuration*: wybierz `Edit Configurations...` z listy rozwijanej w prawym górnym rogu, a następnie kliknij `+` i wybierz *npm*.

Użyj następujących parametrów konfiguracyjnych:

- `package.json`: Ścieżka do pliku twojego projektu `package.json`
- `Command`: `test`

Twoje IDE następnie wykona `npm run test` i w ten sposób wywoła `node_modules/.bin/ava` oraz konfigurację AVA, którą określiłeś w swoim package.json.

W `Node parameters`, przekaż `--inspect-brk`.

Nie zapomnij wybrać interpretera Node.js.

Zapisz konfigurację.

## Debug

Ustaw breakpointy w kodzie.

Kliknij zielony przycisk `Debug` obok listy konfiguracji w prawym górnym rogu. *Debug tool window* pojawi się. Po osiągnięciu punktu przerwania możesz ocenić zmienne i przejść przez kod. Podczas debugowania wielu plików testowych można przełączać się między procesami za pomocą menu rozwijanego w panelu Ramki.
