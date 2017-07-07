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


### Babel projects
The above assumes AVA is used with regular JavaScript projects that do not require compilation. Many users though already have a Babel pipeline in place and wish to use AVA without having to precompile their source files.
At its simplest, setting `"babel": true` in the AVA configuration enables AVA's support for Babel projects. Test and helper files are compiled as per the above, but source files are now automatically compiled as well.
AVA looks at either the project's `package.json` or `.babelrc` files for the Babel options used to compile source files (ideally we can extract a proper Babel configuration object from these two locations). This is a simplification of Babel's actual configuration management, which searches for the options file that is closest to the file being compiled. Looking at these two specific files allows AVA to use cached compilation results without even having to load Babel, while still recompiling source files if options change.
AVA's handling of Babel projects can be further configured by passing an options object instead of `true`:
* `compileSources: true | false`: defaulting to `true`, determines whether sources are compiled.
* `extensions: "js" | ["js", "jsx", ...]`: defaulting to `"js"`, specifies the allowed file extensions. This expands the default test file patterns.
* `sourceOptions: {}`: specify the [Babel options] used to compile source files. In this context `babelrc: true` causes options to be merged with those found in either the project's `package.json` or `.babelrc` files. `babelrc` defaults to `true`.
* `testOptions: {}`: specify the [Babel options] used to compile test and helper files. If provided this completely disables the default Babel configuration AVA uses to compile test and helper files. Like with `sourceOptions`, `babelrc` defaults to `true`. Set `presets: ["ava"]` to apply AVA's transforms.
`sourceOptions` can be used to extend a shared Babel configuration so that the source files can be loaded in AVA tests. For instance users may [rely on webpack to resolve ES2015 module syntax at build time, but still need to apply `babel-plugin-transform-es2015-modules-commonjs` for sources to work in AVA][source options reason].
`sourceOptions` and `testOptions`, being [Babel options], may specify `ignore` and `only` values. These are only used to determine whether the file needs compilation. They do not impact test file selection or source watching.
## Compilation
Based on this [proof of concept](https://github.com/avajs/ava/pull/1082) Babel compilation is moved into the test workers. If source files are to be compiled AVA will load its own require hook, rather than relying on `babel-core/register`.
Babel options for test, helper and source files are prepared in the main process, and then shared with the workers. Caching hashes are derived from these configurations as well as other dependencies that might be involved.
Workers hash the raw file contents and inspect a cache to see if a previously compiled result can be used. (Given that workers can run concurrently, care must be taken to ensure that they read complete cache entries. It's OK though if the same file is compiled more than once.)
## TypeScript projects
TypeScript support can be provided in much the same way as the advanced Babel support described above. Setting `"typescript": true` in the AVA config enables TypeScript support for `.ts` test and helper files, as well as sources. An options object can also be provided:
* `compileSources: true | false`: defaulting to `true`, determines whether sources are compiled.
* `extensions: "ts" | ["ts", "tsx", ...]`: defaulting to `"ts"`, specifies the allowed file extensions. This expands the default test file patterns.
* `sourceOptions: {}`: specify the [TypeScript options] used to compile source files. The `extends` option defaults to the project's `tsconfig.json` file, if any. It must explicitly be set to `null` to avoid extending this file.
* `testOptions: {}`: specify the [TypeScript options] used to compile test and helper files. Behaves the same as `sourceOptions`, there is no default configuration for test and helper files, unlike with Babel projects.
For `sourceOptions` and `testOptions`, being [TypeScript options], `files`, `include` and `exclude` options do not impact test file selection or source watching.
## Further implementation details
Both Babel and TypeScript support can be provided through separate Node.js modules. They should implement the same interface, to make integration with AVA easier.
AVA ships with Babel support, however a separate dependency needs to be installed to make TypeScript support work. A helpful error is logged if this dependency is missing while TypeScript support is enabled.
AVA selects test files based on the combined `babel` and `typescript` configuration.
Relative paths in `sourceOptions` and `testOptions` [must be resolved relative to the `package.json` file](https://github.com/avajs/ava/issues/707).
[1078]: https://github.com/avajs/ava/pull/1078
[631 conclusione]: https://github.com/avajs/ava/issues/631#issuecomment-248659780
[945]: https://github.com/avajs/ava/pull/945
[Babel options]: https://babeljs.io/docs/usage/api/#options
[source options reason]: https://github.com/avajs/ava/issues/1139#issuecomment-267969417
[TypeScript options]: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html