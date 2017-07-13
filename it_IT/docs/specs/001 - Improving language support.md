___
**Nota del traduttore**

Questa è la traduzione del file [001 - Improving language support.md](https://github.com/avajs/ava/blob/master/docs/specs/001%20-%20Improving%20language%20support.md). Qui c'è il [link](https://github.com/avajs/ava/compare/0069a7ef4797a88149031124e7eade090a18ad4a...master#diff-01b90728d033fffd4c5cdacef3735d16) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sul branch master (Se si clicca sul link, e non si vede il file `001 - Improving language support.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Migliorare il supporto ad altri linguaggi

Traduzioni: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/specs/001%20-%20Improving%20language%20support.md),
 [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/specs/001%20-%20Improving%20language%20support.md),

Questo è un [RFC] per proporre un miglioramento del supporto per Babel, React e TypeScript in AVA.

## Enunciazione del problema

Integrare AVA in progetto con Bable può essere veramente compilicato. Gli utenti devono configurare `babel-core/register` per consentire agli helper e ai file sorgente di essere compilati correttamente. Configurare le modalità di compilazione dei file di test non è chiaro. I file sorgente potrebbero [avere configurazioni diverse quando caricati in AVA][source options reason] rispetto a quelle applicate in produzione.
Non esiste alcun supporto per scrivere test e codice sorgente in altri linguaggi come TypeScript o JSX.

## Contesto

