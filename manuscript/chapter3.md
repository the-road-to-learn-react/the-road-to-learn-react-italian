# Renderlo reale con le API

È arrivato il momento di rendere più reale l'applicazione attraverso un'API e abbandonare i dati di esempio. Se non hai famigliarità, ti consiglio di [leggere il mio articolo su come ho imparato a capire le API](https://www.robinwieruch.de/what-is-an-api-javascript/).

Per la nostra prima incursione in questo concetto useremo [Hacker News](https://news.ycombinator.com/), un affidabile aggregatore di news con argomento la tecnologia. In questo esercizio useremo le API di Hacker News per recuperare gli articoli di tendenza. Esistono le modalità API [base](https://github.com/HackerNews/API) e API [ricerca](https://hn.algolia.com/api) per recuperare i dati dalla piattaforma. La modalità ricerca ha senso per la nostra applicazione, perché vogliamo essere in grado di cercare tra gli articoli di Hacker News. Dai un'occhiata alla specifica delle API per capirne la struttura dati.

## Metodi di lifecycle

Potresti ricordarti che abbiamo menzionato i metodi di lifecycle nello scorso capitolo, come modo per agganciarsi nel ciclo di vita di un componente React. Questi possono essere usati in un componente implementato tramite classe ES6, non in un componente funzionale privo di stato. A parte il metodo `render()` ci sono altri metodi che possono essere sovrascritti in un componente implementato tramite una classe ES6. Tutti questi sono metodi di lifecycle.

Ne abbiamo già conosciuti due di questi metodi che possono essere utilizzati in un componente implementato tramite classe ES6:

* Il costruttore è chiamato solo una volta quando viene creata un'istanza del componente e inserita nel DOM. Il componente viene istanziato in un processo chiamato mounting del componente.
* Anche il metodo `render()` è chiamato durante il processo di mounting, ma anche quando il componente viene aggiornato. Ogni volta che lo stato o le props di un componente cambiano, il metodo `render()` viene chiamato.

Ci sono altri due metodi di lifecycle che vengono eseguito durante il mounting di un componente: `getDerivedStateFromProps()` e `componentDidMount()`. Il costruttore è il primo che viene chiamato, `getDerivedStateFromProps()` è chiamato prima del metodo `render()`, e `componentDidMount()` è chiamato dopo il metodo `render()`.

Nel complesso il processo di mounting è composto da quattro metodi di lifecycle, invocati nel seguente ordine:

* constructor()
* getDerivedStateFromProps()
* render()
* componentDidMount()

I metodi di lifecylce che vengono chiamati in aggiornamento di un componente, quando lo stato o le props cambiano, sono cinque e e vengono chiamati in quest'ordine:

* getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()

Infine c'è la fase di unmounting che ha solo un metodo di lifecylce: `componentWillUnmount()`.

Non hai bisogno di conoscere tutti i metodi di lifecycle dall'inizio, e anche in una grossa applicazione React ne userai probabilmente solo un paio oltre ai metodi `constructor()` e `render()`. Tuttavia è bene sapere che ogni metodo di lifecycle può essere usato per scopi specifici:

* **constructor(props)** è chiamato quando il componente viene inizializzato. In questo metodo puoi settare uno stato iniziale al componente e associare metodi di classe.

* **static getDerivedStateFromProps(props, state)** è chiamato prima del metodo `render()`, sia al mount iniziale, sia agli aggiornamenti successivi. Dovrebbe restituire un oggetto per aggiornare lo stato o null per non aggiornare niente. Esiste per quei **rari** casi dove lo stato dipendene dai cambiamenti delle props nel tempo. È importante sapere che questo è un metodo statico e non avrà quindi accesso all'istanza del componente.

* **render()** è un metodo di lifecycle obbligatorio che restutisce gli elementi in output al componente. Il metodo dovrebbe essere puro, quindi non dovrebbe modificare lo stato del componente. Prende in input props e state e restituisce un elemento.

* **componentDidMount()** è chiamato una volta, quando il mounting del componente è completato. Questo è il momento perfetto per eseguire una richiesta asincrono per effettuare il fetching di dati da un API. I dati recuperati sono salvati nello stato locale del componente per renderizzarli nel metodo `render()`.

* **shouldComponentUpdate(nextProps, nextState)** è chiamato ogni volta che di un componente viene aggiornato lo stato o le props. Si usa in applicazioni React mature per otimizzare le performance. In base ad un valore booleano che questo metodo di lifecycle restituisce, il componente ed i suoi figli verranno o no ri-renderizzati. Serve praticamente a prevenire in alcuni casi l'esecuzione del metodo render di un componente.

* **getSnapshotBeforeUpdate(prevProps, prevState)** è un metodo di lifecycle invocato prima che il più recente output renderizzato è applicato al DOM. In casi rari, il componente ha bisogno di catturare l'informazione dal DOM prima che sia potenzialmente cambiato. Questo metodo permette al component di farlo. Un altro metodo (`componentDidUpdate()`) riceverà qualsiasi valore restituito da `getSnapshotBeforeUpdate()` come parametro.

* **componentDidUpdate(prevProps, prevState, snapshot)** è un metodo di lifecycle invocato immediatamente dopo l'aggiornmento, ma non per il render iniziale. Puoi usarlo per eseguire operazioni sul DOM o per eseguire più richieste asincrone. Se il metodo implementa il metodo `getSnapshotBeforeUpdate()`, il valore che restituisce sarà ricevuto come parametro snapshot.

* **componentWillUnmount()** è chiamato prima che il componente venga distrutto. Puoi usare questo metodo di lifecycle per eseguire qualsiasi task di clean up.

Come avrai capito i metodi di lifecycle `constructor()` e `render()` sono i più comunemente usati nei componenti implementati come classi ES6. Il metodo `render()` è sempre obbligatorio per restituire l'istanza di un componente.

Infine, `componentDidCatch(error, info)` è stato introdotto in [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) come metodo per catturare gli errori nei componenti. Per esempio, la visualizzazione della lista di esempio nella nostra applicazione funziona, ma potrebbero esserci casi in cui una lista è salvata nello stato locale come `null` per errore (es. quando viene fatto il fetching della lista da un API esterna, ma la richiesta fallisce e la lista viene salvata nello stato locale come null). In questo caso diventa impossibile filtrare e trasformare la lista, poiché abbiamo null e non una lista vuota. Il componente si romperebbe e l'intera applicazione crasherebbe. Usando `componentDidCatch()` è possibile catturare l'errore, salvarlo nello stato locale e mostrare un messaggio all'utente.

### Esercizi:

* Leggi sui [metodi di lifecycle in React](https://reactjs.org/docs/react-component.html)
* Leggi sullo [stato e le sue relazioni con i metodi di lifecycle in React](https://reactjs.org/docs/state-and-lifecycle.html)
* Leggi sulla [gestione di errori nei componenti](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Fetch di dati

Adesso siamo pronti per recuperare i dati dall'API di Hacker News. C'era un metodo di lifecycle che è stato nominato per essere usato per il recupero di dati: `componentDidMount()`. Prima di usarlo, inizializziamo le costanti URL e i parametri di default per dividere gli endpoint delle URL delle richieste API in parti più piccole.

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-end-insert

...
~~~~~~~~

In JavaScript ES6 è possibile usare [template literals](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals) per la concatenazione di stringhe o interpolazione. La useremo per concatenare la URL per l'endpoint dell'API.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

Questo renderà la composizione delle nostre URL più flessibile in futuro. Qui sotto l'intero processo di recupero dati con ogni passaggio spiegato subito dopo.

{title="src/App.js",lang="javascript"}
~~~~~~~~
...

class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      result: null,
      searchTerm: DEFAULT_QUERY,
# leanpub-end-insert
    };

# leanpub-start-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  setSearchTopStories(result) {
    this.setState({ result });
  }

  componentDidMount() {
    const { searchTerm } = this.state;

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Per prima cosa abbiamo rimosso la lista di esempio, visto che andremo a recuperare la lista reale dalle API di Hacker News. Lo stato iniziale del componente avrà un risultato vuoto e una query di ricerca di default. La stessa query di ricerca di default è utilizzato nel campo di input del componente Search e nella prima richiesta.

Seconda cosa, useremo il metodo di lifecylce `componentDidMount()` per fare il fetching dei dati quando il componente è applicato al DOM. La prima richiesta utilizza la query di ricerca di default dello stato locale. Questo recupererà gli articoli su "redux", visto che questo è il parametro di default.

Terza cosa, l'API nativa fetch è utilizzata. Il template strings di JavaScript ES6 ci permette di comporre l'URL con `searchTerm`. L'URL è l'argomento della funzione nativa fetch. La risposta viene trasformata in un JSON, un passaggio obbligatorio in una chiamata fetch che restituisce un JSON come struttura dati, dopo di ché il risultato verrà salvato nello stato locale del componente. Se si verifica un errore durante la richiesta, la funzione eseguirà il blocco catch invece del blocco then.

Ricordati di effettuare il binding dei metodi del componente nel costruttore.

Adesso possiamo finalmente utilizzare i dati ricevuti invece della lista di esempio. Nota come il risultato non è semplicemente una lista di dati, [ma un oggetto complesso con metadati e una lista di hit che sono i nuovi articoli](https://hn.algolia.com/api). Puoi vedere l'output dello stato locale con un `console.log(this.state);` nel metodo `render()`.

Nel prossimo passaggio useremo il risultato per mostrarlo a video. Ma preverremo il rendering di alcunché se il risultato è null, come inizialmente. Una volta che la richiesta all'API ha avuto successo, il risultato sarà salvato nello stato e il componente App si re-renderizzerà con lo stato aggiornato.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;

    if (!result) { return null; }

# leanpub-end-insert
    return (
      <div className="page">
        ...
        <Table
# leanpub-start-insert
          list={result.hits}
# leanpub-end-insert
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Ora ricapitoliamo cosa succede durante il ciclo di vita del componente. Il componente è inizializzato dal costruttore, dopo di ché sarà renderizzato per la prima volta. Abbiamo evitato che mostrasse niente, perché result nello stato locale è null. È permesso restituire null per un componente per non mostrare niente. A seguire il metodo di lifecycle `componentDidMount()` effettua la richiesta di recupero dei dati asincronamente dall'API di Hacker News. Una volta che i dati sono arrivati, viene aggiornato lo stato locale del componente in `setSearchTopStories()`. Il ciclo di vita si attiva nuovamente perché lo stato è stato aggiornato. Il componente esegue di nuovo il metodo `render()`, ma questa volta con result popolato nello stato locale. Il componente e il componente Table sono mostrati con il loro contenuto.

Abbiamo utilizzato l'API nativa fetch supportata dalla maggior parte dei browser per effettuare una richiesta asincrona all'API. La configurazione di *create-react-app* si assicura che sia supportata da tutti i browser. Ci sono anche pacchetti node di terze parti che si possono usare per sostituire l'API nativa fetch: [axios](https://github.com/mzabriskie/axios). Useremo axios più tardi in questo libro.

In questo libro useremo la notazione concisa di JavaScript per i controlli di veridicità. Nell'esempio precedente `if (!result)` è stato utilizzato al posto di `if (result === null)`. La stessa cosa succederà per altri casi. Per esempio, `if (!list.length)` è utilizzato al posto di `if (list.length === 0)`, oppure `if (someString)` è utilizzato piuttosto che `if (someString !== '')`.

La lista di articoli dovrebbe adesso essere visibile nella nostra applicazione; purtroppo, abbiamo introdotto due regressioni. Primo, il bottone "Dismiss" è rotto, perché non conosce l'oggetto complesso result, ma opera ancora sulla lista semplice dei dati finti. Secondo, quando la lista è mostrata e si prova a cercare qualcos'altro, viene filtrata lato client, sebbene la prima richiesta è stata fatta cercando tra gli articoli sul server di Hacker News. Il comportamento ideale sarebbe di richiedere un altro oggetto come risultato all'API utilizzato il componente Search. Entrambe le regressioni saranno sistemate nei prossimi capitoli.

### Esercizi:

* Leggi sui [template literals di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* Leggi sull'[API nativa fetch](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* Leggi sul [fetching di dati in React](https://www.robinwieruch.de/react-fetching-data/)

## Gli Spread Operator di ES6

Il bottone "Dismiss" non funziona perché il metodo `onDismiss()` non è a conoscenza del complesso oggetto result. Conosce solo la vecchia lista semplice dello stato locale, ma non è più così. Cambione la logica per operare sull'oggetto result invece che sulla lista stessa.

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
# leanpub-start-insert
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

Nella `setState()`, result è ora un oggetto complesso, e la lista di articoli è solo una delle proprietà presenti nell'oggetto. Solo la lista sarà aggiornata quando un oggetto viene rimosso dall'oggetto result, però, mentre le altre proprietà rimarranno le stesse.

Potremmo ovviare a questo problema mutando la lista hits nell'oggetto risultato. Questo modo è mostrato solo per conoscenza, ma procederemo in un altro modo ancora.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// don't do this
this.state.result.hits = updatedHits;
~~~~~~~~

Come sappiamo React abbraccia le strutture dati immutabili, quindi non vogliamo mutare un oggetto (o mutare lo stato direttamente). Quello che vogliamo fare è generare un nuovo oggetto basandoci sulle informazioni date, così che nessun oggetto venga alterato in modo da mantenere le strutture dati immutabili. Restituiremo sempre un oggetto, ma senza alterare quello originale.

Per far questo useremo la funzione di JavaScript ES6 `Object.assign()`. Questa funzione prende un oggetto target come primo argomento. Tutti gli argomenti successivi sono oggetti source, che verranno mergiati nell'oggetto target. L'oggetto target può anche essere un oggetto vuoto. Questo rispetta l'immutabilità, perché nessun oggetto source viene mutato.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Gli oggetti source sovrascriveranno i precedenti oggetti di cui è stato fatto il merge con lo stesso nome di proprietà. Utilizziamo questo metodo nel metodo `onDismiss()`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: Object.assign({}, this.state.result, { hits: updatedHits })
# leanpub-end-insert
  });
}
~~~~~~~~

Questa è una soluzione, ma c'è un modo più semplice in JavaScript ES6 ed è chiamato *spread operator*. Lo spread operator è mostrato con tre puntini: `...` quando è utilizzato, ogni valore di un array o di un oggetto è copiato in un altro array o oggetto.

Esamineremo lo spread operator di ES6 per gli **array** anche se non ne abbiamo ancora bisogno:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

La variabile `allUsers` è un array completamente nuovo. Le altre variabili `userList` e `additionalUser` restano le stesse. È anche possibile fare il merge due array in un nuovo array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Diamo adesso un'occhiata allo spread operator per gli oggetti, che non è parte di JavaScript ES6. Fa parte di una [proposta per una versione successiva di JavaScript](https://github.com/sebmarkbage/ecmascript-rest-spread) che è già utilizzata dalla comunità React, ed è per questo che *create-react-app* incorpora la feature nella sua configurazione. Lo spread operator per gli oggetti funziona come quello per gli array di ES6, eccetto che è per oggetti. Ogni coppia chiave e valore è copiata nel nuovo oggetto:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

È possibile effettuare lo spread di più oggetti, come abbiamo visto nell'esempio per gli array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Questa feature può essere usata al posto di `Object.assign()`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: { ...this.state.result, hits: updatedHits }
# leanpub-end-insert
  });
}
~~~~~~~~

Il bottone "Dismiss" dovrebbe funzionare adesso, poiché il metodo `onDismiss()` è a conoscenza dell'oggetto complesso result e come aggiornarlo dopo che un elemento viene eliminato dalla lista.

### Esercizi:

* Leggi sul metodo [Object.assign() di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* Leggi sullo [spread operator di array e oggetti](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)

## Rendering condizionale

Il rendering condizionale viene di solito introdotto presto nello sviluppo di applicazione React, sebbene non l'abbiamo ancora usato nel nostro esempio. Succede quando decidi di renderizzare due possibili elementi, che di solito è una scelta tra renderizza un elemento o non renderizzare nulla. Il più semplice utilizzo di rendering condizionale può essere espresso da un'istruzione if-else nel JSX.

L'oggetto `result` nello stato locale del componente è `null` inizialmente. Fino ad ora, il componente App non ha restituito elementi quando `result` non è ancora arrivato dalle API. Questo è già un rendering condizione, poiché restituiamo prima l'output di un componente nel metodo `render()` per una certa condizione. Il componente App o non renderizza nulla o renderizza i suoi elementi.

Ma spingiamoci un passo oltre. Ha più senso racchiudere il componente Table, che è l'unico componente che dipende da `result`, in un rendering condizionale indipendente. Tutto il resto dovrebbe essere visualizzato, anche se non c'è ancora un `result`.Possiamo semplicemente usare un [operatore ternario](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) nel JSX:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
        </div>
# leanpub-start-insert
        { result
          ? <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
          : null
        }
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Questa è la seconda opzione per esprimere un rendering condizionale. Una terza opzione è l'operatore logico `&&`. In JavaScript un `true && 'Hello World'` è un'espressione che vale sempre 'Hello World'. Invece `false && 'Hello World'` vale sempre false.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
~~~~~~~~

Possiamo fare uso di questo comportamento anche in React. Se la condizione è vera, l'espressione dopo l'operatore logico `&&` farà parte dell'output. Se la condizione è falsa, React ignorerà l'espressione. Questo é applicabile anche al caso del rendering condizionale del componente Table, perché dovrebbe restituire una Table o nulla.

{title="src/App.js",lang="javascript"}
~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

Questi erano alcuni approcci per usare il rendering condizionale in React. Puoi leggere [più alternative in una lista esaustiva di esempi](https://www.robinwieruch.de/conditional-rendering-react/). Inoltre, conoscerai usi differente e quando applicarli.

Dovresti adesso essere in grado di vedere i dati recuperati dall'API nell'applicazione. Tutto eccetto Table è visualizzato quando il fetching dei dati non è ancora stato completato. Una volta che la richiesta recupera il risultato che viene salvato nello stato locale, il componente Table è visualizzato, poiché il metodo `render()` viene eseguito ancora, e la condizione del rendering condizione si risolve questa volta a favore della visualizzazione del componente Table.

### Esercizi:

* Leggi dei [differenti metodi per il rendering condizionale](https://www.robinwieruch.de/conditional-rendering-react/)
* Leggi sul [rendering condizionale in React](https://reactjs.org/docs/conditional-rendering.html)

## Ricerca lato client e lato server

Adesso, quando usiamo il componente Search, con il suo campo di input, questo filtrerà la lista. Questo accade lato client però. Noi vogliamo usare le API di Hacker News per cercare lato server. Altrimenti avremmo a che fare solo con la prima risposta API che facciamo in `componentDidMount()` con il parametro query di ricerca di default.

Possiamo definire un metodo `onSearchSubmit()` nel componente App che effettui il fetch dei risultati dall'API di Hacker News quando viene effettuata una ricerca nel componente Search.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-start-insert
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  onSearchSubmit() {
    const { searchTerm } = this.state;
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Il metodo `onSearchSubmit()` dovrebbe usare la stessa funzionalità del metodo di lifecycle `componentDidMount`, ma questa volta con una query di ricerca modificato dallo stato locale e non con quello di default. Quindi possiamo estrarre la funzionalità come un metodo riusabile della classe.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-start-insert
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }
# leanpub-end-insert

  componentDidMount() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...

  onSearchSubmit() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

Ora il componente Search ha bisogno di un bottone addizionale che scateni esplicitamente la richiesta. Altrimenti effettueremmo il fetch dei dati dall'API di Hacker News ogni volta che il valore del campo di input cambia. Vogliamo farlo esplicitamente in un handler `onClick()`.

In alternativa, potremmo effettuare il debounce (ritardare) la funzione `onChange()` e risparmiarci il bottone, ma aggiungerebbe più complessità, e noi vogliamo mantenere le cose semplici per ora.

Prima cosa, passiamo il metodo `onSearchSubmit()` al componente Search.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
# leanpub-start-insert
            onSubmit={this.onSearchSubmit}
# leanpub-end-insert
          >
            Search
          </Search>
        </div>
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
~~~~~~~~

Seconda, introduciamo un bottone nel componente Search. Il bottone avrà `type="submit"` and il form userà l'attributo `onSubmit` per passare il metodo `onSubmit()`. Possiamo usare la proprietà `children`, ma questa volta sarà utilizzata come contenuto del bottone.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) =>
  <form onSubmit={onSubmit}>
    <input
      type="text"
      value={value}
      onChange={onChange}
    />
    <button type="submit">
      {children}
    </button>
  </form>
# leanpub-end-insert
~~~~~~~~

Nel componente Table possiamo rimuovere la funzionalità di filtro, poiché non ci sarà più la possibilità di filtrare (ricerca) lato client. Non dimenticarti di rimuovere anche la funzione `isSearched()`, che non useremo più. Adesso result arriverà direttamente dall'API di Hacker News quando l'utente farà il click sul bottone "Search".

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        ...
        { result &&
          <Table
# leanpub-start-insert
            list={result.hits}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
      </div>
    );
  }
}

...

# leanpub-start-insert
const Table = ({ list, onDismiss }) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {list.map(item =>
# leanpub-end-insert
      ...
    )}
  </div>
~~~~~~~~

Se adesso provi ad effettuare una ricerca noterai il browser ricaricare. Questo è un comportamento nativo del browser per una callback submit in un form HTML. In React, ti imbatterai spesso nel metodo `preventDefault()` degli eventi per evitare questo comportamento nativo dei browser.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
onSearchSubmit(event) {
# leanpub-end-insert
  const { searchTerm } = this.state;
  this.fetchSearchTopStories(searchTerm);
# leanpub-start-insert
  event.preventDefault();
# leanpub-end-insert
}
~~~~~~~~

Dovresti essere in grado di cercare tra gli articoli di Hacker News ora. Abbiamo introdotto un'API reale e non ci dovrebbero essere più ricerche lato client.

### Esercizi:

* Leggi sui [synthetic events di React](https://reactjs.org/docs/events.html)
* Sperimenta con l'[API di Hacker News](https://hn.algolia.com/api)

## Fetch paginata

Dai un'occhiata più approfondita alla struttura dati e osserva come l'[API di Hacker News](https://hn.algolia.com/api) restituisce più di una lista di articoli. Precisamente restituisce una lista paginata. La property page, che è `0` nella prima risposta, può essere usata per fare il fetch di altre sottoliste paginate come risultato. Dobbiamo solo passare la pagina successiva con la stessa query di ricerca all'API.

Estendiamo le nostre costanti componibile dell'API così che possano affrontare dei dati paginati:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert
~~~~~~~~

Ora possiamo utilizzare la nuova costante per aggiungere un parametro page alla richiesta all'API:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

Il metodo `fetchSearchTopStories()` prenderà page come secondo argomento. Se il secondo argomento non viene fornito userà 0 come fallback per la richiesta inziale. Dunque i metodi `componentDidMount()` e `onSearchSubmit()` recupereranno la prima pagina alla prima richiesta. Ogni fetch addizionale effettuare il fetch della pagina successiva fornendola come secondo argomento.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
# leanpub-end-insert
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }

  ...

}
~~~~~~~~

L'argomento page utilizza un default di JavaScript ES6 per introdurre un fallback page `0` nel caso un argomento page definito venga fornito alla funzione.

Adesso possiamo usare la pagina corrente dalla risposta dell'API nel metodo `fetchSearchTopStories()`. Possiamo usare questo metodo in un bottone per ottenere più articoli con un handler sull'`onClick` del bottone. Utilizziamo Button per richiedere più dati paginati dall'API di Hacker News. Per fare questo definiremo un handler `onClick`, che prenderà la query di ricerca corrente e la pagina successiva (la pagina corrente + 1).

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
# leanpub-start-insert
    const page = (result && result.page) || 0;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-start-insert
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopStories(searchTerm, page + 1)}>
            More
          </Button>
        </div>
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Nel metodo `render()`, assicurati di mettere di default page a 0 quando non abbiamo ancora ottenuto un risultato. Ricorda, il metodo `render()` è chiamato prima che i dati siano recuperati asincronamente nel metodo di lifecycle `componentDidMount()`.

Manca ancora un passaggio, poiché recuperare la pagina successiva di dati sovrascriverà quella precedente. Quello che vogliamo è concatenare la vecchia e la nuova lista di articoli dallo stato locale in un nuovo oggetto result, quindi aggiusteremo il comportamente per aggiungere nuovi dati più che sovrascriverli.

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
# leanpub-start-insert
  const { hits, page } = result;

  const oldHits = page !== 0
    ? this.state.result.hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    result: { hits: updatedHits, page }
  });
