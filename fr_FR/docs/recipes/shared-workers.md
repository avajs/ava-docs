___
**Note du traducteur**

C'est la traduction du fichier [shared-workers.md](https://github.com/avajs/ava/blob/main/docs/recipes/shared-workers.md). Voici un [lien](https://github.com/avajs/ava/compare/f5caa8fb823e369517053181494374c64dfa58b2...main#diff-cb7b934b320f5b19f28a3fc02b2231622da7be34bb939995071378a000668f46) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `shared-workers.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Extension de AVA en utilisant des workers partagés

Traductions : [English](https://github.com/avajs/ava/blob/main/docs/recipes/shared-workers.md)

Les workers partagés sont une fonctionnalité puissante d'AVA. Un programme peut être chargé dans une [tâche de worker](https://nodejs.org/docs/latest/api/worker_threads.html) dans le processus principal d'AVA, puis communiquer avec le code exécuté dans les workers de test. Cela permet à vos tests de mieux utiliser les ressources partagées pendant l'exécution d'un test, tout en offrant la possibilité de configurer ces ressources avant le début des tests (ou de les nettoyer après).

Lorsque vous utilisez le mode watch, les workers partagés restent chargés d'une exécution à l'autre.

## Plugins disponibles

* [`@ava/get-port`](https://github.com/avajs/get-port) fonctionne comme [`get-port`](https://github.com/sindresorhus/get-port), mais garantit que le port est verrouillé sur tous les fichiers de test.
* [`@ava/cooperate`](https://github.com/avajs/cooperate) implémente la fonctionnalité de verrouillage et de réservation de valeur.

## Ecriture de plugins

Vous pouvez écrire vos propres plugins. Ils peuvent être de haut niveau, comme [`@ava/get-port`](https://github.com/avajs/get-port) qui est construit en utilisant [`@ava/cooperate`](https://github.com/avajs/cooperate), ou de bas niveau comme [`@ava/cooperate`](https://github.com/avajs/cooperate) lui même.

Ici, nous discuterons de la construction de plugins de bas niveau.

### Enregistrement d'un worker partagé

Les plugins sont enregistrés à l'intérieur des workers de test. Ils fournissent le chemin pour le worker partagé, que AVA chargera dans un [thread de worker](https://nodejs.org/docs/latest/api/worker_threads.html) dans son processus principal. Pour chaque chemin unique, un thread de worker est démarré.

Les plugins communiquent avec leur worker partagé en utilisant un *protocole*. Les protocoles sont versionnés indépendamment de celle d'AVA. Cela nous permet d'apporter des améliorations sans casser les plugins existants. Les protocoles ne sont supprimés que dans les versions majeures d'AVA.

Les plugins peuvent être compatibles avec plusieurs protocoles. AVA sélectionnera le meilleur protocole qu'il prend en charge.  Si AVA ne supporte aucun des protocoles spécifiés, il lancera une erreur. Le protocole sélectionné est disponible sur l'objet worker retourné.

```js
import {registerSharedWorker} from 'ava/plugin';

const shared = registerSharedWorker({
  filename: path.resolve(__dirname, 'worker.js'),
  supportedProtocols: ['ava-4']
});
```

Dans un processus de test, vous ne pouvez enregistrer qu'un seul worker pour chaque `filename`. Les noms de fichiers sont comparés tels quels, sans normalisation. Si vous appelez `registerSharedWorker()` une seconde fois, la même instance de worker est retournée.

Si pour une raison quelconque vous voulez charger le même fichier avec plusieurs workers différents, vous pouvez ajouter un hash unique à la fin du nom du fichier :

```js
import crypto from 'crypto';
import {registerSharedWorker} from 'ava/plugin';

const key = Math.random() > 0.5 ? 'worker-a' : 'worker-b';

const shared = registerSharedWorker<any>({
  filename: new URL(
    `file:${path.resolve(
      __dirname,
      'worker.js'
    )}#${encodeURIComponent(key)}`
  ),
  initialData: {workerKey: key},
  supportedProtocols: ['ava-4']
});
```

Cela fonctionne parce que le paramètre `filename` accepte les objets [URL](https://nodejs.org/api/url.html), ce qui signifie que vous pourriez utiliser un composant de requête pour la clé si vous le souhaitez.

Vous pouvez fournir une fonction `teardown()` qui sera appelée après la fin de tous les tests. Si vous appelez `registerSharedWorker()` plusieurs fois, la fonction `teardown()` sera appelée pour chaque enregistrement, même si vous n'avez qu'une seule instance de worker. La fonction `teardown()` la plus récemment enregistrée est appelée en premier, et ainsi de suite. Les fonctions `teardown()` s'exécutent de manière séquentielle.

```js
const worker = registerSharedWorker({
  filename: path.resolve(__dirname, 'worker.js'),
  supportedProtocols: ['ava-4'],
  teardown () {
    // Effectue un nettoyage éventuel dans le cadre du processus de test lui-même.
  }
});
```

Vous pouvez également fournir certaines données transmises au worker partagé lorsqu'il est chargé. Bien sûr, il n'est chargé qu'une seule fois, donc cela n'est utile que dans des situations limitées :

```js
const shared = registerSharedWorker({
  filename: path.resolve(__dirname, 'worker.js'),
  initialData: {hello: 'world'},
  supportedProtocols: ['ava-4']
});
```

Sur cet objet `shared`, `protocol` est défini par le protocole sélectionné. Puisque le worker partagé est chargé de manière asynchrone, `available` fournit une promesse qui se réalisera lorsque le worker partagé sera disponible pour la première fois. `currentlyAvailable` indique si le worker est, disons, actuellement disponible.

Il y a deux autres méthodes disponibles sur l'objet `shared`, que nous aborderons bientôt.

#### Initialisation du worker partagé

AVA charge le worker partagé (tel qu'identifié par l'option `filename`) dans un thread de worker. Cela doit être un fichier module ES avec une exportation par défaut. Le `filename` doit être un chemin absolu utilisant le protocole `file:` ou une instance `URL`.

L'exportation par défaut doit être une méthode de factory. Comme lors de l'appel à `registerSharedWorker()`, elle doit gérer un protocole :

```js
export default ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']);
}
```

Sur cet objet `main`, `protocol` est défini par le protocole sélectionné. `initialData` contient les données fournies lorsque le worker a été enregistré pour la première fois.

Lorsque vous avez fini d'initialiser le worker partagé, vous devez appeler `main.ready()`. Cela rend le worker disponible dans les workers de test. Vous pouvez appeler `main.ready()` de manière asynchrone.

Toute erreur lancée par la méthode factory fera planter le thread du worker et rendra le worker indisponible dans les test workers. Il en va de même pour les rejets non gérés. La méthode factory peut retourner une promesse.

### Communication entre les workers de test et le worker partagé

L'infrastructure de bas niveau du worker partagé d'AVA est principalement axée sur la communication. Vous pouvez envoyer des messages des workers de test au worker partagé, et vice-versa. Une logique de plus haut niveau peut être implémentée au dessus de cette infrastructure de passage de messages.

Les données des messages sont sérialisées à l'aide de l'[API de sérialisation V8](https://nodejs.org/docs/latest-v12.x/api/v8.html#v8_serialization_api). Veuillez prendre connaissance de certaines [limitations importantes](https://nodejs.org/docs/latest-v12.x/api/worker_threads.html#worker_threads_port_postmessage_value_transferlist).

Dans le worker partagé, vous pouvez vous abonner aux messages des workers de test :

```js
export default async ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  for await (const message of main.subscribe()) {
    // …
  }
}
```

Les messages ont des ID qui sont uniques pour le processus AVA principal. A travers les différentes exécutions d'AVA, vous pouvez voir le même ID. Accédez à l'ID en utilisant la propriété `id`.

Accédez aux données du message en utilisant la propriété `data`.

Vous pouvez répondre à un message reçu en appelant `reply()`. Cela publie un message dans le processus de test d'où provient le message. Vous pouvez ensuite vous abonner aux réponses à *ce* message en utilisant `replies()`.

Pour illustrer ceci, voici un « jeu » de Marco Polo :

```js
export default ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  play(main.subscribe());
};