AVA utilizza Bable per permettere agli utenti di usare [ES2015](https://babeljs.io/docs/plugins/preset-es2015/) e [stage-2](https://babeljs.io/docs/plugins/preset-stage-2/). I messaggi di asserzione sono ["aumentati" mediante l'uso di un plugin Babel](https://github.com/avajs/ava/pull/46), ed un altro plugin viene utilizzato per controllare  [l'uso improprio di `t.throws()`](https://github.com/avajs/ava/pull/742).

Inizialmente `babel/register` era [usato direttamente](https://github.com/avajs/ava/pull/23) e applicato ai file di test, agli helper e ai file sorgente. Successivamente il comportamento è stato cambiato per [transpilare solo i file di test](https://github.com/avajs/ava/issues/50). Il comportamento iniziale era stato [considerato  come bug](https://github.com/avajs/ava/issues/108#issuecomment-151245367), probabilmente perchè AVA compilava i fie sorgente contro l'intento originario dell'utente.

In seguito gli utenti vennero [consigliati di aggiungere `babel-core/register` alla lista di moduli richiesti in automatico all'esecuzione dei test](https://github.com/avajs/ava#transpiling-imported-modules). Questo ha mostrato però che caricare Babel in ogni processo è un compito molto lento, e furono fatti vari tentativi per compilare i [file helper][1078] e i [file sorgente][945] nel processo AVA principale.

Nel mentre AVA ha deciso di allontanarsi dalla pratica di utilizzo di `babel/register` e iniziato ad utilizzare Babel in modo diretto. Un [livello di cache](https://github.com/avajs/ava/pull/352) è stato quindi costruito per il codice compilato in Babel.

AVA controlla solo file che hanno un'estensione `.js`, anche se il pattern glob fornito include altri file. Per definizione questo previene [la selezione di file JSX e TypeScript](https://github.com/avajs/ava/issues/631).

## Soluzioni possibili

La soluzione [#945][945] prova a compilare tutte le dipendenze dei file di test (quidi file helper e file sorgente) nel processo principale. Senza considerare alcuni bug non ancora risolti, un grande svantaggio risiede nell'impossibilità di supportare `require` dinamici, poichè questi avvengono nei processi figli durante l'esecuzione e non nel processo principale dove sono compilati. Inoltre risolvere tutte le dipendenze aggiunge un certo costo di gestione alla funzionalità alto.
La soluzione [#1078][1078] permette di precompilare i file helper nel processo principale, esattamente come avviene per i file di test. Questo funziona decisamente bene ma è limitato unicamente ai file helper.
Con 
The conclusione raggiunta nella [#631][631 conclusione] fu di permettere diverse estensioni per i file di test. Sfortunatamente abilitare unicamente altre estensioni non è sufficiente a risolvere il problema iniziale. AVA infatti assume che questi file contengano codice JavaScript al loro interno, quindi non sarà in grado di eseguire codice JSX o TypeScript.
La soluzione [#1122](https://github.com/avajs/ava/pull/1122) è un avanzamento della [proposta contenuta nella #631][631 conclusione] con una configurazione relativa all'estensione `.ts` e compilando automaticamente questi file di test con TypeScript.
Sfortunatamente non è ancora chiaro come questo approccio possa funzionare senza incorrere a svantaggi prestazionali già incontrati con Babel sopra. Inoltre i file di test che utilizzano TypeScript non godono di asserzioni avanzate e controlli per l'uso improprio di `.throws()`. Questa mancanza è molto difficile da comunicare agli utenti quando per abilitare TypeScript è sufficiente utilizzare unicamente l'opzione `extensions`.

## Proposte specifiche

AVA compila i file di test e helper in modo predefinito. Per fare ciò utilizza Babel, ma solo i plugin con le proposte allo stage-4 e con standard ratificati (al momento è ES2016 pi`u le proposte che hanno raggiunto lo stage-4 e che verranno incluse in ES2017). questo vuol dire che AVA supporta la stessa sintassi di ESLint.

AVA non applica più il plugin [`babel-plugin-transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/). Questo plugin crea degli alias per variabili globali in ES2015 che sono non necessarie, poichè al momento la piattaforma target di AVA è Node.js 4. Questo è un [problema piuttosto comune](https://github.com/avajs/ava/issues/1089).

Le altre trasformazioni che AVA utilizza, come `babel-plugin-espower` e `babel-plugin-ava-throws-helper` saranno incluse in un unico `babel-preset-ava` che verrà automaticamente applicato. (Se fosse necessario potremmo aggiungere una opzione per applicare `babel-plugin-transform-runtime` insieme ad una [logica di riscrittura](https://github.com/avajs/ava/blob/033d4dcdcbdadbf665c740ff450c2a775a8373dc/lib/babel-config.js#L53:L61) al momento applicata per correggere i percorsi. È necessario utilizzare un approccio di attesa-e-controllo per questa aggiunta.)

### Progetti Babel

La sezione precedente è basata sull'assunzione che AVA venga utilizzato su progetto JavaScript che non richiedano compilazione. Molti utenti però già utilizzano un flusso Bable già configurato e desiderano usare AVA senza precompilare i loro file sorgenti. Semplicemente, impostare `"babel": true` nella configurazione AVA abilita il supporto AVA per progetti Babel. I file di test e helper sono già compilati come descritto sopra, ma i file sorgenti verranno ora compilati automaticamente.
AVA controlla per la presenza dei file `package.json` e `.babel.rc` nella cartella del progetto per determinare la configurazione Babel utilizzata per compilare i file sorgente (l'obiettivo è quello di estrarre un oggetto di configurazione Babel da questi due file). Questa descritta è una semplificazione del processo di gestione della configurazione Babel, dove si ricerca per i file di configurazione vicini ai file da compilare. Utilizzando questi due file AVA può creare una cache di compilazione senza caricare Babel ogni volta, e allo stesso tempo poter ricompilare i file sorgenti se qualcosa dovesse cambiare.
La gestione di progetti Babel da parte di AVA può essere più precisa qualora si passi un oggetto di configurazione invece al valore `true` descritto sopra:
* `compileSources: true | false`: stabilisce se compilare o meno i file sorgenti. Il valore predefinito è `true`.
* `extensions: "js" | ["js", "jsx", ...]`: Specifica quali file sono consentiti. Può essere usato per estendere i tipi di file da caricare. Il valore predefinito è `"js"`.
* `sourceOptions: {}`: permette di specificare [configurazioni Babel] precise per i file sorgenti. In questo contesto `babelrc: true` dichiara di unire la configurazione con le configurazioni trovate nei file `package.json` e `.babelrc` nel progetto. Il valore predefinito di `babelrc` è `true`.
* `testOptions: {}`: permette di specificare [configurazioni Babel] per compilare i file helper e test. Quando questa configurazione viene specificata, la configurazione predefinita in AVA per Babel viene disabilitata e viene utilizzata questa. Come con `sourceOptions` anche in questo caso `babelrc` ha come valore predefinito `true`. Impostare il valore `presets: ["ava"]` se si vogliono utilizzare le trasformate Babel di AVA con questa configurazione.
`sourceOptions` può essere usata per estendere una configurazione Babel condivisa così da poter caricare i file sorgenti nei test AVA. Ad esempio un utente potrebbe [dipendere da webpack per far funzionare la sintassi moduli ES2015 quando compilano, ma richiedere l'uso di `babel-plugin-transform-es2015-modules-commonjs` quando si usano i sorgenti nei test AVA][perchè avere configurazione sorgenti].
Le opzioni `sourceOptions` e `testOptions` nella [configurazione Babel] potrebbero contenere valori `ignore` e `only`. In questo caso verranno usati per determinare se un file richiede la compilazione. Non vengono usati per la selezione dei file o per il watch mode.

## Compilazione

Sulla base di questo [proof of concept](https://github.com/avajs/ava/pull/1082) la compilazione Babel viene spostata nel processo del test stesso. Se i file sorgenti devono essere compilati allora AVA caricherà i suoi hook *require* piuttosto che usare `babel-core/register`.
Le opzioni Babel per i file di test, helper e sorgenti sono preparati nel processo principale e poi condivisi nei sotto processi. Dei valori hash verranno creati da queste configurazioni e salvati nella cache come succede per altre dipendenze utilizzate per i test. I sotto processi utilizzerà l'hash dei file per controllare la cache per usare valore precedentemente salvati. (Poichè i sotto processi vengono eseguiti in concorrenza, bisogna essere sicuri che possano leggere i valori completi nella cache e non parziali. Va bene se lo stesso file viene compilato più volte.)

## Supporto per progetti TypeScript

Il supporto per TypeScript può essere fornito più o meno nello stesso modo del supporto avanzato per Babel descritto sopra. Configurando `"typescript": true` nella configurazione AVA il supporto per TypeScript viene abilitato quindi l'estensione `.ts` funzionerà per i file di test, helper e file sorgente. Un oggetto di configurazione può essere utilizzato al posto di `true`:
* `compileSources: true | false`: stabilisce se compilare o meno i file sorgenti. Il valore predefinito è `true`.
* `extensions: "ts" | ["ts", "tsx", ...]`: Specifica quali file sono consentiti. Può essere usato per estendere i tipi di file da caricare. Il valore predefinito è `"ts"`.
* `sourceOptions: {}`: permette di specificare [configurazioni TypeScript] precise per i file sorgenti. L'opzione `extends` ha come valore predefinito il file `tsconfig.json`, se viene trovato. È necessario configurarlo come `null` per evitare di estendere questo file.
* `testOptions: {}`: permette di specificare [configurazioni TypeScript] per compilare i file helper e test. Il comportamento è lo stesso di `sourceOptions`, ma non esiste una configurazione predefinita per i file di test e helper in questo caso, al contrario dei progetti Babel.
Le opzioni `sourceOptions` e `testOptions` nella [configurazione TypeScript] potrebbero contenere dei valori per `files`, `include` e `exclude`: questi valori non vengono utilizzati per selezionare i file o per il watch mode.

## Per maggiori dettagli di implementazione

Il supporto per Babel e TypeScript in AVA si possono utilizzare moduli Node.js distinti. L'interfaccia di questi moduli deve essere la stessa, così da semplificare l'integrazione con AVA.
AVA offre il supporto per Babel in modo nativo, mentre è necessario installare dipendenze aggiuntive per attivare il supporto per TypeScript. Qualora il supporto per TypeScript venisse abilitato ma AVA non riesce a trovare le dipendenze richieste, verrà mostrato un errore con informazioni utili per correggere il problema.
I file selezionati duranti l'esecuzione dei test sarà la combinazione delle configurazioni trovate in `babel` e `typescript`.
I percorsi relativi dichiarati in `sourceOptions` e `testOptions` dovranno fare riferimento [al percorso originario del file `package.json`.](https://github.com/avajs/ava/issues/707).
[1078]: https://github.com/avajs/ava/pull/1078
[631 conclusione]: https://github.com/avajs/ava/issues/631#issuecomment-248659780
[945]: https://github.com/avajs/ava/pull/945
[configurazione Babel]: https://babeljs.io/docs/usage/api/#options
[perchè avere configurazione sorgenti]: https://github.com/avajs/ava/issues/1139#issuecomment-267969417
[configurazione TypeScript]: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html