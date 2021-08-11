___
**Note du traducteur**

C'est la traduction du fichier [new-rule.md](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/new-rule.md). Voici un [lien](https://github.com/avajs/eslint-plugin-ava/compare/4846440797d9c8b84ed0e1a373e934d8736d3739...main#diff-064bdc96ec67d46e25fa387f8421f8ca) vers les différences avec le master de eslint-plugin-ava (Si en cliquant sur le lien, vous ne trouvez pas le fichier `new-rule.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Création d'une nouvelle règle

Traductions : [English](https://github.com/avajs/eslint-plugin-ava/blob/master/docs/new-rule.md)

## Prérequis

- [Lisez les docs de ESLint sur la création d'une nouvelle règle.](https://eslint.org/docs/developer-guide/working-with-rules)
- Consulter les commits des règles précédemment ajoutées pour vous aider. Par exemple, la [règle `no-async-fn-without-await`](https://github.com/avajs/eslint-plugin-ava/commit/a443d7a9c94165f42749938e6b491a7c10749b6c).


## Astuce

Utilisez le [site `astexplorer`](https://astexplorer.net) avec l'analyseur `espree` et la transformation `ESLint v4` pour créer de manière interactive l'implémentation initiale de la règle. Il vous permet d'inspecter l'AST complet que vous obtiendrez avec ESLint et vous pouvez même voir le résultat de votre implémentation pour corriger automatiquement.


## Les étapes

- Allez dans le répertoire `test` et dupliquez le fichier `no-todo-test.js`, renommez le avec le nom de votre règle. Ensuite, écrivez quelques tests avant de commencer à implémenter la règle.
- Allez dans le répertoire `rules` et dupliquez le fichier `no-todo-test.js`, renommez le avec le nom de votre règle. Ensuite, commencez l'implémentation logique de la nouvelle règle.
- Ajouter le bon [`meta.type`](https://eslint.org/docs/developer-guide/working-with-rules#rule-basics) à la règle.
- Allez dans le répertoire `docs/rules` et dupliquez le fichier `no-todo-test.md`, renommez le avec le nom de votre règle. Ensuite écrivez la documentation.
- Ajouter la règle par ordre alphabétique dans :
	- [La config recommandée](https://github.com/avajs/eslint-plugin-ava/blob/0ded4b5c3cd09504e846309760566c9499a24196/index.js#L19)
	- [La config recommandée dans le readme](https://github.com/avajs/eslint-plugin-ava/blame/0ded4b5c3cd09504e846309760566c9499a24196/readme.md#L35)
	- [La liste des règles dans le readme](https://github.com/avajs/eslint-plugin-ava/blame/0ded4b5c3cd09504e846309760566c9499a24196/readme.md#L73)<br>
	*(La description doit être la même que l'entête du fichier de documentation).*
- Exécutez `$ npm test` pour s'assurer que les tests passent.
- Exécutez `$ npm run integration` pour re-exécuter les règles dans de vrais projets afin de s'assurer que votre règle n'échoue dans le code du monde réel.
- Ouvrez une pull request avec un titre qui correspond exactement à ce format `` Add `nom-règle` rule ``, par exemple, `` Add `no-unused-properties` rule ``.
- La description de la pull request doit inclure l'issue qu'elle corrige, par exemple, `Fixes #123`.
