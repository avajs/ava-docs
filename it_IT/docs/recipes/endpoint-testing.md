___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/f2c070987ecee3caf7613190acf2c8a90700e058...master#diff-f2c070987ecee3caf7613190acf2c8a90700e058) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Test di endpoint

Traduzioni: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/endpoint-testing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/endpoint-testing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/endpoint-testing.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/endpoint-testing.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/endpoint-testing.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/recipes/endpoint-testing.md)

AVA non ha un metodo nativo per il test di endpoint, ma puoi utilizzare un'altra libreria si asserzione per questo. In questo esempio utilizzeremo [`supertest-as-promised`](https://github.com/WhoopInc/supertest-as-promised).

Poichè i test saranno eseguiti in concorrenza, è meglio creare una nuova instanza del server per ciascun file di test, poichè se utilizzassimo sempre la stessa istanza, questa potrebbe cambiare il suo stato tra un test e l'altro. Questo si può fare utilizzando `test.beforeEach` e `t.context`, o semplicemente con una funzione factory:

```js
function makeApp() {
	const app = express();
	app.post('/signup', signupHandler);
	return app;
}
```

Poi, iniettiamo l'istanza server in supertest. L'unica particolarità da tenere a mente è quella di usare una Promise o async/await invece del metodo `end` di supertest:

```js
test('signup:Success', async t => {
	t.plan(2);

	const res = await request(makeApp())
		.post('/signup')
		.send({email: 'ava@rocks.com', password: '123123'});

	t.is(res.status, 200);
	t.is(res.body.email, 'ava@rocks.com');
});
```
