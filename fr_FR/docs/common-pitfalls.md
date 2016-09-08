___
**Note du traducteur**

C'est la traduction du fichier [common-pitfalls.md](https://github.com/avajs/ava/blob/master/docs/common-pitfalls.md). Voici un [lien](https://github.com/avajs/ava/compare/fe3cf4c88a73719981a210e962ecd4947aa75428...master#diff-7eb46230db3eba276054b9adbc6c82ca) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `common-pitfalls.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Pièges classiques

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/common-pitfalls.md)

## Plugin ESLint

Si vous utilisez [ESLint](http://eslint.org/), vous pouvez installer [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava). Il vous aidera à utiliser correctement AVA et ainsi d'éviter certains pièges classiques.

## AVA dans Docker

Si vous exécutez AVA dans Docker comme faisant partie de votre CI, vous devez ajuster les variables d'environnement adéquates. Plus précisément, en ajoutant `-e CI=true` dans la commande `docker exec`. Regardez [#751](https://github.com/avajs/ava/issues/751).

AVA utilise [is-ci](https://github.com/watson/is-ci) pour décider s'il se trouve ou pas dans un environnement de CI en utilisant [ces variables](https://github.com/watson/is-ci/blob/master/index.js).

## AVA et les limites du nombre de clients connectés

Vous utilisez peut-être un service qui permet seulement un nombre de connexions simultanées limitées. Par exemple, de nombreux services de base de données d'entreprises offrent un forfait gratuit avec une limite sur le nombre de clients pouvant être utiliser en même temps. AVA peut atteindre ces limites car il exécute plusieurs processus, mais les services bien écrit doivent émettre une erreur ou créer un goulot d'étranglement dans ces cas. Si celui que vous utilisez ne le fait pas, les tests se bloqueront.

Utilisez l'option `concurrency` pour limiter le nombre de processus exécutés. Par exemple, si votre abonnement au service permet 5 clients, vous devez exécuter AVA avec `concurrency=5` ou moins.

## Opérations asynchrones

Vous pouvez exécuter une opération asynchrone à l'intérieur d'un test et vous demander pourquoi elle ne se finit pas. Si votre opération asynchrone utilise des promesses, vous devez retourner la promesse :

```js
test(t => {
  return fetch().then(data => {
    t.is(data, 'foo');
  });
});
```

Si l'on utilise des callbacks, veuillez utilisez [`test.cb`](https://github.com/avajs/ava#callback-support):

```js
test.cb(t => {
  fetch((err, data) => {
    t.is(data, 'bar');
    t.end();
  });
});
```

Alternativement, vous pouvez transformer une fonction callback en promesse en utilisant quelque chose comme [pify](https://github.com/sindresorhus/pify).

---

Votre problème n'est pas listé ici ? Soumettez un pull request ou commentez sur [cette issue](https://github.com/avajs/ava/issues/404).