# leanpub-end-insert
}
~~~~~~~~

Un paio di cose succedono adesso nel metodo `setSearchTopStories()`. Prima cosa, recuperiamo hits e page da result.

Secondo, dobbiamo controllare che non ci siano già vecchi dati. Quando page è 0 è una nuova richiesta di ricerca da `componentDidMount()` o `onSearchSubmit()`. Le hits sono vuote. Ma quando clicchiamo il pulsante "More" per recuperare dati paginati la pagina non è 0. Le vecchie hits sono già salvate nello stato e quindi possono essere utilizzate.

Terzo, non vogliamo sovrascrivere le vecchie hits. Possiamo effettuare il merge delle vecchie e delle nuove con lo spread operator di JavaScript ES6.

Quarto, salviamo hits e page nello stato locale del componente.

Adesso faremo un ultimo aggiustamento. Se provi il bottone "More" noterai che recupera solo pochi elementi. L'URL dell'API può essere estesa per recuperare più elementi per ogni richiesta, quindi aggiungeremo questa parte alle nostre costanti:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const DEFAULT_QUERY = 'redux';
# leanpub-start-insert
const DEFAULT_HPP = '100';
# leanpub-end-insert

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
# leanpub-start-insert
const PARAM_HPP = 'hitsPerPage=';
# leanpub-end-insert
~~~~~~~~

