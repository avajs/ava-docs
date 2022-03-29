___
**역자주**

이 문서는 [puppeteer.md](https://github.com/avajs/ava/blob/main/docs/recipes/puppeteer.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# Puppeteer를 사용해서 웹앱 테스트하기

## 의존성(dependencies) 설치

- [Puppeteer](https://github.com/GoogleChrome/puppeteer): `npm install --save-dev puppeteer`

## 설정하기

첫 번째 단계는 환경을 구성할 helper를 설정하는 것입니다.

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

## 사용 예제

`./test/main.js`

```js
import test from 'ava';
import withPage from './_withPage';

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
