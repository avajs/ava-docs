___
**Note du traducteur**

C'est la traduction du fichier [watch-mode.md](https://github.com/avajs/ava/blob/main/docs/recipes/watch-mode.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-e759e03510c7ba375dbdc805e9348e62c3a3b3be18520c167a15c0d7e9751480) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `watch-mode` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Mode watch

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/watch-mode.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/docs/recipes/watch-mode.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/docs/recipes/watch-mode.md)

AVA est livré avec un mode watch intelligent. Il surveille les fichiers modifiés et exécute seulement les tests qui sont impactés.

## Exécution des tests avec le mode watch activé

Vous pouvez activer le mode watch en utilisant l'option `--watch` ou `-w`.

```console
$ npx ava --watch
```

Veuillez noter que le débogage intégré et le reporter TAP ne sont pas disponibles lors de l'utilisation du mode watch.

## Prérequis

AVA utilise [`chokidar`] pour surveiller les fichiers. Notez que même si vous voyez des avertissements lors de l'installation sur les dépendances optionnelles, il fonctionnera correctement. Veuillez vous référer à la section *[Install Troubleshooting]* de la documentation de `chokidar` pour savoir comment résoudre les problèmes d'installation avec chokidar.

## Ignorer les modifications

Par défaut AVA surveille les modifications de tous les fichiers, à l'exception de ceux avec une extension `.snap.md`, `ava.config.*` et des fichiers dans [certains répertoires](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) qui ont été fournis par le package [`ignore-by-default`].

Vous pouvez configurer des patterns supplémentaires pour ignorer des fichiers dans la [section `ava` de votre `package.json` ou le fichier `ava.config.*`][config] en utilisant la clé `ignoredByWatcher`.

Si vos tests écrivent quelque chose sur le disque, ils peuvent déclencher le mode watch pour re-exécuter vos tests. La configuration de patterns supplémentaires pour ignorer permet d'éviter cela.

## La surveillance de dépendance

AVA surveille les fichiers source qui dépendent de vos fichiers de tests. Si vous modifiez une telle dépendance, seul le fichier de test qui en dépend pourra être ré-exécuter. AVA ré-exécutera tous les tests s'il n'arrive pas à déterminer quel est le fichier de test qui dépend du fichier source.

La surveillance de dépendance fonctionne pour les modules exigés (require). Les extensions et transpileurs personnalisés sont pris en charge, à condition que [vous les ajoutiez dans votre `package.json` ou le fichier `ava.config.*`][config] au lieu de le faire à l'intérieur de votre fichier de test. Les fichiers accessibles à l'aide du module `fs` ne sont pas surveillés.

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
$ DEBUG=ava:watcher npx ava --watch
```

## Aidez-nous à améliorer le mode watch

Le mode watch est relativement jeune et il peut y avoir des petites erreurs. Veuillez [signaler](https://github.com/avajs/ava/issues) (en anglais) les problèmes que vous rencontrez. Merci !

[`chokidar`]: https://github.com/paulmillr/chokidar
[Install Troubleshooting]: https://github.com/paulmillr/chokidar#install-troubleshooting
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`.only`]: ../01-writing-tests.md#exécution-de-tests-spécifiques
[config]: ../06-configuration.md
