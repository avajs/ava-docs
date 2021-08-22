___
**Note du traducteur**

C'est la traduction du fichier [contributing.md](https://github.com/avajs/ava/blob/main/.github/CONTRIBUTING.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...main#diff-cc4aac3e9be04e0413c9520f223b493c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `contributing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Contribuer à AVA

✨ Merci pour votre contribution à AVA ! ✨

Veuillez noter que ce projet est publié avec un [Code de conduite du contributeur](code-of-conduct.md). En participant à ce projet, vous vous engagez à respecter ses termes.

Traductions : [English](https://github.com/avajs/ava/blob/main/.github/CONTRIBUTING.md), [Español](https://github.com/avajs/ava-docs/blob/main/es_ES/contributing.md), [Italiano](https://github.com/avajs/ava-docs/blob/main/it_IT/contributing.md), [日本語](https://github.com/avajs/ava-docs/blob/main/ja_JP/contributing.md), [Português](https://github.com/avajs/ava-docs/blob/main/pt_BR/contributing.md), [Русский](https://github.com/avajs/ava-docs/blob/main/ru_RU/contributing.md), [简体中文](https://github.com/avajs/ava-docs/blob/main/zh_CN/contributing.md)

## Comment puis-je contribuer ?

### Améliorez la documentation

En tant qu'utilisateur de AVA, vous êtes le candidat idéal pour nous aider à améliorer notre documentation : corrections de typo, corrections d'erreur, de meilleures explications, plus d'exemples, etc. Ouvrez des issues pour des choses qui pourraient être améliorées. [Aidez à traduire nos docs.](https://github.com/avajs/ava-docs) N'importe quoi. Même les améliorations apportées à ce document.

Utilisez l'[étiquette `docs`](https://github.com/avajs/ava/labels/docs) pour trouver des suggestions, sur ce que nous aimerions voir en plus dans la documentation.

### Améliorez les issues

Certaines issues sont créées avec des informations manquantes, non reproductibles, ou simplement invalides. Aidez nous à rendre leur résolution plus facile. La gestion des issues prend beaucoup de temps, grâce à votre aide, nous pouvons consacrer plus de temps à la correction de bugs et l'ajout de fonctionnalités.

### Faites-nous un retour sur les issues

Nous sommes toujours intéressés par d'autres avis dans les discussions des issues. C'est une bonne occasion d'influencer l'orientation future de l'AVA.

L'[étiquette `question`](https://github.com/avajs/ava/labels/question) est le bon endroit pour trouver des discussions en cours.

### Écrivez du code

Vous pouvez utiliser les étiquettes pour découvrir les issues où vous pourriez aider :

* [`blocked`](https://github.com/avajs/ava/labels/blocked) où nous avons besoin d'aide pour débloquer la situation
* [`bug`](https://github.com/avajs/ava/labels/bug) pour les bugs connus que nous aimerions corriger
* [`enhancement`](https://github.com/avajs/ava/labels/enhancement) sont des fonctionnalités à ajouter
* [`performance`](https://github.com/avajs/ava/labels/performance) propose des idées sur la façon d'améliorer les performances de AVA

Les étiquettes [`help wanted`](https://github.com/avajs/ava/labels/help%20wanted) (aide demandée) et [`good for beginner`](https://github.com/avajs/ava/labels/good%20for%20beginner) (bon pour les débutants) sont particulièrement utiles.

Vous trouverez peut-être une issue qui est attribuée, ou qui a l'[étiquette `assigned`](https://github.com/avajs/ava/labels/assigned). Veuillez vérifier avant de commencer sur cette issue parce que quelqu'un d'autre est probablement en train de travailler dessus.

Nous aimerions corriger les [issues `priority`](https://github.com/avajs/ava/labels/priority) (prioritaires) en premier. Nous aimerions aussi voir des avancées sur les [issues `low-priority`](https://github.com/avajs/ava/labels/low%20priority) (de faible priorité). Les [issues `future`](https://github.com/avajs/ava/labels/future) sont celles que nous aimerions avoir, mais pas tout de suite. Veuillez vérifier avant de travailler sur celles-ci, car nous voulons peut-être pas pour l'instant assumer la charge de ces fonctionnalités.

Si vous mettez à jour des dépendances, assurez-vous que vous utilisez npm@5.6.0 et faites un commit de la mise à jour du fichier `package-lock.json`.

### Promenez-vous sur notre chat

Nous avons un [chat](https://spectrum.chat/ava). Venez, espionnez, parlez-nous et aidez les autres.

## Soumettez une issue

- Les issues sont pour les problèmes. Utilisez notre [chat](https://spectrum.chat/ava) ou [Stack Overflow](https://stackoverflow.com/questions/tagged/ava) pour le support.
- Cherchez l'erreur dans les issues existantes avant d'en ouvrir une.
- Assurez-vous d'utiliser la dernière version de AVA.
- Utilisez un titre clair et descriptif (en anglais).
- Donnez le maximum d'informations possibles : étapes pour reproduire le problème, message d'erreur, la version de Node.js, le système d'exploitation, etc (en anglais).
- Plus vous passerez du temps pour rédiger votre issue, plus vite nous la traiterons.
- [La meilleure façon de reporter une issue, c'est de le prouver par un test qui échoue.](https://twitter.com/sindresorhus/status/579306280495357953)

## Soumettez un pull request

- Les changements non triviaux doivent d'abord être débattus dans une issue : pour éviter de faire un travail inutile.
- Pour des tâches importantes, nous vous conseillons dès que possible de présenter votre travail à la communauté pour avoir son avis. Ouvrez un pull request, dès que vous avez fait le minimum nécessaire, pour démontrer votre idée. À ce stade précoce, ne vous inquiétez pas si les choses ne sont pas parfaites ou 100% terminées. Ajoutez un préfixe [WIP] dans le titre et décrivez ce que vous avez encore besoin de faire. Cela permet à ceux qui l'examine de ne pas chercher la petite bête ou de signaler des améliorations dont vous savez déjà que vous devez les faire.
- Les nouvelles fonctionnalités doivent être accompagnées avec des tests et de la documentation (en anglais).
- N'ajoutez pas des changements qui ne sont pas en rapport avec le pull request.
- Vérifiez (Lint) et testez avant de soumettre un pull request en exécutant `$ npm test`.
- Faites un pull request depuis une [branche](https://github.com/dchelimsky/rspec/wiki/Topic-Branches), pas depuis le master.
- Utilisez un titre clair et descriptif pour le pull request et les commits.
- Écrivez une description convaincante pour expliquer pourquoi nous devrions intégrer votre pull request. Il est de votre devoir de nous convaincre. Répondre à la question «pourquoi» est nécessaire et fournissez des cas d'utilisation.
- Nous pouvons vous demander de changer votre pull request. Il n'est pas utile d'ouvrir un autre pull request. [Mettez juste à jour celui qui existe déjà.](https://github.com/RichardLitt/knowledge/blob/master/github/amending-a-commit-guide.md)

Remarque: lorsque vous modifiez du code, n'oubliez pas que la devise de AVA (volé à Python), c'est d'avoir une unique méthode pour faire quelque chose. Par exemple, une demande pour ajouter un alias à l'API ([comme ceci](https://github.com/avajs/ava/pull/663)) sera probablement refusée s'il n'y a pas l'ajout d'un autre avantage important.

*Vous cherchez à faire votre première contribution à un projet open-source ? Cherchez pas plus loin ! AVA peut être l'un des projets et l'une des communautés les plus accueillants. Lisez l'article de blog ["Making your first contribution"](https://medium.com/@vadimdemedes/making-your-first-contribution-de6576ddb190) pour démarrer correctement et faire votre première participation pour AVA !*