Adesso possiamo usare le costanti per estendere l'URL dell'API.

{title="src/App.js",lang="javascript"}
~~~~~~~~
fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-end-insert
    .then(response => response.json())
    .then(result => this.setSearchTopStories(result))
    .catch(error => error);
}
~~~~~~~~

Adesso la richiesta all'API di Hacker News recupera più elementi alla volta di prima. Come puoi vedere, un'API potente come quella di Hacker News offre più modi di sperimentare con dati del mondo reale. Dovresti farne uso per fare i tuoi sforzi quando apprendi qualcosa di nuovo più eccitante. Questo è [come ho imparato la forza che le API forniscono](https://www.robinwieruch.de/what-is-an-api-javascript/) quando si impara un nuovo linguaggio di programmazione o una libreria.

### Esercizi:

* Leggi sui [parametri di default di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)
* Sperimenta con i [parametri dell'API di Hacker News](https://hn.algolia.com/api)

## Cache lato client

Ogni ricerca scatena una richiesta all'API di Hacker News. Potresti per esempio cercare "redux", poi "react", e poi ancora "redux", per un totale di tre richieste. Però abbiamo cercato due volte "redux" e entrambe le volte abbiamo effettuato un'intera chiamata asincrona per recuperare i dati. Una cache lato client salverebbe ogni risultato. Quando viene effettuata una ricerca, prima viene controllato che il risultato non sia già presente, se così, viene usata la cache, altrimenti la richiesta viene fatta davvero.

Per avere una cache lato client per ogni risultato, dobbiamo salvare più `results` piuttosto che uno solo nello stato locale del nostro componente. Gli oggetti results saranno una mappa con la query di ricerca come chiave e il risultato come valore. Ogni risultato dall'API sarà salvato come chiave di ricerca (key).

Al momento, il risultato nello stato locale assomiglia a questo:

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Immagina di aver fatto due richieste: una per la query "redux", l'altra per la query "react". L'oggetto di risultati dovrebbe essere come il seguente:

{title="Code Playground",lang="javascript"}
~~~~~~~~
results: {
  redux: {
    hits: [ ... ],
    page: 2,
  },
  react: {
    hits: [ ... ],
    page: 1,
  },
  ...
}
~~~~~~~~

Implementiamo una cache lato client con la `setState()` di React. Prima cosa rinominiamo l'oggetto `result` in `results` nello stato iniziale del componente. Secondo, definiamo un temporaneo `searchKey` per salvare ogni `result`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      results: null,
      searchKey: '',
# leanpub-end-insert
      searchTerm: DEFAULT_QUERY,
    };

    ...

  }

  ...

}
~~~~~~~~

