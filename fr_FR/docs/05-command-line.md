___
**Note du traducteur**

C'est la traduction du fichier [05-command-line.md](https://github.com/avajs/ava/blob/main/docs/05-command-line.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-a89aa74aa84d41f245185beac2e62ca024ba87d84740430e06aacf9b156cf5ba) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `05-command-line.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# CLI

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/05-command-line.md)

```console
ava [<pattern>...]
ava debug [<pattern>...]
ava reset-cache

Commands: (Commandes:)
  ava [<pattern>...]       Run tests (Exécute les tests)               [default]
  ava debug [<pattern>...] Activate Node.js inspector and run the test file
                           (Active l'inspecteur Node.js et exécute le fichier
                           de test)
  ava reset-cache          Delete any temporary files and state kept by AVA,
                           then exit (Supprime tous les fichiers temporaires et
                           l'état conservés par AVA, puis quitte)

Positionals: (Positionnels:)
  pattern  Select which test files to run. Leave empty if you want AVA to run
           all test files as per your configuration. Accepts glob patterns,
           directories that (recursively) contain test files, and file paths
           optionally suffixed with a colon and comma-separated numbers and/or
           ranges identifying the 1-based line(s) of specific tests to run
           (Sélectionne les fichiers de test à exécuter. Laissez vide si vous
           voulez qu'AVA exécute tous les fichiers de test conformément à votre
           configuration. Accepte les glob patterns, les répertoires qui
           contiennent (récursivement) des fichiers de test, et les chemins de
           fichier éventuellement suffixés par deux points et des nombres
           et/ou des plages séparés par des virgules identifiant la ou les
           lignes basées sur 1 des tests spécifiques à exécuter.)
                                                                        [string]

Options
        --version         Show version number (Affiche n° de version)  [boolean]
        --color           Force color output                           [boolean]
                          (Force la sortie en couleur)
        --config          Specific JavaScript file for AVA to read its config
                          from, instead of using package.json or ava.config.*
                          files (Fichier JavaScript spécifique pour AVA pour lire
                          sa configuration, au lieu d'utiliser des fichiers
                          package.json ou ava.config.*)
        --help            Show help (Affiche l'aide)                   [boolean]
  -c, --concurrency        Max number of test files running at the same time
                          (Default: CPU cores)                          [number]
                          (Nombre max de fichiers de test exécutés en même
                          temps (Par défaut : coeurs CPU))
        --fail-fast       Stop after first test failure                [boolean]
                          (S'arrête dès qu'un test échoue)
  -m, --match             Only run tests with matching title (Can be repeated)
                          (Exécute seulement les tests qui correspondent au
                          titre (peut être répété))
                                                                        [string]
      --no-worker-threads Don't use worker threads
			                    (N'utilise pas de processus de travail)      [boolean]
      --node-arguments    Additional Node.js arguments for launching worker
                          processes (specify as a single string)
                          (Arguments Node.js supplémentaires pour le lancement de
                          processus (à spécifier comme une seule chaîne))
                                                                        [string]
  -s, --serial            Run tests serially                           [boolean]
                          (Exécute les tests en série)
  -t, --tap               Generate TAP output                          [boolean]
                          (Génére une sortie au format TAP)
  -T, --timeout           Set global timeout (milliseconds or human-readable,
                          e.g. 10s, 2m)
                          (Définir un timeout global (millisecondes ou
                          lisible par l'homme, exemple 10s, 2m))        [string]
  -u, --update-snapshots  Update snapshots (Met à jour les snapshots)  [boolean]
  -v, --verbose           Enable verbose output                        [boolean]
                          (Active le mode verbose)
  -w, --watch             Re-run tests when files change               [boolean]
                          (Re-exécute les tests quand les fichiers ont changé)

Examples:
  ava
  ava test.js
  ava test.js:4,7-9
```

AVA recherche les fichiers de tests en utilisant les patterns suivants :

* `test.js`
* `src/test.js`
* `source/test.js`
* `**/test-*.js`
* `**/*.spec.js`
* `**/*.test.js`
* `**/test/**/*.js`
* `**/tests/**/*.js`
* `**/__tests__/**/*.js`

Les fichiers à l'intérieur de `node_modules` sont *toujours* ignorés. Idem pour les fichiers commençant par `_` ou à l'intérieur de répertoires commençant par un seul `_`. De plus, les fichiers correspondant à ces patterns sont ignorés par défaut, sauf si différents patterns sont configurés :

* `**/__tests__/**/__helper__/**/*`
* `**/__tests__/**/__helpers__/**/*`
* `**/__tests__/**/__fixture__/**/*`
* `**/__tests__/**/__fixtures__/**/*`
* `**/test/**/helper/**/*`
* `**/test/**/helpers/**/*`
* `**/test/**/fixture/**/*`
* `**/test/**/fixtures/**/*`
* `**/tests/**/helper/**/*`
* `**/tests/**/helpers/**/*`
* `**/tests/**/fixture/**/*`
* `**/tests/**/fixtures/**/*`

Lors de l'utilisation de `npm test`, vous pouvez passer directement des arguments `npm test test2.js`, mais pour les options, vous devez les passez ainsi `npm test -- --verbose`.

## Exécution de tests correspondants à des titres

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/matching-titles?file=test.js&terminal=test&view=editor)

