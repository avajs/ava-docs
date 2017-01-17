___
**Note du traducteur**

C'est la traduction du fichier [001 - Improving language support.md](https://github.com/avajs/ava/blob/master/docs/specs/001 - Improving language support.md). Voici un [lien](https://github.com/avajs/ava/compare/a380fe4f5ffa1d334a2a3786215d3c1ff10a9898...master#diff-01b90728d033fffd4c5cdacef3735d16) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `001 - Improving language support.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Amélioration du support de langages

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/specs/001 - Improving language support.md)

Une [RFC](http://blog.npmjs.org/post/153881413635/some-notes-on-rfcs) ([Définition de RFC sur Wikipédia](https://fr.wikipedia.org/wiki/Request_for_comments)) avec une proposition pour améliorer le support de AVA pour les projets Babel, React et TypeScript.

## Énoncé du problème

L'intégration de AVA avec des projets basés sur Babel est trop lourde. Les utilisateurs doivent configurer `babel-core/register` pour que les fichiers des sources et des helpers soient compilés. La configuration pour compiler des fichiers de test est déroutante. Les fichiers sources peuvent avoir [besoin d'une configuration différente s'ils sont chargés dans AVA][source options reason] ou s'ils sont construits pour la production.

Il n'y a aucun support pour écrire des tests et des sources dans d'autres langages tels que TypeScript ou JSX.

## Contexte

AVA utilise Babel pour permettre aux utilisateurs d'écrire des tests à l'aide de [ES2015](https://babeljs.io/docs/plugins/preset-es2015/) et des propositions de [stage-2](https://babeljs.io/docs/plugins/preset-stage-2/). Les messages d'assertion sont [améliorés à l'aide d'un plugin Babel](https://github.com/avajs/ava/pull/46) et un autre plugin est utilisé pour [pour détecter une utilisation inappropriée de `t.throws()`](https://github.com/avajs/ava/pull/742).

Initialement, `babel/register` était [directement utilisé](https://github.com/avajs/ava/pull/23) et appliqué aux fichiers de test, aux helpers et aux sources. Peu de temps après, cela a été modifié afin que les [fichiers de test soient transpilés](https://github.com/avajs/ava/issues/50). L'ancien comportement a été [considéré comme un bogue](https://github.com/avajs/ava/issues/108#issuecomment-151245367), probablement parce que AVA compilait les fichiers source contre l'avis de l'utilisateur.

Par la suite, les utilisateurs ont été [invités à ajouter `babel-core/register` à la liste des modules requis automatiquement lors de l'exécution des tests](https://github.com/avajs/ava#transpiling-imported-modules). Il s'avère que le chargement de Babel dans chaque processus est assez lent, et des tentatives ont été faites pour compiler plutôt les fichiers d'[helper][1078] et de [source][945] dans le processus principal de AVA.

Pendant ce temps, AVA a arrêté `babel/register` et a utilisé Babel directement. Une [implémentation de cache](https://github.com/avajs/ava/pull/352) a été faite par dessus.

AVA recherche uniquement les fichiers de test avec l'extension `.js`, même si les patterns du glob correspondent explicitement à d'autres fichiers. Par conséquent, cela empêche les [fichiers JSX et TypeScript d'être sélectionnés](https://github.com/avajs/ava/issues/631).

## Solutions possibles

[#945][945] tente de compiler toutes les dépendances du fichier de test (à la fois les fichiers de helper et les fichiers de source) dans le processus principal. Mis à part les problèmes non résolus, le gros inconvénient, c'est de ne pas pouvoir gérer les requires dynamiques, puisqu'ils se produisent dans les processus de travail plutôt que dans le processus principal. La recherche des dépendances ajoute sa propre surcharge.

[#1078][1078] précompile les fichiers helper dans le processus principal, comme sont précompilés les fichiers de test. Cela devrait fonctionner assez bien, mais sera bien sûr limité à la compilation des fichiers helper.

La [conclusion du #631][631 conclusion] était de permettre la définition de différentes extensions de fichiers de test. Malheureusement, le simple fait d'autoriser d'autres extensions est insuffisant. AVA continuera à supposer que les fichiers de test ne contiennent que du JavaScript. Il ne sera pas en mesure d’exécuter du JSX ou du TypeScript.

[#1122](https://github.com/avajs/ava/pull/1122) s'appuie sur la [proposition dans le #631][631 conclusion] en détectant si l'extension `.ts` est configurée et compile automatiquement ces fichiers de test à l'aide de TypeScript. Malheureusement, il n'est pas certain que cela fonctionnera pour les fichiers source sans rencontrer les mêmes problèmes de performances que avons déjà avec Babel. Les fichiers de test TypeScript  ne recevront pas d'assertions améliorées ou de protection contre une utilisation inappropriée de `t.throws()`. Il est difficile de le communiquer aux utilisateurs, sachant que la manière d'activer le support TypeScript, consiste à spécifier une option `extensions`.

## Proposition spécifique

Par défaut, AVA compile les fichiers test et les fichiers helper. Il utilise Babel, mais seulement avec des plugins pour les propositions de stage-4 et des normes ratifiées (actuellement c'est ES2016 plus les propositions qui ont atteint le stage-4 et seront inclus dans ES2017). Cela signifie que AVA prend en charge la même syntaxe que ESLint.

AVA n'applique plus [`babel-plugin-transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/). Ce plugin qui est un alias global d'ES2015 est inutile, puisque nous ciblons désormais Node.js 4.  Il s’agit d’un [piège connu](https://github.com/avajs/ava/issues/1089).

Les autres transformations d'AVA, telles que `babel-plugin-espower` et `babel-plugin-ava-throws-helper`, seront regroupées dans un preset `babel-preset-ava` qui sera automatiquement appliqué. (Si nécessaire, nous pourrions ajouter une option pour appliquer `babel-plugin-transform-runtime` avec la [logique de réécriture](https://github.com/avajs/ava/blob/033d4dcdcbdadbf665c740ff450c2a775a8373dc/lib/babel-config.js#L53:L61) que nous appliquons pour corriger les chemins d’accès (path). Nous pouvons adopter une approche passive et attendre de voir sur ce point.)

### Les projets avec Babel

Ce qui précède suppose que AVA est utilisé avec des projets JavaScript ne nécessitant pas de compilation. De nombreux utilisateurs ont déjà un pipeline Babel en place et souhaitent utiliser AVA sans avoir à précompiler leurs fichiers source.

Pour faire simple, le paramétrage `"babel": true` dans la configuration de AVA permet à AVA de gèrer les projets Babel. Les fichiers de test et les fichiers de helper sont compilés comme indiqués ci-dessus, mais les fichiers source sont maintenant automatiquement compilés.

AVA examine les fichiers `package.json` ou `.babelrc` du projet pour utiliser les options de Babel pour compiler les fichiers source (idéalement nous pouvons extraire un objet de configuration de Babel de ces deux endroits). Il s'agit d'une simplification de la gestion de la configuration actuelle de Babel, le processus recherche le fichier d’options le plus proche du fichier en cours de compilation.  Le fait de regarder ces deux fichiers spécifiques permet à AVA d'utiliser les résultats de compilation mis en cache sans même devoir charger Babel, tout en recompilant les fichiers source si les options changent.

La gestion des projets Babel par AVA peut être mieux configurée en passant un objet d'options à la place de `true` :

* `compileSources: true | false`: par défaut à `true`, détermine si les sources sont compilées.
* `extensions: "js" | ["js", "jsx", ...]`: par défaut à `"js"`, spécifie les extensions de fichiers autorisées. Ceci permet d'étendre le pattern des fichiers par défaut.
* `sourceOptions: {}`: spécifie les [options de Babel] utilisées pour compiler les fichiers source. Dans ce contexte, `babelrc: true` fait fusionner les options trouvées dans les fichiers `package.json` ou `.babelrc` du projet. `babelrc` est par défaut est à `true`.
* `testOptions: {}`: spécifie les [options de Babel] utilisées pour compiler les fichiers test et helper. Si cette option est activée, la configuration par défaut de Babel utilisée par AVA pour compiler les fichiers de test et de helper est désactivée. comme pour `sourceOptions` et `babelrc` par défaut à `true`. Définir `presets: ["ava"]` applique les transformations de AVA.

`sourceOptions` peut être utilisé pour étendre une configuration Babel partagée afin que les fichiers sources puissent être chargés dans les tests de AVA. Par exemple les utilisateurs peuvent [compter sur webpack pour résoudre la syntaxe du module ES2015 au moment de la compilation, mais doivent quand même appliquer `babel-plugin-transform-es2015-modules-commonjs` pour que les sources fonctionnent dans AVA][source options reason].

`sourceOptions` et `testOptions`, étant des [options de Babel], elles peuvent spécifier les valeurs `ignore` et `only`. Elles ne sont utilisées que pour déterminer si le fichier doit être compilé. Elles n'affectent pas la sélection des fichiers de test ni la surveillance des sources.

## Compilation

Basé sur cette [étude de faisabilité](https://github.com/avajs/ava/pull/1082), la compilation de Babel est déplacée dans des workers de test. Si les fichiers source doivent être compilés, AVA chargera son propre hook de require, plutôt que de s'appuyer sur `babel-core/register`.

Les options de Babel pour les fichiers de test, de helper et de source sont préparées dans le processus principal, puis partagées avec les workers. Les manipulations de mise en cache sont dérivées de ces configurations, c'est aussi le cas pour les autres dépendances qui pourraient être impliquées.

Les workers manipulent le contenu du fichier brut et inspectent un cache pour voir si un résultat précédemment compilé peut être utilisé. (Étant donné que les workers peuvent s'exécuter simultanément, il faut veiller à ce qu'ils lisent toutes les entrées du cache. C'est bien si le même fichier est compilé plus d'une fois.)

## Les projets avec TypeScript

Le support de TypeScript peut être fourni de la même manière que le support avancé de Babel décrit ci-dessus. Le paramétrage `"typescript": true` dans la config de AVA active le support de TypeScript pour les fichiers  `.ts` de test et helper, comme les sources. Un objet d'options peut également être fourni :

* `compileSources: true | false`: par défaut à `true`, détermine si les sources sont compilées.
* `extensions: "ts" | ["ts", "tsx", ...]`: par défaut à `"ts"`, spécifie les extensions de fichiers autorisées.  Ceci permet d'étendre le pattern des fichiers par défaut.
* `sourceOptions: {}`: spécifie les [options de TypeScript] utilisées pour compiler les fichiers source. L'option `extends` prend par défaut le fichier  `tsconfig.json` du projet, le cas échéant. Il doit être défini explicitement à  `null` pour éviter d'étendre ce fichier.
* `testOptions: {}`: spécifie les [options de TypeScript] utilisées pour compiler les fichiers test et helper. Se comporte comme `sourceOptions`, il n'existe pas de configuration par défaut pour les fichiers de test et de helper, contrairement aux projets de Babel.

Pour `sourceOptions` et `testOptions`, étant des [options de TypeScript], les options `files`, `include` et `exclude` n'affectent pas la sélection des fichiers de test ni la surveillance des sources.

## Détails supplémentaires d'implémentation

Le support de Babel et de TypeScript peut être fourni via des modules Node.js distincts. Ils doivent implémenter la même interface, pour faciliter l'intégration avec AVA.

AVA est livré avec le support de Babel, mais une dépendance distincte doit être installée pour que le support de TypeScript fonctionne. Une erreur utile est décrite si cette dépendance est manquante si le support TypeScript est activé.

AVA sélectionne les fichiers de test en fonction de la configuration combinée de `babel` et de `typescript`.

Les chemins relatifs dans `sourceOptions` et `testOptions` [doivent être relatifs au fichier `package.json`](https://github.com/avajs/ava/issues/707).

[1078]: https://github.com/avajs/ava/pull/1078
[631 conclusion]: https://github.com/avajs/ava/issues/631#issuecomment-248659780
[945]: https://github.com/avajs/ava/pull/945
[options de Babel]: https://babeljs.io/docs/usage/api/#options
[source options reason]: https://github.com/avajs/ava/issues/1139#issuecomment-267969417
[options de TypeScript]: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
