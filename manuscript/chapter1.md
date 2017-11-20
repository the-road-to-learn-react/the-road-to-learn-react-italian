# Introduzione a React

Questo capitolo vuole essere una introduzione a React. Puoi chiederti: perché dovrei imparare React? L'obiettivo di questo capitolo è rispondere a questa domanda. Quindi ti catapulterai nell'ecosistema React con una applicazione da zero senza alcuna configurazione particolare. Lungo questo percorso sarai introdotto ad argomenti come JSX e ReactDOM. Infine sarai pronto per i tuoi primi componenti React.

## Ciao, mi chiamo React.

**Perchè dovresti occuparti di React?** Negli anni recenti le applicazioni single-page ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) sono diventate sempre più popolari. Framework come Angular, Ember e Backbone hanno aiutato gli sviluppatori JavaScript a costruire moderne applicazioni attraverso l'utilizzo di JavaScript puro e jQuery. La lista di questi progetti popolari non è esaustiva. In giro troviamo una grande quantità di framework SPA. Se consideriamo le date di rilascio, la maggior parte di loro sono della prima generazione di SPA: Angular 2010, Backbone 2010 e Ember 2011.

La prima release di React è del 2013 ad opera di Facebook. React non è un framework SPA ma una view library. Rappresenta la lettara "V" nel concetto [MVC](https://de.wikipedia.org/wiki/Model_View_Controller) (model view controller).
Il suo compito è renderizzare i componenti come elementi view nel browser. Tutto l'ecosistema attorno a React è orientato alla creazione di applicazioni single-page.

Ma perché dobbiamo considerare React come un passo più avanti le prime generazioni di SPA? Mentre le prime generazioni di framework cercavano di risolvere una serie di cose alla volta, React si occupa solo di costruire il tuo *view layer*. E' una libreria, non un framework. L'idea che c'è dietro è la seguente: le tue view rappresentano una gerarchia di componenti "componibili".

In React puoi mantenere il focus sul tuo livello view prima di affrontare ulteriori aspetti della tua applicazionee. Ogni altro aspetto è un altro *building block* per la tua SPA. Questi *building block* sono essenziali per costruire una applicazione complessa. Hanno due vantaggi.

Prima di tutto puoi apprendere i *building block* un passo alla volta. Non devi preoccuparti di comprenderli nella loro completezza. E' differente rispetto ad un framework che ti obbliga a comprendere un *building block* sin dall'inizio. Ulteriori *building block* saranno affrontati più in là.

In secondo luogo, tutti i *building block* sono intercambiabili. Questo rende l'ecosistema attorno a REact davvero innovativo. Più soluzioni sono in competizione tra di loro. Puoi scegliere quelle più utili per te e per i tuoi casi d'uso.

La prima generazione di framework SPA è giunta ad un livello enterprise. Sono piuttosto rigidi. React resta innovativo ed è stato adottato da diverse compagnie leader come [Airbnb, Netflix e naturalmente Facebook](https://github.com/facebook/react/wiki/Sites-Using-React). Ognuna di loro ha investito nel futuro di React ed è soddisfatta di React e del suo ecosistema.

React è probabilmente una delle migliori scelte per costruire moderne applicazioni web al giorno d'oggi. Il suo focus è rappresentato dalle *view layer*, [ma l'ecosistema React è un immenso e flessibile e intercambiabile framework](https://www.robinwieruch.de/essential-react-libraries-framework/).
React possiede uno snello API, un fantastico ecosistema e una grande community. Puoi dare un'occhiata a perché [sono passato da Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/). Comprendere bene le differenze tra React e gli altri framework è caldamente consigliato. Dopo tutto chiunque è ansioso di sapere dove React ci porterà nei prossimi anni.

### Esercizi

* leggi [perché sono passato da Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* leggi [L'ecosistema flessibile di React](https://www.robinwieruch.de/essential-react-libraries-framework/)

## Requisiti

Se hai già usato SPA framework, avrai familiarità con questi concetti base dello sviluppo web. Se hai appena iniziato, invece, dovresti sentirti almeno a tuo agio con HTML, CSS e JavaScript ES5 per apprendere React. Il libro rappresenta un ottimo strumento per passare a JavaScript ES6 e oltre. Invito sempre tutti ad unirsi al [Gruppo ufficiale Slack](https://slack-the-road-to-learn-react.wieruch.com/) del libro per avere aiuto ed aiutare gli altri.

### Editor e Terminale

Come la mettiamo con l'ambiente di sviluppo? Avrai bisogno di un editor o IDE e di un terminale (la riga di comando). Puoi dare un'occhiata alla mia [guida di setup](https://www.robinwieruch.de/developer-setup/). E' illustrata per gli utenti MacOS, ma molti dei tool sono adatti per altri sistemi operativi. Ci sono migliaia di articoli che mostrano come impostare l'ambiente di sviluppo nel migliore dei modi.

In alternativa puoi usare git e GitHub mentre fai gli esercizi di questo libro, per tenere traccia dei progressi nelle repository di GitHub. C'è anche una [piccola guida](https://www.robinwieruch.de/git-essential-commands/) su come usare questi tool. Ma ancora una volta, queste attività non sono obbligatorie per seguire questo libro perché imparerai ogni cosa da zero. Dunque puoi saltare questa parte se sei un beginner nello sviluppo web per focalizzarti sulle parti essenziali illustrate in questo testo.

### Node e NPM

Ultimo, ma non per importanza: avrai bisogno di un'installazione di [node e npm](https://nodejs.org/en/). Entrambi saranno usati per gestire le librerie di cui avrai bisogno nel tuo percorso. In questo libro installerai i package esterni node via npm (node package manager). Questi package node possono essere librerie o interi framework.

Puoi verificare le tue versioni di node e npm attraverso riga di comando. Se il terminale non ti restituisce nulla, allora devi installare node e npm prima. Queste sono le mie versioni mentre ti scrivo questo libro:

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.3.0
npm --version
*v5.5.1
~~~~~~~~

## node e npm

Questo capitolo vuole essere un piccolo crash course in node e npm. Non è esaustivo, ma ti aiuterà a prendere dimestichezza con concetti chiave. Se hai familiarità con entrambi i concetti, puoi tranquillamente saltare questo capitolo.

Il **node package manager** (npm) ti permette di installare **package node** esterni attraverso la riga di comando. Questi package possono essere un set di metodi, librerie o framework interi. Sono le dipendenze delle tue applicazioni. Puoi installarli entrambi nella tua cartella globale dove risiederanno tutti i pacchetti node oppure nella tua cartella locale.

Se installi tutto in modo "global" i pacchetti saranno utilizzabili ovunque dal terminale e dovrai installarli solo una volta nella tua directory globale. Per installare i package globalmente digita nel tuo terminale:

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

La `-g` dice a npm di installare il package globalmente. I package locali sono usati nella tua applicazione. Per esempio, React come libreria sarà un package locale che può essere utilizzato nella tua applicazione. Puoi installarlo via terminale digitando:

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

Per React dovrai scrivere:

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

I package installati appariranno automaticamente nella cartella chiamata *node_modules/* e saranno elencati nel file *package.json* insieme alle relative dipendenze.

Ma come inizializzare la cartella *node_modules/* e il file *package.json* per il tuo progetto? C'è un comando npm utile per l'inizializzazione attraverso il file *package.json*. Solo quando avrai quel file, puoi installare nuovi package locali via npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

La lettera `-y` è uno shortcut per inizializzare tutti i default nel tuo file *package.json*. Se non usi questa istruzione, dovrai occuparti della configurazione del file. Dopo l'inizializzazione del tuo progetto npm sei pronto per installare nuovi package attraverso `npm install <package>`.

Ancora una parola sul file *package.json*. Questo file ti permette di condividere il tuo progetto con altri sviluppatori senza condividere tutto il package node. Il file, infatti, ha tutte le referenze dei package node usati nel progetto. Questi package sono chiamati dipendenze. Chiunque può copiare il tuo progetto senza le dipendenze. Queste, infatti, sono referenziate nel file *package.json*. Chi vuol copiare il tuo progetto deve semplicemente installare tutti i package usando il comando `npm install` via riga di comando. Lo script `npm install` recupera tutte le dipendenze elencate nel file *package.json* e le installa nella cartella *node_modules/*.

Vorrei parlare di un altro comando npm:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

L'istruzione `--save-dev` indica che il package node è solo usato per l'ambiente di sviluppo. Non sarà usato in produzione quando effettui il deploy dell'applicazione sul server. Che tipo di package node potrebbe essere? Immagina che tu voglia testare la tua applicazione con l'aiuto di un package node. Avrai la necessità di installare quel package via npm, ma vuoi escluderlo dal tuo ambiente di produzione. Il testing dovrebbe essere utile durante il processo di sviluppo ma non quando la tua applicazione è già in produzione. In questo caso non puoi più testare la tua applicazione. Dovrebbe essere testata e funzionare per i tuoi utenti. Quello è l'unico caso d'uso in cui dovresti usare l'istruzione  `--save-dev`.

Ti capiterà di utilizzare diversi comandi npm lungo il tuo percorso di lavoro o di studio. 
Ma questi sono sufficienti per adesso.

### Esercizi:

  * predisponi un progetto di lancio attraverso npm
  * crea una nuova cartella con  `mkdir <folder_name>`
  * vai nella cartella con  `cd <folder_name>`
  * esegui `npm init -y` oppure `npm init`
  * installa un package localmente come React attraverso `npm install react`
  * dai un'occhiata nel file *package.json* e nella cartella *node_modules/*
  * scopri da solo come disinstallare il package *react*
  * dai un'occhiata a [npm](https://docs.npmjs.com/)

## Installazione

Sono diversi gli approcci per iniziare una applicazione React.

La prima è usare CDN. Potrebbe sembrare più complicato di quello che realmente è. CDN è l'acronimo di [content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network). Sono numerose le aziende che utilizzano CDN per rilasciare pubblicamente i propri file e permettere agli utenti di utilizzarli. Questi file possono essere librerie come React, perché in effetti React è solo un file *react.js*. Può quindi essere ospitato su qualunque server.

Come usare un CDN per iniziare a lavorare con React? Attraverso un tag `<script>` inline nella tua pagina HTML che punta all'url CDN. Per iniziare a lavorare con React hai bisogno di ude file (librerie): *react* e *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

Ma perché dovresti usare un CDN quando hai a disposizione una installazione attraverso npm?

Se la tua applicazione ha un file *package.json*, puoi installare *react* e *react-dom* da riga di comando. Il requisito fondamentale è che la cartella debba essere inizializzata come progetto npm utilizzando il comando `npm init -y` con all'interno il file *package.json*. E' possibile installare diversi pacchetti node tutti in una linea attraverso npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

Questo approccio è quello più usato per aggiungere React ad una applicazione esistente gestita attraverso npm.

Sfortunatamente non è tutto. Dovresti utilizzare [Babel](http://babeljs.io/) per rendere la tua applicazione idonea a JSX (cioè, la sintassi utilizzata da React) e a JavaScript ES6. Babel compila il tuo codice in modo tale che il browser può interpretare JavaScript ES6 e JSX. Non tutti i browser sono capaci di interpretare la sintassi. Il setup include una serie di configurazioni e di strumenti. Può essere frustrante per chi utilizza per la prima volta React preoccuparsi di tutta la configurazione.

Proprio per questo motivo Facebook intrudusse *create-react-app* come step iniziale per imparare a configurare React. Il capitolo successivo ti mostrerà come fare il setup della tua applicazione utilizzando il tool di bootstrapping.

### Esercizi:

* dai un'occhiata alle [installazioni di React](https://facebook.github.io/react/docs/installation.html)

## Zero-Configuration Setup

Nel tuo cammino per imparare React, userai [create-react-app](https://github.com/facebookincubator/create-react-app) per far partire la tua applicazione. E' uno starter kit con zero-configurazione per iniziare con React, introdotto da Facebook nel 2016. C'è chi (96%) lo [raccomanda ai principianti](https://twitter.com/dan_abramov/status/806985854099062785). All'interno di *create-react-app* tutto ciò che è configurazione procede in background mentre il focus resta sull'implementazione dell'applicazione.

Per iniziare, devi installare il package globalmente. Dopo aver fatto ciò, avrai sempre a disposizione con riga di comando la possibilità di creare nuove applicazioni React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

Controlla la versione di *create-react-app* da riga di comando:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~


Adesso puoi far partire la tua prima applicazione React. La chiameremo *hackernews*, ma puoi dargli un nome diverso. L'installazione richiede un paio di secondi. Dopo questo step, vai all'interno della cartella:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Adesso puoi aprire l'applicazione nel tuo editor. La seguente struttura di cartelle, o una sua variante dipende dalla versione di *create-react-app* che scaricherai:

{title="Folder Structure",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
~~~~~~~~

Diamo un'occhiata alle cartelle e ai file. E' un passo essenziale per comprendere React.

* **README.md:** L'estensione .md indica che il file è in markdown. Markdown è un linguaggio di markup molto snello utilizzato per formattare il testo. La maggior parte dei progetti ha al suo interno un file *README.md* con le istruzioni iniziali. Quando fai un *push* su piattaforme come GitHub, il file *README.md* mostrerà il contenuto del progetto a chiunque avrà accesso ail tuo repository. Siccome hai usato *create-react-app*, il tuo file *README.md* dovrebbe essere il medesimo usato sul [repository ufficiale di create-react-app](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** All'interno di questa cartella troverai tutti i package node che sono stati installati via npm. Dal momento in cui hai usato *create-react-app*, troverai un paio di moduli node installati. Di solito questa è una cartella da non toccare, utile solo per installare e disinstallare package node via riga di comando (con npm).

* **package.json:** Questo file mostra una lista di dipendenze dei package node e altre configurazioni legate al progetto.

* **.gitignore:** Il file elenca tutti i file e cartelle che non dovranno essere aggiunti al repository remoto se usi git. Saranno solo presenti nel tuo progetto locale. La cartella *node_modules/* ne è un esempio. E' sufficiente condividere ilfile *package.json* con i tuoi collaboratori per permettere loro di installare tutte le dipendenze sui loro progetti locali senza condividere la cartella stessa.

* **public/:** La cartella contiene tutti i file per lavorare al tuo progetto in produzione. Di solito tutto il codice scritto lo troverai nella cartella *src/* e sarà raggruppato in un paio di file quando costruisci il tuo progetto e lo organizzi in questa cartella *public*.

In effetti, non hai bisogno di toccare i file e le cartelle menzionate. All'inizo qualsiasi cosa di cui tu abbia bisogno è presente all'interno della cartella *src/*. Il focus è nel file *src/App.js* per implementare i componenti React. Saranno usati per implementare la tua applicazione, ma più in laà potresti aver bisogno di dividere i tuoi componenti in file diversi dove ogni file mantiene uno o più componenti.

In più, utilizzerai il file *src/App.test.js* per i tuoi test e il file *src/index.js* come entry point al mondo React. Comprenderai entrambi i file più in là. Inoltre, c'è il file *src/index.css* e *src/App.css* per gestire lo stile della tua applicazione e dei tuoi componenti. Al loro interno troverai già delle regole di default.

L'applicazione *create-react-app* è un progetto npm. Puoi dunque usare npm per installare e disinstallare i package node nel tuo progetto. La riga di comando è la seguente:

{title="Command Line",lang="text"}
~~~~~~~~
// Runs the application in http://localhost:3000
npm start

// Runs the tests
npm test

// Builds the application for production
npm run build
~~~~~~~~


Gli script sono definiti nel file *package.json* La tua applicazione base di React è pronta. La parte eccitante arriverà negli esercizi da testare nel tuo browser.

### Esercizi:

* `npm start` per avviare la tua apllicazione e vederla nel browser
* avvia lo script interattivo `npm test`
* dai un'occhiata al contenuto nella cartella *public/*, lancia il comando `npm run build` e verifica che i file siano stati aggiunti alla cartella (puoi anche rimuovere questi file, ma non è necessario)
* prendi dimestichezza con la struttura della cartella
* prendi dimenstichezza con il contenuto dei file
* approfondisci i comandi [npm e create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduzione a JSX

Adesso ti toccherà sapere di più su JSX. E' la sintassi che troveremo in React. Come menzionato prima, *create-react-app* ha già di partenza una applicazione pronta per te. Tutti i file sono utili per una implementazione di default. Entriamo nel codice sorgente. L'unico file che toccherai all'inizio sarà *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Non lasciarti confondere dalle dichiarazioni import/export e dalle classi. Queste caratteristiche sono già presenti in JavaScript ES6. Gli daremo un'occhiata più tardi.

Nel file avrai un **React ES6 class component** come nome dell'App. E' una dichiarazione di un componente. Di base una volta che hai dichiarato un componente, puoi usarlo ovunque nell'applicazione. Produrrà una **istanza** del tuo **componente** o in altre parole: il componente verrà istanziato.

L'**element** viene specificato nel metodo `render()`. Gli elementi sono prodotti dai componenti. E' utile comprendere le differenze tra componenti, istanze ed elementi.

Molto presto, vedrai dove la componente App è istanziata. Altrimenti non riusciresti a vedere l'output renderizzato nel browser. La componente App è solo la dichiarazione, non il suo utilizzo. Devi istanziare il componente in qualche punto nel suto JSX con `<App />`.

Il contenuto nel blocco renderizzato è molto simile ad HTML, ma è in JSX.
JSX ti permette di mixare HTML e JavaScript. E' potente ma può confondere quando sei abituato a separare il tuo contenuto HTML dal JavaScript. Ecco perché è una buona idea usare HTML base nel tuo JSX. All'inizio, rimuovi tutti i contenuti che distraggono.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Adesso restituisci solo HTML nel tuo metodo `render()` senza JavaScript. Definiamo il "Welcome to the Road to learn React" come variabile. Una variabile può essere usata nel tuo JSX usando le parentesi graffe.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~


Dovrebbe funzionare non appena lanci da riga di comando l'istruzione `npm start`. 

In più dovresti dare un'occhiata all'attributo `className`. Riflette l'attributo standard `class` nell'HTML. Per ragioni tecniche, JSX doveva sostituire una serie di attributi HTML. Maggiori approfondimenti circa [gli attributi HTML supportati li trovi nella documentazione React](https://facebook.github.io/react/docs/dom-elements.html). La convenzione usata è *camelCase*. Nel tuo percorso di apprendimento di React, dovrai tenere in considrazione alcuni attributi specifici JSX.

### Esercizi:

* definisci una serie di variabili e renderizzali nel tuo JSX
  * usa un oggetto complesso per rappresentare un utente con un nome e cognome
  * renderizza le proprietà dell'utente nel tuo JSX
* approfondisci di più su [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* approfondisci di più sui [componenti React, elementi e istanze](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const e let

Sono sicuro che tu abbia notato che abbiamo dichiarato la variabile `helloWorld` con una dichiarazione `var`. JavaScript ES6 ha due opzioni in più per dichiarare le tue variabili: `const` e `let`. In JavaScript ES6, raramente troverai l'istruzione `var`.

Una variabile dichiarata con `const` non può essere riassegnata o dichiarata nuovamente. Non può mutare (cambiare, essere modificata). Puoi concepire strutture dati immutabili in questo modo. Una volta che la struttura dei dati è definita, non puoi cambiarla.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~


Una variabile dichiarata con `let` puoi cambiarla.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

Dovresti usare quando devi riassegnare una variabile.

Ad ogni modo, fai attenzione con `const`. Una variabile dichiarata con `const` non può essere modificata. Ma quando la variabile è un array o un oggetto, il valore può essere aggiornato. Il valore non è immutabile.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~


Ma quando usare una delle due dichiarazioni? Ci sono differenti opinioni a riguardo. Suggerisco di usare `const` ogni volta che puoi. E' indicata quando vuoi mantenere la tua struttura dati immutabile anche se i valori in oggetti o array possono essere modificati. Se vuoi modificare la tua variabile, puoi usare `let`.

L'immutabilità è concepita in React e nel suo ecosistema. Ecco perché `const` dovrebbe essere la scelta di default quando definisci una variabile. Ancora, negli oggetti complessi i valori all'interno possono essere modificati. Fa attenzione a questo comportamento.

Nella tua applicazione dovresti usare `const` al posto di `var`.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

### Esercizi:

* dai un'occhiata all'istruzione [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* dai un'occhiata all'istruzione [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* cerca ulteriori strutture dati immutabili
  * perchè hanno un senso nella programmazione in generale
  * perché sono usate in React e nel suo ecosistema

## ReactDOM

Prima di continuare con la tua App component, avrai voglia di vedere dove è usata. E' localizzata nel tuo entry point: il file *src/index.js*.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~


Di base `ReactDOM.render()` usa un nodo DOM nel tuo HTML per sostituirlo con JSX. Ecco perché puoi integrare facilmente React in qualsiasi applicazione esterna. Non è vietato usare `ReactDOM.render()` diverse volte nella tua applicazione. Puoi usarlo in differenti punti per impostare una semplice sintassi JSX, un component React, multipli componenti React o una intera applicazione. Ma in una applicazione React lo userai solo una volta per impostare il tuo intero "albero" di componenti.

`ReactDOM.render()` accetta due argomenti. Il primo argomento è JSX che attiva la renderizzazione. Il secondo argomento specifica il posto dove l'applicazione React interviene nel tuo HTML. Accetta un elemento con un `id='root'`. Puoi aprire il tuo file *public/index.html* per trovare l'attributo id.

Nell'implementazione `ReactDOM.render()` gestisce già la tua App component. 
Ad ogni modo, è utile passare il più semplice JSX dal momento che è un JSX. Non ha bisogno di istanziare un componente.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Esercizi:

* apri *public/index.html* per vedere dove le applicazioni React impattano sul tuo HTML
*  approfondisci il [rendering degli elementi in React](https://facebook.github.io/react/docs/rendering-elements.html)

## Hot Module Replacement

C'è una cosa che puoi fare nel file *src/index.js* per migliorare la tua esperienza di sviluppo. Ma è opzionale e non dovresti abusarne all'inizio dell'apprendimento di React.

In *create-react-app* è già un vantaggio che il browser automaticamente effettua il refresh della pagina quando cambia il tuo codice sorgente. Prova a cambiare la variabile `helloWorld` nel tuo file *src/App.js*. Il browser dovrebbe fare il refresh della pagina. Ma c'è un modo migliore per fare questo.

L'Hot Module Replacement (HMR) è un tool per ricaricare la tua applicazione nel browser. Il browser non si occupa del refresh della pagina. Invece l'HMR la puoi facilmente attivare in *create-react-app*. Nel tuo file *src/index.js*, devi aggiungere qualche riga di configurazione.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

# leanpub-start-insert
if (module.hot) {
  module.hot.accept();
}
# leanpub-end-insert
~~~~~~~~

Ecco fatto. Prova ancora una volta a cambiare la variabile `helloWorld` nel tuo file *src/App.js*. Il browser non ricarica la pagina, ma l'applicazione ricarica e mostra il corretto output. HMR ha diversi vantaggi:

Immagina di essere alle prese con il debugging del tuo codice e stai utilizzando `console.log()`. Queste dichiarazioni restano nella tua developer console, anche quando cambi il tuo codice, perché il browser non fa il refresh della pagina. Una situazione molto conveniente per il debugging.

In una applicazione complessa il refresh di una pagina rallenta la tua produttività. Devi attendere fino a quando la pagina non ricaricata. Una pagina in ricaricamento può richiedere diversi secondi in una applicazione complessa. HMR azzera questi svantaggi.

Il più grande beneficio è che puoi mantenere lo stato dell'applicazione con HMR. Immagina di avere un dialogo nella tua applicazione con differenti step e sei al terzo step. Di solito è un wizard. Senza HMR quando cambi il tuo codice, il tuo browser ricarica la pagina e dovrai ripartire dallo step 1 per arrivare allo step 3. Con HMR resti allo step 3. Mantiene lo stato dell'applicazione anche se il codice sorgente cambia. L'applicazione stessa si ricarica, non la pagina.

### Esercizi:

* cambia il tuo codice sorgente in *src/App.js* un po' di volte per vedere HMR in azione
* dai un'occhiata ai primi 10 minuti d [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) di Dan Abramov

## Complex JavaScript in JSX

Torniamo alla nostra applicazione. Finora renderizzavi le variabili primitive nel tuo JSX. Adesso inizierai a renderizzare una lista di item. La lista sarà composta da semplici dati all'inizio, ma più tardi recupererai i dati da un'[API esterna](https://www.robinwieruch.de/what-is-an-api-javascript/). Questo sarà molto più divertente.

Prima di tutto definiamo la lista di item.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

I dati semplici saranno simili ai dati che andremo a recuperare più tardi attraverso l'API. Un item nella lista ha un titolo, un url e un autore. In più avrà un identificativo, punteggio (che indicherà la popolarità dell'articolo) e un conteggio dei commenti.

Adesso puoi usare il metodo built-in di JavaScript `map` nel tuo JSX. Ti permette di ciclare la tua lista per mostrare gli item. Inoltre utilizzerai le parentesi graffe per incapsulare l'espressione JavaScript nel tuo JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Usare JavaScript nell'HTML è molto più efficace nel JSX. Di solito dovresti usare `map` per convertire una lista di item in un'altra lista di item. Questa volta usa `map` per convertire una lista di item in elementi HTML.

Per adesso solo il `title` sarà mostrato per ogni item. Mostriamo qualcosa in più delle proprietà dell'item.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Puoi notare come il metodo `map` è semplicemente *inlined* nel tuo JSX. Ogni proprietà dell'item è mostrata in un tag `<span>`. Inoltre la proprietà url dell'item è usata nell'attributo `href` dell'anchor tag.

React farà tutto il lavoro per te e mostrerà ogni item. Ma puoi aggiungere un helper per React per racchiudere la sua potenzialità e migliorare la sua performance. Devi assegnare un attributo chiave ad ogni elemento della lista. In questo modo React sarà in grado di identificare gli item aggiunti, modificati e rimossi quando la lista cambia. La lista semplice degli item ha già un identificativo.

{title="src/App.js",lang=javascript}
~~~~~~~~
{list.map(function(item) {
  return (
# leanpub-start-insert
    <div key={item.objectID}>
# leanpub-end-insert
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Dovresti assicurarti che l'attributo chiave sia un identificativo stabile. Non fare l'errore di usare index dell'item nell'array. L'index dell'array non è stabile dopo tutto. Per esempio, quando la lista cambia il suo ordine, React avrà difficoltà a identificare gli item in maniera corretta.

{title="src/App.js",lang=javascript}
~~~~~~~~
// don't do this
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

Stai mostrando entrambe le liste adesso. Puoi avviare la tua app, aprire il tuo browser e vedere entrambi gli item della lista.

### Esercizi:

* Approfondisci sulle [liste React e le chiavi](https://facebook.github.io/react/docs/lists-and-keys.html)
* fai un ripasso degli [array built-in di JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* utilizza più espressioni JavaScript sul tuo stesso JSX

## ES6 Arrow Functions

JavaScript ES6 introdusse i metodi arrow. Un metodo arrow è più breve di una classico metodo.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

Ma devi comprendere bene le sue caratteristiche. Una di esse è il differente comportamento dell'oggetto `this`. Un metodo classico definisce sempre il suo oggetto `this`. Un metodo arrow ha l'oggetto `this` nel suo contesto chiuso. Non confondere quando utilizzi `this`  nel metodo arrow.

C'è un'altra questione che riguarda i metodi arrow. E riguarda le parentesi. Puoi rimuovere le parentesi quando il metodo restituisce solo un argomento, ma devi tenerle quando nel restituisce più di uno.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, key => { ... }

// allowed
(item, key) => { ... }
~~~~~~~~

Dunque diamo un'occhiata al metodo `map`. Puoi scriverlo in maniera più concisa con un metodo arrow ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item => {
# leanpub-end-insert
  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

In più, puoi rimuovere il *block body*, cioè le parentesi graffe del metodo arrow ES6. In un *concise body* un implicito return è incluso. Quindi puoi rimuovere l'istruzione return. Questo accadrà diverse volte nel libro, quindi assicurati di comprendere la differenza tra un *block body* e un *concise body* quando usi i metodi arrow.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item =>
# leanpub-end-insert
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
# leanpub-start-insert
)}
# leanpub-end-insert
~~~~~~~~

Il tuo JSX sembra più conciso e leggibile adesso. E stata omessa l'istruzione *function*, le parentesi graffe e il l'istruzione *return*. Perché uno sviluppatore deve concentrarsi sull'implementazione dei dettagli.

### Esercizi:

* dai un'occhiata ai [metodi arrow ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## Le classi ES6

JavaScript ES6 introdusse le classi. Una classe è comunemente usata nella programmazione object-oriented (orientata agli oggetti). JavaScript era ed è molto flessibile nei suoi paradigmi di programmazione. Puoi lavorare con la programmazione funzionale e object-oriented a seconda dei casi.

Anche se React predilige la programmazione funzionale, per esempio con le strutture dati immutabili, le classi sono usate per dichiarare i componenti. Sono chiamate ES6 *class components*. React miscela le parti buone di entrambi i paradigmi.

Consideriamo la segunete classe per esaminare una classe JavaScript ES6 senza pensare ad un componente.

{title="Code Playground",lang="javascript"}
~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~


Una classe ha un costruttore per renderla istanziabile. Il costruttore può prendere argomenti per assegnarli alla classe istanziata. Inoltre una classe può avere al suo interno metodi. 

La classe Developer è solo una dichiarazione di classe. Puoi creare multiple istanze della classe invocandola. E' simile alla *class component*, che ha una dichiarazione, ma puoi usarla ovunque.

Vediamo come puoi istanziare la classe e come usare i suoi methodi.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React usa le classi JavaScript ES6 per le *class component*. Eccone una.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

La classe App estende `Component`. Di base dichiari il component App, ma è l'estensione di un altro componente. Cosa significa estendere? Nella programmazione object-oriented affronterai il principio dell'ereditarietà. E' usata per passare funzionalità da una classe ad un'altra.

La classe App estende le funzionalità da una classe Component. Per essere più specifico, eredità le funzionalità della classe Component. La classe Component è usata per estendere una classe basic ES6 in una *component class* ES6. Ha anche tutte le funzionalità che un component React deve avere. Il metodo render è una di queste funzionalità che tu già usi. Imparerai altro sui metodi delle *class component* più tardi.

La classe `Component` incapsula tutti i dettagli di implementazione di un componente React. Permette agli sviluppatori di usare classi come componenti in React.

I metodi che espone un `Component` React sono rappresentati da un'interfaccia pubblica. Uno di questi metodi è sovrascritto, gli altri non hanno bisogno di essere sovrascritti. Imparerai questi ultimi quando il libro parlerà del ciclo di vita dei metodi in un capitolo. Il metodo `render()` sarà sovrascritto, perché definisce l'output di un `Component` React. 

Adesso sei a conoscenza delle classi JavaScript ES6 e come sono utilizzate all'interno di React per estenderle ai componenti. 

### Esercizi:

* dai un'occhiata alle [classi ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

Devi padroneggiare le basi di una applicazione React! Riassumiamo l'ultimo capitolo:

* React
  * fai partire una applicazione React create-react-app
  * JSX miscela HTML e JavaScript per definire l'output dei componenti React nei loro metodi render
  * componenti, istanze ed elementi sono cose differenti in React
  * `ReactDOM.render()` è un entry point per un'applicazione React per agganciare React al DOM
  * le funzionalità built-in di JavaScript possono essere usate nel JSX
    * map può essere usato per renderizzare una lista di item come elementi HTML
* ES6
  * le variabili `const` e `let` possono essere utilizzate per casi d'uso specifici
    * usa *const* al posto di *let* nelle applicazioni React
  *  i metodi arrow possono essere usati per tenere le tue funzioni concise
  *  le classi sono usate per definire i componenti in React estendendoli

Ha senso prendere una pausa in questo momento. Ripassa con la mente quanto hai appreso. Puoi sperimentare con il codice sorgente scritto.

Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/rwieruch/hackernews-client/tree/4.1).
