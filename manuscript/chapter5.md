# Componenti React avanzati

Questo capitolo si concentra sull'implementazione di componenti React avanzati. Imparerai cosa sono i componenti di ordine superiore (higher order components, anche abbreviato HOC) e come implementarli. Inoltre ci immergeremo in argomenti avanzati di React e implementeremo interazioni complesse tra le parti.

## Ref su un elemento del DOM

A volte hai bisogno di interagire con i nodi del DOM in React. L'attributo `ref` ti dà accesso ai nodi dei tuoi elementi. Di solito questo è considerato un anti pattern in React perché dovresti usare il suo modo dichiarativo e il passaggio unidirezionale dei dati per fare le cose. Hai imparato queste cose quando abbiamo introdotto il tuo primo campo di ricerca. Ma ci sono certi casi dove hai bisogno di avere accesso al nodo del DOM. La documentazione ufficiale cita tre casi:

* per usare l'API del DOM (focus, riproduzione di media ecc.)
* per innescare animazioni del DOM
* per integrarsi con librerie di terze parti che hanno bisogno di nodi del DOM (esempio [D3.js](https://d3js.org/))

Facciamo un esempio con il componente Search. QUando l'applicazione viene renderizzata la prima volta, il campo di input dovrebbe avere il focus. Questo è un caso d'uso dove avresti bisogno di accedere all'API del DOM. Questo capitolo ti mostrerà come funziona, ma siccome non è molto utile nel complesso dell'applicazione, ometteremo queste modifiche nei capitoli seguenti. Puoi mantenerli comunque nella tua applicazione.

In generale, puoi usare l'attributo `ref` sia in componenti funzionali privi di stato sia in componenti realizzati con classi ES6. Nell'esempio del focus avremo bisogno di un metodo di lifecycle, per questo l'approccio di questo primo caso è realizzato usando l'attributo `ref` con un componente che è una classe ES6.

Il primo passaggio è refattorizzare il componente funzionale stateless in un componente tramite class ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
# leanpub-end-insert
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
# leanpub-start-insert
    );
  }
}
# leanpub-end-insert
~~~~~~~~

L'oggetto `this` di un componente in una classe ES6 ci aiuta a referenziare un nodo del DOM con l'attributo `ref`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
# leanpub-start-insert
          ref={(node) => { this.input = node; }}
# leanpub-end-insert
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Ora possiamo dare il focus al campo di testo quando il componente è inserito nel DOM usando l'oggetto `this`, il metodo di lifecycle appropriato, e le API del DOM.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
# leanpub-start-insert
  componentDidMount() {
    this.input.focus();
  }
# leanpub-end-insert

  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
          ref={(node) => { this.input = node; }}
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Il campo di testo dovrebbe ora avere il focus quando l'applicazione viene renderizzata. Questo è praticamente tutto per quanto riguarda l'attributo `ref`.

Ma come possiamo accedere a `ref` in un componente funzionale stateless considerata l'assenza dell'oggetto `this`? Il seguente componente privo di stato lo ne è un esempio.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) => {
# leanpub-start-insert
  let input;
# leanpub-end-insert
  return (
    <form onSubmit={onSubmit}>
      <input
        type="text"
        value={value}
        onChange={onChange}
# leanpub-start-insert
        ref={(node) => input = node}
# leanpub-end-insert
      />
      <button type="submit">
        {children}
      </button>
    </form>
  );
}
~~~~~~~~

Ora dovresti essere in grado di accedere all'elemento di input del dom. Nell'esempio del caso d'uso del focus questo non ti avrebbe aiutato, considerata l'assenza, nei componenti stateless, di metodi di lifecycle per effettuare il trigger del focus. Ma in futuro potresti imbatterti in altri casi dove ha senso usare un componente funzionale stateless con l'attribute `ref`.

### Esercizi

* leggi di più su [l'attributo ref in generale in React](https://facebook.github.io/react/docs/refs-and-the-dom.html)
* leggi di più su [l'utilizzo dell'attributo ref in React](https://www.robinwieruch.de/react-ref-attribute-dom-node/)

## Caricamento ...

Ora torniamo alla nostra applicazione. Potresti voler aggiungere un indicatore di caricamento quando viene eseguita una richiesta di ricerca alle API di Hacker News. La richiesta è asincrona e dovrebbe dare all'utente un feedback del fatto che qualcosa sta accadendo. Definiamo un componente Loading riusabile in un file *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Loading = () =>
  <div>Loading ...</div>
~~~~~~~~

Ora avremo bisogno di una proprietà per salvare lo stato del caricamento. In base al caricamento nello stato possiamo decidere di mostrare il componente Loading.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
# leanpub-start-insert
      isLoading: false,
# leanpub-end-insert
    };

    ...
  }

  ...

}
~~~~~~~~

