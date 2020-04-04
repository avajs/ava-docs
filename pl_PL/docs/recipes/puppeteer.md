# Testowanie aplikacji internetowych za pomocą Puppeteer

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/puppeteer.md)

## Zależności

- [Puppeteer](https://github.com/GoogleChrome/puppeteer): `npm install --save-dev puppeteer`

## Ustawienie

Pierwszym krokiem jest skonfigurowanie pomocnika do skonfigurowania środowiska:

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

## Przykład użycia

`./test/main.js`

```js
const test = require('ava');
const withPage = require('./_withPage');

const url = 'https://google.com';

test('page title should contain "Google"', withPage, async (t, page) => {
	await page.goto(url);
	t.true((await page.title()).includes('Google'));
});

test('page should contain an element with `#hplogo` selector', withPage, async (t, page) => {
	await page.goto(url);
	t.not(await page.$('#hplogo'), null);
});

test('search form should match the snapshot', withPage, async (t, page) => {
	await page.goto(url);
	const innerHTML = await page.evaluate(form => form.innerHTML, await page.$('#searchform'));
	t.snapshot(innerHTML);
});
```
