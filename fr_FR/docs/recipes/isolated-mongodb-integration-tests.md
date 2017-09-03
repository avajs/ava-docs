___
**Note du traducteur**

C'est la traduction du fichier [isolated-mongodb-integration-tests.md](https://github.com/avajs/ava/blob/master/docs/recipes/isolated-mongodb-integration-tests.md). Voici un [lien](https://github.com/avajs/ava/compare/c9fe8dbf166d9d23e3575eb5780ccdffda984e90...master#diff-fa5b24a10aa6bb9abd440902e222e84c) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `isolated-mongodb-integration-tests.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
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

## Extra : configuration et utilisation dans Mongoose

[Mongoose](http://mongoosejs.com) est un robuste Object-Document-Mapper (ODM) pour MongoDB. Reportez-vous à sa documentation pour débuter avec Mongoose.

Pour utiliser efficacement Mongoose avec AVA, consultez les [documents d'intégration Mongoose](../endpoint-testing-with-mongoose.md).