Il valore iniziale della proprietà `isLoading` è false. Non vogliamo caricare niente prima che il componente App è eseguito.

Quando la richiesta è eseguita, possiamo settare la proprietà a true. Quando la richiesta andrà a buon fine possiamo settare la proprietà nuovamente a false.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    ...

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
# leanpub-start-insert
      isLoading: false
# leanpub-end-insert
    });
  }

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    this.setState({ isLoading: true });
# leanpub-end-insert

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(e => this.setState({ error: e }));
  }

  ...

}
~~~~~~~~

In quest'ultimo passaggio useremo il componente Loading nel nostro componente App. Un controllo è eseguito prima di decidere se mostrare il componente Loading o il componente Button (conditional rendering). Quest'ultimo è il bottone che permette di recuperare più dati.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
# leanpub-start-insert
      isLoading
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          { isLoading
            ? <Loading />
            : <Button
              onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
              More
            </Button>
          }
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Inizialmente il componente Loading sarà mostrato quando eseguiremo la nostra applicazione perché eseguiamo la nostra richiesta nel metodo `componentDidMount()`. Non c'è il componente Table, perché la lista di risulta è ancora vuota. Quando riceviamo la risposta dall'API di Hacker News, il risultato viene mostrato, la proprietà `isLoading` è settata a false e il componente Loading scompare. Di conseguenza il bottone "More" per caricare più dati compare. Se viene eseguita una richiesta per caricare più dati, il bottone scompare nuovamente e il componente Loading è mostrato.

### Esercizi:

