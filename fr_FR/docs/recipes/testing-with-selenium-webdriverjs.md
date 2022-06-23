___
**Note du traducteur**

C'est la traduction du fichier [testing-with-selenium-webdriverjs.md](https://github.com/avajs/ava/blob/main/docs/recipes/testing-with-selenium-webdriverjs.md). Voici un [lien](https://github.com/avajs/ava/compare/576f534b345259055c95fa0c2b33bef10847a2af...main#diff-b3079259ee3cd6fee65018e738d4215c0431556b4ade8f459d81892391ae6651) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `testing-with-selenium-webdriverjs.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration d'AVA avec Selenium webDriverjs

Cette recette vous montre comment utiliser Selenium WebDriverjs (implémentation officielle en JavaScript) avec AVA pour tester les applications web.

## Installation

Cette recette utilise les packages suivants :

1. [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver)
2. [chromedriver](https://www.npmjs.com/package/chromedriver)

Installez-les ainsi :

```console
$ npm install selenium-webdriver chromedriver
```

Dans le cadre de cette recette, nous utiliserons Selenium pour vérifier les recherches web sur [Bing](https://www.bing.com) et [Google](https://www.google.com).

Créez les fichiers suivants :

- `./test/bingtest.js`
- `./test/googletest.js`

Dans les deux fichiers, incluons d'abord les packages :

```js
import test from 'ava';
import {Builder, By, Key, until} from 'selenium-webdriver';
import 'chromedriver';
```

Dans le fichier `bingtest.js`, ajoutez le code suivant, qui teste si la recherche pour le terme « webdriver » sur Bing, retourne des résultats.

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

Dans le fichier `googlest.js`, au lieu d'un seul test, ajoutons deux tests, un pour le terme « webdriver » et l'autre pour « avajs ».

Puisque nous souhaitons initialiser le webdriver avant chaque test, nous utilisons les hooks [`beforeEach` et `afterEach`](../01-writing-tests.md#les-hooks-before--after) pour respectivement configurer et nettoyer le pilote.L'utilisation de ces hooks aide à réduire la quantité de code que nous écrivions dans chaque `test()`.

```js
test.beforeEach(async t => {
	t.context.driver = new Builder().forBrowser('chrome').build();
	await t.context.driver.get('https://www.google.com');
});

test.afterEach('cleanup', async t => {
	await t.context.driver.close();
});
```

Maintenant, ajoutons le code de test :

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

Vous êtes maintenant prêt à exécuter les tests. La sortie devrait ressembler à quelque chose comme ceci :

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
