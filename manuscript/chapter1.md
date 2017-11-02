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

### Exercises:

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

* **README.md:** The .md extension indicates that the file is a markdown file. Markdown is used as a lightweight markup language with plain text formatting syntax. Many source code projects come with a *README.md* file to give you initial instructions about the project. When pushing your project to a platform such as GitHub eventually, the *README.md* file will show its content prominently when you access the repository. Because you have used *create-react-app*, your *README.md* should be the same as shown in the official [create-react-app GitHub repository](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** The folder has all the node packages that were and are installed via npm. Since you have used *create-react-app*, there should be already a couple of node modules installed for you. Usually you will never touch this folder, but only install and uninstall node packages with npm from the command line.

* **package.json:** The file shows you a list of node package dependencies and other project configuration.

* **.gitignore:** The file indicates all files and folders that shouldn't be added to your remote git repository when using git. They should only live in your local project. The *node_modules/* folder is such a use case. It is sufficient to share the *package.json* file with your peers to enable them to install all dependencies on their own without sharing the whole dependency folder.

* **public/:** The folder holds all your files when building your project for production. Eventually all your written code in the *src/* folder will be bundled into a couple of files when building your project and placed in the public folder.

After all, you don't need to touch the mentioned files and folders. In the beginning everything you need is located in the *src/* folder. The main focus lies on the *src/App.js* file to implement React components. It will be used to implement your application, but later you might want to split up your components into multiple files whereas each file maintains one or a few components on its own.

Additionally, you will find a *src/App.test.js* file for your tests and a *src/index.js* as entry point to the React world. You will get to know both files in a later chapter. In addition, there is a *src/index.css* and a *src/App.css* file to style your general application and your components. They all come with default style when you open them.

The *create-react-app* application is a npm project. You can use npm to install and uninstall node packages to your project. Additionally it comes with the following npm scripts for your command line:

{title="Command Line",lang="text"}
~~~~~~~~
// Runs the application in http://localhost:3000
npm start

// Runs the tests
npm test

// Builds the application for production
npm run build
~~~~~~~~

The scripts are defined in your *package.json*. Your boilerplate React application is bootstrapped now. The exciting part comes in the exercises to finally run your bootstrapped application in the browser.

### Exercises:

* `npm start` your application and visit the application in your browser
* run the interactive `npm test` script
* check the content of your *public/* folder, run the `npm run build` script and verify that files were added to the folder (you can remove these files again, but they don't do any harm)
* make yourself familiar with the folder structure
* make yourself familiar with the content of the files
* read more about [the npm scripts and create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduction to JSX

Now you will get to know JSX. It is the syntax in React. As mentioned before, *create-react-app* has already bootstrapped a boilerplate application for you. All files come with default implementations. Let's dive into the source code. The only file you will touch in the beginning will be the *src/App.js* file.

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

Don't let yourself get confused by the import/export statements and class declaration. These features are already JavaScript ES6. We will revisit those in a later chapter.

In the file you have an **React ES6 class component** with the name App. It is a component declaration. Basically after you have declared a component, you can use it as element everywhere in your application. It will produce an **instance** of your **component** or in other words: the component gets instantiated.

The **element** it returns is specified in the `render()` method. Elements are what components are made of. It is useful to understand the differences between component, instance and element.

Pretty soon, you will see where the App component is instantiated. Otherwise you wouldn't see the rendered output in the browser, would you? The App component is only the declaration, but not the usage. You would instantiate the component somewhere in your JSX with `<App />`.

The content in the render block looks pretty similar to HTML, but it's JSX. JSX allows you to mix HTML and JavaScript. It's powerful yet confusing when you are used to separate your HTML and JavaScript. That's why a good starting point is to use basic HTML in your JSX. In the beginning, remove all the distracting content in the file.

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

Now, you only return HTML in your `render()` method without JavaScript. Let's define the "Welcome to the Road to learn React" as a variable. A variable can be used in your JSX by using curly braces.

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

It should work when you start your application on the command line with `npm start` again.

Additionally you might have noticed the `className` attribute. It reflects the standard `class` attribute in HTML. Because of technical reasons, JSX had to replace a handful of internal HTML attributes. You can find all of the [supported HTML attributes in the React documentation](https://facebook.github.io/react/docs/dom-elements.html). They all follow the camelCase convention. On your way to learn React, you will come across some more JSX specific attributes.

### Exercises:

* define more variables and render them in your JSX
  * use a complex object to represent an user with a first name and last name
  * render the user properties in your JSX
* read more about [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* read more about [React components, elements and instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const and let

I guess you noticed that we declared the variable `helloWorld` with a `var` statement. JavaScript ES6 comes with two more options to declare your variables: `const` and `let`. In JavaScript ES6, you will rarely find `var` anymore.

A variable declared with `const` cannot be re-assigned or re-declared. It cannot get mutated (changed, modified). You embrace immutable data structures by using it. Once the data structure is defined, you cannot change it.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

A variable declared with `let` can get mutated.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

You would use it when you would need to re-assign a variable.

However, you have to be careful with `const`. A variable declared with `const` cannot get modified. But when the variable is an array or object, the value it holds can get updated. The value it holds is not immutable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

But when to use each declaration? There are different opinions about the usage. I suggest using `const` whenever you can. It indicates that you want to keep your data structure immutable even though values in objects and arrays can get modified. If you want to modify your variable, you can use `let`.

Immutability is embraced in React and its ecosystem. That's why `const` should be your default choice when you define a variable. Still, in complex objects the values within can get modified. Be careful about this behavior.

In your application, you should use `const` over `var`.

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

### Exercises:

* read more about [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* read more about [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* research more about immutable data structures
  * why do they make sense in programming in general
  * why are they used in React and its ecosystem

## ReactDOM

Before you continue with the App component, you might want to see where it is used. It is located in your entry point to the React world: the *src/index.js* file.

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

Basically `ReactDOM.render()` uses a DOM node in your HTML to replace it with your JSX. That's how you can easily integrate React in every foreign application. It is not forbidden to use `ReactDOM.render()` multiple times across your application. You can use it at multiple places to bootstrap simple JSX syntax, a React component, multiple React components or a whole application. But in plain React application you will only use it once to bootstrap your whole component tree.

`ReactDOM.render()` expects two arguments. The first argument is JSX that gets rendered. The second argument specifies the place where the React application hooks into your HTML. It expects an element with an `id='root'`. You can open your *public/index.html* file to find the id attribute.

In the implementation `ReactDOM.render()` already takes your App component. However, it would be fine to pass simpler JSX as long as it is JSX. It doesn't have to be an instantiation of a component.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Exercises:

* open the *public/index.html* to see where the React applications hooks into your HTML
* read more about [rendering elements in React](https://facebook.github.io/react/docs/rendering-elements.html)

## Hot Module Replacement

There is one thing that you can do in the *src/index.js* file to improve your development experience as a developer. But it is optional and shouldn't overwhelm you in the beginning when learning React.

In *create-react-app* it is already an advantage that the browser automatically refreshes the page when you change your source code. Try it by changing the `helloWorld` variable in your *src/App.js* file. The browser should refresh the page. But there is a better way of doing it.

Hot Module Replacement (HMR) is a tool to reload your application in the browser. The browser doesn't perform a page refresh. You can easily activate it in *create-react-app*. In your *src/index.js*, your entry point to React, you have to add one little configuration.

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

That's it. Try again to change the `helloWorld` variable in your *src/App.js* file. The browser shouldn't perform a page refresh, but the application reloads and shows the correct output. HMR comes with multiple advantages:

Imagine you are debugging your code with `console.log()` statements. These statements will stay in your developer console, even though you change your code, because the browser doesn't refresh the page anymore. That can be convenient for debugging purposes.

In a growing application a page refresh delays your productivity. You have to wait until the page loads. A page reload can take several seconds in a large application. HMR takes away this disadvantage.

The biggest benefit is that you can keep the application state with HMR. Imagine you have a dialog in your application with multiple steps and you are at step 3. Basically it is a wizard. Without HMR you would change the source code and your browser refreshes the page. You would have to open the dialog again and would have to navigate from step 1 to step 3. With HMR your dialog stays open at step 3. It keeps the application state even though the source code changes. The application itself reloads, but not the page.

### Exercises:

* change your *src/App.js* source code a few times to see HMR in action
* watch the first 10 minutes of [Live React: Hot Reloading with Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) by Dan Abramov

## Complex JavaScript in JSX

Let's get back to your App component. So far you rendered some primitive variables in your JSX. Now you will start to render a list of items. The list will be sample data in the beginning, but later you will fetch the data from an external [API](https://www.robinwieruch.de/what-is-an-api-javascript/). That will be far more exciting.

First you have to define the list of items.

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

The sample data will reflect the data we will fetch later on from the API. An item in the list has a title, an url and an author. Additionally it comes with an identifier, points (which indicate how popular an article is) and a count of comments.

Now you can use the built-in JavaScript `map` functionality in your JSX. It enables you to iterate over your list of items to display them. Again you will use curly braces to encapsulate the JavaScript expression in your JSX.

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

Using JavaScript in HTML is pretty powerful in JSX. Usually you might have used `map` to convert one list of items to another list of items. This time you use `map` to convert a list of items to HTML elements.

So far, only the `title` will be displayed for each item. Let's display some more of the item properties.

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

You can see how the map function is simply inlined in your JSX. Each item property is displayed in a `<span>` tag. Moreover the url property of the item is used in the `href` attribute of the anchor tag.

React will do all the work for you and display each item. But you should add one helper for React to embrace its full potential and improve its performance. You have to assign a key attribute to each list element. That way React is able to identify added, changed and removed items when the list changes. The sample list items come with an identifier already.

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

You should make sure that the key attribute is a stable identifier. Don't make the mistake of using index of the item in the array. The array index isn't stable at all. For instance, when the list changes its order, React will have a hard time identifying the items properly.

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

You are displaying both list items now. You can start your app, open your browser and see both items of the list displayed.

### Exercises:

* read more about [React lists and keys](https://facebook.github.io/react/docs/lists-and-keys.html)
* recap the [standard built-in array functionalities in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* use more JavaScript expressions on your own in JSX

## ES6 Arrow Functions

JavaScript ES6 introduced arrow functions. An arrow function expression is shorter than a function expression.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

But you have to be aware of its functionalities. One of them is a different behavior with the `this` object. A function expression always defines its own `this` object. Arrow function expressions still have the `this` object of the enclosing context. Don't get confused when using `this` in an arrow function.

There is another valuable fact about arrow functions regarding the parenthesis. You can remove the parenthesis when the function gets only one argument, but have to keep them when it gets multiple arguments.

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

However, let's have a look at the `map` function. You can write it more concisely with an ES6 arrow function.

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

Additionally, you can remove the *block body*, meaning the curly braces, of the ES6 arrow function. In a *concise body* an implicit return is attached. Thus you can remove the return statement. That will happen more often in the book, so be sure to understand the difference between a block body and a concise body when using arrow functions.

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

Your JSX looks more concise and readable now. It omits the function statement, the curly braces and the return statement. Instead a developer can focus on the implementation details.

### Exercises:

* read more about [ES6 arrow functions](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 Classes

JavaScript ES6 introduced classes. A class is commonly used in object-oriented programming languages. JavaScript was and is very flexible in its programming paradigms. You can do functional programming and object-oriented programming side by side for their particular use cases.

Even though React embraces functional programming, for instance with immutable data structures, classes are used to declare components. They are called ES6 class components. React mixes the good parts of both programming paradigms.

Let's consider the following Developer class to examine a JavaScript ES6 class without thinking about a component.

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

A class has a constructor to make it instantiable. The constructor can take arguments to assign it to the class instance. Additionally a class can define functions. Because the function is associated with a class, it is called a method. Often it is referenced as a class method.

The Developer class is only the class declaration. You can create multiple instances of the class by invoking it. It is similar to the ES6 class component, that has a declaration, but you have to use it somewhere else to instantiate it.

Let's see how you can instantiate the class and how you can use its methods.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React uses JavaScript ES6 classes for ES6 class components. You already used one ES6 class component.

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

The App class extends from `Component`. Basically you declare the App component, but it extends from another component. What does extend mean? In object-oriented programming you have the principle of inheritance. It is used to pass over functionalities from one class to another class.

The App class extends functionality from the Component class. To be more specific, it inherits functionalities from the Component class. The Component class is used to extend a basic ES6 class to a ES6 component class. It has all the functionalities that a component in React needs to have. The render method is one of these functionalities that you have already used. You will learn about other component class methods later on.

The `Component` class encapsulates all the implementation details of a React component. It enables developers to use classes as components in React.

The methods a React `Component` exposes is the public interface. One of these methods has to be overridden, the others don't need to be overridden. You will learn about the latter ones when the book arrives at lifecycle methods in a later chapter. The `render()` method has to be overridden, because it defines the output of a React `Component`. It has to be defined.

Now you know the basics around JavaScript ES6 classes and how they are used in React to extend them to components. You will learn more about the Component methods when the book describes React lifecycle methods.

### Exercises:

* read more about [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

You have learned to bootstrap your own React application! Let's recap the last chapters:

* React
  * create-react-app bootstraps a React application
  * JSX mixes up HTML and JavaScript to define the output of React components in their render methods
  * components, instances and elements are different things in React
  * `ReactDOM.render()` is an entry point for a React application to hook React into the DOM
  * built-in JavaScript functionalities can be used in JSX
    * map can be used to render a list of items as HTML elements
* ES6
  * variable declarations with `const` and `let` can be used for specific use cases
    * use const over let in React applications
  * arrow functions can be used to keep your functions concise
  * classes are used to define components in React by extending them

It makes sense to take a break at this point. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far.

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.1).
