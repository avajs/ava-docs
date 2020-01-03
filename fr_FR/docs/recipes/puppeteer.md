___
**Note du traducteur**

C'est la traduction du fichier [puppeteer.md](https://github.com/avajs/ava/blob/master/docs/recipes/puppeteer.md). Voici un [lien](https://github.com/avajs/ava/compare/79b2ea30c125f44e4d47bdafdeec351cddb5911a...master#diff-b813a3ab6d7f84d0c4210e7cb047c774) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `puppeteer.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Test d'applications Web à l'aide de Puppeteer

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/puppeteer.md)

## Les dépendances

- [Puppeteer](https://github.com/GoogleChrome/puppeteer) : `npm install --save-dev puppeteer`

## Configuration

La première étape consiste à configurer un helper pour configurer l'environnement :

`./test/_withPage.js`

```js
const puppeteer = require('puppeteer');

export default async function withPage(t, run) {
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
const test = require('ava');
const withPage = require('./_withPage');

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