La `searchKey` deve essere settata prima che venga effettuata ogni richiesta. Questa riflette la query di ricerca. Capire perché non usiamo `searchTerm` qui una parte cruciale da capire prima di procedere con l'implementazione. `searchTerm` è una variabile temporanea, perché verrà cambiata ogni volta che viene digitato qualcosa nel campo di input della ricerca. Vogliamo invece una variabile persistente che determina la più recente query di ricerca usata nella richiesta all'API e può essere usata per recuperare il corretto risultato dalla mappa di risultati. È un puntatore al risultato corrente nella cache, che può essere usato per mostrare il risultato corrente nel metodo `render()`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
componentDidMount() {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
}

onSearchSubmit(event) {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
  event.preventDefault();
}
~~~~~~~~

Ora cambieremo dove il risultato è salvato all'interno dello stato locale del componente. Ogni risultato dovrebbe essere salvato in base alla `searchKey`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    const { hits, page } = result;
# leanpub-start-insert
    const { searchKey, results } = this.state;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];
# leanpub-end-insert

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    this.setState({
# leanpub-start-insert
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
# leanpub-end-insert
    });
  }

  ...

}
~~~~~~~~

La `searchKey` sarà utilizzata come chiave per salvare le hits aggiornate e la page nella mappa `results`.

