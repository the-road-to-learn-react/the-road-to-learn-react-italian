# Introduzione a React

Questo capitolo è un'introduzione a React, una libreria JavaScript per il rendering di applicazioni single-page e mobile, dove spiego perché i programmatori dovrebbero considerare di aggiungere la libreria React alla loro cassetta degli attrezzi. Ci immergeremo nell'ecosistema React, creando la nostra prima applicazione React da zero, senza configurazioni. Lungo la strada introdurremo JSX, la sintassi per React, e ReactDOM, così da darti un'idea degli utilizzi pratici di React nello sviluppo di moderne applicazioni web.

## Ciao, mi chiamo React.

Le applicazioni single-page ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) sono diventate sempre più popolari negli ultimi anni, da quando framework come Angular, Ember e Backbone, hanno permesso ai programmatori JavaScript di sviluppare applicazioni web moderne, utilizzando tecniche oltre al vanilla JavaScript e jQuery. I tre menzionati sono stati tra i primi per SPA, ognuno uscito tra il 2010 e il 2011, ma ci sono molte altre opzioni per lo sviluppo di applicazioni single-page. La prima generazione di framework SPA è arrivata al livello enterprise, quindi questi sono più rigidi. React, d'altro canto, rimane una libreria innovativa che è stata adottata da molti leader tecnologici quali [Airbnb, Netflix, e Facebook](https://github.com/facebook/react/wiki/Sites-Using-React).

