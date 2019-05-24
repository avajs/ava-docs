___
**Note du traducteur**

C'est la traduction du fichier [watch-mode.md](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md). Voici un [lien](https://github.com/avajs/ava/compare/4f4dfc7534c9b2b35e20ec04e2c3d084556baa00...master#diff-92da4f3d087d796fdf4a45be88586b62) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `watch-mode` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Mode watch

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/watch-mode.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/watch-mode.md)

AVA est livré avec un mode watch intelligent. Il surveille les fichiers modifiés et exécute seulement les tests qui sont impactés.

## Exécution des tests avec le mode watch activé

Vous pouvez activer le mode watch en utilisant l'option `--watch` ou `-w`. Si vous avez installé AVA globalement :

```console
$ ava --watch
```

Si vous l'avez configuré dans votre `package.json` de cette manière :

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

Vous pouvez lancer :

```console
$ npm test -- --watch
```

Vous pouvez aussi mettre en place un script spécial :

```json
{
	"scripts": {
		"test": "ava",
		"watch:test": "ava --watch"
	}
}
```

Et l'utiliser ainsi :

```console
$ npm run watch:test
```

Enfin, vous pouvez configurer AVA pour qu'il s'exécute *toujours* en mode watch en définissant la clé `watch` dans la [section `ava` de votre `package.json` ou le fichier `ava.config.js`][config].

**`package.json` :**

```json
{
	"ava": {
		"watch": true
	}
}
```

Veuillez noter que le reporter TAP est indisponible lors de l'utilisation du mode watch.

## Prérequis

AVA utilise [`chokidar`] pour surveiller les fichiers. Notez que même si vous voyez des avertissements lors de l'installation sur les dépendances optionnelles, il fonctionnera correctement. Veuillez vous référer à la section *[Install Troubleshooting]* de la documentation de `chokidar` pour savoir comment résoudre les problèmes d'installation avec chokidar.

## Les fichiers sources et les fichiers de test

Dans AVA, il y a un distinction entre les *fichiers source* et les *fichiers de test*. Comme vous pouvez le devinez, les *fichiers de tests* contiennent vos tests. Les *fichiers sources* sont tous les autres fichiers que vous avez besoin pour que les tests s'exécutent, ce sont soit vos codes source ou soit vos fixtures.

Par défaut AVA surveille les modifications des fichiers de test, des fichiers d'instantanés, du `package.json`, et des autres fichiers `.js`. Il ignore les fichiers dans [certains répertoires](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) qui ont été fournis par le package [`ignore-by-default`].

Vous pouvez configurer des patterns pour les fichiers source dans la [section `ava` de votre `package.json` ou le fichier `ava.config.js`][config] en utilisant la clé `sources`.

Si vos tests écrivent quelque chose sur le disque, ils peuvent déclencher le mode watch pour re-exécuter vos tests. Configurez des patterns pour les fichiers source afin d'éviter cela.

## La surveillance de dépendance

AVA surveille les fichiers source qui dépendent de vos fichiers de tests. Si vous modifiez une telle dépendance, seul le fichier de test qui en dépend pourra être ré-exécuter. AVA ré-exécutera tous les tests s'il n'arrive pas à déterminer quel est le fichier de test qui dépend du fichier source.

La surveillance de dépendance fonctionne pour les modules exigés (require). Les extensions et transpileurs personnalisés sont pris en charge, à condition que [vous les ajoutiez dans votre `package.json` ou le fichier `ava.config.js`][config] au lieu de le faire à l'intérieur de votre fichier de test. Les fichiers accessibles à l'aide du module `fs` ne sont pas surveillés.

## Le mode watch et `.only`

[`.only`] désactive l'algorithme de suivi des dépendances du mode watch. Lorsqu'une modification est effectuée, tous les tests avec `.only` seront ré-exécutés, indépendamment du fait que le test dépend du fichier modifié.

## Mode watch et CI

Si vous exécutez AVA dans votre CI avec le mode watch, l'exécution s'arrêtera avec une erreur (`Error : Watch mode is not available in CI, as it prevents AVA from terminating.`). AVA ne s'exécutera pas avec l'option `--watch` (`-w`) dans le CI, car les processus du CI doivent se terminer et avec l'option `--watch`, AVA ne se terminera jamais.

## Relancer manuellement tous les tests

Vous pouvez rapidement relancer tous les tests en tapant <kbd>r</kbd> sur la console, suivie par <kbd>Entrée</kbd>.

## Mise à jour des instantanés

Vous pouvez mettre à jour les instantanés défaillants en tapant <kbd>u</kbd> sur la console, suivi par <kbd>Entrée</kbd>.

## Débogage

Parfois, le mode watch fait des choses surprenantes comme ré-exécuter tous les tests alors que vous pensiez qu'un seul test aurait été ré-exécuté. Pour voir son raisonnement, vous pouvez activer un mode de débogage. Cela fonctionne mieux avec le reporter verbose :

```console
$ DEBUG=ava:watcher npm test -- --watch --verbose
```

Sous Windows utilisez :

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch --verbose
```

## Aidez-nous à améliorer le mode watch

Le mode watch est relativement jeune et il peut y avoir des petites erreurs. Veuillez [signaler](https://github.com/avajs/ava/issues) (en anglais) les problèmes que vous rencontrez. Merci !

[`chokidar`]: https://github.com/paulmillr/chokidar
[Install Troubleshooting]: https://github.com/paulmillr/chokidar#install-troubleshooting
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`.only`]: ../01-writing-tests.md#exécution-de-tests-spécifiques
[config]: ../06-configuration.md
