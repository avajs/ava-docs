___
**Note du traducteur**

C'est la traduction du fichier [05-command-line.md](https://github.com/avajs/ava/blob/master/docs/05-command-line.md). Voici un [lien](https://github.com/avajs/ava/compare/2dae2bfaf4b4ae53700fa439f34923b5a2c35a83...master#diff-691ae7cb3c0d49e7c7e7d1887739ecf8) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `05-command-line.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# CLI

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/05-command-line.md)

```console
$ npx ava --help

  Usage
    ava [<file> ...]

  Options
    --watch, -w             Re-run tests when tests and source files change (Re-exécute les tests quand les tests et les fichiers sources ont changé)
    --match, -m             Only run tests with matching title (Can be repeated) (Exécute seulement les tests qui correspondent au titre (peut être répété))
    --update-snapshots, -u  Update all snapshots (Mettre à jour les snapshots)
    --fail-fast             Stop after first test failure (Arrêter dès qu'un test échoue)
    --timeout, -T           Set global timeout (milliseconds or human-readable, e.g. 10s, 2m) (Définir un timeout global (millisecondes ou lisible par l'homme, exemple 10s, 2m))
    --serial, -s            Run tests serially (Lancer les tests en série)
    --concurrency, -c       Max number of test files running at the same time (Default: CPU cores) (Nombre max de fichiers de test exécutés en même temps (Par défaut : coeurs CPU))
    --verbose, -v           Enable verbose output (Activer le mode verbose)
    --tap, -t               Generate TAP output (Générer une sortie au format TAP)
    --color                 Force color output (Force l'affichage avec des couleurs)
    --no-color              Disable color output (Désactiver l'affichage avec des couleurs)
    --reset-cache           Reset AVA's compilation cache and exit (Réinitialise le cache de compilation d'AVA et sort)
    --config                JavaScript file for AVA to read its config from, instead of using package.json
                            or ava.config.js files (Le fichier JavaScript permettant à AVA de lire sa configuration, à la place des fichiers package.json ou ava.config.js)

  Examples (Exemples)
    ava
    ava test.js test2.js
    ava test-*.js
    ava test

  The above relies on your shell expanding the glob patterns. (Ce qui précède dépend de votre shell pour étendre les patterns globaux.)
  Without arguments, AVA uses the following patterns: (Sans arguments, AVA utilise les patterns suivants :)
    **/test.js **/test-*.js **/*.spec.js **/*.test.js **/test/**/*.js **/tests/**/*.js **/__tests__/**/*.js
```

*Notez que le CLI utilisera votre installation locale de AVA lorsqu'il est disponible, même lorsqu'il est exécuté de manière globale.*

AVA recherche les fichiers de tests en utilisant les patterns suivants :

* `**/test.js`
* `**/test-*.js`
* `**/*.spec.js`
* `**/*.test.js`
* `**/test/**/*.js`
* `**/tests/**/*.js`
* `**/__tests__/**/*.js`

Les fichiers à l'intérieur de `node_modules` sont *toujours* ignorés. Idem pour les fichiers commençant par `_`, ils sont traités comme des helpers.

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

AVA met en cache les fichiers de test compilés et les helpers. Il recompile automatiquement ces fichiers lorsque vous les modifiez. AVA fait de son mieux pour détecter les modifications apportées à vos fichiers de configuration, plugins et preset Babel. Toutefois, s'il semble que votre dernière configuration Babel ne soit pas appliquée, vous pouvez exécuter AVA avec l'option `--reset-cache` pour réinitialiser le cache d'AVA. S'il est défini, tous les fichiers du répertoire `node_modules/.cache/ava` sont supprimés. Exécutez AVA normalement pour appliquer votre nouvelle configuration Babel.

## Reporters

Par défaut, Ava utilise le mini reporter :

<img src="https://github.com/avajs/ava/blob/master/media/mini-reporter.gif" width="460">

Utilisez l'option `--verbose` pour activer le reporter verbose. Il est toujours utilisé dans les environnements de CI, sauf si [`--tap`](#reporter-de-tap) est activé.

<img src="https://github.com/avajs/ava/blob/master/media/verbose-reporter.png" width="294">

### Reporter de TAP

AVA prend en charge le format de TAP et est compatible avec [tous les reporters de TAP](https://github.com/sindresorhus/awesome-tap#reporters). Utilisez l'option `--tap` pour activer la restitution de TAP.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/avajs/ava/blob/master/media/tap-reporter.png" width="420">

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du [mode watch](./recipes/watch-mode.md).
