# Konfigurowanie AVA za pomocą Selenium WebDriverJS

Ten przepis pokazuje, jak używać Selenium WebDriverJS (oficjalna implementacja JavaScript) z AVA do testowania aplikacji internetowych.

## Ustawienie

Ten przepis wykorzystuje następujące pakiety:

1. [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver)
2. [chromedriver](https://www.npmjs.com/package/chromedriver)

Zainstaluj je za pomocą:

```console
$ npm install selenium-webdriver chromedriver
```

W ramach tego przepisu użyjemy Selenium do weryfikacji wyszukiwań w Internecie [Bing](https://www.bing.com) i [Google](https://www.google.com).

Utwórz następujące pliki:

- `./test/bingtest.js`
- `./test/googletest.js`

W obu plikach najpierw dołączmy pakiety:

```js
const test = require('ava');
const {Builder, By, Key, until} = require('selenium-webdriver');

require('chromedriver');
```

W pliku `bingtest.js`, dodaj następujący kod, który sprawdza, czy szukasz `webdriver` w Bing, zwracając wyniki.

```js
test('Bing Search', async t => {
	const keyword = 'webdriver';
	const driver = new Builder().forBrowser('chrome').build();
	await driver.get('https://www.bing.com');
	await driver.findElement(By.name('q')).sendKeys(keyword + Key.ENTER);
	await driver.wait(until.titleIs(keyword + ' - Bing'));
	t.true((await driver.findElements(By.css('#b_content #b_results li'))).length > 0);
	await driver.close();
});
```

W pliku `googletest.js`, zamiast jednego testu, dodajmy dwa testy, każdy dla warunków 'webdriver' i 'avajs'.

Ponieważ chcielibyśmy zainicjować webdriver przed każdym testem, używamy hooków [`beforeEach` and `afterEach`](../01-writing-tests.md#before--after-hooks) aby odpowiednio skonfigurować i usunąć sterownik. Korzystanie z tych hooków pomaga zmniejszyć ilość kodu, który napisalibyśmy w każdym z nich `test()`.

```js
test.beforeEach(async t => {
	t.context.driver = new Builder().forBrowser('chrome').build();
	await t.context.driver.get('https://www.google.com');
});

test.afterEach('cleanup', async t => {
	await t.context.driver.close();
});
```

Teraz dodajmy kod testowy:

```js
async function searchGoogle(driver, keyword) {
	await driver.findElement(By.name('q')).sendKeys(keyword + Key.ENTER);
	await driver.wait(until.titleIs(`${keyword} - Google Search`));
}

test('Google Search for avajs', async t => {
	const {driver} = t.context;
	await searchGoogle(driver, 'avajs');
	t.true((await driver.findElement(By.id('resultStats')).getText()).includes('results'));
});

test('Google Search for webdriver', async t => {
	const {driver} = t.context;
	await searchGoogle(driver, 'webdriver');
	t.true((await driver.findElement(By.id('resultStats')).getText()).includes('results'));
});
```

Jesteś teraz gotowy do uruchomienia testów. Dane wyjściowe powinny wyglądać mniej więcej tak:

```console
npx ava
DevTools listening on ws://127.0.0.1:49720/devtools/browser/9ebf4394-447b-4916-91cc-692d06d88896

DevTools listening on ws://127.0.0.1:49756/devtools/browser/6e19d9fe-4de6-40a3-b120-17067b3125ca

DevTools listening on ws://127.0.0.1:49757/devtools/browser/ac12c2da-eeed-40d8-9b23-4d2103ec8fac

  bingtest » Bing Search (7.2s)
  googletest » Google Search for avajs (2.5s)
  googletest » Google Search for webdriver (3.3s)

  3 tests passed
```