Prima cosa dobbiamo recuperare la `searchKey` dallo stato del componente. Ricorda che la `searchKey` viene valorizzata in `componentDidMount()` e in `onSearchSubmit()`.

Seconda cosa, dobbiamo effettuare il merge dei vecchi articoli con i nuovi, come facevamo anche prima. Ma questa volta gli articoli vecchi sono recuperati dalla mappa `results` con `searchKey` come chiave.

Terzo, un nuovo risultato può essere settato nella mappa `results` nello stato. Esaminiamo l'oggetto `results` nella `setState()`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

La parte sotto ci assicura di salvare il risultato aggiornato per `searchKey` nella mappa results. Il valore è un oggetto con proprietà hits e page. `searchKey` è la query di ricerca. Hai già imparato la sintassi `[searchKey]: ...`. È un nome di proprietà calcolato in base al valore della variabile (computed property). È utile per allocare dinamicamente valori in un oggetto.

La parte in alto ha bisogno di fare lo spread di tutti gli altri risultati nello stato, altrimenti perderemmo tutti i risultati salvati fino a quel momento.

Adesso salviamo tutti i risultati in base alla query di ricerca. Questo è il primo step per innescare la nostra cache. Nel prossimo passaggio recupereremo il risultato in base a `searchKey` dalla mappa di risultati. Ecco perché abbiamo dovuto introdurre `searchKey`. Altrimenti il recupero dei risultati sarebbe non funzionante se usassi il valore temporaneo `searchTerm` per recuperare il risultato corrente, poiché il valore potrebbe cambiare quando usiamo il componente Search.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        <div className="interactions">
# leanpub-start-insert
          <Button onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