React è stato rilasciato dal team di sviluppo web di Facebook nel 2013 come libreria per lo sviluppo di view, il che lo rende la 'V' nel pattern [MVC](https://en.wikipedia.org/wiki/Model–view–controller) (model view controller). Come view, ti permette di effettuare il render di componenti come elementi visualizzabili in un browser, mentre il suo ecosistema permette di sviluppare applicazioni single-page complete. Mentre la prima generazione di framework, tentò di risolvere molti problemi in una volta, React è utilizzato solamente per il layer di view; nello specifico, è una libreria dove la view è una gerarchia di componenti componibili. Se non hai mai sentito prima MVC, non preoccuparti, ci serve solo a collocare React storicamente in un contesto, per le persone che vengono da altri linguaggi di programmazione.

In React il focus rimane sul layer di view fino a che altri concetti sono introdotti nell'applicazione. Questi sono i componenti fondamentali per una SPA, che sono essenziali per sviluppare un'applciazione matura. Questo porta due vantaggi:

* Puoi imparare gli elementi essenziali uno alla volta, senza dover capire tutto insieme. In contrasto, i framework SPA ti offrono ognuno di questi elementi dall'inizio. Questo libro pone il focus su React come primo tassello. Altri argomenti arriverrano in futuro.

* Tutti questi elementi essenziali sono intercambiabili, il che rende l'ecosistema attorno React altamente innovativo. Più soluzioni possono competere tra loro e tu puoi scegliere la soluzione che più ti convince per ogni sfida in cui ti imbatterai.

React è una delle scelte migliori per realizzare applicazioni web moderne ma ripeto, offre solo il layer di vista, è con l'ecosistema che lo circonda che diventa un framework flessibile e con parti intercambiabili. React ha un'API snella, un ecosistema robusto e in evoluzione, e una grande community.

### Esercizi

Se ti interessa sapere il perché ho scelto React, o dare un'occhiata più approfondita agli argomenti menzionati sopra, questi articoli forniscono una prospettiva più approfondita:

* [Perché sono passato da Angular a React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* [L'ecosistema flessibile di React](https://www.robinwieruch.de/essential-react-libraries-framework/)
* [Come imparare un framework](https://www.robinwieruch.de/how-to-learn-framework/)

## Requisiti

Per seguire questo libro dovresti aver familiarità con le basi dello sviluppo web, es. saper usare HTML, CSS e JavaScript. Ha anche senso capire come funzionano le [API](https://www.robinwieruch.de/what-is-an-api-javascript/), considerato che saranno trattate a fondo. Inoltre, ti invito ad entrare a far parte del [Gruppo Slack](https://slack-the-road-to-learn-react.wieruch.com/) ufficiale, per far parte di una community su React in crescita, da cui puoi imparare e/o aiutare altri.

### Editor e Terminale

Per le lezioni avrai bisogno di un editor di testo o di un IDE e di un terminale (tool a linea di comando). Ho fornito [una guida di setup](https://www.robinwieruch.de/developer-setup/) se dovessi aver bisogno di un aiuto ulteriore. Opzionalmente consiglio di tenere i tuoi progetti su GitHub durante lo svolgimento degli esercizi del libro. C'è una [breve guida](https://www.robinwieruch.de/git-essential-commands/) sul come utilizzare questi tool. Github ha un eccellente controllo di versione, così che tu possa vedere quali modifiche sono state fatte se fai un errore, o se semplicemente vuoi un modo più diretto per seguire gli sviluppi.

### Node e NPM

Infine, avrai bisogno di installare [node ed npm](https://nodejs.org/en/). Entrambi sono utilizzati per gestire le librerie di cui avrai bisogno lungo la strada. In questo libro installerai package node esterni tramite npm (node package manager). Questi package node possono essere librerie o interi framework.

Puoi verificare le tue versioni di node e npm dalla linea di comando. Se non ottieni alcun output nel terminale, avrai prima bisogno di installare node ed npm. Queste sono le mie versioni al momento della scrittura di questo libro:

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v10.11.0
npm --version
*v6.4.1
~~~~~~~~

Il restante contenuto di questa sezione è un crash course su node e npm. Non è esaustivo, ma coprirà tutti gli strumenti necessari. Se hai già famigliarità con entrambi puoi passare alla prossima sezione.

**node package manager** (npm) ti permette di installare pacchetti node esterni dalla linea di comando. Questi pacchetti possono essere un insieme di funzioni di utilità, librerie, interi framework, e sono le dipendenze della tua applicazione. Puoi installare questi pacchetti nella tua cartella di package node globale o in quella locale al progetto.

I pacchetti node globali sono accessibili ovunque nel terminale, e necessitano di essere installati una sola volta. Puoi installare un pacchetto in modo globale digitando nel terminale:

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

Il flag `-g` istruisce npm ad installare il pacchetto in modalità globale. I pacchetti locali sono invece usati dalla tua applicazione di default. Per i nostri scopi installeremo React nella cartella locale, da terminale, con il comando:

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

Il pacchetto installato apparirà automaticamente nella cartella *node_modules/* e nel file *package.json* insieme alle altre dipendenze.

Per inizializzare la cartella *node_modules/* e il file *package.json* per il tuo progetto, usa il comando npm seguente. Poi, potrai installare nuovi pacchetti locali via npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

Il flag `-y` utilizza tutti i default per il tuo file *package.json*. Dopo aver inizializzato il tuo progetto npm, sei pronto per installare nuovi pacchetti con `npm install <package>`.

Il file *package.json* ti permette di condividere il tuo progetto con altri programmatori senza dover condividere anche tutti i pachetti node. Conterrà tutte le reference a tutti i pacchetti utilizzati nel tuo progetto, che sono chiamati **dipendenze** (dependencies). Altri utenti possono copiare un progetto senza le dipendenze usando le reference nel *package.json* per installarle facilmente con il comando `npm install`. Lo script `npm install` prenderà tutte le dipendenze elencate nel file *package.json* e le installerà nella cartella *node_modules/*.

Infine, c'è ancora un comando di npm da trattare:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

Il flag `--save-dev` indica che il pacchetto node è utilizzato solo nell'ambiente di sviuppo, che significa che non sarà utilizzato quando l'applicazione sarà deployata su un server o usata in produzione. È utile per testare un'applicazione usando un pacchetto node ma escludendolo dal tuo ambiente di produzione.

Alcuni di voi potrebbero voler usare altri gestori di pacchetti per lavorare con i pacchetti node in una certa applicazione. **Yarn** è un gestore delle dipendenze che funziona in modo simile a
**npm**. Ha la sua lista di comandi, ma al contempo hai accesso allo stesso npm registry (stessi pacchetti). Yarn è stato creato per risolvere problemi che npm non poteva risolvere, ma entrambi i tool sono evoluti al punto che uno qualsiasi dei due è sufficiente.

### Esercizi:

* Fai il setup di un progetto npm usa e getta utilizzando il terminale:
  * Crea una nuova cartella con `mkdir <folder_name>`
  * Naviga nella cartella con `cd <folder_name>`
  * Esegui `npm init -y` o `npm init`
  * Installa un pacchetto locale come React con `npm install react`
  * Controlla il file *package.json* e la cartella *node_modules/*
  * Prova a disinstallare e reinstallare il pacchetto node *react*
* Leggi di più su [npm](https://docs.npmjs.com/)
* Leggi di più sul gestore di pacchetti [yarn](https://yarnpkg.com/en/docs/)

## Installazione

Ci sono molti approcci per iniziare con un'applicazione React. Il primo che esploreremo è attraverso una CND, acronimo per
[content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network). Non preoccuparti troppo delle CDN ora, perché non le useremo in questo libro, ma ha senso spiegarle brevemente. Molte aziende usano CDN per fare l'host di file pubblici per i loro utenti. Alcuni di questi file sono librerie come React, dal momento che la libreria React è un solo file Javascript di nome *react.js*.

Per iniziare con React usando una CDN, inserisci dei tag `<script>` nella pagina web HTML puntando agli url della CDN. Avrai bisogno di due librerie: *react* e *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script
  crossorigin
  src="https://unpkg.com/react@16/umd/react.development.js"
></script>

<script
  crossorigin
  src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"
></script>
~~~~~~~~

Puoi anche ottenere React nella tua applicazione inizializzandola come progetto node. Con un file *package.json* puoi installare *react* e *react-dom* dalla linea di comando. In questo caso dobbiamo inizializzare il progetto con npm usando `npm init -y` per creare il file *package.json*. Adesso puoi installare più pacchetti node con npm:

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

Questo approccio è usato spesso per aggiungere React ad applicazioni esistenti gestite con npm.

Potresti anche dover avere a che fare con [Babel](http://babeljs.io/) per rendere la tua applicazione a conoscenza del JSX (la sintassi di React) e di JavaScript ES6. Babel effettua il transpile del tuo codice--vale a dire, lo converte in vanilla JavaScript--così che la maggior parte dei browser moderni possano interpretare JavaScript ES6 e JSX. Per via di questo difficile setup, Facebook ha introdotto *create-react-app* come soluzione React a zero configurazione. La prossima sezione mostrerà come fare il setup della tua applicazione usando questo tool di boostrapping.

### Esercizi:

* Leggi di più sulle possibili modalità di [installazione di React](https://reactjs.org/docs/getting-started.html)

## Setup con zero configurazioni

In the Road to learn React, useremo [create-react-app](https://github.com/facebookincubator/create-react-app) per fare il bootstrap della nostrap applicazione. Questo è uno starter kit per React, a configurazione zero, introdotto da Facebook nel 2016. È [consigliato ai principianti dal 96% degli utenti React](https://twitter.com/dan_abramov/status/806985854099062785). Con *create-react-app* il tool e la configuazione evolvono in background, mentre il focus è sull'implementazione dell'applicazione.

Per iniziare, installa il pacchetto tra i tuoi pacchetti node globali, così da averlo disponibile nella linea di comando per fare il bootstrap di applicazioni React:

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

Puoi controllare la versione di *create-react-app*, per verificare che l'installazione sia avvenuta con successo, dalla linea di comando:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v2.0.2
~~~~~~~~

Ora sei pronto per iniziare con la tua prima applicazione React. Nel testo verrà chiamata *hackernews*, ma tu puoi scegliere il nome che preferisci. Dopodiché, naviga nella cartella del progetto:

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Ora puoi aprire l'applicazione in un editor di testo. Ti dovresti trovare la seguente struttura di directory, o una variante, in base alla versione di *create-react-app*:

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
    manifest.json
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    serviceWorker.js
~~~~~~~~

Segue un'analisi dettagliata delle directory e dei file creati:

* **README.md:** L'estensione .md indica che il file è un file di markdown. Il markdown è usato in quanto linguaggio di markup snello con sintassi di formattazione del testo. Il codice di molti progetti contiene un file *README.md* che fornisce le indicazioni principali sul progetto. Quando si mette il proprio progetto su una piattaforma come GitHub, il file *README.md* mostra informazioni circa il contenuto del repository. Siccome abbiamo utilizzato *create-react-app*, il nostro *README.md* dovrebbe essere lo stesso del [repository GitHub ufficiale di create-react-app](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** Questa cartella contiene tutti i pacchetti node che sono stati installati via npm. Siccome
abbiamo usato *create-react-app*, dovrebbe contenere già alcuni moduli node installati per noi. Toccheremo raramente questa cartella, visto che i pacchetti node sono generalmente installati e disinstallati dalla linea di comando attraverso npm.

* **package.json:** Questo file mostra una lista di pacchetti node utilizzati come dipendenze e altre configurazioni del progetto.

* **.gitignore:** Questo file mostra tutti i file e le directory che non devono essere aggiunte al repository git; sono quindi path/file presenti solo nel progetto locale. La cartella *node_modules/* ne è un esempio. È sufficiente condividere il file *package.json* con altri, così che essi possano installare le dipendenze con `npm install` senza dover condividere la directory di dipendenze.

* **public/:** Questa directory contiene file di sviluppo, come *public/index.html*. La index è mostrata all'indirizzo localhost:3000 mentre sviluppi la tua app. Il boilerplate di *create-react-app* si preoccuperà di fornire a questa index tutti gli script in *src/*.

* **build/** Questa direcyory è creata quando si effettua la build del progetto, tipicamente per la produzione. Quando si effettua la build del progetto per la produzione, tutti i sorgenti nelle cartelle *src/* e *public/* sono impacchettati e posti nella cartella build.

* **manifest.json** e **serviceWorker.js:** Questi file non saranno utilizzati per questo progetto, quindi possiamo ignorarli per ora.

All'inizio, tutto quello di cui hai bisogno è collocato nella cartella *src/*. Il focus principale è sul file *src/App.js* che è utilizzato per implementare componenti React. Sarà utilizzato per implementare la tua applicazione, ma più tardi potresti voler dividere i tuoi componenti in svariati file, dove ogni file contiene uno o più componenti.

Inoltre, troverai un file *src/App.test.js* per i test, e un entry point al mondo React in *src/index.js*. Conoscerai più a fondo entrambi i file nei capitoli successivi. Ci sono anche i file *src/index.css* e *src/App.css* per stilizzare l'applicazione e i singoli componenti, che attualmente contengono degli stili di default. Modificheremo anche questi più avanti.

L'applicazione *create-react-app* è un progetto npm che puoi usare per installare e disinstallare pacchetti node. È fornito con i seguenti script per la linea di comando:

{title="Command Line",lang="text"}
~~~~~~~~
# Runs the application in http://localhost:3000
npm start

# Runs the tests
npm test

# Builds the application for production
npm run build
~~~~~~~~

Gli script sono definiti nel *package.json*, e la nostra applicazione React di prova è pronta. I seguenti esercizi ti permetteranno finalmente di eseguire l'applicazione in un browser.

### Esercizi:

* Lancia l'applicazione con `npm start` e provala nel browser (fermala digitando Control + C)
* Lancia lo script `npm test`
* Lancia lo script `npm run build` e verifica che una cartella *build/* è stata aggiunta al progetto (puoi rimuoverla subito dopo, ma nota che questa cartella può essere usata successivamente per effettuare il [deploy della tua applicazione](https://www.robinwieruch.de/deploy-applications-digital-ocean/))
* Prendi familiarità con la struttura delle directory
* Controlla il contenuto dei file
* Leggi di più sugli [script npm e create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduzione a JSX

Ora inizieremo a conoscere JSX, la sintassi di React. Come menzionato prima, *create-react-app* ha già effettuato il bootstrap di un'applicazione base per noi, e tutti i file sono forniti con una loro implementazione di default. Per ora, l'unico file che andremo a modificare è *src/App.js*.

{title="src/App.js",lang="javascript"}
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
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Non preoccuparti se ti senti confuso dalle istruzioni di import/export e dalla dichiarazione della classe per ora. Queste sono funzionalità di JavaScript ES6 che vedremo in un capitolo successivo.

Nel file dovresti vedere un **componente React implementato con una classe ES6** dal nome App. È in sostanza la dichiarazione di un componente. Dopo che hai dichiarato un componente, puoi usarlo come un elemento qualsiasi, ovunque nell'applicazione. Questo produrrà un'**istanza** del tuo **componente** o, in altre parole, il componente verrà istanziato.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// component declaration
class App extends Component {
  ...
}

// component usage (also called instantiation for a class)
// creates an instance of the component
<App />
~~~~~~~~

L'**elemento** restituito è specificato nel metodo `render()`. I componenti che hai istanziato prima sono fatti da elementi, quindi è importante capire la differenza tra componente, istanza di un componente, ed elemento.

Dovresti capire dove il componente App è istanziato, altrimenti non vedresti l'output renderizzato nel browser. Il componente App è solo la dichiarazione, ma non l'utilizzo. Puoi istanziare il componente ovunque nel tuo JSX con `<App />`. Vedremo dopo dove questo avviene nella nostra applicazione.

Il contenuto del blocco render potrebbe sembrare simile a dell'HTML, ma in realtà è JSX. JSX ti permette di mischiare HTML e JavaScript. E' molto potente, ma può essere disorientante quando sei abituato a vedere i due linguaggi separati. È una buona idea iniziare usando HTML di base nel tuo JSX. Apri il file `App.js` e rimuovi tutto il codice HTML non necessario come mostrato:

{title="src/App.js",lang="javascript"}
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

Adesso stiamo restituendo solo HTML nel mostro metodo `render()`, senza alcun JavaScript. Definiamo il valore "Welcome to the Road to learn React" in una variabile. Una variabile in JSX si utilizza tramite parentesi graffe.

{title="src/App.js",lang="javascript"}
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

Avvia l'applicazione da linea di comando con `npm start` per verificare le modifiche che abbiamo introdotto.

Potresti aver notato l'attributo `className`. Questo si riflette nell'attributo HTML standard `class`. JSX ha sostituito alcuni attributi HTML interni, ma puoi trovare tutti gli [attributi HTML supportati nella documentazione React](https://reactjs.org/docs/dom-elements.html#all-supported-html-attributes), dove tutti seguono la convenzione camelCase. Durante il tuo apprendimento di React aspettati di imbatterti in altri attributi specifici di JSX.

### Esercizi:

* Definisci più variabili e renderizzale nel JSX
  * Utilizza un oggetto complesso per rappresentare un utente con nome e cognome
  * Renderizza le proprietà dell'utente nel JSX
* Leggi di più sul [JSX](https://reactjs.org/docs/introducing-jsx.html)
* Leggi di più su [componenti React, elementi ed istanze](https://reactjs.org/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const e let

Nota come abbiamo dichiarato la variabile `helloWorld` con la parola chiave `var`. JavaScript ES6 ci fornisce due modi in più per dichiarare variabili: `const` e `let`. In JavaScript ES6, raramente troverai più `var`. Una variabile dichiarata con `const` non può essere riassegnata o ridichiarata, e non può essere cambiata o modificata. Una volta che la variabile è assegnata, non la puoi cambiare:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

Diversamente, una variabile dichiarata con `let` può essere modificata:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

**CONSIGLIO:** Dichiara le variabili con `let` se pensi che vorrai riassegnarle in seguito.

Nota che una variabile dichiarata direttamente con `const` non può essere modificata. Però, quando la variabile è un array o un oggetto, il valore che contiene può essere aggiornato tramite mezzi indiretti:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

Ci sono varie opinioni sul quando utilizzare *const* e quando utilizzare *let*. Consiglierei di usare `const` ovunque possibile per mostare l'intento di mantenere la struttura dati immutabile, così che rimanga da preoccuparsi solo dei valori contenuti in oggetti ed array. Il concetto di immutability è abbracciato dall'ecosistema React, quindi `const` dovrebbe essere la scelta di default quando si parla di dichiarare una variabile, sebbene non sia davvero immutabile, ma è più un assegnare le variabili una sola volta. Questo sottolinea l'intento di non voler cambiare (riassegnare) la variabile nonostante il suo contenuto possa essere alterato indirettamente.

{title="src/App.js",lang="javascript"}
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

Nella nostra applicazione useremo `const` e `let` anziché `var` nel resto del libro.

### Esercizi:

* Leggi di più su [const di ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* Leggi di più su [let di ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* Capire le strutture dati immutabili:
  * Perché hanno senso in programmazione?
  * Perché sono abbracciate da React e dal suo ecosistema?

## ReactDOM

Il componente App è utilizzato nel nostro entry-point al mondo React: il file *src/index.js*.

{title="src/index.js",lang="javascript"}
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

`ReactDOM.render()` usa un nodo del DOM nel tuo HTML da sostituire con del JSX. È un modo per integrare React in una qualsiasi applicazione facilmente, ed è possibile utilizzare `ReactDOM.render()` più volte nell'applicazione. È possibile usarlo per fare il bootstrap di semplice sintassi JSX, di un componente React, di componenti React multipli, o di un'intera applicazione. In un'applicazione React pura tipicamente è utilizzato solo una volta per fare il bootstrap dell'intero albero dei componenti.

`ReactDOM.render()` si aspetta due argomenti. Il primo serve a renderizzare il JSX, il secondo specifica il posto dove l'applicazione React si deve agganciare all'HTML, e nel nostro caso si aspetta un elemento con `id='root'` nel file *public/index.html*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

Durante l'inizializzazione, `ReactDOM.render()` prende come parametro il componente App, sebbene gli si possa passare anche semplice JSX. Non richiede necessariamente l'istanza di un componente.

### Esercizi:

* Apri il file *public/index.html* per vedere dove l'applicazione React si aggancia al tuo HTML
* Leggi di più su come [renderizzare elementi con React](https://reactjs.org/docs/rendering-elements.html)

## Hot Module Replacement

L'Hot Module Replacement è un tool che può essere utilizzato nel file *src/index.js* per migliorare la tua esperienza come sviluppatore. Di default, *create-react-app* forzerà il browser a fare il refresh della pagina ogniqualvolta venga fatta una modifica al codice sorgente dell'applicazione. Provalo cambiando il valore della variabile `helloWorld` nel file *src/App.js*, che dovrebbe forzare il browser a refreshare la pagina. Detto questo, c'è un modo migliore di gestire le modifiche al codice durante lo sviluppo.

L'Hot Module Replacement (HMR) è un tool per ricaricare la tua applicazione nel browser senza il refresh dell'intera pagina. Puoi attivarlo in *create-react-app* aggiungendo la seguente configurazione nel file *src/index.js*:

{title="src/index.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

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

Prova a cambiare ancora il valore della variabile `helloWorld` nel file *src/app.js*. Il browser non dovrebbe effettuare il refresh, ma l'applicazione dovrebbe ricaricare e mostrare l'output corretto. L'HMR porta moltplici vantaggi:

Immagina che stai effettuando il debug del tuo codice con delle istruzioni `console.log()`. Queste istruzioni rimarranno nella tua console da sviluppatore anche quando modificherai il tuo codice, perché il browser non effettuerà più il refresh della pagina. In un'applicazione di dimensioni importanti i refresh delle pagine impattano sulla produttività; l'HMR rimuove questo ostacolo eliminando l'accumularsi di tempo perso dal browser per ricaricare.

Il vantaggio più utile dell'HMR è che puoi mantenere lo stato dell'applicazione dopo il reload della stessa. Per esempio, immagina di avere una finestra di dialogo o uno wizard nella tua applicazione, formata da più passi, e di essere al passo 3. Senza l'HMR, quando effettui una modifica al codice, il browser refresha la pagina. A questo punto dovresti riaprire la finestra di dialogo e navigare dal passo 1 al passo 3 ogni volta. Con l'HMR la tua finestra di dialogo rimarrà aperta al punto 3, così che tu possa continuare ad effettuare il debug dall'esatto punto in cui stavi lavorando. Con il tempo risparmiato dal caricamento della pagina l'HMR diventa un prezioso strumento per gli sviluppatori React.

### Esercizi:

* Cambia il codice di *src/App.js* un po' di volte per vedere l'HMR in azione
* Guarda i primi dieci minuti di [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) di Dan Abramov

## JavaScript complesso nel JSX

Finora, abbbiamo renderizzato poche variabili primitive nel nostro JSX. Ora renderizzeremo una lista di oggetti. La lista conterrà dati di esempio all'inizio, ma più tardi imparerai a fare il fetch di dati da un'API esterna.

Innanzitutto definiamo la lista di oggetti:

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://redux.js.org/',
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

Questi dati di esempio rappresentano le informazioni di cui faremo il fetch da un'API in seguito. Ogni oggetto in questa lista ha un titolo, un'url, un autore, un identificativo, dei punti (che indicano quanto è popolare un articolo) e un contatore di commenti.

Adesso possiamo usare la [funzionalità built-in di JavaScript map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) nel JSX, la quale itera su una lista di oggetti per mostrarne attributi specifici. Usiamo ancora le parentesi graffe per incapsulare espressioni JavaScript nel JSX:

{title="src/App.js",lang="javascript"}
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

Utilizzare JavaScript assieme all'HTML nel JSX è uno strumento molto potente. Per un altro compito avresti potuto dover usare `map` per convertire una lista di oggetti in un'altra lista. Questa volta abbiamo utilizzato `map` per convertire una lista di oggetti in elementi HTML.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const array = [1, 4, 9, 16];

// pass a function to map
const newArray = array.map(function (x) { return x * 2; });

console.log(newArray);
// expected output: Array [2, 8, 18, 32]
~~~~~~~~

Finora, solamente il `title` è mostrato per ogni oggetto. Sperimentiamo con più di una proprietà dell'oggetto:

{title="src/App.js",lang="javascript"}
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

Nota come la funzione `map` è in ordine nel nostro JSX. Ogni proprietà dell'oggetto è mostrata con un tag `<span>` e la proprietà url dell'oggetto è nell'attributo `href` del tag `<a>`.

React mostrerà ogni oggetto, ma possiamo fare ancora di più per aiutare React a rendere al massimo del suo potenziale. Assegnando un attributo key ad ogni elemento della lista, React può identificare gli oggetti modificati quando la lista cambia. Questi oggetti di esempio hanno un identificativo:

{title="src/App.js",lang="javascript"}
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

Assicurati che l'attributo key sia un identificativo stabile. Evita di utilizzare l'indice dell'oggetto nell'array, perché l'indice dell'array non è stabile. Se la lista cambia il suo ordine, per esempio, React non sarà in grado di identificare gli oggetti adeguatamente.

{title="src/App.js",lang="javascript"}
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

Avvia l'app nel browser e dovresti vedere entrambi gli elementi della lista renderizzati.

### Esercizi:

* Leggi di più su [liste React e chiavi](https://reactjs.org/docs/lists-and-keys.html)
* Riassunto delle [funzionalità standard degli array in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/)
* Utilizza più espressioni JavaScript nel tuo JSX

## ES6 Arrow Functions

JavaScript ES6 ha introdotto le espressioni arrow functions, che sono più concise delle classiche espressioni di dichiarazione di funzione.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function declaration
function () { ... }

// arrow function declaration
() => { ... }
~~~~~~~~

Puoi rimuovere le parentesi da un'espressione arrow function se ha solo un argomento, ma devi mantenerle se ne possiede multipli:

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

Puoi anche scrivere funzioni `map` più concise con le arrow function di ES6:

{title="src/App.js",lang="javascript"}
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

Puoi inoltre rimuovere il *corpo della funzione*, le parentesi graffe. In un *corpo conciso* è aggiunto un return implicito; dunque, puoi rimuovere il `return`. Questo succederà spesso in questo libro, quindi assicurati di capire la differenza tra un corpo della funzione e uno conciso nel contesto di una arrow function.

{title="src/App.js",lang="javascript"}
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

Il tuo JSX dovrebbe apparire più conciso e leggibile adesso, con l'omissione dell'istruzione `function`, le parentesi graffe, e l'istruzione return.

### Esercizi:

* Leggi di più sulle [arrow function di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## Classi ES6

JavaScript ES6 ha introdotto le classi, che sono usate comunemente nei linguaggi di programmazione orientati agli oggetti. JavaScript, sempre flessibile nei suoi paradigmi di programmazione, permette alla programmazione funzionale e a quella orientata agli oggetti di funzionare fianco a fianco.

Mentre React abbraccia la programmazione funzionale, es. strutture dati immutabili e composizione di funzioni, le classi sono utilizzate per dichiarare componenti ES6. React mischia le parti buone di entrambi i paradigmi di programmazione.

Considera la seguente classe Developer per esaminare una classe JavaScript ES6 che non sia un componente.

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

Una classe ha un costruttore per renderla instanziabile. Il costruttore prende in input degli argomenti e li assegna all'istanza della classe. Una classe può definire funzioni. Poiché la funzione è associata alla classe, è chiamata metodo, o metodo di classe.

La classe Developer è solo la dichiarazione della classe che usiamo qui, poiché puoi creare più istanze di una classe invocandola. È simile ai componenti dichiarati tramite classe ES6, che hanno una dichiarazione, ma va utilizzata altrove per istanziarla:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React utilizza classi JavaScript ES6 per creare componenti, che hai già visto almeno una volta fino a qui:

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

Quando dichiari il componente App viene esteso da un altro componente. Nella programmazione orientata agli oggetti, il termine "estendere" si riferisce al principio di ereditarietà, che significa quella funzionalità può essere passata da una classe ad un'altra. La classe App estende la classe Component, il che significa eredita le funzionalità della classe Component. La classe Component è utilizata per estendere classi basiche ES6 in classi Component ES6. Ha quindi tutti le funzionalità di cui un componente in React ha bisogno. Il metodo render è una funzione che hai già utilizzato e ne è un esempio. Imparerai altri metodi della classe Component più avanti.

La classe `Component` incapsula tutti i dettagli implementativi di un componente React, il che permette agli sviluppatori di usare le classi come componenti in React.

I metodi esposti da un `Component` React sono la sua interfaccia pubblica. Uno di questi metodi deve essere ridefinito, mentre gli altri non ne hanno bisogno. Imparerai queste cose quando discuteremo i metodi di lifecycle più avanti. Il metodo `render()` deve essere ridefinito, perché definisce l'output di un `Component` React, quindi è necessario. Queste sono le basi delle classi ES6 di JavaScript, e di come sono utilizzate in React per estenderle ai componenti.

### Esercizi:

* Leggi di più sulle [basi di JavaScript prima di imparare React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/)
* Leggi di più sulle [classi di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

Congratulazioni, hai imparato come si fa il bootstrap della tua prima applicazione React! Ricapitoliamo:

* **React**
  * create-react-app fa il bootstrap di un'applicazione React
  * JSX mischia HTML e JavaScript per definire l'output di componenti React nei loro metodi render
  * Componenti, instanze e elementi sono cose differenti in React
  * `ReactDOM.render()` è l'entry point di un'applicazione React per agganciarsi a React nel DOM
  * Funzionalità built-in di JavaScript possono essere usate nel JSX
  * map può essere usato per renderizzare una lista di oggetti come elementi HTML
* **ES6**
  * Dichiarazioni di variabile con `const` e `let` possono essere usate per use case specifiche
  * Il maggior utilizzo di const su let nelle applicazioni React
  * Le arrow function possono essere usate per mantenere le proprie funzioni concise
  * Le classi sono usate per definire componenti in React estendendo la classe Component

Ora che hai completato il primo capitolo, è consigliabile sperimentare con il codice sorgente scritto fino a qui e capire quali modifiche puoi apportare per conto tuo. Puoi trovare i sorgenti nel
[repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.1).
