___
**Note du traducteur**

C'est la traduction du fichier [04-snapshot-testing.md](https://github.com/avajs/ava/blob/main/docs/04-snapshot-testing.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-4393cc239e3dbd4a27a2947be85a7bad4310a5dfa35c343e56f79baffe8d48fc) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `04-snapshot-testing.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Test d'instantané

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/04-snapshot-testing.md)

AVA prend en charge les tests instantanés, [comme présentés par Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), à travers son interface [Assertions](./03-assertions.md). Vous pouvez faire un instantané de n'importe quelle valeur.

Les instantanés sont stockés aux côtés de vos fichiers de test. Si vos tests se trouvent dans un dossier `test` ou `tests`, les instantanés seront stockés dans un dossier `snapshots`. Si vos tests se trouvent dans un dossier `__tests__` alors ils seront stockés dans un dossier `__snapshots__`.

Si nous avons un `~/project/test/main.js` qui contient des assertions d'instantané. AVA créera deux fichiers :

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

Le premier dossier contient l'instantané réel et est requis pour les comparaisons futures. Le deuxième fichier contient votre *rapport d'instantané*. Il est régénéré lorsque vous mettez à jour vos instantanés. Si vous le confiez au contrôle des sources, vous pouvez le comparer pour voir les modifications apportées à votre instantané.

AVA montrera pourquoi votre assertion d'instantané a échoué :

<img src="https://github.com/avajs/ava/raw/main/media/snapshot-testing.png" width="1048">

Vous pouvez ensuite vérifier votre code. Si le changement était intentionnel, vous pouvez utiliser l'option `--update-snapshots` (ou `-u`) pour mettre à jour vos instantanés :

```console
$ ava --update-snapshots
```

Si vous avez besoin de mettre à jour les instantanés pour un test particulier, vous pouvez utiliser `--update-snapshots` avec par exemple `--match` ou `.only()` pour sélectionner le test.

Vous pouvez définir un emplacement fixe pour stocker les fichiers d'instantanés dans la configuration d'AVA [inclus dans le `package.json`](./06-configuration.md) :

**`package.json` :**

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

Les fichiers instantanés seront enregistrés dans une arborescence qui reflète celle de vos fichiers de test.

Si vous utilisez AVA avec des fichiers de test précompilés, AVA essayera d'utiliser les source maps pour déterminer l'emplacement des fichiers d'origine. Les instantanés seront stockés à côté de ces fichiers, en suivant les mêmes règles que si AVA avait exécuté les fichiers originaux directement. C'est bien si vous écrivez vos tests en TypeScript (consultez notre [recette TypeScript](./recipes/typescript.md)).