L'option `--match` permet d'exécuter des tests où le titre correspond à la demande. Ceci est réalisé avec des patterns génériques simples. Les patterns sont insensibles à la casse. Consultez [matcher](https://github.com/sindresorhus/matcher) pour plus d'informations.

Trouve les titres se terminant par `foo` :

```console
npx ava --match='*foo'
```

Trouve les titres commençant par `foo` :

```console
npx ava --match='foo*'
```

Trouve les titres contenant `foo` :

```console
npx ava --match='*foo*'
```

Trouve les titres qui ont *exactement* `foo` (bien que insensible à la casse) :

```console
npx ava --match='foo'
```

Trouve les titres ne contenant pas `foo` :

```console
npx ava --match='!*foo*'
```

Trouve les titres commençant par `foo` et se terminant par `bar` :

```console
npx ava --match='foo*bar'
```

Trouve les titres commençant par `foo` ou se terminant par `bar` :

```console
npx ava --match='foo*' --match='*bar'
```

Remarquez qu'un pattern qui correspond est prioritaire sur le modificateur `.only`. Seuls les tests avec un titre explicite sont recherchés. Les tests, sans titre ou dont le titre est dérivé de la fonction d'implémentation, seront sautés si `--match` est utilisé.

Voici ce qui arrive lorsque vous exécutez AVA avec un pattern `*oo*` pour les tests suivants :

```js
test('foo s\'exécutera', t => {
	t.pass();
});

test('moo s\'exécutera aussi', t => {
	t.pass();
});

test.only('boo s\'exécutera mais pas de manière exclusive', t => {
	t.pass();
});

// Ne s'exécutera pas, car n'a pas de titre
test(function (t) {
	t.fail();
});

// Ne s'exécutera pas, car n'a pas de titre explicite
test(function foo(t) {
	t.fail();
});
```

## Exécution de tests à des numéros de ligne spécifiques

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/specific-line-numbers?file=test.js&terminal=test&view=editor)

AVA vous permet de lancer des tests exclusivement en vous référant à leur numéro de ligne. Ciblez une seule ligne, une série de lignes ou les deux. Vous pouvez sélectionner n'importe quel numéro de ligne d'un test.

Le format est une liste séparée par des virgules de `[X|Y-Z]` où `X`, `Y` et `Z` sont des entiers entre `1` et le dernier numéro de ligne du fichier.

Cette fonctionnalité n'est disponible que depuis la ligne de commande.
### Exécution d'un seul test

Pour n'exécuter qu'un test particulier dans un fichier, ajoutez le numéro de ligne du test au chemin ou au modèle transmis à AVA.

Soit le fichier de test suivant :

`test.js`

```js
1: test('unicorn', t => {
2:   t.pass();
3: });
4:
5: test('rainbow', t => {
6:  t.fail();
7: });
```

En exécutant `npx ava test.js:2`, vous exécuterez le test `unicorn`. En fait, vous pouvez utiliser n'importe quel numéro de ligne entre `1` et `3`.

### Exécution de plusieurs tests

Pour exécuter plusieurs tests, vous pouvez les cibler un par un ou sélectionner une série de numéros de ligne. Comme les numéros de ligne sont donnés par fichier, vous pouvez exécuter plusieurs fichiers avec des numéros de ligne différents pour chaque fichier. Si le même fichier est fourni plusieurs fois, les numéros de ligne sont fusionnés et ne sont exécutés qu'une seule fois.

### Exemples

Numéros de ligne unique :

```console
npx ava test.js:2,9
```

Plage :

```console
npx ava test.js:4-7
```

Mélange de numéro de ligne unique et de plage :

```console
npx ava test.js:4,9-12
```

Différents fichiers :

```console
npx ava test.js:3 test2.js:4,7-9
```

Lorsqu'on exécute un fichier avec et sans numéros de ligne, les numéros de ligne sont prioritaires.

## Réinitialisation du cache de AVA

AVA maintient un certain état temporaire. Vous pouvez effacer cet état en exécutant :

```console
npx ava reset-cache
```

Cela supprime tous les fichiers du répertoire `node_modules/.cache/ava`.

## Reporters

AVA utilise par défaut un rapporteur lisible par l'homme :

<img src="https://github.com/avajs/ava/raw/main/media/verbose-reporter.png" width="294">

### Reporter de TAP

[![Ouvrir dans StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/tap-reporter?file=test.js&terminal=test&view=editor)

AVA prend en charge le format de TAP et est compatible avec [tous les reporters de TAP](https://github.com/sindresorhus/awesome-tap#reporters). Utilisez l'option `--tap` pour activer la restitution de TAP.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/raw/main/media/tap-reporter.png" width="420">

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du [mode watch](./recipes/watch-mode.md).

## Node arguments

L'argument `--node-arguments` peut être utilisé pour spécifier des arguments supplémentaires pour le lancement des processus de travail. Ceux-ci sont combinés avec la configuration `nodeArguments` et tous les arguments passés au binaire `node` lors du démarrage d'AVA.

**Ne passez que des valeurs de confiance.**

Spécifiez les arguments comme une seule chaîne :

```console
npx ava --node-arguments="--throw-deprecation --zero-fill-buffers"
```

**Ne passez que des valeurs de confiance.**