* utilizza una libreria come [Font Awesome](http://fontawesome.io/) per mostrare un'icona di caricamento invece del testo "Loading ..."

## Componenti di ordine superiore (HOC)

I componenti di ordine superiore (higher order components, HOC) sono un concetto avanzato di React. Gli HOC sono un equivalente delle funzioni di ordine superiore. Prendono dell'input - la maggior parte delle volte un componente, ma anche parametri opzionali - e restituiscono un componente come output. Il componente restituito è una versione con più funzionalità del componente in input e può essere usato nel tuo JSX.

Gli HOC hanno diversi casi d'uso. Possono prepare delle proprietà, gestire lo stato o alterare la rappresentazione di un componente. Un caso d'uso potrebbe essere quello di usare un HOC come helper per del conditional rendering. Immagina di avere un componente List che renderizza una lista di oggetti o niente, perché la lista è vuota o null. Il HOC può occuparsi del caso in cui la lista non renderizzerebbe niente perché non presente. Dopotutto, il semplice componente List non dovrebbe occuparsi anche del caso in cui la lista non è presente. Così si preoccupa solo di renerizzare la lista.

Implementiamo un semplice HOC che prendere in input un componente e restituisce un altro componente. Puoi metterlo nel file *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
~~~~~~~~

Una convenzione diffusa è quella di mettere un prefisso `with` al nome dell'HOC. Siccome stai usando JavaScript ES6, puoi implementare lo stesso HOC in modo più conciso con una arrow function in ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
const withFoo = (Component) => (props) =>
  <Component { ...props } />
~~~~~~~~

Nell'esempio, il componente di input resta uguale al componente di output. Non succede niente di interessante. Renderizza l'istanza dello stesso componente e passa tutte le props al componente in output. Ma questo è inutile. Aggiungiamo delle funzionalità al componente di output. Il componente di output dovrebbe mostrare il componente Loading, quando la property loading è true, altrimenti mostrare il componente in input. Un conditional rendering è un'ottima use case per un HOC.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => (props) =>
  props.isLoading
    ? <Loading />
    : <Component { ...props } />
# leanpub-end-insert
~~~~~~~~

In base alla proprietà loading possiamo applicare il conditional rendering. La funzione restituirà il componente Loading o il componente in input.

In generale può essere molto efficiente fare lo spread di un oggetto, come le proprietà dell'oggetto nell'esempio precedente, come input di un componente. Guardiamo la differenza con il seguente frammento di codice.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// before you would have to destructure the props before passing them
const { foo, bar } = props;
<SomeComponent foo={foo} bar={bar} />

// but you can use the object spread operator to pass all object properties
<SomeComponent { ...props } />
~~~~~~~~

C'è un piccolo dettaglio che possiamo evitare. Abbiamo passato tutte le proprietà inclusa la proprietà `isLoading`, facendo lo spread dell'oggetto, dentro al componente di input. Tuttavia, al componente di input potrebbe non interessare più la proprietà `isLoading`. Possiamo usare il rest destructuring per evitarlo.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />
# leanpub-end-insert
~~~~~~~~

Estrae una proprietà dall'oggetto ma mantiene il resto dell'oggetto. Funziona anche con proprietà multiple. Potresti aver già letto a riguardo nel [compito sul destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

Ora puoi usare l'HOC nel tuo JSX. Un altro caso d'uso nella nostra applicazione potrebbe essere quello di mostrare il bottone "More" o il componente Loading. Il componente Loading è già incapsulato in un HOC ma un componente di input non c'è. Nell'esempio di mostrare un componente Button o un componente Loading, il Button è il componente in input all'HOC. Il componente in output, arricchito di funzionalità, è il componente ButtonWithLoading.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({ onClick, className = '', children }) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

const Loading = () =>
  <div>Loading ...</div>

const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />

# leanpub-start-insert
const ButtonWithLoading = withLoading(Button);
# leanpub-end-insert
~~~~~~~~

Tutto è definito ora. Come ultimo passaggio dobbiamo utilizzare il componente ButtonWithLoading, che riceve la proprietà loading come proprietà addizionale. Mentre il HOC fa uso della proprietà loading, tutte le altre proprietà vengono passate al componente Button.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    ...
    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          <ButtonWithLoading
            isLoading={isLoading}
            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
            More
          </ButtonWithLoading>
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Se esegui nuovamente i test, noterai che lo snapshot test per il componente App fallisce. La differenza potrebbe apparire la seguente nel terminale:

{title="Command Line",lang="text"}
~~~~~~~~
-    <button
-      className=""
-      onClick={[Function]}
-      type="button"
-    >
-      More
-    </button>
+    <div>
+      Loading ...
+    </div>
~~~~~~~~

Puoi o sistmare subito il componente, quando pensi che c'è qualcosa di sbagliato, o accettare la sua nuova snapshot generata. Poiché abbiamo introdotto il componente Loading in questo capitolo, potresti accettare le modifiche alla snapshot nella riga di comando del test interattivo.

I componenti di ordine superiore sono una tecnica avanzata in React. Hanno molti scopi come migliorare la reusabilità dei componenti, migliorare le astrazioni, l'interazione tra i componenti e la manipolazione di proprietà, stato e viste. Non preoccuparti se non li capisci immediatamente, richiedono tempo per assimilarli.

Ti invito a leggere [gentle introduction to higher order components](https://www.robinwieruch.de/gentle-introduction-higher-order-components/). Ti offre un altro approccio per impararli, mostra un modo elegante per usarli nella programmazione funzionale e risolve specificatamente il problema del conditional rendering con componenti di ordine superiore.

### Esercizi:

* leggi [a gentle introduction to higher order components](https://www.robinwieruch.de/gentle-introduction-higher-order-components/)
* sperimenta con gli HOC che hai creato
* pensa a un altro caso in cui un HOC avrebbe senso
  * implementa il HOC, se hai trovato un caso

## Ordinamento avanzato

Hai già implementato un'interazione tra client e server. Siccome hai un componente Table, avrebbe senso arricchire il componente Table con interazioni avanzate. E se introducessimo una funzionalità di ordinamento su ogni colonna, usando gli header di colonna di Table?

Sarebbe possibile scrivere la propria funzione di ordinamento ma personalmente preferisco usare una libreria per questi casi.
[Lodash](https://lodash.com/) è una di queste librerie di utilità ma puoi usare quella che preferisci. Installiamo lodash e usiamola per la funzionalità di ordinamento.

{title="Command Line",lang="text"}
~~~~~~~~
npm install lodash
~~~~~~~~

Ora puoi importare la funzionalità di ordinamento di Lodash nel tuo file *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import fetch from 'isomorphic-fetch';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Abbiamo diverse colonne in Table. Ci sono le colonne titolo, autore, commenti e punti. Possiamo definire funzioni di ordinamento dove ogni funzione prendere una lista e restituisce una lista di oggetti ordinati per quella specifica proprietà. Inoltre, avremo bisogno di una funzione di ordinamento di default, che non ordina niente ma restituisce la lista non ordinata. Quello sarà il nostro stato iniziale.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
  POINTS: list => sortBy(list, 'points').reverse(),
};
# leanpub-end-insert

class App extends Component {
  ...
}
...
~~~~~~~~

Puoi vedere come due delle funzioni di ordinamento restituisco una lista al contrario. Questo perché vogliamo vedere gli oggetti con più commenti e più punti in alto, piuttosto che quelli con i valori più bassi, quando ordiniamo la lista per la prima volta.

L'oggetto `SORTS` ci permette ora di referenziare ogni funzione di ordinamento.

Di nuovo il componente App è responsabile per mantenere lo stato dell'ordinamento. Lo stato inziale sarà la funzione di ordinamento di default, che non ordina gli elementi e restituisce la lista in input come output.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
# leanpub-start-insert
  sortKey: 'NONE',
# leanpub-end-insert
};
~~~~~~~~

Ogni volta che una diversa `sortKey` è scelta, diciamo la chiave `AUTHOR`, ordineremo la lista con la funzione di ordinamento dell'oggetto `SORTS` appropriata.

Ora possiamo definire un nuovo metodo di classe nel nostro componente App che semplicemente setta una `sortKey` nello stato locale del componente. Dopodiché, la `sortKey` può essere usata per recuperare la funzione di ordinamento da applicare alla lista.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-start-insert
    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    this.setState({ sortKey });
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

Il prossimo passaggio è quello di passare il metodo e la `sortKey` al componente Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
# leanpub-start-insert
      sortKey
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
# leanpub-start-insert
          sortKey={sortKey}
          onSort={this.onSort}
# leanpub-end-insert
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Il componente Table è responsabile dell'ordinamento della lista. Prende una delle funzioni dell'oggetto `SORT` passando la `sortKey` e passa la lista come input. A questo punto procede con il map sulla lista ordinata.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {SORTS[sortKey](list).map(item =>
# leanpub-end-insert
      <div key={item.objectID} className="table-row">
        ...
      </div>
    )}
  </div>
~~~~~~~~

In teoria la lista sarebbe ordinata da una delle funzione di ordinamente ma il l'ordinamento di default è settato a `NONE` quindi non è stato ancora applicato nessun ordinamento, fin qui nessuno ha eseguito il metodo `onSort()` per cambiare la `sortKey`. Estendiamo il componente Table con una riga contenente gli header delle colonne utilizzando i componente Sort nelle colonne per ordinare ogni singola colonna.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
# leanpub-start-insert
    <div className="table-header">
      <span style={{ width: '40%' }}>
        <Sort
          sortKey={'TITLE'}
          onSort={onSort}
        >
          Title
        </Sort>
      </span>
      <span style={{ width: '30%' }}>
        <Sort
          sortKey={'AUTHOR'}
          onSort={onSort}
        >
          Author
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'COMMENTS'}
          onSort={onSort}
        >
          Comments
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'POINTS'}
          onSort={onSort}
        >
          Points
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        Archive
      </span>
    </div>
# leanpub-end-insert
    {SORTS[sortKey](list).map(item =>
      ...
    )}
  </div>
~~~~~~~~

Ogni componente Sort richiede una specifica `sortKey` e la funzione generale `onSort()`. Internamente chiamere il metodo con la `sortKey` per impostare la chiave specifica.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
~~~~~~~~

Come puoi vedere, il componente Sort riusa il nostro comune componente Button. Al click di un bottone verrà settata la `sortKey` dal metodo `onSort()`. Ora dovremmo essere in grado di ordinare la lista quando avviene un click su un header di colonna.

Possiamo effettuare un piccolo miglioramento all'aspetto della UI. Assegnamo al bottone nel componente Sort un `className`.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
# leanpub-start-insert
  <Button
    onClick={() => onSort(sortKey)}
    className="button-inline"
  >
# leanpub-end-insert
    {children}
  </Button>
~~~~~~~~

Dovrebbe avere un aspetto migliore adesso. Il prossimo obiettivo potrebbe essere quello di implementare anche l'ordinamento inverso. La lista dovrebbe essere ordinata al contrario quando un componente Sort è cliccato due volte. Prima cosa, abbiamo bisogno di definire una proprietà booleana nello stato. L'ordinamento può essere al contrario o no.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
  sortKey: 'NONE',
# leanpub-start-insert
  isSortReverse: false,
# leanpub-end-insert
};
~~~~~~~~

