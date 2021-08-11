# Testowanie endpoint z Mongoose

Tłumaczenie: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/recipes/endpoint-testing-with-mongoose.md)

Ten przepis pokazuje, jak przetestować punkty końcowe za pomocą AVA i Mongoose, zakładając, że używasz Express jako swojej struktury.

## Ustawienie

Ten przepis wykorzystuje następujące biblioteki:

1. [`mongodb-memory-server`](https://github.com/nodkz/mongodb-memory-server) (A MongoDB in-memory Server)
2. [SuperTest](https://github.com/visionmedia/supertest) (An endpoint testing library)
3. [Mongoose](http://mongoosejs.com)

Zainstaluj dwie pierwsze biblioteki, uruchamiając następujący kod:

```console
$ npm install --save-dev mongodb-memory-server supertest
```

Powinieneś już mieć zainstalowaną Mongoose. Jeśli nie, uruchom następujący kod, aby go zainstalować:

(Uwaga: Potrzebujesz przynajmniej Mongoose v4.11.3)

```console
$ npm install mongoose
```

## Wymagania wstępne

Potrzebujesz pliku serwera i modelu Mongoose. Zobacz przykłady [`server.js`](https://github.com/zellwk/ava-mdb-test/blob/master/server.js) i [`models/User.js`](https://github.com/zellwk/ava-mdb-test/blob/master/models/User.js).

Zauważ że `server.js` nie uruchamia aplikacji. Zamiast tego musi to zrobić SuperTest, aby punkty końcowe aplikacji mogły zostać przetestowane. Jeśli używasz Express dla swojej aplikacji, upewnij się, że masz plik startowy, który się importuje `app` i wywołuje `app.listen()`.

## Twój plik testu

Najpierw dołącz potrzebne biblioteki:

```js
// Libraries required for testing
const test = require('ava');
const request = require('supertest');
const {MongoMemoryServer} = require('mongodb-memory-server');
const mongoose = require('mongoose');

// Your server and models
const app = require('../server');
const User = require('../models/User');
```

Następnie uruchom instancję in-memory MongoDB i połącz się z Mongoose:

```js
// Start MongoDB instance
const mongod = new MongoMemoryServer()

// Create connection to Mongoose before tests are run
test.before(async () => {
	const uri = await mongod.getConnectionString();
	await mongoose.connect(uri, {useMongoClient: true});
});
```

Po uruchomieniu pierwszego testu MongoDB pobiera najnowsze pliki binarne MongoDB. Pobieranie jest ~70MB więc może to chwilę potrwać.

Będziesz chciał zapełnić bazę danych danymi zastępczymi. Oto przykład:

```js
test.beforeEach(async () => {
	const user = new User({
		email: 'one@example.com',
		name: 'One'
	});
	await user.save();
});
```

Dane fikcyjne powinny być usuwane po każdym teście:

```js
test.afterEach.always(() => User.remove());
```

Teraz możesz użyć SuperTestu, aby wysłać żądanie dotyczące punktu końcowego aplikacji. Użyj AVA dla swoich asercji:

```js
// Note that the tests are run serially. See below as to why.

test.serial('litmus get user', async t => {
	const {app} = t.context;
	const res = await request(app)
		.get('/litmus')
		.send({email: 'one@example.com'});
	t.is(res.status, 200);
	t.is(res.body.name, 'One');
});

test.serial('litmus create user', async t => {
	const {app} = t.context;
	const res = await request(app)
		.post('/litmus')
		.send({
			email: 'new@example.com',
			name: 'New name'
		});

	t.is(res.status, 200);
	t.is(res.body.name, 'New name');

	// Verify that user is created in DB
	const newUser = await User.findOne({email: 'new@example.com'});
	t.is(newUser.name, 'New name');
});
```

Na koniec odłącz i zatrzymaj MongoDB po zakończeniu wszystkich testów:

```js
test.after.always(async () => {
	mongoose.disconnect()
	mongod.stop()
})

```

I skończyłeś!

## Ponowne użycie konfiguracji w plikach

Możesz wyodrębnić kod dla hook'ów `test.before`, `test.beforeEach`, `test.afterEach.always` i `test.after.always` do osobnego pliku. Spójrz na https://github.com/zellwk/ava-mdb-test po przykład.

## Używanie `test.serial` zamiast `test`

Twoje testy prawdopodobnie zmienią bazę danych. Używając `test()` oznacza, że działają równolegle, co może spowodować, że jeden test wpłynie na inny. Zamiast tego, jeśli używasz `test.serial()` wtedy testy będą uruchamiane pojedynczo. Następnie możesz wyczyścić bazę danych między uruchomieniami testowymi, dzięki czemu testy są bardziej przewidywalne.

Możesz uruchomić testy jednocześnie, jeśli utworzysz osobne połączenia Mongoose dla każdego testu. Jest to jednak trudniejsze do skonfigurowania. Więcej informacji można znaleźć [tutaj](https://github.com/nodkz/mongodb-memory-server#several-mongoose-connections-simultaneously).
