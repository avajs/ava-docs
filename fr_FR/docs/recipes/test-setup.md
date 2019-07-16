___
**Note du traducteur**

C'est la traduction du fichier [test-setup.md](https://github.com/avajs/ava/blob/master/docs/recipes/es-modules.md). Voici un [lien](https://github.com/avajs/ava/compare/0948d87a79ac6f91afa5766a45dee212e511b6f9...master#diff-68fa5258df1d0b29e70b94f0d08b9f28) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `test-setup.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Configuration de test

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/recipes/test-setup.md)

Les tests peuvent être mis en place en utilisant le hook `beforeEach()`. Souvent, vous pouvez utiliser  à la place une simple fonction de configuration. Cette recette peut vous aider pour décider ce qui convient le mieux à votre cas d'utilisation.

# Le hook `beforeEach()` versus les fonctions de configuration

Le hook `beforeEach()` a quelques inconvénients. Par exemple, vous ne pouvez pas le désactiver pour des tests spécifiques, ni l'appliquer à des tests spécifiques. Comme alternative, vous pouvez utiliser de simples fonctions. Cela vous permet d'utiliser plusieurs fonctions de configuration pour différentes exigences de configuration et appelez différentes parties de configuration à partir de différents tests. Vous pouvez même avoir des fonctions de configuration avec des paramètres afin que les tests puissent personnaliser leur propre configuration.

Disons que vous avez une fonction qui interagit avec le système de fichiers. Peut-être que vous exécutez quelques tests qui utilisent `mock-fs`, et puis d'autres qui utilisent le vrai système de fichiers et un répertoire temporaire. Ou vous avez une fonction de configuration que vous exécutez avec des données valides pour certains tests et des données invalides pour d'autres tests, tous dans le même fichier de test.

Vous pouvez faire toutes ces choses en utilisant des fonctions de configuration simples, mais il y a des compromis :

|`beforeEach()`| fonctions de configuration
|---|---
| ⛔️ &nbsp; utilisé pour tous les tests| ✅ &nbsp; peut changer ou passer selon le test
| ⛔️ &nbsp; plus compliqué pour les débutants, "un peu magique"| ✅ &nbsp; plus facile pour les débutants, "aucune magie"
| ✅ &nbsp; prend en charge le mode callback, prise en charge intégré pour les observables| ⛔️ &nbsp; doit utiliser des promesses pour le comportement asynchrone
| ✅ &nbsp; l'échec a une sortie sympathique| ⛔️ &nbsp; les erreurs sont attribuées au test
| ✅ &nbsp; `afterEach` et `afterEach.always` utiles pour le nettoyage| ⛔️ &nbsp; ne peut pas facilement nettoyer

## Configuration de test complexe

Dans cet exemple, nous avons à la fois un hook `beforeEach()` et aussi des modifications dans chaque test.

```js
test.beforeEach(t => {
	setupConditionA(t);
	setupConditionB(t);
	setupConditionC(t);
});

test('premier scénario', t => {
	tweakSomething(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

test('second scénario', t => {
	tweakSomethingElse(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

Si trop de variables doivent être modifiées pour chaque test, envisagez d'omettre le hook `beforeEach()` et effectuer des étapes de configuration dans les tests eux-mêmes.

```js
test('premier scénario', t => {
	setupConditionA(t);
	setupConditionB(t, {/* options */});
	setupConditionC(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});

// Dans ce test, setupConditionB() n'est jamais appelé.
test('second scénario', t => {
	setupConditionA(t);
	setupConditionC(t);
	const someOtherCondition = t.context.thingUnderTest();
	t.true(someOtherCondition);
});
```

## Un exemple pratique

```js
test.beforeEach(t => {
	t.context = {
		authenticator: new Authenticator(),
		credentials: new Credentials('admin', 's3cr3t')
	};
});

test('authentification avec des informations d\'identification valides', async t => {
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.true(await isValid);
});

test('authentification avec un nom d\'utilisateur invalide', async t => {
	t.context.credentials.username = 'bad_username';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});

test('authentification avec un mot de passe invalide', async t => {
	t.context.credentials.password = 'bad_password';
	const isValid = t.context.authenticator.authenticate(t.context.credentials);
	t.false(await isValid);
});
```

Les mêmes tests, en utilisant maintenant les fonctions de configuration, ressembleraient à ceci.

```js
function setup({username = 'admin', password = 's3cr3t'} = {}) {
	return {
		authenticator: new Authenticator(),
		credentials: new Credentials(username, password)
	};
}

test('authentification avec des informations d\'identification valides', async t => {
	const {authenticator, credentials} = setup();
	const isValid = authenticator.authenticate(credentials);
	t.true(await isValid);
});

test('authentification avec un nom d\'utilisateur invalide', async t => {
	const {authenticator, credentials} = setup({username: 'bad_username'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});

test('authentification avec un mot de passe invalide', async t => {
	const {authenticator, credentials} = setup({password: 'bad_password'});
	const isValid = authenticator.authenticate(credentials);
	t.false(await isValid);
});
```

## Combinaison de hooks et de fonctions de configuration

Bien sûr, `beforeEach()` et les fonctions simples de configuration peuvent être utilisés ensemble :

```js
test.beforeEach(t => {
	t.context = setupAllTests();
});

test('premier scénario', t => {
	firstSetup(t);
	const someCondition = t.context.thingUnderTest();
	t.true(someCondition);
});
```
