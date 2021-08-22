___
**Note du traducteur**

C'est la traduction du fichier [05-command-line.md](https://github.com/avajs/ava/blob/main/docs/05-command-line.md). Voici un [lien](https://github.com/avajs/ava/compare/8fa28254dbebef32cbde05c0c9a49061d0ef82f8...main#diff-691ae7cb3c0d49e7c7e7d1887739ecf8) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `05-command-line.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
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
  ava reset-cache          Reset AVA's compilation cache and exit
                           (Réinitialise le cache de compilation d'AVA et sort)

Positionals: (Positionnels:)
  pattern  Glob patterns to select what test files to run. Leave empty if you
           want AVA to run all test files instead                       [string]
           (Glob patterns pour sélectionner les fichiers de test à exécuter.
           Laissez vide si vous voulez que AVA exécute tous les fichiers de
           test à la place)

Options
  --version               Show version number (Affiche n° de version)  [boolean]
  --color                 Force color output                           [boolean]
                          (Force la sortie en couleur)
  --config                Specific JavaScript file for AVA to read its config
                          from, instead of using package.json or ava.config.*
                          files (Fichier JavaScript spécifique pour AVA pour lire
                          sa configuration, au lieu d'utiliser des fichiers
                          package.json ou ava.config.*)
  --help                  Show help (Affiche l'aide)                   [boolean]
  --concurrency, -c       Max number of test files running at the same time
                          (Default: CPU cores)                          [number]
                          (Nombre max de fichiers de test exécutés en même
                          temps (Par défaut : coeurs CPU))
  --fail-fast             Stop after first test failure                [boolean]
                          (S'arrête dès qu'un test échoue)
  --match, -m             Only run tests with matching title (Can be repeated)
                                                                        [string]
                          (Exécute seulement les tests qui correspondent au
                          titre (peut être répété))
  --serial, -s            Run tests serially                           [boolean]
                          (Exécute les tests en série)
  --tap, -t               Generate TAP output                          [boolean]
                          (Génére une sortie au format TAP)
  --timeout, -T           Set global timeout (milliseconds or human-readable,
                          e.g. 10s, 2m)                                 [string]
                          (Définir un timeout global (millisecondes ou
                          lisible par l'homme, exemple 10s, 2m))
  --update-snapshots, -u  Update snapshots (Met à jour les snapshots)  [boolean]
  --verbose, -v           Enable verbose output                        [boolean]
                          (Active le mode verbose)
  --watch, -w             Re-run tests when files change               [boolean]
                          (Re-exécute les tests quand les fichiers ont changé)

Examples:
  ava
  ava test.js
```

*Notez que le CLI utilisera votre installation locale de AVA lorsqu'il est disponible, même lorsqu'il est exécuté de manière globale.*

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

## Réinitialisation du cache de AVA

AVA peut mettre en cache certains fichiers, en particulier lorsque vous utilisez notre fournisseur [`@ava/babel`](https://github.com/avajs/babel). S'il semble que vos derniers changements ne sont pas pris en compte par AVA, vous pouvez réinitialiser le cache en exécutant :

```console
npx ava reset-cache
```

Cela supprime tous les fichiers du répertoire `node_modules/.cache/ava`.

## Reporters

Par défaut, Ava utilise le mini reporter :

<img src="https://github.com/avajs/ava/raw/main/media/mini-reporter.gif" width="460">

Utilisez l'option `--verbose` pour activer le reporter verbose. Il est toujours utilisé dans les environnements de CI, sauf si [`--tap`](#reporter-de-tap) est activé.

<img src="https://github.com/avajs/ava/raw/main/media/verbose-reporter.png" width="294">

### Reporter de TAP

AVA prend en charge le format de TAP et est compatible avec [tous les reporters de TAP](https://github.com/sindresorhus/awesome-tap#reporters). Utilisez l'option `--tap` pour activer la restitution de TAP.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/raw/main/media/tap-reporter.png" width="420">

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du [mode watch](./recipes/watch-mode.md).