Ora nel metodo di ordinamento, possiamo verificare e impostare se la lista richiede un ordinamento inverso. Si tratta di un ordinamento inverso se la `sortKey` nello stato è la stessa di quella scelta e la proprietà di reverse non è stata già settata a true.

{title="src/App.js",lang=javascript}
~~~~~~~~
onSort(sortKey) {
# leanpub-start-insert
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
# leanpub-end-insert
}
~~~~~~~~

Passiamo anche la proprietà che specifica se l'ordinamento è inverso al componente Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
      sortKey,
# leanpub-start-insert
      isSortReverse
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
          sortKey={sortKey}
# leanpub-start-insert
          isSortReverse={isSortReverse}
# leanpub-end-insert
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Adesso è necessario che Table faccia uso di un arrow function per eseguire l'operazione sui dati.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
# leanpub-end-insert
    <div className="table">
      <div className="table-header">
        ...
      </div>
# leanpub-start-insert
      {reverseSortedList.map(item =>
# leanpub-end-insert
        ...
      )}
    </div>
# leanpub-start-insert
  );
}
# leanpub-end-insert
~~~~~~~~

L'ordinamento inverso dovrebbe funzionare ora.

A questo punto è bene porsi una domanda nel nome di una migliore user experience. Può un utente distinguere quale colonna è correntemente ordinata? Per ora non è possibile. Diamo all'utente un feedback visuale.

