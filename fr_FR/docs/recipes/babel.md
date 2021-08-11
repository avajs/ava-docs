___
**Note du traducteur**

C'est la traduction du fichier [babel.md](https://github.com/avajs/ava/blob/main/docs/recipes/babel.md). Voici un [lien](https://github.com/avajs/ava/compare/b4ea43529a6d058a96055735cfa6e7056c009112...master#diff-dc9bcfba97caa3c85c58f839ac3f6d37) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `babel.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de Babel

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/recipes/babel.md)

**La prochaine version 3 de AVA supprime le support Babel intégré. Consultez plutôt la documentation [AVA 2](https://github.com/avajs/ava-docs/blob/2c3ceef77e84f23025753adb14ebf948946a7438/fr_FR/docs/recipes/babel.md).**

Vous pouvez activer la prise en charge de Babel en installant `@ava/babel`, et ensuite dans la configuration de AVA, en mettant `babel` à `true`.

**`package.json` :**

```json
{
	"ava": {
		"babel": true
	}
}
```

En savoir plus sur [`@ava/babel`](https://github.com/avajs/babel).
