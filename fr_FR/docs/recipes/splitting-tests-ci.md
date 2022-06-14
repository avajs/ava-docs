___
**Note du traducteur**

C'est la traduction du fichier [splitting-tests-ci.md](https://github.com/avajs/ava/blob/main/docs/recipes/splitting-tests-ci.md). Voici un [lien](https://github.com/avajs/ava/compare/c299fdb425c7078a098e26d5fbe95b09d7320343...main#diff-e73642919435e90af62b7d834d4596d2552c29f2f783035c2b43556ad87bf454) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `splitting-tests-ci.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Découpage des tests dans le CI

AVA détecte automatiquement si votre environnement CI prend en charge les constructions en parallèle en utilisant [ci-parallel-vars](https://www.npmjs.com/package/ci-parallel-vars). Lorsque la prise en charge des constructions en parallèle est détectée, AVA trie tous les fichiers de test détectés par nom, et les divise en morceaux. Chaque machine CI est assignée à un morceau (sous-ensemble) des tests, et ensuite chaque morceau est exécuté en parallèle.

Pour désactiver cette fonctionnalité, mettez `utilizeParallelBuilds` à `false` dans votre [configuration AVA](/docs/06-configuration.md#options).

Pour mieux répartir les tests sur les machines, vous pouvez configurer une fonction de comparaison personnalisée :

**`ava.config.js` :**

```js
import fs from 'node:fs';

// En supposant que 'test-data.json' a la structure suivante :
// {
// 	'tests/test1.js': { order: 1 },
// 	'tests/test2.js': { order: 0 }
// }
const testData = JSON.parse(fs.readFileSync('test-data.json', 'utf8'));

export default {
	sortTestFiles: (file1, file2) => testData[file1].order - testData[file2].order,
};
```

## Découpage des tests sur GitHub Actions

Bien que GitHub Actions ne prenne pas en charge les constructions en parallèle avec AVA, vous pouvez le configurer manuellement en utilisant une matrice :

**`.github/workflows/test.yml`:**

```yml
on: push
jobs:
  test:
    strategy:
      # N'annule pas les tests si cela échoue
      fail-fast: false
      # Exécute 4 jobs en parallèle, chacun exécutant un sous-ensemble de la totalité des tests
      matrix:
        node_index: [0, 1, 2, 3]
        total_nodes: [4]

    runs-on: ubuntu-latest
    steps:
      # Consulte le code et effectue des étapes de configuration
      # ...

      - name: Test
        run: npx ava
        env:
          CI_NODE_INDEX: ${{ matrix.node_index }}
          CI_NODE_TOTAL: ${{ matrix.total_nodes }}
```
