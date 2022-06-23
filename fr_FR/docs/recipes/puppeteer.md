___
**Note du traducteur**

C'est la traduction du fichier [puppeteer.md](https://github.com/avajs/ava/blob/main/docs/recipes/puppeteer.md). Voici un [lien](https://github.com/avajs/ava/compare/b208d143ad852dc95aa8b44eed94ac1f404a25f4...main#diff-6e1e8222bd9f948401f5a793dfe3f3f871e805a6d009426d82a098799fc19f7e) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `puppeteer.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Test d'applications Web à l'aide de Puppeteer

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/recipes/puppeteer.md)

## Les dépendances

- [Puppeteer](https://github.com/GoogleChrome/puppeteer) : `npm install --save-dev puppeteer`

## Configuration

La première étape consiste à configurer un helper pour configurer l'environnement :

`./test/_withPage.js`

```js
import puppeteer from 'puppeteer';

module.exports = async (t, run) => {
	const browser = await puppeteer.launch();
	const page = await browser.newPage();
	try {
		await run(t, page);
	} finally {
		await page.close();
		await browser.close();
	}
}
```

## Exemple d'utilisation

`./test/main.js`

```js
import test from 'ava';
import withPage from './_withPage';

const url = 'https://google.com';

test('le titre de la page doit contenir "Google"', withPage, async (t, page) => {
	await page.goto(url);
	t.true((await page.title()).includes('Google'));
});

test('la page doit contenir un élément avec le sélecteur `#hplogo`', withPage, async (t, page) => {
	await page.goto(url);
	t.not(await page.$('#hplogo'), null);
});

test('le formulaire de recherche doit correspondre au snapshot', withPage, async (t, page) => {
	await page.goto(url);
	const innerHTML = await page.evaluate(form => form.innerHTML, await page.$('#searchform'));
	t.snapshot(innerHTML);
});
```