# leanpub-end-insert
            More
          </Button>
        </div>
      </div>
    );
  }
}
~~~~~~~~

Siccome abbiamo messo un default ad una lista vuota quando non ci sono risultato per `searchKey`, possiamo risparmiarci il rendering condizionale per il componente Table. Inoltre, dobbiamo passare `searchKey` piuttosto che `searchTerm` al bottone "More". Altrimenti, la fetch paginata dipenderebbe dal valore di `searchTerm` che è temporaneo. Assicurati anche di mantenere la property `searchTerm` per il campo di input nel componente Search.

Adesso la funzionalità di ricerca dovrebbe salvare tutti i risultati dall'API di Hacker News.

Adesso possiamo vedere che il metodo `onDismiss()` ha bisogno di una sistemata, siccome ha anche a che fare con l'oggetto `result`. Noi vogliamo che gestisca multipli `results`:

{title="src/App.js",lang="javascript"}
~~~~~~~~
  onDismiss(id) {
# leanpub-start-insert
    const { searchKey, results } = this.state;
    const { hits, page } = results[searchKey];
# leanpub-end-insert

    const isNotId = item => item.objectID !== id;
# leanpub-start-insert
    const updatedHits = hits.filter(isNotId);

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
    });
# leanpub-end-insert
  }
