# In pratica con le API

Adesso è il momento di entrare nel mondo reale con le API, perché a lungo andare può risultare noioso utilizzare dati di esempio

Se non hai dimestichezza con le API, ti suggerisco di [leggere il mio viaggio di apprendimento delle API](https://www.robinwieruch.de/what-is-an-api-javascript/).

Conosci la piattaforma [Hacker News](https://news.ycombinator.com/)? E' un aggregatore di notizie sul mondo tech. In questo libro, utilizzerai le API di Hacker News per recuperare le storie trend dalla piattaforma. Ci occuperemo di una versione API [basic](https://github.com/HackerNews/API) e del [search](https://hn.algolia.com/api) per recuperare dati dalla piattaforma. L'ultimo è utile nel caso l'applicazione abbia la necessità di cercare su Hacker News. Puoi visitare le specifiche API per avere una maggiore comprensione della struttura dati.

## Lifecycle Methods

Devi comprendere il ciclo di vita dei metodi di React prima di iniziare a recuperare dati nei tuoi componenti utilizzando le API. Questi metodi possono essere utilizzati nei componenti di classe ES6, ma non nei componenti functional stateless.

Ti ricordi quando nel precedente capitolo ti ho parlato delle classi JavaScript ES6 e di come vengono utilizzate in React? Mettendo da parte il metodo `render()`, ci sono diversi metodi che possono essere sovrascritti in un componente di classe React ES6. Sono questi i lifecycle methods. Vediamo di approfondire:

già conosci due lifecycle methods che possono essere utilizzati in un componente di classe ES6: `constructor()` e `render()`.

Il costruttore è solo chiamato quando un'istanza del componente è creata ed inserita nel DOM. Il componente sarà istanziato. Quel processo è chiamato mounting del componente.

Il metodo `render()` è chiamato durante il mounting process, ma anche quando il componente si aggiorna. Ogni volta che lo stato o le proprietà di un componente cambiano, il metodo `render()` del componente è richiamato.

Adesso sai di più su questi due lifecycle methods e quando sono richiamati.
Puoi già utilizzarli. Ma c'è qualcos'altro.

Il mounting di un componente ha due ulteriori lifecycle methods: `getDerivedStateFromProps()` e `componentDidMount()`. Il costruttore è chiamato per primo, `getDerivedStateFromProps()` è chiamato prima del metodo `render()` e `componentDidMount()` è chiamato dopo il metodo `render()`.

In totale il processo di mounting ha quattro lifecycle methods. Sono invocati nel seguente ordine:

* constructor()
* getDerivedStateFromProps()
* render()
* componentDidMount()

Ma cosa accade ad un componente quando lo stato o i props cambiano? In totale ci sono 5 lifecycle methods nel seguente ordine:

* getDerivedStateFromProps()
* shouldComponentUpdate()
* getSnapshotBeforeUpdate()
* render()
* componentDidUpdate()

Ultimo, ma non per importanza il metodo `componentWillUnmount()`.

In effetti non hai bisogno di comprendere tutti questi lifecycle methods all'inizio del tuo percorso di apprendimento. Anche nel caso di applicazioni complesse ne userai solo una parte oltre ai metodi `constructor()` e `render()`. Ad ogni modo è bene comprenderne il loro utilizzo:

* **constructor(props)** - E' richiamato quando il componente viene inizializzato. Puoi assegnare uno stato iniziale e vincolarlo ai metodi di classe durante quel lifecycle method.

* **static getDerivedStateFromProps(props, state)** - E' chiamato prima del metodo `render()`, entrambi al mount iniziale e ad ogni aggiornamento. Dovrebbe restituire un oggetto per aggiornare lo stato o null per non cambiare nulla. Esiste per **rari** casi dove lo stato dipende da cambiamenti di props nel tempo. E' importante notare che questo è un metodo statico e non ha accesso all'istanza del componente.

* **render()** - Questo lifecycle method è obbligatorio e restituisce gli elementi come output del componente. Il metodo dovrebbe essere puro e dunque non modificherebbe lo stato del componente. Richiede un input come props e state e restituisce un elemento.

* **componentDidMount()** - E' richiamato solo quando il componente è inizializzato. E' questo il momento perfetto per una richiesta asincrona che recuperi dati da un'API. I dati recuperati dovrebbero essere salvati in un internal component state per mostrarli con il metodo ` render()`.

* **shouldComponentUpdate(nextProps, nextState)** - E' sempre richiamato quando il componente si aggiorna a seguito di un cambiamento di state o props. E' da prendere in considerazione per le applicazioni mature, complesse. Utile per ottimizzare le performance. A seguito di un valore booleano restituito avrai la possibilità di renderizzare o meno un eventuale aggiornamento di un lifecycle. Puoi, quindi, prevenire il rendering di un lifecycle method di un componente.

* **getSnapshotBeforeUpdate(prevProps, prevState)** - Questo metodo di lifecycle è invocato subito prima del commit dell'output nel DOM. In rari casi, il componente ha bisogno di catturare informazioni dal DOM prima che sia potenzialmente cambiato. Questo metodo permette questo. Un altro metodo (`componentDidUpdate()`) riceverà il valore restituito da `getSnapshotBeforeUpdate()` come parametro.

* **componentDidUpdate(prevProps, prevState, snapshot)** - Questo metodo è invocato subito dopo che avvengono aggiornamenti, ma non nel render iniziale. Puoi usarlo per eseguire operazioni sul DOM o effettuare ulteriori richieste asincrone. Se il tuo componente implementa il metodo `getSnapshotBeforeUpdate()`, il valore che quel metodo restituisce sarà ricevuto come parametro `snapshot`.

* **componentWillUnmount()** - E' richiamato prima che tu distrugga il tuo componente. Puoi utilizzare il lifecycle method per svolgere qualsiasi azione di pulizia dei task.

I lifecycle method `constructor()` e `render()` sono già stati utilizzati da te. Sono i più comuni nell'ambito delle componenti di classe ES6. In effetti il metodo `render()` è richiesto, altrimenti non è possibile restiture un'istanza del componente.

C'è un altro lifecycle method: `componentDidCatch(error, info)`. E' stato introdotto in [React 16](https://www.robinwieruch.de/what-is-new-in-react-16/) ed è utilizzato per gestire gli errori nei componenti. Per esempio, mostrare una semplice lista nella tua applicazione è molto facile. Ma potrebbe esserci un caso in cui la lista all'interno di un local state è impostata a `null` accidentalmente (es. quando si recupera una lista da una chiamata esterna API, ma la richieste fallisce e tu imposti il local state della lista a null). In seguito, non sarebbe possibile filtrare e mappare la lista, perché è `null` e non è una lista vuota. Il componente restituirebbe errore e l'intera applicazione fallirebbe. Adesso con l'utilizzo di `componentDidCatch()` puoi gestire l'errore, salvarlo nel tuo local state, e mostrarlo (opzionalmente) come messaggio nella tua applicazione.

### Esercizi:

* approfondisci [lifecycle methods in React](https://reactjs.org/docs/react-component.html)
* approfondisci [lo state relazionato ai lifecycle methods in React](https://reactjs.org/docs/state-and-lifecycle.html)
* approfondisci [la gestione degli errori nei componenti](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Recupero dei dati

Adesso devi prepararti a recuperare i dati attraverso le API di Hacker News. C'era un lifecycle method illustrato che può essere utilizzato per il recupero delle news: `componentDidMount()`. Utilizzerai le API native in JavaScript per effettuare la richiesta.

Prima, però, settiamo le costanti dell'URL e i parametri di default per l'API.

{title="src/App.js",lang=javascript}
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

In JavaScript ES6, puoi utilizzare i [template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals) per concatenare le stringhe. Lo utilizzerai per concatenare il tuo URL per l'endpoint delle API.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

In questo modo terrai la composizione del tuo URL flessibile, anche per il futuro.

Ma andiamo a vedere come ottenere le API che utilizzerai per l'URL. Tutto l'intero processo di recupero sarà presentato in una volta, ma ogni step sarà spiegato.

{title="src/App.js",lang=javascript}
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


Beh, accadono un sacco di cose. Ho pensato di spezzarlo in porzioni, ma sarebbe stato difficile farti comprendere le relazioni tra le porzioni di codice. Vediamo nel dettaglio.

Prima di tutto, puoi rimuovere la lista di esempio di item, perché otterrai la reale lista dalle API di Hacker News. I dati di esempio non sono comunque utilizzati. Lo state iniziale del tuo componente ha un risultato vuoto e un termine di ricerca di default. Lo stesso termine di default di ricerca è utilizzato nel campo input del componente Search e nella tua prima richiesta.

In seconda battuta, utilizzerai `componentDidMount()` per recuperare dati subito dopo il mounting del componente. Nel primo vero recupero dati, il termine di default di ricerca è prelevato dal local state.

Terzo, API native sono utilizzate. Il template string JavaScript ES6 permette di comporre l'URL con il `searchTerm`. L'URL è l'argomento per le API native. I dati restituiti devono essere trasformati in una struttura JSON, che è uno step obbligatorio quando si ha a che fare con una struttura dati JSON. Alla fine il risultato sarà impostato nell'internal component state. Inoltre, il blocco *catch* è utilizzato in caso di errore. Se si verifica un errore durante la richiesta, il flusso si sposterà nel blocco *catch*. In un altro capitolo del libro parleremo della gestione degli errori.

Ultimo, ma non per importanza, non dimentichiamo di richiamare il tuo metodo del componente nel costruttore.

Adesso puoi utilizzare i dati recuperati invece di una lista di esempio. Fa attenzione, però. Il risultato non è una semplice lista dati. [E' un oggetto complesso con meta informazioni e una lista di hits che - nel nostro caso - sono le storie](https://hn.algolia.com/api). Puoi verificare l'output con `console.log(this.state)` nel tuo metodo `render()` per visualizzare i dati.

Nel prossimo step utilizzerai il risultato per renderizzarlo. Ma dobbiamo prevenire la renderizzazione di qualunque cosa, quindi restituiremo *null*, quando non ci sarà risultato. Nel momento in cui parte la richiesta API, il risultato sarà salvato nello state e il componente App sarà renderizzato con lo stato aggiornato.

{title="src/App.js",lang=javascript}
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

Let's recap what happens during the component lifecycle. Your component gets initialized by the constructor. After that, it renders for the first time. But you prevent it from displaying anything, because the result in the local state is null. It is allowed to return null for a component in order to display nothing. Then the `componentDidMount()` lifecycle method runs. In that method you fetch the data from the Hacker News API asynchronously. Once the data arrives, it changes your internal component state in `setSearchTopStories()`. Afterward, the update lifecycle comes into play because the local state was updated. The component runs the `render()` method again, but this time with populated result in your internal component state. The component and thus the Table component with its content will be rendered.

You used the native fetch API that is supported by most browsers to perform an asynchronous request to an API. The *create-react-app* configuration makes sure that it is supported in every browser. There are third-party node packages that you can use to substitute the native fetch API: [superagent](https://github.com/visionmedia/superagent) and [axios](https://github.com/mzabriskie/axios).

Keep in mind that the book builds up on the JavaScript's shorthand notation for truthfulness checks. In the previous example, `if (!result)` was used in favor of `if (result === null)`. The same applies for other cases throughout the book too. For instance, `if (!list.length)` is used in favor of `if (list.length === 0)` or `if (someString)` is used in favor of `if (someString !== '')`. Read up about the topic if you are not too familiar with it.

Back to your application: The list of hits should be visible now. However, there are two regression bugs in the application now. First, the "Dismiss" button is broken. It doesn't know about the complex result object and still operates on the plain list from the local state when dismissing an item. Second, when the list is displayed but you try to search for something else, the list gets filtered on the client-side even though the initial search was made by searching for stories on the server-side. The perfect behvaior would be to fetch another result object from the API when using the Search component. Both regression bugs will be fixed in the following chapters.

### Exercises:

* read more about [ES6 template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* read more about [the native fetch API](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* read more about [data fetching in React](https://www.robinwieruch.de/react-fetching-data/)

## ES6 Spread Operators

The "Dismiss" button doesn't work because the `onDismiss()` method is not aware of the complex result object. It only knows about a plain list in the local state. But it isn't a plain list anymore. Let's change it to operate on the result object instead of the list itself.

{title="src/App.js",lang=javascript}
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

But what happens in `setState()` now? Unfortunately the result is a complex object. The list of hits is only one of multiple properties in the object. However, only the list gets updated, when an item gets removed in the result object, while the other properties stay the same.

One approach could be to mutate the hits in the result object. I will demonstrate it, but we won't do it that way.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// don`t do this
this.state.result.hits = updatedHits;
~~~~~~~~

React embraces immutable data structures. Thus you shouldn't mutate an object (or mutate the state directly). A better approach is to generate a new object based on the information you have. Thereby none of the objects get altered. You will keep the immutable data structures. You will always return a new object and never alter an object.

Therefore you can use JavaScript ES6 `Object.assign()`. It takes as first argument a target object. All following arguments are source objects. These objects are merged into the target object. The target object can be an empty object. It embraces immutability, because no source object gets mutated. It would look similar to the following:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Latter objects will override former merged objects when they share the same property names. Now let's do it in the `onDismiss()` method:

{title="src/App.js",lang=javascript}
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

That would already be the solution. But there is a simpler way in JavaScript ES6 and future JavaScript releases. May I introduce the spread operator to you? It only consists of three dots: `...` When it is used, every value from an array or object gets copied to another array or object.

Let's examine the ES6 **array** spread operator even though you don't need it yet.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

The `allUsers` variable is a completely new array. The other variables `userList` and `additionalUser` stay the same. You can even merge two arrays that way into a new array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Now let's have a look at the object spread operator. It is not JavaScript ES6. It is a [proposal for a next JavaScript version](https://github.com/sebmarkbage/ecmascript-rest-spread) yet already used by the React community. That's why *create-react-app* incorporated the feature in the configuration.

Basically it is the same as the JavaScript ES6 array spread operator but with objects. It copies each key value pair into a new object.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Multiple objects can be spread like in the array spread example.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

After all, it can be used to replace `Object.assign()`.

{title="src/App.js",lang=javascript}
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

Now the "Dismiss" button should work again, because the `onDismiss()` method is aware of the complex result object and how to update it after dismissing an item from the list.

### Exercises:

* read more about the [ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* read more about the [ES6 array spread operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * the object spread operator is briefly mentioned

## Conditional Rendering

Conditional rendering is introduced pretty early in React applications. But not in the case of the book, because there wasn't such an use case yet. The conditional rendering happens when you want to make a decision to render either one or another element. Sometimes it means to render an element or nothing. After all, a conditional rendering simplest usage can be expressed by an if-else statement in JSX.

The `result` object in the internal component state is `null` in the beginning. So far, the App component returned no elements when the `result` hasn't arrived from the API. That's already a conditional rendering, because you return earlier from the `render()` lifecycle method for a certain condition. The App component either renders nothing or its elements.

But let's go one step further. It makes more sense to wrap the Table component, which is the only component that depends on the `result`, in an independent conditional rendering. Everything else should be displayed, even though there is no `result` yet. You can simply use a ternary operator in your JSX.

{title="src/App.js",lang=javascript}
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

That's your second option to express a conditional rendering. A third option is the logical `&&` operator. In JavaScript a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
~~~~~~~~

In React you can make use of that behavior. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores and skips the expression. It is applicable in the Table conditional rendering case, because it should return a Table or nothing.

{title="src/App.js",lang=javascript}
~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

These were a few approaches to use conditional rendering in React. You can read about [more alternatives in an exhaustive list of examples for conditional rendering approaches](https://www.robinwieruch.de/conditional-rendering-react/). Moreover you will get to know their different use cases and when to apply them.

After all, you should be able to see the fetched data in your application. Everything except the Table is displayed when the data fetching is pending. Once the request resolves the result and stores it into the local state, the Table is displayed because the `render()` method runs again and the condition in the conditional rendering resolves in favor of displaying the Table component.

### Exercises:

* read more about [different ways for conditional renderings](https://www.robinwieruch.de/conditional-rendering-react/)
* read more about [React conditional rendering](https://reactjs.org/docs/conditional-rendering.html)

## Client- or Server-side Search

When you use the Search component with its input field now, you will filter the list. That's happening on the client-side though. Now you are going to use the Hacker News API to search on the server-side. Otherwise you would deal only with the first API response which you got on `componentDidMount()` with the default search term parameter.

You can define an `onSearchSubmit()` method in your App component which fetches results from the Hacker News API when executing a search in the Search component.

{title="src/App.js",lang=javascript}

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

The `onSearchSubmit()` method should use the same functionality as the `componentDidMount()` lifecycle method, but this time with a modified search term from the local state and not with the initial default search term. Thus you can extract the functionality as a reusable class method.

{title="src/App.js",lang=javascript}
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

Now the Search component has to add an additional button. The button has to explicitly trigger the search request. Otherwise you would fetch data from the Hacker News API every time when your input field changes. But you want to do it explicitly in an `onClick` handler.

As alternative you could debounce (delay) the `onChange()` function and spare the button, but it would add more complexity at this time and maybe wouldn't be the desired effect. Let's keep it simple without a debounce for now.

First, pass the `onSearchSubmit()` method to your Search component.

{title="src/App.js",lang=javascript}
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

Second, introduce a button in your Search component. The button has the `type="submit"` and the form uses its `onSubmit()` attribute to pass the `onSubmit()` method. You can reuse the children property, but this time it will be used as the content of the button.

{title="src/App.js",lang=javascript}
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

In the Table, you can remove the filter functionality, because there will be no client-side filter (search) anymore. Don't forget to remove the `isSearched()` function as well. It will not be used anymore. The result comes directly from the Hacker News API now after you have clicked the "Search" button.

{title="src/App.js",lang=javascript}
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

When you try to search now, you will notice that the browser reloads. That's a native browser behavior for a submit callback in a HTML form. In React you will often come across the `preventDefault()` event method to suppress the native browser behavior.

{title="src/App.js",lang=javascript}
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

Now you should be able to search different Hacker News stories. Perfect, you interact with a real world API. There should be no client-side search anymore.

### Exercises:

* read more about [synthetic events in React](https://reactjs.org/docs/events.html)
* experiment with the [Hacker News API](https://hn.algolia.com/api)

## Paginated Fetch

Did you have a closer look at the returned data structure yet? The [Hacker News API](https://hn.algolia.com/api) returns more than a list of hits. Precisely it returns a paginated list. The page property, which is `0` in the first response, can be used to fetch more paginated sublists as result. You only need to pass the next page with the same search term to the API.

Let's extend the composable API constants so that it can deal with paginated data.

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert
~~~~~~~~

Now you can use the new constant to add the page parameter to your API request.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

The `fetchSearchTopStories()` method will take the page as second argument. If you don't provide the second argument, it will fallback to the `0` page for the initial request. Thus the `componentDidMount()` and `onSearchSubmit()` methods fetch the first page on the first request. Every additional fetch should fetch the next page by providing the second argument.

{title="src/App.js",lang=javascript}
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

The page argument uses the JavaScript ES6 default parameter to introduce the fallback to page `0` in case no defined page argument is provided for the function.

Now you can use the current page from the API response in `fetchSearchTopStories()`. You can use this method in a button to fetch more stories on a `onClick` button handler. Let's use the Button to fetch more paginated data from the Hacker News API. You only need to define the `onClick()` handler which takes the current search term and the next page (current page + 1).

{title="src/App.js",lang=javascript}
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

In addition, in your `render()` method you should make sure to default to page 0 when there is no result yet. Remember that the `render()` method is called before the data is fetched asynchronously in the `componentDidMount()` lifecycle method.

There is one step missing. You fetch the next page of data, but it will override your previous page of data. It would be ideal to concatenate the old and new list of hits from the local state and new result object. Let's adjust the functionality to add the new data rather than to override it.

{title="src/App.js",lang=javascript}
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

A couple of things happen in the `setSearchTopStories()` method now. First, you get the hits and page from the result.

Second, you have to check if there are already old hits. When the page is 0, it is a new search request from `componentDidMount()` or `onSearchSubmit()`. The hits are empty. But when you click the "More" button to fetch paginated data the page isn't 0. It is the next page. The old hits are already stored in your state and thus can be used.

Third, you don't want to override the old hits. You can merge old and new hits from the recent API request. The merge of both lists can be done with the JavaScript ES6 array spread operator.

Fourth, you set the merged hits and page in the local component state.

You can make one last adjustment. When you try the "More" button it only fetches a few list items. The API URL can be extended to fetch more list items with each request. Again, you can add more composable path constants.

{title="src/App.js",lang=javascript}
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

Now you can use the constants to extend the API URL.

{title="src/App.js",lang=javascript}
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

Afterward, the request to the Hacker News API fetches more list items in one request than before. As you can see, a powerful API such as the Hacker News API gives you plenty of ways to experiment with real world data. You should make use of it to make your endeavours when learning something new more exciting. That's [how I learned about the empowerment that APIs provide](https://www.robinwieruch.de/what-is-an-api-javascript/) when learning a new programming language or library.

### Exercises:

* experiment with the [Hacker News API parameters](https://hn.algolia.com/api)
* read more about [ES6 default parameters](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Client Cache

Each search submit makes a request to the Hacker News API. You might search for "redux", followed by "react" and eventually "redux" again. In total it makes 3 requests. But you searched for "redux" twice and both times it took a whole asynchronous roundtrip to fetch the data. In a client-sided cache you would store each result. When a request to the API is made, it checks if a result is already there. If it is there, the cache is used. Otherwise an API request is made to fetch the data.

In order to have a client cache for each result, you have to store multiple `results` rather than one `result` in your internal component state. The results object will be a map with the search term as key and the result as value. Each result from the API will be saved by search term (key).

At the moment, your result in the local state looks similar to the following:

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Imagine you have made two API requests. One for the search term "redux" and another one for "react". The results object should look like the following:

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

Let's implement a client-side cache with React `setState()`. First, rename the `result` object to `results` in the initial component state. Second, define a temporary `searchKey` which is used to store each `result`.

{title="src/App.js",lang=javascript}
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

The `searchKey` has to be set before each request is made. It reflects the `searchTerm`. You might wonder: Why don't we use the `searchTerm` in the first place? That's a crucial part to understand before continuing with the implementation. The `searchTerm` is a fluctuant variable, because it gets changed every time you type into the Search input field. However, in the end you will need a non fluctuant variable. It determines the recent submitted search term to the API and can be used to retrieve the correct result from the map of results. It is a pointer to your current result in the cache and thus can be used to display the current result in your `render()` method.

{title="src/App.js",lang=javascript}
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

Now you have to adjust the functionality where the result is stored to the internal component state. It should store each result by `searchKey`.

{title="src/App.js",lang=javascript}
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

The `searchKey` will be used as the key to save the updated hits and page in a `results` map.

First, you have to retrieve the `searchKey` from the component state. Remember that the `searchKey` gets set on `componentDidMount()` and `onSearchSubmit()`.

Second, the old hits have to get merged with the new hits as before. But this time the old hits get retrieved from the `results` map with the `searchKey` as key.

Third, a new result can be set in the `results` map in the state. Let's examine the `results` object in `setState()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

The bottom part makes sure to store the updated result by `searchKey` in the results map. The value is an object with a hits and page property. The `searchKey` is the search term. You already learned the `[searchKey]: ...` syntax. It is an ES6 computed property name. It helps you to allocate values dynamically in an object.

The upper part needs to spread all other results by `searchKey` in the state by using the object spread operator. Otherwise you would lose all results that you have stored before.

Now you store all results by search term. That's the first step to enable your cache. In the next step, you can retrieve the result depending on the non fluctuant `searchKey` from your map of results. That's why you had to introduce the `searchKey` in the first place as non fluctuant variable. Otherwise the retrieval would be broken when you would use the fluctuant `searchTerm` to retrieve the current result, because this value might change when you would use the Search component.

{title="src/App.js",lang=javascript}
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

Since you default to an empty list when there is no result by `searchKey`, you can spare the conditional rendering for the Table component now. Additionally you will need to pass the `searchKey` rather than the `searchTerm` to the "More" button. Otherwise your paginated fetch depends on the `searchTerm` value which is fluctuant. Moreover make sure to keep the fluctuant `searchTerm` property for the input field in the "Search" component.

The search functionality should work again. It stores all results from the Hacker News API.

Additionally the `onDismiss()` method needs to get improved. It still deals with the `result` object. Now it has to deal with multiple `results`.

{title="src/App.js",lang=javascript}
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

The "Dismiss" button should work again.

However, nothing stops the application from sending an API request on each search submit. Even though there might be already a result, there is no check that prevents the request. Thus the cache functionality is not complete yet. It caches the results, but it doesn't make use of them. The last step would be to prevent the API request when a result is available in the cache.

{title="src/App.js",lang=javascript}
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

Now your client makes a request to the API only once although you search for a search term twice. Even paginated data with several pages gets cached that way, because you always save the last page for each result in the `results` map. Isn't that a powerful approach to introduce caching to your application? The Hacker News API provides you with everything you need to even cache paginated data effectively.

## Error Handling

Everything is in place for your interactions with the Hacker News API. You even have introduced an elegant way to cache your results from the API and make use of its paginated list functionality to fetch an endless list of sublists of stories from the API. But there is one piece missing. Unfortunately it is often missed when developing applications nowadays: error handling. It is too easy to implement the happy path without worrying about the errors that can happen along the way.

In this chapter, you will introduce an efficient solution to add error handling for your application in case of an erroneous API request. You have already learned about the necessary building blocks in React to introduce error handling: local state and conditional rendering. Basically, the error is only another state in React. When an error occurs, you will store it in the local state and display it with a conditional rendering in your component. That's it. Let's implement it in the App component, because it's the component that is used to fetch the data from the Hacker News API in the first place. First, you have to introduce the error in the local state. It is initialized as null, but will be set to the error object in case of an error.

{title="src/App.js",lang=javascript}
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

Second, you can use the catch block in your native fetch to store the error object in the local state by using `setState()`. Every time the API request isn't successful, the catch block would be executed.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(e => this.setState({ error: e }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

Third, you can retrieve the error object from your local state in the `render()` method and display a message in case of an error by using React's conditional rendering.

{title="src/App.js",lang=javascript}
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

That's it. If you want to test that your error handling is working, you can change the API URL to something else that is non existent.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

Afterward, you should get the error message instead of your application. It is up to you where you want to place the conditional rendering for the error message. In this case, the whole app isn't displayed anymore. That wouldn't be the best user experience. So what about displaying either the Table component or the error message? The remaining application would still be visible in case of an error.

{title="src/App.js",lang=javascript}
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

In the end, don't forget to revert the URL for the API to the existent one.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

Your application should still work, but this time with error handling in case the API request fails.

### Exercises:

* read more about [React's Error Handling for Components](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Axios instead of Fetch

In one of the previous chapters, you have introduced the native fetch API to perform a request to the Hacker News platform. The browser enables you to use this native fetch API. However, not all browsers, especially older browsers, support it. In addition, once you start to test your application in a headless browser environment (there is no browser, instead it is only mocked), there can be issues regarding the fetch API. Such a headless browser environment can happen when writing and executing tests for your application which don't run in a real browser. There are a couple of ways to make fetch work in older browsers (polyfills) and in tests ([isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch)), but we won't go down this rabbit hole in this book.

An alternative way to solve it would be to substitute the native fetch API with a stable library such as [axios](https://github.com/axios/axios). Axios is a library that solves only one problem, but it solves it with a high quality: performing asynchronous requests to remote APIs. That's why you will use it in this book. On a concrete level, the chapter should show you how you can substitute a library (which is a native API of the browser in this case) with another library. On an abstract level, it should show you how you can always find a solution for the quirks (e.g. old browsers, headless browser tests) in web development. So never stop to look for solutions if anything gets in your way.

Let's see how the native fetch API can be substituted with axios. Actually everything said before sounds more difficult than it is. First, you have to install axios on the command line:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save axios
~~~~~~~~

Second, you can import axios in your App component's file:

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
# leanpub-start-insert
import axios from 'axios';
# leanpub-end-insert
import './App.css';
 ...
~~~~~~~~

And last but not least, you can use it instead of `fetch()`. Its usage looks almost identical to the native fetch API. It takes the URL as argument and returns a promise. You don't have to transform the returned response to JSON anymore. Axios is doing it for you and wraps the result into a `data` object in JavaScript. Thus make sure to adapt your code to the returned data structure.

{title="src/App.js",lang=javascript}
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

That's it for replacing fetch with axios in this chapter. In your code, you are calling `axios()` which uses by default a HTTP GET request. You can make the GET request explicit by calling `axios.get()`. Also you can use another HTTP method such as HTTP POST with `axios.post()` instead. There you can already see how axios is a powerful library to perform requests to remote APIs. I often recommend to use it over the native fetch API when your API requests become complex or you have to deal with web development quirks with promises. In addition, in a later chapter, you will introduce testing in your application. Then you don't need to worry anymore about a browser or headless browser environment.

I want to introduce another improvement for the Hacker News request in the App component. Imagine your component mounts when the page is rendered for the first time in the browser. In `componentDidMount()` the component starts to make the request, but then, because your application introduced some kind of navigation, you navigate away from this page to another page. Your App component unmounts, but there is still a pending request from your `componentDidMount()` lifecycle method. It will attempt to use `this.setState()` eventually in the `then()` or `catch()` block of the promise. Perhaps then it's the first time you will see the following warning on your command line or in your browser's developer output:

{title="Command Line",lang="text"}
~~~~~~~~
Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.
~~~~~~~~

You can deal with this issue by aborting the request when your component unmounts or preventing to call `this.setState()` on an unmounted component. It's a best practice in React, even though it's not followed by many developers, to preserve an clean application without any annoying warnings. However, the current promise API doesn't implement aborting a request. Thus you need to help yourself on this issue. This might also be the case why not many developers are following this best practice. The following implementation seems more like a workaround than a sustainable implementation. Because of that, you can decide on your own if you want to implement it to work around the warning because of an unmounted component. Nevertheless, keep the warning in mind in case it comes up in a later chapter of this book or in your own application one day. Then you know how to deal with it.

Let's start to work around it. You can introduce a class field which holds the lifecycle state of your component. It can be initialized as `false` when the component initializes, changed to `true` when the component mounted, but then again set to `false` when the component unmounted. This way, you can keep track of your component's lifecycle state. It has nothing to do with the local state stored and modified with `this.state` and `this.setState()`, because you should be able to access it directly on the component instance without relying on React's local state management. Moreover, it doesn't lead to any re-rendering of the component when the class field is changed this way.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
# leanpub-start-insert
  _isMounted = false;
# leanpub-end-insert
   constructor(props) {
    ...
  }
   ...
   componentDidMount() {
# leanpub-start-insert
    this._isMounted = true;
# leanpub-end-insert
     const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
    this.fetchSearchTopStories(searchTerm);
  }
 # leanpub-start-insert
  componentWillUnmount() {
    this._isMounted = false;
  }
# leanpub-end-insert
   ...
 }
~~~~~~~~

Finally, you can use this knowledge not to abort the request itself but to avoid calling `this.setState()` on your component instance even though the component already unmounted. It will prevent the mentioned warning.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
   ...
   fetchSearchTopStories(searchTerm, page = 0) {
    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-start-insert
      .then(result => this._isMounted && this.setSearchTopStories(result.data))
      .catch(error => this._isMounted && this.setState({ error }));
# leanpub-end-insert
  }
   ...
 }
~~~~~~~~

Overall the chapter has shown you how you can replace one library with another library in React. If you run into any issues, you can use the vast library ecosystem in JavaScript to help yourself. In addition, you have seen a way how you can avoid calling `this.setState()` in React on an unmounted component. If you dig deeper into the axios library, you will find a way to prevent the cancel the request in the first place too. It's up to you to read up more about this topic.

### Exercises:

* read more about [why frameworks matter](https://www.robinwieruch.de/why-frameworks-matter/)
* learn more about [an alternative React component syntax](https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax)

{pagebreak}

You have learned to interact with an API in React! Let's recap the last chapters:

* React
  * ES6 class component lifecycle methods for different use cases
  * componentDidMount() for API interactions
  * conditional renderings
  * synthetic events on forms
  * error handling
  * aborting a remote API request
* ES6 and beyond
  * template strings to compose strings
  * spread operator for immutable data structures
  * computed property names
  * class fields
* General
  * Hacker News API interaction
  * native fetch browser API
  * client- and server-side search
  * pagination of data
  * client-side caching
  * axios as an alternative for the native fetch API

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far. You can find the source code in the [official repository](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.3.1).
