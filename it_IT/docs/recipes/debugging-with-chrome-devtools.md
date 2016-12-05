# Fare debugging con i Chrome DevTools

Traduzioni:
[Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-chrome-devtools.md),
[Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/debugging-with-chrome-devtools.md)

Usa [inspect-process](https://github.com/jaridmargolin/inspect-process) per lanciare una sessione di debugging con i Chrome DevTools.

```console
$ npm install --global inspect-process
```

```console
$ inspect node_modules/ava/profile.js some/test/file.js
```