~~~~~~~~

Controlla che il bottone "Dismiss" funzioni ancora.

Nota come niente fermi l'applicazione dal fare una richiesta API ogni volta che una ricerca è scatenata. Sebbene potrebbe già esserci un risultato, non c'è nessun controllo a prevenire la richiesta, quindi la funzionalità di cache non è ancora completa. Viene effettuato il cache dei risultati, ma non ne fa uso. L'ultimo passaggio è prevenire le richiesta all'API quando c'è un risultato disponibile nella cache:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

# leanpub-start-insert
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
# leanpub-end-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  needsToSearchTopStories(searchTerm) {
    return !this.state.results[searchTerm];
  }
# leanpub-end-insert

  ...

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
# leanpub-start-insert

    if (this.needsToSearchTopStories(searchTerm)) {
      this.fetchSearchTopStories(searchTerm);
    }

# leanpub-end-insert
    event.preventDefault();
  }

  ...

}
~~~~~~~~

Adesso il nostro client fa una determinata richiesta all'API soltanto una volta, anche se cerchi una query più volte. Anche dati paginati con più pagine vengono cachati in questo modo, poiché salvamo sempre l'ultima pagina per ogni risultato nella mappa `results`. Questo è un potente approccio per introdurre il caching in un'applicazione. L'API di Hacker News fornisce tutto quanto è necessario per effettuare il caching dei dati paginati efficacemente.

## Gestione degli errori

Finora ci siamo presi cura delle interazioni con l'API di Hacker News. Abbiamo introdotto un modo elegante per effettuare il caching dei risultati dell'API e fatto uso della sua funzionalità di risultati paginati per recupare una lista infinita di articoli dall'API. Ma nessuna applicazione è completa senza gestione degli errori.

In questo capitolo introdurremo un'efficente soluzione per aggiungere la gestione degli errori nell'applicazione in caso di errori nella richiesta all'API. Abbiamo visto le basi per introdurre la gestione degli errori in React: stato locale e rendering condizionale. L'errore è semplicemente un'altra proprietà nello stato che salviamo e visualizziamo con del rendering condizionale nel componente.

Adesso la implementeremo nel componente App, visto che è dove effettuaiamo il recupero dei dati dall'API di Hacker News. Per prima cosa, introduciamo una proprietà errore nello stato locale. È inizializzata a null ma sarà settato con l'oggetto dell'errore in caso di errori.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
# leanpub-start-insert
      error: null,
# leanpub-end-insert
    };

    ...
  }

...

}
~~~~~~~~

Poi, usiamo il blocco catch nella chiamata fetch per salvare l'errore nello stato locale utilizzando `setState()`. Ogni volta che la richiesta all'API non finisce con successo, il blocco catch viene eseguito.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    // be careful with the "\" which shows up in the PDF/print version of the book
    // it's only a line break a should not be in the actual code
    // https://github.com/the-road-to-learn-react/the-road-to-learn-react/issues/43
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(error => this.setState({ error }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

Terzo, recuperiamo l'oggetto error dallo stato locale nel metodo `render()` e visualizziamo un messaggio in caso di errori, utilizzando il rendering condizionale di React.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
# leanpub-start-insert
      error
# leanpub-end-insert
    } = this.state;

    ...

# leanpub-start-insert
    if (error) {
      return <p>Something went wrong.</p>;
    }
# leanpub-end-insert

    return (
      <div className="page">
        ...
      </div>
    );
  }
}
~~~~~~~~

Se volessimo testare che la nostra gestione degli errori funzioni, potremmo cambiare l'URL dell'API in qualcosa di non esistente per forzare un errore.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const PATH_BASE = 'https://hn.mydomain.com/api/v1';
~~~~~~~~