const play = async (messages) => {
  for await (const message of messages) {
    if (message.data === 'Marco') {
      const response = message.reply('Polo');
      play(response.replies());
    }
  }
}
```

(Bien sûr, cela met en place de nombreux écouteurs de réponse, ce qui est plutôt inefficace).

Vous pouvez également diffuser des messages à tous les workers de test connectés :

```js
export default async ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  for await (const message of main.subscribe()) {
  	if (message.data === 'Bingo!') {
      main.broadcast('Bingo!');
    }
  }
}
```

Comme avec `reply()`, `broadcast()` renvoie un message publié qui peut recevoir des réponses. Appelez `replies()` pour obtenir un itérateur asynchrone pour les messages de réponse.

Chaque message reçu a une propriété `testWorker` pour représenter le travailleur de test d'où provient le message. Tous les messages provenant du même worker de test ont la même valeur pour cette propriété.

Ces workers de test ont un ID unique (qui, comme les ID des messages, est unique pour le processus principal). Vous pouvez y accéder en utilisant la propriété `id`. Le chemin du fichier de test est disponible via la propriété `file`. Utilisez `publish()` pour envoyer des messages directement au test worker, et `subscribe()` pour recevoir des messages du test worker. Cela fonctionne de la même manière que `main.subscribe()`.

Bien sûr, vous n'avez pas besoin d'attendre un message *du* worker de test pour accéder à cet objet. Utilisez `main.testWorkers()` pour obtenir un itérateur asynchrone qui fournit chaque worker de test nouvellement connecté :

```js
export default async ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  for await (const testWorker of main.testWorkers()) {
    main.broadcast(`New test file: ${testWorker.file}`);
  }
}
```

Au sein des workers de test, une fois que le worker partagé est disponible, vous pouvez publier des messages :

```js
shared.publish('Marco');
```

Ou s'abonner aux messages :

```js
(async () => {
  for await (const message of shared.subscribe()) {
    if (message.data === 'Polo') {
      message.reply('Marco');
    }
  }
})();
```

`publish()` lève une exception lorsque le travailleur partagé n'est pas encore disponible. `publish()` et `subscribe()` lèvent une exception lorsque le worker s'est planté.

Les ID des messages sont disponibles et sont uniques pour le processus principal d'AVA.

Les messages sont toujours produits dans leur propre tour de la boucle d'événement. Cela signifie que vous pouvez utiliser `async`/`await` pour traiter un message précédent ou vous abonner à des réponses et vous aurez la garantie de les recevoir.

### Nettoyage des ressources

Les workers de test entrent et sortent tandis que le worker partagé reste. Il est donc important de nettoyer les ressources.

Les messages sont souscrits à l'aide d'itérateurs asynchrones. Ceux-ci sont retournés lorsque le worker de test sort.

Vous pouvez enregistrer des fonctions de nettoyage qui seront exécutées lorsque le worker de test sort :

```js
export default async ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  for await (const testWorker of main.testWorkers()) {
    testWorker.teardown(() => {
      // Au revoir…
    });
  }
}
```

La fonction la plus récemment enregistrée est appelée en premier, et ainsi de suite. Les fonctions s'exécutent de manière séquentielle.

Plus intéressant encore, une fonction de nettoyage enveloppée est retournée pour que vous puissiez l'appeler manuellement. AVA s'assure toujours que la fonction ne s'exécute qu'une seule fois.

```js
export default ({negotiateProtocol}) => {
  const main = negotiateProtocol(['ava-4']).ready();

  for await (const worker of testWorkers) {
    counters.set(worker, 0);
    const teardown = worker.teardown(() => {
      counters.delete(worker);
    });

    waitForTen(worker.subscribe(), teardown);
  }
}

const counters = new WeakMap();

const waitForTen = async (messages, teardown) => {
  for await (const {testWorker} of messages) {
    const count = counters.get(testWorker) + 1;
    if (count === 10) {
      teardown();
    } else {
      counters.set(testWorker, count);
    }
  }
};
```

## Maintenant c'est votre tour

Nous sommes très enthousiastes à propos de cette fonctionnalité ! Mais nous avons besoin de plus d'expérience réelle dans la construction de plugins AVA avant de pouvoir la rendre disponible de manière générale. Merci de nous faire part de vos commentaires et de créer des plugins. Nous serions plus qu'heureux de les promouvoir.

Vous ne savez pas quoi construire ? Auparavant, les gens ont exprimé le désir d'avoir des mutex, de gérer des instances de Puppeteer, de démarrer des serveurs (de base de données), etc.

Nous pourrions également étendre l'implémentation du worker partagé dans AVA lui-même. Peut-être pour que vous puissiez exécuter du code avant un nouveau test, même en mode watch. Ou pour que vous puissiez initialiser un worker partagé basé sur la configuration d'AVA, et non pas lorsqu'un fichier de test s'exécute.

Veuillez [commenter ici](https://github.com/avajs/ava/discussions/2703) avec vos idées, questions et commentaires.
