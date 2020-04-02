# Debugowanie testów z Visual Studio Code

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-vscode.md)

Możesz debugować swoje testy za pomocą [Visual Studio Code](https://code.visualstudio.com/).

## Tworzenie konfiguracji uruchamiania

1. Otwórz obszar roboczy dla swojego projektu.
1. Na pasku bocznym kliknij uchwyt *Debug*.
1. Utwórz plik `launch.json`.
1. Wybierz środowisko Node.js.
1. Dodaj następujące do tablicy `configurations` i zapisz zmiany:

  ```json
  {
    "type": "node",
    "request": "launch",
    "name": "Debug AVA test file",
    "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/ava",
    "runtimeArgs": [
      "debug",
      "--break",
      "${file}"
    ],
    "port": 9229,
    "outputCapture": "std",
    "skipFiles": [
      "<node_internals>/**/*.js"
    ]
  }
  ```

## Używanie debuggera

Otwórz plik(i), które chcesz debugować. Możesz ustawić punkty przerwania (breakpoints) lub użyć słowa kluczowego `debugger`.

Teraz, *z otwartym plikiem testowym*, z menu *Debug* uruchom konfigurację *Debug AVA test file*.

## Debugowanie wstępnie skompilowanych testów

Jeśli skompilujesz swoje pliki testowe w innym katalogu i uruchom testy *z* tego katalogu, powyższa konfiguracja nie będzie działać.

Zakładając, że nazwy plików testowych są unikalne, możesz zamiast tego wypróbować następującą konfigurację. Zakłada się, że dane wyjściowe kompilacji są zapisywane w katalogu `build`. Dostosuj odpowiednio:


```json
{
  "type": "node",
  "request": "launch",
  "name": "Debug AVA test file",
  "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/ava",
  "runtimeArgs": [
    "debug",
    "--break",
    "build/**/${fileBasenameNoExtension}.*"
  ],
  "port": 9229,
  "outputCapture": "std",
  "skipFiles": [
    "<node_internals>/**/*.js"
  ]
}
```

## Seryjne debugowanie

Domyślnie AVA uruchamia testy jednocześnie. Może to skomplikować debugowanie. Dodaj konfigurację za pomocą argumentu `--serial` więc AVA uruchamia tylko jeden test na raz:

```json
{
  "type": "node",
  "request": "launch",
  "name": "Debug AVA test file",
  "runtimeExecutable": "${workspaceFolder}/node_modules/.bin/ava",
  "runtimeArgs": [
    "debug",
    "--break",
    "--serial",
    "${file}"
  ],
  "port": 9229,
  "outputCapture": "std",
  "skipFiles": [
    "<node_internals>/**/*.js"
  ]
}
```

*Zauważ, że jeśli twoje testy nie są odpowiednio izolowane, niektóre niepowodzenia testów mogą nie pojawić się podczas ich seryjnego uruchamiania.*
