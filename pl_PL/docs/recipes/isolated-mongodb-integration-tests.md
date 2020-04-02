# Izolowane testy integracyjne MongoDB

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/isolated-mongodb-integration-tests.md)

> Jak uruchamiać jednorazowe bazy danych MongoDB w testach AVA z izolacją na test.

To używa [`MongoMem`](https://github.com/CImrie/mongomem), co pozwala szybko uruchomić lokalnie tymczasowy serwer MongoDB. Wykorzystuje tymczasowe miejsce do przechowywania plików, które jest niszczone po zatrzymaniu serwera.


## Zainstaluj MongoDB in-memory Server (MongoMem)

W katalogu głównym aplikacji uruchom:

```console
$ npm install --save-dev mongomem
```


## Używanie MongoMem

W pliku testowym zaimportuj moduł i uruchom serwer.

**Pamiętaj, aby uruchomić serwer na początku pliku, poza przypadkami testowymi.**

```js
const test = require('ava');
const {MongoDBServer} = require('mongomem');

test.before('start server', async t => {
	await MongoDBServer.start();
})

test('some feature', async t => {
	const connectionString = await MongoDBServer.getConnectionString();

	// connectionString === 'mongodb://localhost:27017/3411fd12-b5d6-4860-854c-5bbdb011cb93'
	// Use `connectionString` to connect to the database with a client of your choice. See below for usage with Mongoose.
});
```


## Sprzątanie

Po uruchomieniu testów należy dołączyć metodę `test.after.always()` aby wyczyścić serwer MongoDB. Spowoduje to usunięcie plików tymczasowych używanych przez serwer podczas działania.

Jest to zwykle usuwane przez system operacyjny, ale dobrą praktyką jest robienie tego ręcznie.

```js
test.after.always('cleanup', t => {
	MongoDBServer.tearDown(); // Cleans up temporary file storage
});
```


## Debugowanie

Jeśli serwer się nie uruchamia, możesz ustawić opcję `MongoDBServer.debug = true;` zanim wywołasz `MongoDBServer.start()`. Umożliwi to serwerowi MongoDB drukowanie błędów połączenia lub uprawnień do plików podczas uruchamiania. Sprawdza i wybiera dostępny port do uruchomienia serwera, więc błędy mogą być związane z uprawnieniami do plików.

## Dodatkowo: Konfiguracja i użycie w Mongoose

[Mongoose](http://mongoosejs.com) to solidny Object-Document-Mapper (ODM) dla MongoDB. Zapoznaj się z jego dokumentacją, aby rozpocząć korzystanie z Mongoose.

Aby skutecznie używać Mongoose z AVA, sprawdź [Dokumentacja integracji Mongoose](endpoint-testing-with-mongoose.md).
