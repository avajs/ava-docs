___
**Note du traducteur**

C'est la traduction du fichier [isolated-mongodb-integration-tests.md](https://github.com/avajs/ava/blob/master/docs/recipes/isolated-mongodb-integration-tests.md). Voici un [lien](https://github.com/avajs/ava/compare/0b4707181cf03dae1934d35c77102ba8e2618a03...master#diff-fa5b24a10aa6bb9abd440902e222e84c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `isolated-mongodb-integration-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration d'AVA pour les tests d'intégration isolés de MongoDB

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/isolated-mongodb-integration-tests.md)

Comment exécuter les bases de données MongoDB à usage unique dans vos tests AVA avec une isolation pour chaque test. Pour cela, on utilise `mongomem` qui est disponible sur [npm](https://www.npmjs.com/package/mongomem).

Pour cela, on utilise [`MongoMem`](https://github.com/CImrie/mongomem) qui vous permet de gérer rapidement un serveur temporaire MongoDB localement. Il utilise temporairement un stockage de fichiers qui sera détruit lorsque le serveur s'arrêtera.


## Installez un serveur MongoDB en mémoire (MongoMem)

A la racine de votre application, exécutez :

```console
$ npm install --save-dev mongomem
```


## Utilisation de MongoMem

Dans votre fichier de test, importez le module et exécuter le serveur.
 
**Assurez-vous d'exécuter le serveur au début de votre fichier et en dehors des cas de test.**

```js
import test from 'ava';
import {MongoDBServer} from 'mongomem';

test.before('start server', async t => {
	await MongoDBServer.start();
})

test('some feature', async t => {
	const connectionString = await MongoDBServer.getConnectionString();

	// connectionString === 'mongodb://localhost:27017/3411fd12-b5d6-4860-854c-5bbdb011cb93'
	// utilisez `connectionString` pour se connecter à la base de données avec un client de votre choix. Regardez ci-dessous pour une utilisation avec Mongoose.
});
```


## Nettoyage

Après avoir exécuté vos tests, vous devez inclure une méthode `test.after.always` pour nettoyer le serveur MongoDB. Cela supprimera tous les fichiers temporaires utilisés par le serveur lors de l'exécution.

Ceci est normalement nettoyé par votre système d'exploitation, mais il est recommandé de le faire manuellement.

```js
test.after.always('cleanup', t => {
	MongoDBServer.tearDown(); // Nettoie les fichiers temporaires de stockage
});
```


## Débogage

Si le serveur ne semble pas démarrer, vous pouvez définir l'option `MongoDBServer.debug = true;` avant d'appeler `MongoDBServer.start()`. Cela permettra au serveur MongoDB d'afficher des erreurs de connexion ou d'autorisation de fichier lors de son démarrage. Comme il vérifie et choisit un port disponible pour exécuter le serveur, les erreurs sont donc susceptibles d'être liées aux autorisations de fichier.


## Extra : configuration et utilisation dans Mongoose (MongoDB ODM)

[Mongoose](http://mongoosejs.com) est un robuste Object-Document-Mapper (ODM) pour MongoDB. Reportez-vous à sa documentation pour débuter avec Mongoose.

### Importez Mongoose

```javascript
// myTestCase.test.js - (Votre fichier de test !)
import mongoose from 'mongoose';
```

`mongoose` dans ce cas est une instance unique de l'ODM de Mongoose et elle est disponible globalement. C'est bien pour votre application, car elle maintient un point d'accès unique à votre base de données, mais ça l'ait moins pour les tests isolés.

Vous devez isoler les instances de Mongoose entre vos tests, de sorte que l'ordre d'exécution d'un test n'en soit jamais dépendant. Cela peut être fait avec un petit peu du travail.

### Isolez une instance Mongoose

Vous pouvez facilement demander une nouvelle instance de Mongoose. D'abord, appelez `new mongoose.Mongoose()` pour obtenir la nouvelle instance et ensuite appelez `connect` avec la chaîne de connexion de la base de données fourni par le package `mongomem`.

**Vous devrez copier manuellement des modèles de l'instance globale vers votre nouvelle instance.**

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

test('test d\'intégration de mon modèle de Mongoose', async t => {
	const {db} = t.context;
	// Maintenant utilisez l'instance de BD isolée dans votre test
});
```
