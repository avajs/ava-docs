___
**Note du traducteur**

C'est la traduction du fichier [support-statement.md](https://github.com/avajs/ava/blob/main/docs/support-statement.md). Voici un [lien](https://github.com/avajs/ava/compare/ed32b8138c4e8e23bbe75ce7c945a4b5afffe1eb...main#diff-37d8e3b2a468fc951f0ab91473264dd1) vers les différences avec le master de AVA (Si en cliquant sur le lien, vous ne trouvez pas le fichier `support-statement.md` parmi les fichiers modifiés, vous pouvez donc en déduire que la traduction est à jour).
___
# Les versions Node.js supportées

Traductions : [English](https://github.com/avajs/ava/raw/main/docs/support-statement.md)

AVA prend en charge la dernière version de toute version majeure [prise en charge par Node.js](https://github.com/nodejs/Release#release-schedule).

Le *Support* signifie que nous exécutons notre suite de tests sous les versions de Node.js spécifiées et acceptons les pull requests pour corriger les bogues. (à condition que les bogues ne soient pas connus dans Node.js car ils seront fixés de manière imminente). Par conséquent, *l'abandon du support* signifie que nous supprimerons ces versions de Node.js depuis notre matrice de test et nous n'accepterons plus les pull requests spécifiques pour corriger les bogues sur ces versions.

Lorsque nous abandonnerons la prise en charge d'une version majeure couverte par LTS, nous augmenterons la version majeure d'AVA.

Nous abandonnerons le support des versions Node.js impaires (par exemple `11` ou `13`) *sans augmenter* le numéro de version majeur de AVA.

Nous essayons d'éviter la suppression *accidentelle* du support des versions de Node.js qui ne sont pas les dernières. Si une telle rupture se produit, nous acceptons les pull requests pour restaurer les fonctionnalités. Nous pourrions décider de désapprouver la version AVA incriminée et augmenter à la place le numéro de version majeur de AVA.

À chaque fois que nous augmentons le numéro de version majeur d'AVA, nous *abandonnerons* explicitement le support des versions non-récentes de Node.js. Cela nous assure de pouvoir compter sur des API rétroportées ou sur la disponibilité de versions V8 plus récentes dans les versions ultérieures de Node.js, que ce soit dans AVA lui-même ou dans l'une de nos dépendances.

Nous pouvons abandonner le support pour une version de Node.js, dans une pré-version-pour-une-version-majeure, si cette nouvelle version de AVA est supposée devenir stable autour ou après la date de fin de vie de la version Node.js en question.

Les fonctionnalités expérimentales sélectionnées via la configuration `nonSemVerExperiments` peuvent être modifiées ou supprimées à tout moment.
