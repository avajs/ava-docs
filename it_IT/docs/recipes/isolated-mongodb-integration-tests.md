___
**Nota del traduttore**

Questa è la traduzione del file [isolated-mongodb-integration-tests.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/isolated-mongodb-integration-tests.md). Qui c'è il [link](https://github.com/avajs/ava/compare/0069a7ef4797a88149031124e7eade090a18ad4a...master#diff-fa5b24a10aa6bb9abd440902e222e84c) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `isolated-mongodb-integration-tests.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Test di integrazione per MongoDB in isolamento

Translations: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/isolated-mongodb-integration-tests.md)
[Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/isolated-mongodb-integration-tests.md)

> Configurazione di un database MongoDB per i tuoi test AVA con isolamento per singolo test file

Questa guida usa [`MongoMem`](https://github.com/CImrie/mongomem), una libreria che permette di eseguire velocemente un server MongoDB temporaneo localmente. Utilizza file temporanei su disco che vengono distrutti al termine dell'esecuzione del server.

## Installare il server MongoDB in-memory (MongoMem)

Nella cartella principale della tua app esegui:

```console
$ npm install --save-dev mongomem
```

## Come usare MongoMem

Nei tuoi file di test, importa il modulo ed esegui il server.

**Assicurati di eseguire il server all'inizio del tuo file, prima di tutti i test.**

```js
import test from 'ava';
import {MongoDBServer} from 'mongomem';

test.before('start server', async t => {
	await MongoDBServer.start();
});

test('some feature', async t => {
	const connectionString = await MongoDBServer.getConnectionString();

	// connectionString === 'mongodb://localhost:27017/3411fd12-b5d6-4860-854c-5bbdb011cb93'
	// Usa `connectionString` per connetterti al database con qualsiasi client. Vedi sotto per l'utilizzo di Mongoose.
});
```

## Ripulire l'ambiente

Dopo aver eseguito i test ricordati di includere il metodo `test.after.always()` per ripulire il server MongoDB. Questo rimuoverà tutti i file temporanei che il server ha creato durante l'esecuzione.

Questi file temporanei vengono solitamente rimossi dal sistema operativo, ma è comunque buona pratica rimuoverli manualmente.

```js
test.after.always('cleanup', t => {
	MongoDBServer.tearDown(); // Ripulisce i file temporanei generati
});
```

## Debugging

Se il server non dovesse partire, puoi impostare l'opzione `MongoDBServer.debug = true;` prima di chiamare `MongoDBServer.start()`. Questa opzione abiliterà la stampa di eventuali errori di connessione o permessi sui file duranete l'avvio. Solitamente in fase di avvio viene verificata la disponibilità di una porta prima di avviare la connessione quindi spesso gli errori sono relativi a permessi sui file più che alla connessione.

## Extra: Setup e uso di Mongoose (MongoDB ODM)

[Mongoose](http://mongoosejs.com) è un robusto Object-Document-Mapper (ODM) per MongoDB. Fai riferimento alla sua documentazione per vedere come configurare inizialmente Mongoose.

### Importare Mongoose

```js
// `myTestCase.test.js` - (Il tuo file di test)
import mongoose from 'mongoose';
```

`mongoose` in questo caso è una singola istanza di Mongoose ODM ed è disponibile globalmente. Questo va bene per mantenere un singolo punto d'accesso al tuo database, ma può essere problematico per il testing isolato.

Le istanze di Mongoose dovrebbero essere isolate tra i tuoi test, in questo modo l'ordine di esecuzione dei test non ha effetti sul risultato finale. Questo può essere fatto con un po' di lavoro.

### Isolare istanze Mongoose

Richiedere nuove istanze a Mongoose è molto facile. Come prima cosa chiama `new mongoose.Mongoose()` per ottenere una nuova istanza, poi esegui `connect` con una stringa di connessione del database ottenuta dal pacchetto `mongomem`.

**Dovrai copiare manualmente i modelli dalla tua istanza globali alle nuove istanze isolate.**

```js
import mongoose from 'mongoose';
import {MongoDBServer} from 'mongomem';

test.before('start server', async t => {
	await MongoDBServer.start();
});

test.beforeEach(async t => {
	const db = new mongoose.Mongoose();
	await db.connect(await MongoDBServer.getConnectionString());
	for (const name of mongoose.modelNames()) {
		db.model(name, mongoose.model(name).schema);
	}
	t.context.db = db;
});

test('my Mongoose model integration test', async t => {
	const {db} = t.context;
	// Addesso puoi usare un'istanza DB isolata nei tuoi test
});
```