___
**Note du traducteur**

C'est la traduction du fichier [endpoint-testing-with-mongoose.md](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing-with-mongoose.md). Voici un [lien](https://github.com/avajs/ava/compare/9c9dfefb548dc5b18b4d72b15ed18ef1e8084e9f...master#diff-0d4979fa38dcd3f583de1cad2529b9c4) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `endpoint-testing-with-mongoose.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Tester un endpoint avec Mongoose

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/endpoint-testing-with-mongoose.md)

Cette recette montre comment vous pouvez tester vos endpoints avec AVA et Mongoose, en supposant que vous utilisez Express comme framework.

## Installation

Cette recette utilise les bibliothèques suivantes :

1. [`mongod-memory-server`](https://github.com/nodkz/mongodb-memory-server) (Un serveur de MongoDB en mémoire)
2. [SuperTest](https://github.com/visionmedia/supertest) (Une bibliothèque de test pour endpoint)
3. [Mongoose](http://mongoosejs.com)

Veuillez d'abord installer les deux premières bibliothèques en exécutant le code suivant :

```console
$ npm install --save-dev mongodb-memory-server supertest
```

Vous devriez avoir Mongoose déjà installé. Sinon, exécutez le code suivant pour l'installer :

(Remarque : Vous avez au moins besoin de Mongoose v4.11.3)

```console
$ npm install mongoose
```

## Prérequis

Vous aurez besoin d'un fichier serveur et d'un modèle Mongoose. Consultez les examples [`server.js`](https://github.com/zellwk/ava-mdb-test/blob/master/server.js) et [`models/User.js`](https://github.com/zellwk/ava-mdb-test/blob/master/models/User.js).

Veuillez noter que `server.js` ne démarre pas l'application. Cela doit être fait par SuperTest, afin que les endpoints de l'application puissent être testés.  Si vous utilisez Express pour votre application, assurez-vous d'avoir un fichier de démarrage qui importe `app` et appelle `app.listen()`.

## Votre fichier de test

Incluez d'abord les bibliothèques dont vous avez besoin :

```js
// Bibliothèques utiles pour les tests
import test from 'ava'
import request from 'supertest'
import MongodbMemoryServer from 'mongodb-memory-server'
import mongoose from 'mongoose'

// Votre serveur et vos modèles
import app from '../server'
import User from '../models/User'
```

Ensuite lancez l'instance MongoDB en mémoire et connectez-vous à Mongoose :

```js
// Démarre l'instance MongoDB
const mongod = new MongodbMemoryServer()

// Créer la connexion à Mongoose avant d'exécuter les tests
test.before(async () => {
	const uri = await mongod.getConnectionString();
	await mongoose.connect(uri, {useMongoClient: true});
});
```

Lorsque vous exécutez votre premier test, MongoDB télécharge les derniers binaires MongoDB. Le téléchargement est de ~70 MB donc cela peut prendre une minute.

Vous devrez remplir votre base de données avec des données factices. Voici un exemple :

```js
test.beforeEach(async () => {
	const user = new User({
		email: 'one@example.com',
		name: 'One'
	});
	await user.save();
});
```

Les données fictives doivent être effacées après chaque test :

```js
test.afterEach.always(() => User.remove());
```

Maintenant, vous pouvez utiliser SuperTest pour envoyer une requête pour le endpoint de votre application. Utilisez AVA pour vos assertions :

```js
// Remarquez que les tests sont exécutés en séerie. Veuillez regarder plus bas pourquoi c'est fait ainsi.

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

	// Verifie que le user est crée dans la BD
	const newUser = await User.findOne({email: 'new@example.com'});
	t.is(newUser.name, 'New name');
});
```

Enfin, déconnectez et arrêtez MongoDB quand tous les tests sont faits :

```js
test.after.always(async () => {
	mongoose.disconnect()
	mongod.stop()
})

```

Et vous avez fini !

## Réutilisation de la configuration à travers des fichier

Vous pouvez choisir d'extraire le code des hooks `test.before`, `test.beforeEach`, `test.afterEach.always` et `test.after.always` dans un fichier séparé. Jetez un œil à https://github.com/zellwk/ava-mdb-test pour un exemple.

## Utilisation de `test.serial` plutôt que `test`

Vos tests changent probablement la base de données. L'utilisation de `test ()` signifie l'exécution simultanée, donc un test peut en affecter un autre. Au lieu de cela, si vous utilisez `test.serial ()`, les tests seront exécutés les uns après les autres. Vous pouvez aussi nettoyer votre base de données entre les tests, ce qui rend les tests plus prévisibles.

Vous pouvez exécuter des tests simultanément, si vous créez des connexions Mongoose distinctes pour chaque test. C'est toutefois plus difficile à configurer. Plus d'informations peuvent être trouvées [ici]	(https://github.com/nodkz/mongodb-memory-server#several-mongoose-connections-simultaneously).
