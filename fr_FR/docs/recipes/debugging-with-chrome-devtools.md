___
**Note du traducteur**

C'est la traduction du fichier [debugging-with-chrome-devtools.md](https://github.com/avajs/ava/blob/master/docs/recipes/debugging-with-chrome-devtools.md). Voici un [lien](https://github.com/avajs/ava/compare/c39b9dc5ba5a727fb88f55503e9ec5c657928d5e...master#diff-7621c4f96f622506986f9cf7a125d6a1) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `debugging-with-chrome-devtools.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Débogage des tests avec Chrome DevTools

Utilisez [inspect-process](https://github.com/jaridmargolin/inspect-process) pour lancer facilement une session de débogage avec Chrome DevTools.

```console
$ npm install --global inspect-process
```

```console
$ inspect node_modules/ava/profile.js some/test/file.js
```
