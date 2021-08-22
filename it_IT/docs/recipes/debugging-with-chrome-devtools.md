___
**Nota del traduttore**

Questa è la traduzione del file [debugging-with-chrome-devtools.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-chrome-devtools.md). Qui c'è il [link](https://github.com/avajs/ava/compare/c09462c3e515c41da8177a3d9ba5fb0f19759653...main#diff-7621c4f96f622506986f9cf7a125d6a1) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `debugging=with-chrome-devtools.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Fare debugging con i Chrome DevTools

Traduzioni:
[Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/debugging-with-chrome-devtools.md),
[Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/debugging-with-chrome-devtools.md)

Usa [inspect-process](https://github.com/jaridmargolin/inspect-process) per lanciare una sessione di debugging con i Chrome DevTools.

```console
$ npm install --global inspect-process
```

```console
$ inspect node_modules/ava/profile.js some/test/file.js
```
