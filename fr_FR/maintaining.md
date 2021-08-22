___
**Note du traducteur**

C'est la traduction du fichier [maintaining.md](https://github.com/avajs/ava/blob/main/maintaining.md). Voici un [lien](https://github.com/avajs/ava/compare/7a668a63b19efdd1b6658eda404a1e5f06d9aa17...main#diff-af20adbc8ab4842b04d1f5c7df6f563a) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `maintaining.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Maintenance

## Conduite

**Soyez gentil avec tout le monde.** Lisez et adhérez au [Code de Conduite](code-of-conduct.md).

## Test

* `npm test`: Vérifie le code (Lint) et exécute tous les tests avec une couverture de code
* `tap test/fork.js --bail`: Exécute un fichier de test spécifique et s'arrête au premier échec (utile quand on cherche des bugs).

## CI

* Les tests échouent parfois sous Windows. Lisez attentivement les erreurs.
* Au moins un job Windows doit réussir.
* Tous les autres jobs doivent passer.

## Mise à jour des dépendances

* Assurez-vous que les nouvelles versions des dépendances soient compatibles avec nos versions Node.js prises en charge.
* Laissez la dépendance TypeScript en l'état pour éviter toute rupture accidentelle.
* Ouvrez un PR avec les mises à jour et ne fusionnez que lorsque le CI réussit (voir la section précédente).

## Mise à jour de TypeScript

TypeScript lui-même ne suit pas SemVer. Par conséquent, il se peut que nous devions apporter des modifications à la définition de type qui, techniquement, annulent les modifications pour les utilisateurs ayant une version plus ancienne de TypeScript. C'est OK, mais nous devrions en être conscients.

Mettez à jour la dépendance TypeScript uniquement lorsque cela est vraiment nécessaire. Cela permet d'éviter les ruptures accidentelles. Par exemple, nous ne nous appuierons pas accidentellement sur les nouvelles fonctionnalités de TypeScript.

Lorsqu'on parle d'utiliser les nouvelles fonctionnalités de TypeScript, ça pourrait être considéré comme un changement radical. Cela doit être évalué au cas par cas.

## Pull requests

- Les nouvelles fonctionnalités doivent être livrées avec des tests et de la documentation.
- Assurez vous que le [guide de contribution](contributing.md) est respecté.
- Faites un squash des commits avant de merger.

## Expérimentations

* Commencez par implémenter des modifications contenant des ruptures en tant qu'expérimentation, elles doivent nécessiter l'adhésion.
* Expédiez rapidement les nouvelles fonctionnalités en les considérant comme une expérimentation nécessitant l'adhésion.

## Processus de version

* Mettez à jour les dépendances (voir la section précédente).
* Si [nécessaire](docs/support-statement.md), mettez à jour le champ `engines` dans `package.json`.
	* Supprimez les versions non prises en charge (ou sur le point de l'être) de Node.js.
    * Lorsque vous effectuez une modification majeure de la version, veillez à exiger les dernières versions de chaque version de Node.js prise en charge.
* Publier une nouvelle version en utilisant [`np`](https://github.com/sindresorhus/np) avec un numéro de version selon [SemVer](http://semver.org).
* Ecrivez une [note de version](https://github.com/avajs/ava/releases/new) en suivant le style des notes des versions précédentes.
