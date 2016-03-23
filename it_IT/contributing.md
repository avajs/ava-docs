___
**Nota del traduttore**

Questa è la traduzione del file [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md). Qui c'è il [link](https://github.com/sindresorhus/ava/compare/f2c070987ecee3caf7613190acf2c8a90700e058...master#diff-f2c070987ecee3caf7613190acf2c8a90700e058) dove si confrontano le differenze tra commit di riferimento di questa traduzione e l'ultimo commit di AVA sulla branch master (Se si clicca sul link, e non si vede il file `readme.md` nella lista dei file modificati, questa traduzione è aggiornata).
___
# Contribuire ad AVA

✨ Grazie per contribuire ad AVA! ✨

Considera che questo progetto utilizza un [Codice di Condotta per i Collaboratori](code-of-conduct.md). Partecipando al progetto accetti i suoi termini.

Traduzioni: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/contributing.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/contributing.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/contributing.md), [Portugués](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/contributing.md)

## Come posso contribuire?

### Migliora la documentazione

Come utente di AVA tu sei il perfetto candidato per aiutarci a migliorare la nostra documentazione. Correzioni ortografici, correzione di errori, migliorare le spiegazioni, più esempi, etc... Apri nuove issue per problemi che pensi possano essere migliorati. [Aiutaci a tradurre la documentazione](https://github.com/sindresorhus/ava-docs). Ogni aiuto è il benvenuto. Anche migliorare questa stessa documentazione.

### Migliora le issues

Alcune issues vengono create senza tutte le informazioni importanti, oppure non sono riproducibili, o semplicemente non valide. Aiutaci a migliorarle per risolvere rapidamente e meglio. Occuparsi delle issues richiede molto tempo che potremmo invece spendere nel risolvere bug o aggiungere nuove funzionalità.

### Dai il tuo feedback nelle issues

Siamo sempre alla ricerca di diverse opinioni nell'issue tracker. È una buona opportunità per influenzare le future decisioni di AVA.

### Passa nella nostra chat

Abbiamo una [chat](https://gitter.im/sindresorhus/ava). Visita e dai un'occhiata, parla con noi ed aiuta chi ne ha bisogno.

### Notifica un issue

- L'issue tracker è per le issue (bug, etc...). Utilizza la nostra [chat](https://gitter.im/sindresorhus/ava) o [Stack Overflow](https://stackoverflow.com/questions/tagged/ava) per il supporto.
- Cerca se già esiste l'issue o bug che hai prima di aprirne uno nuovo.
- Assicurati di usare l'ultima versione di AVA.
- Utilizza un titolo per l'issue chiaro e descrittivo.
- Inserisci più informazioni che puoi: passi per riprodurre il problema, messaggio di errore, versione Node.js, sistema operativo, etc...
- Più tempo ci impieghi per scrivere una issue, più tempo ce ne impiegheremo noi per risolverla.
- [Il miglior tipo di issue che puoi inviare è provarlo con un test che fallisce.](https://twitter.com/sindresorhus/status/579306280495357953)

### Invia una pull request

- È meglio discutere cambiamenti non banali in una issue prima di inviare una PR, per evitarti spesso lavoro non necessario.
- Per cambiamenti ambiziosi, cerca di portare il tuo lavoro davanti la comunità per feedback il prima possibile. Invia una pull request non appena hai lo stretto necessario per dimostrare la tua idea. In questa prima fase non preoccuparti di fare tutto perfetto, o completo al 100%. Aggiungi il prefisso [WIP] nel titolo e descrivi cosa manca ancora. Questo permette a reviewer di sapere che non è necessario essere troppo puntigliosi o proporre miglioramenti che hai già in piano di fare.
- Nuove funzionalità devono essere supportate da test e documentazione.
- Non includere cambiamenti non necessari alla funzionalità.
- Verifica con Lint il codice e testalo prima di inviare la pull request eseguent `$ npm test`
- Crea la pull request da una [branch dedicata](https://github.com/dchelimsky/rspec/wiki/Topic-Branches), non master.
- Usa un titolo chiaro e descrittivo per la pull request ed i commit.
- Scrivi una descrizione che ci convinca sul perchè dovremmo accettare la tua pull request. È il tuo lavoro convincerci. Fornisci il "perchè" sia necessaria e fornisci dei casi d'uso.
- Potremmo chiederti di fare dei cambiamenti alla tua pull request. Non c'è bisogno di aprire una nuova pull request, semplicemente [aggiorna quella già aperta](https://github.com/RichardLitt/docs/blob/master/amending-a-commit-guide.md).

Attenzione: quando fai modifiche al codice, ricorda che il mantra di AVA (preso da Python) è avere preferibilmente un unico modo di fare qualcosa. Ad esempio, una pull request per aggiungere un alias per una parte dell'API ([come questo](https://github.com/sindresorhus/ava/pull/663))) sarà molto probabilmente rifiutata se non introduce qualche ulteriore beneficio.