Adesso dovresti vedere un messaggio di errore. Sta a noi decidere dove mettere il rendering condizionale per il messaggio di errore. In questo caso l'app è sovrascritta dal messaggio di errore, ma questo non è il massimo come esperienza utente. In questo modo il resto dell'applicazione sarebbe ancora visibile quindi l'utente sa che sta ancora girando:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
            list={list}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

**Ricordati** di risistemare l'URL per l'API a quella corretta:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

L'applicazione adesso gestisce gli errori nel caso una richiesta all'API fallisse.

### Esercizi:

* Leggi sulla [gestione degli errori per componente in React](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Axios al posto di Fetch

Prima abbiamo introdotto il metodo nativo fetch per eseguire una richiesta alla piattaforma di Hacker News. Il browser ti permette di utilizzare questa API nativa fetch. Purtroppo però non è supportata ancora da tutti i browser, in particolare da quelli più vecchi. Se provi a testare l'applicazione con un browser headless (dove non c'è un browser, è simulato), potrebbero esserci problemi con l'API fetch. Ci sono alcuni modi per fare funzionare fetch anche con i browser più vecchi (polyfill) e nei test ([isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch)), ma questi concetti esulano un po' dallo scopo di imparare React.

Un'alternativa è sostituire l'API nativa fetch con una libreria stabile come [axios](https://github.com/axios/axios), che effettua richieste asincrone a API remote. In questo capitolo scopriremo come sostituire una libreria, un'API nativa del browser in questo caso, con un'altra libreria.

Sotto, l'API nativa fetch è sostituita con axios. Prima cosa, installiamo axios dalla riga di comando:

{title="Command Line",lang="text"}
~~~~~~~~
npm install axios
~~~~~~~~

Successivamente, importiamo axios nel file del componente App:

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert
import './App.css';

...
~~~~~~~~

Adesso possiamo usare axios al posto di `fetch()` e il suo utilizzo è pressoché identico a quello dell'API nativa fetch. Prende un URL come argomento e restituisce una promise. Non dovremo più trasformare la risposta restituita in JSON, siccome axios racchiude il risultato in un oggetto `data` in JavaScript. Assicuriamoci solo di adattare il codice alla struttura dati restituita:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(result => this.setSearchTopStories(result.data))
# leanpub-end-insert
      .catch(error => this.setState({ error }));
  }

  ...

}
~~~~~~~~

In questo snippet di codice chiamiamo `axios()` che utilizza una richiesta HTTP GET di default. Possiamo esplicitare la richiesta GET chiamando `axios.get()`, oppure usare gli altri metodi HTTP come `axios.post()`. Già da questi esempi possiamo capire come axios sia una potente libreria per eseguire richieste ad API remote. Ti consiglio di utilizzarla al posto dell'API fetch quando le richieste diventano complesse o dovrai affrontare le promises.

C'è un altro miglioramento che possiamo fare nel componente App per la richiesta ad Hacker News. Immagina che il componente si agganci al DOM quando la pagina viene renderizzata per la prima volta nel browser. In `componentDidMount()` il componente effettua la richiesta, ma poi l'utente naviga in un'altra pagina con questo componente renderizzato. Poi il componente App viene sganciato dal DOM ma c'è ancora una richiesta pending dal metodo di lifecycle `componentDidMount()`. A questo punto tenterà di utilizzare `this.setState()` nel `then()` o nel blocco `catch()` della promise. Probabilmente visualizzeresti i seguenti warning nella riga di comando o nella console del browser.

* *Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.*

* *Warning: Can’t call setState (or forceUpdate) on an unmounted component. This is a no-op, but it indicates a memory leak in your application. To fix, cancel all subscriptions and asynchronous tasks in the componentWillUnmount method.*

Se dovessi incontrare uno di questi nel tool per sviluppatori del tuo browser dai un'occhiata a [questa guida passo passo su come prevenire memory leaks in React](https://www.robinwieruch.de/react-warning-cant-call-setstate-on-an-unmounted-component/). Non è così importante per chi sta iniziando con React, ma ho visto molti principianti in React confusi da questi warning.

Questo capitolo ha mostrato come si può sostituire una libreria con un'altra in React. Se ti imbatti in qualche problema puoi far uso del vasto ecosistema JavaScript per trovare una soluzione.

### Esercizi:

* Leggi sul [perché i framework contano](https://www.robinwieruch.de/why-frameworks-matter/)

{pagebreak}

Adesso hai imparato come interagire con un'API in React! Ricapitoliamo l'ultimo capitolo:

* **React**
  * I metodi di lifecycle nei componenti implementati come classi ES6 per svariati utilizzi
  * `componentDidMount()` per le interazioni con le API
  * Rendiring condizionale
  * Synthetic events nei form
  * Gestione degli errori
  * Prevenire `this.setState()` in componenti ormai sganciati dal DOM
* **ES6 e oltre**
  * Template strings per la composizione di stringhe
  * Spread operator per strutture dati immutabili
  * Nomi di proprietà calcolati (computed properties)
  * Campi di classe
* **Generale**
  * Interazione con le API di Hacker News
  * L'API nativa del browser fetch
  * Ricerca lato client e lato server
  * Paginazione di dati
  * Cache lato client
  * Axios come alternativa per l'API nativa fetch

È ancora ora di una pausa: interiorizza le lezioni e applicale da solo. Sperimenta con i parametri dell'API per ottenere diversi risultati. Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.3.1).
