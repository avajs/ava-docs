___
**Note du traducteur**

C'est la traduction du fichier [contributing.md](https://github.com/avajs/ava/blob/main/.github/CONTRIBUTING.md). Voici un [lien](https://github.com/avajs/ava/compare/711bcf268cb40ec21b967bef4d0927f9de1a71ac...main#diff-98e64bc1cd2db9333c6effe87bbe0d6dfe8714aba4c6bde45aa037fe0796e44c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `contributing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Contribuer à AVA

✨ Merci pour votre contribution à AVA ! ✨

Veuillez noter que ce projet est publié avec un [Code de conduite du contributeur](code-of-conduct.md). En participant à ce projet, vous vous engagez à respecter ses termes.

Traductions : [English](https://github.com/avajs/ava/blob/main/.github/CONTRIBUTING.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/contributing.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/contributing.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/contributing.md), [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/contributing.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/contributing.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/contributing.md)

## Comment puis-je contribuer ?

### Améliorez la documentation

En tant qu'utilisateur de AVA, vous êtes le candidat idéal pour nous aider à améliorer notre documentation : corrections de typo, corrections d'erreur, de meilleures explications, plus d'exemples, etc. Ouvrez des issues pour des choses qui pourraient être améliorées. [Aidez à traduire nos docs.](https://github.com/avajs/ava-docs) N'importe quoi. Même les améliorations apportées à ce document.

Utilisez l'[étiquette `scope:documentation`](https://github.com/avajs/ava/labels/scope%3Adocumentation) pour trouver des suggestions, sur ce que nous aimerions voir en plus dans la documentation.

### Améliorez les issues

Certaines issues sont créées avec des informations manquantes, non reproductibles, ou simplement invalides. Aidez nous à rendre leur résolution plus facile. La gestion des issues prend beaucoup de temps, grâce à votre aide, nous pouvons consacrer plus de temps à la correction de bugs et l'ajout de fonctionnalités.

### Faites-nous un retour sur les issues

Nous sommes toujours intéressés par d'autres avis dans les discussions des issues. C'est une bonne occasion d'influencer l'orientation future de AVA.

Les étiquettes [`needs triage`](https://github.com/avajs/ava/labels/needs%20triage) et [`question`](https://github.com/avajs/ava/labels/question) sont les bons endroits pour trouver des discussions en cours.

### Aidez-nous

Vous pouvez utiliser les étiquettes pour découvrir les issues où vous pourriez aider :

* [`blocked`](https://github.com/avajs/ava/labels/blocked) où nous avons besoin d'aide pour débloquer la situation
* [`bug`](https://github.com/avajs/ava/labels/bug) pour les bugs connus que nous aimerions corriger
* [`enhancement`](https://github.com/avajs/ava/labels/enhancement) sont des fonctionnalités à ajouter
* [`performance`](https://github.com/avajs/ava/labels/performance) propose des idées sur la façon d'améliorer les performances de AVA

Les étiquettes [`help wanted`](https://github.com/avajs/ava/labels/help%20wanted) (aide demandée) et [`good for beginner`](https://github.com/avajs/ava/labels/good%20for%20beginner) (bon pour les débutants) sont particulièrement utiles.

Vous trouverez peut-être une issue qui est attribuée. Veuillez vérifier avant de commencer sur cette issue parce que quelqu'un d'autre est probablement en train de travailler dessus.

Nous aimerions corriger les [issues `priority`](https://github.com/avajs/ava/labels/priority) (prioritaires) en premier. Nous aimerions aussi voir des avancées sur les [issues `low-priority`](https://github.com/avajs/ava/labels/low%20priority) (de faible priorité). Les [issues `future`](https://github.com/avajs/ava/labels/future) sont celles que nous aimerions avoir, mais pas tout de suite. Veuillez vérifier avant de travailler sur celles-ci, car nous voulons peut-être pas pour l'instant assumer la charge de ces fonctionnalités.

Vous trouverez ci-dessous des conseils pour contribuer au code.

### Promenez-vous et discutez

Nous utilisons [GitHub Discussions] (https://github.com/avajs/ava/discussions). Allez-y pour observer, nous parler et aider les autres.

## Contribuer au code

Lorsque vous trouvez une issue sur laquelle vous aimeriez travailler, laissez un commentaire pour que les autres soient au courant. Nous vous affecterons alors l'issue.

Bien sûr, vous pouvez travailler sur des choses qui n'ont pas encore d'issue. Cependant, si vous comptez fournir un effort important, il est préférable d'en discuter avant.

Lorsque vous êtes prêt à recevoir des commentaires sur votre travail, ouvrez une [draft pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests#draft-pull-requests). Ce n'est pas grave si le travail n'est pas encore terminé, mais faites-nous savoir ce qu'il reste à faire. Les relecteurs sauront ainsi qu'il ne faut pas pinailler sur les petits détails ou signaler les améliorations que vous savez déjà devoir apporter.

La relecture de pull request importantes peut prendre beaucoup de temps. Un temps qui n'est pas toujours disponible. Les demandes plus petites peuvent être traitées plus rapidement. Si vous introduisez une nouvelle fonctionnalité, réfléchissez à la manière dont elle pourrait être décomposée. Il n'y a pas de mal à introduire des fonctionnalités comme [expérimentales](https://github.com/avajs/ava/blob/main/docs/06-configuration.md#experiments). Celles-ci nécessitent moins de documentation et de couverture de test.

Essayez d'éviter d'apporter des modifications de ruptures (breaking changes). Celles-ci sont plus longues à livrer. Au lieu de cela, faites en sorte que le nouveau comportement soit expérimental. De cette façon, votre fonctionnalité peut être livrée, et vous pouvez l'utiliser, selon son propre échéancier.

Les fonctionnalités non expérimentales doivent être accompagnées de tests et de documentation.

N'incluez pas de modifications sans rapport avec la fonctionnalité dans votre pull request. Assurez-vous que les tests passent sur votre machine en exécutant npm test. Vous pouvez également lancer des fichiers de test spécifiques en utilisant `npx tap test-tap/{file}.js` or `npx test-ava test/{file}.js`.

Lorsque vous faites une pull request, veuillez utiliser un titre clair et descriptif. Soyez précis sur ce qui a été modifié et pourquoi.

Assurez-vous que la case *Allow edits from maintainers* est cochée. De cette façon, nous pourrons apporter nous-mêmes certaines modifications mineures, ce qui permettra de fusionner votre pull request plus rapidement.

Il se peut que l'on vous demande d'apporter des modifications à votre pull request. Il n'est jamais nécessaire d'ouvrir une autre pull request. Envoyez plus de commits à votre branche existante. Nous les écraserons (squash) lorsque nous fusionnerons le PR.

Les dépendances sont gérées par `npm`. Ne mettez à jour les dépendances que lorsque cela est nécessaire pour votre pull request. Ne reconstruisez pas le fichier de verrouillage (lockfile).

Et enfin, amusez-vous bien !