Ogni componente Sort prendere già una specifica `sortKey`. Potrebbe essere usata come modo per identificare l'ordinamento selezionato. Possiamo passare la `sortKey` dallo stato del componente al componente Sort come chiave attiva di ordinamento.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
    <div className="table">
      <div className="table-header">
        <span style={{ width: '40%' }}>
          <Sort
            sortKey={'TITLE'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Title
          </Sort>
        </span>
        <span style={{ width: '30%' }}>
          <Sort
            sortKey={'AUTHOR'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Author
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'COMMENTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Comments
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'POINTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Points
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          Archive
        </span>
      </div>
      {reverseSortedList.map(item =>
          ...
      )}
    </div>
  );
}
~~~~~~~~

Ora nel componente Sort basandoci sulla `sortKey` e su `activeSortKey` sappiamo se l'ordinamento è quello attivo correntemente. Diamo al componente Sort un attributo `className` extra, in caso sia quello ordinato, per dare all'utente un feedback visivo.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
  const sortClass = ['button-inline'];

  if (sortKey === activeSortKey) {
    sortClass.push('button-active');
  }

  return (
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass.join(' ')}
    >
      {children}
    </Button>
  );
}
# leanpub-end-insert
~~~~~~~~

Il modo per definire il `className` è un po' articolato, vero? C'è una piccola libreria che fa esattamento qeusto. Per prima cosa installiamola.

{title="Command Line",lang="text"}
~~~~~~~~
npm install classnames
~~~~~~~~

Seconda cosa importiamola all'inizio del file *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import fetch from 'isomorphic-fetch';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Ora possiamo usarla per definire il `className` del componente con classi condizionali.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
# leanpub-start-insert
  const sortClass = classNames(
    'button-inline',
    { 'button-active': sortKey === activeSortKey }
  );
# leanpub-end-insert

  return (
# leanpub-start-insert
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass}
    >
# leanpub-end-insert
      {children}
    </Button>
  );
}
~~~~~~~~

Se eseguiamo i nostri test dovremmo vedere i test di snapshot fallire così come gli unit test del componente Table. Siccome abbiamo modifica la rappresentazione dei nostri componenti possiamo accettare la nuova snapshot, ma dobbiamo correggere gli unit test. Nel file *src/App.test.js* dobbiamo fornire una `sortKey` e il booleano `isSortReverse` per il componente Table.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
# leanpub-start-insert
    sortKey: 'TITLE',
    isSortReverse: false,
# leanpub-end-insert
  };

  ...

});
~~~~~~~~

Ancora una volta potrebbe essere necessario accettare le snapshot dei test che falliscono per il componente Table, perché abbiamo previsto nuove proprietà.

Finalmente il nostro sistema di ordinamento avanzato è completo.

### Esercizi:

* usa una libreria tipo [Font Awesome](http://fontawesome.io/) per indicare l'ordinamento inverso
  * potrebbe essere una freccia verso l'alto o verso il basso vicino ad ogni header in Sort
* leggi di più sulla [libreria classnames](https://github.com/JedWatson/classnames)

{pagebreak}

Hai imparato tecniche avanzate di gestione dei componenti React! Ricapitoliamo gli ultimi capitoli:

* React
  * l'attributo ref per riferirsi a nodi del DOM
  * componenti di ordine superiore come modo per costruire componenti avanzati
  * implementazione di interazioni avanzate in React
  * nomi di classi CSS condizioni con l'aiuto di una piccola libreria
* ES6
  * rest destructuring per estrarre valori da oggetti ed array

Trovi i codici sorgenti nel [repository ufficiale](https://github.com/rwieruch/hackernews-client/tree/4.5).
