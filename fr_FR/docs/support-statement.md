___
**Note du traducteur**

C'est la traduction du fichier [support-statement.md](https://github.com/avajs/ava/blob/master/docs/support-statement.md). Voici un [lien](https://github.com/avajs/ava/compare/47b8b5657177197fac570c72a606cc3b754e0ff6...master#diff-37d8e3b2a468fc951f0ab91473264dd1) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `support-statement.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Les versions Node.js supportées

Traductions : [English](https://github.com/avajs/ava/blob/master/docs/support-statement.md)

AVA prend en charge la dernière version de toute version majeure [prise en charge par Node.js](https://github.com/nodejs/Release#release-schedule).

Le *Support* signifie que nous exécutons notre suite de tests sous les versions de Node.js spécifiées et acceptons les pull requests pour corriger les bogues. (à condition que les bogues ne soient pas connus dans Node.js car ils seront fixés de manière imminente). Par conséquent, *l'abandon du support* signifie que nous supprimerons ces versions de Node.js depuis notre matrice de test et nous n'accepterons plus les pull requests spécifiques pour corriger les bogues sur ces versions.

Lorsque nous abandonnerons la prise en charge d'une version majeure couverte par LTS, nous augmenterons la version majeure d'AVA (ou, alors tant que nous sommes dans la phase `0.`, le numéro de version mineure).

Nous abandonnerons le support des versions Node.js impaires (par exemple `7` ou` 9`) *sans augmenter* le numéro de version majeur de AVA.

Nous essayons d'éviter la suppression *accidentelle* du support des versions de Node.js qui ne sont pas les dernières. Si une telle rupture se produit, nous acceptons les pull requests pour restaurer les fonctionnalités. Nous pourrions décider de désapprouver la version AVA incriminée et augmenter à la place le numéro de version majeur de AVA.

Nous pouvons explicitement abandonner la prise en charge des versions non récentes de Node.js. Si cela se produit, nous augmenterons la version majeure de AVA. Cela peut être dû à l'adoption d'API rétroportées ou la disponibilité de nouvelles versions V8 dans les versions ultérieures de Node.js, soit dans AVA lui-même ou l'une de nos dépendances.
