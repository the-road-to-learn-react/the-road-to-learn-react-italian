# Componenti React avanzati

Questo capitolo è dedicato all'implementazione di componenti React più avanzati. Imparerai come implementare componenti di ordine superiore (higher-order components) e approfondiremo argomenti più avanzati di React.

## Accedere ad un elemento del DOM

Alcune volte capita di aver bisogno di interagire con dei nodi del DOM in React. L'attributo `ref` offre l'accesso al nodo dei tuoi elementi. Questo è solitamente un anti pattern in React, poiché dovremmo usare la sua modalità dichiarativa per svolgere il lavoro e sfuttare il suo unidirectional data flow, ne abbiamo parlato quando abbiamo introdotto il primo campo di input per la ricerca, ma ci sono casi dove si ha bisogno di accedere ad un nodo del DOM. La documentazione ufficiale ne menziona tre:

* usare l'API del DOM (focus, media playback ecc.)
* Invocare in modo imperativo animazioni di nodi del DOM
* Integrarsi con librerie di terze parti che hanno bisogno del nodo del DOM (es. [D3.js](https://d3js.org/))

Useremo il componente Search come esempio. Quando l'applicazione si renderizza per la prima volta, il campo di input dovrebbe avere il focus. Questo è uno dei casi dove abbiamo bisogno di accedere all'API del DOM. L'attributo `ref` può essere usato sia in componenti funzionali privi di stato sia in componenti implementati tramite classi ES6. In questo esempio avremo bisogno di un metodo di lifecycle, quindi l'approccio è mostrato usando l'attributo `ref` in un componente implementato come classe ES6.

Lo step iniziale è rifattorizzare il componente funzionale privo di stato in un componente come classe ES6.

{title="src/App.js",lang="javascript"}
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

L'oggetto `this` di una classe ES6 ci aiuta a referenziare l'elemento del DOM con l'attributo `ref`.

{title="src/App.js",lang="javascript"}
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
          ref={el => this.input = el}
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

Ora possiamo dare il focus al campo di input quando il componente è caricato usando l'oggetto `this`, il metodo di lifecycle appropriato e l'API del DOM.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Search extends Component {
# leanpub-start-insert
  componentDidMount() {
    if (this.input) {
      this.input.focus();
    }
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
          ref={el => this.input = el}
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Il campo di input dovrebbe avere il focus quando l'applicazione è renderizzata. Accediamo al `ref` in un componente funzionale privo di stato senza l'oggetto `this` usando la seguente modalità:

{title="src/App.js",lang="javascript"}
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
        ref={el => this.input = el}
# leanpub-end-insert
      />
      <button type="submit">
        {children}
      </button>
    </form>
  );
}
~~~~~~~~

Adesso possiamo accedere all'input dell'elemento del DOM. In questo caso non aiuterebbe molto siccome non abbiamo un metodo di lifecycle in un componente funzionale dove innescare il focus. Quindi non andremo ad utilizzare la variabile input qui. In futuro però potresti incontrare casi dove ha senso usare un componente funzionale con un attributo `ref`.

### Esercizi

* Leggi sull'[uso dell'attributo ref in React](https://www.robinwieruch.de/react-ref-attribute-dom-node/)
* Leggi sull'[attributo ref in generale in React](https://reactjs.org/docs/refs-and-the-dom.html)

## Caricamento ...

Adesso torniamo alla nostra applicazione, dove mostreremo un indicatore di caricamento quando viene effettuata una ricerca all'API di Hacker News. La richiesta è asincrona quindi dovremmo mostrare un feedback all'utente che qualcosa sta succedendo. Definiamo un componente Loading riusabile nel file *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Loading = () =>
  <div>Loading ...</div>
~~~~~~~~

Adesso avremo bisogno di una proprietà dove salvare lo stato di loading. In base allo stato di loading decideremo se mostrare il componente.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {
  _isMounted = false;

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

Il valore iniziale della proprietà `isLoading` è false. Non carichiamo niente prima che il componente App sia montato. Quando viene effettuata la richiesta lo stato loading è settato a true. Quando la richiesta sarà stata gestita potremo risettare lo stato loading a false.

{title="src/App.js",lang="javascript"}
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

    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(result => this._isMounted && this.setSearchTopStories(result.data))
      .catch(error => this._isMounted && this.setState({ error }));
  }

  ...

}
~~~~~~~~

Nell'ultimo step usiamo il componente Loading nel componente App. Un rendering condizionale sullo stato di loading deciderà se mostrare il componente Loading o il componente Button. Quest'ultimo è il bottone per cercare altri dati.

{title="src/App.js",lang="javascript"}
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
                onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}
              >
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

Inizialmente il componente Loading verrà mostrato quando l'applicazione viene eseguita, poiché facciamo una richeista nel metodo `componentDidMount()`. Non c'è componente Table, poiché la lista è vuota. Quando si ottiene la risposta dall'API di Hacker News, il risultato è mostrato, lo stato loading è settato a false e il componente Loading sparisce. Il bottone "More" per cercare altri dati appare, se si ricerca altro il bottone sparirà di nuovo e il componente Loading farà la sua comparsa.

### Esercizi:

* Usa una libreria come [Font Awesome](https://fontawesome.com/) per mostrare un'icona di caricamento invece del testo "Loading ..."

## Componenti di ordine superiore

I componenti di ordine superiore (higher-order components, HOC) sono un argomento avanzato di React. Sono una specie di equivalente delle funzioni di ordine superiore. Questi prendono un input, di solito un componente, ma anche argomenti opzionali, e restituiscono un componente in output. Il componente restituito è un versione arricchita di funzionalità del componente di input, che può essere usato nel JSX.

Gli HOC sono usati in diversi casi. Possono preparare proprietà, gestire stati, alterare la rappresentazione di un componente. Una use case è quella di usare gli HOC come helper per del conditional rendering. Immagina di avere una componente List che renderizza una lista di oggetti o nulla se la lista è vuota o nulla. L'HOC potrebbe mascherare il non renderizzare niente quando non c'è una lista. D'altra parte il componente List semplice (non HOC) non ha più bisogno di fare controlli su una lista non esistente e si preoccuperà quindi solo di renderizzare una lista.

Implementiamo un semplice HOC che prende in input un componente e restituisce un componente. Possiamo metterlo sempre nel file *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
~~~~~~~~

Un'utile convezione consiste nel mettere un prefisso `with` ai componenti di ordine superiore. Siccome stiamo usando JavaScript ES6 possiamo esprimere meglio l'HOC con una arrow function di ES6.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const withEnhancement = (Component) => (props) =>
  <Component { ...props } />
~~~~~~~~

Nel nostro esempio il componente di input rimane come componente di output, quindi non succede niente. Il componente di output potrebbe mostrare il componente Loading quando lo stato loading è true, altrimenti mostrare il componente di input. Un rendering condizionale è un ottimo caso d'uso per un HOC.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => (props) =>
  props.isLoading
    ? <Loading />
    : <Component { ...props } />
# leanpub-end-insert
~~~~~~~~

In base alla proprietà loading possiamo applicare un conditional rendering. La funzione restituirà il componente Loading o il componente di input. In generale, può essere molto efficiente effettuare lo spread di un oggetto come l'oggetto props nell'esempio precedente in input ad un componente. Vediamo la differenza nel seguente codice:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// before you would have to destructure the props before passing them
const { firstname, lastname } = props;
<UserProfile firstname={firstname} lastname={lastname} />

// but you can use the object spread operator to pass all object properties
<UserProfile { ...props } />
~~~~~~~~

Abbiamo passato tutte le props inclusa la proprietà `isLoading` facendo lo spreading dell'oggetto come input del componente. Al componente di output potrebbe non interessare della proprietà `isLoading`. Possiamo usare la funzionalità di rest destructuring di ES6 per evitarlo:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />
# leanpub-end-insert
~~~~~~~~

Questo estrare una proprietà dall'oggetto ma mantiene le restanti, e funziona anche con più di una proprietà. Puoi leggerne di più sul [destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment) di Mozilla.

Ora possiamo usare l'HOC nel JSX. Magari vuoi mostrare o il bottone "More" o il componente Loading. Il componente Loading è già racchiuso nell'HOC, ma manca un componente di input. Per mostrare o un componente Button o un componente Loading il Button sarà il componente di input dell'HOC. Il componente di output arricchito di funzionalità sarà il componente ButtonWithLoading.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children,
}) =>
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

Adesso abbiamo definito tutto. Come ultima cosa non ci rimane che usare il componente ButtonWithLoading, che riceve lo stato loading come proprietà addizionale. Mentre l'HOC utilizza la proprietà loading, tutte le altre props sono passate al componente Button.

{title="src/App.js",lang="javascript"}
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
            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}
          >
            More
          </ButtonWithLoading>
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Nota che se esegui nuovamente i test lo snapshot test per il componente App fallirà. La differenza sarà qualcosa del genere nella riga di comando:

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

Puoi o sistemare subito il componente, quando pensi che abbia qualcosa che non va, o accettare la sua nuova snapshot. Poiché abbiamo appena introdotto il componente Loading accetteremo le modifiche alla snapshot dalla riga di comando nel test interattivo.

Gli higher-order components sono un pattern avanzato di React. Questi hanno molti scopi: migliorare la riusabilità dei componenti, migliorare le astrazioni, favorire la composability dei componenti, e la manipolazione di props, stato e vista. Ti suggerisco di leggere questa [leggera introduzione ai componenti di ordine superiore](https://www.robinwieruch.de/gentle-introduction-higher-order-components/), che ti darà un altro approccio per impararli, ti mostrerà un modo elegante per usarli con la programmazione funzionale.

### Esercizi:

* Leggere questa [leggera introduzione ai componenti di ordine superiore](https://www.robinwieruch.de/gentle-introduction-higher-order-components/)
* Sperimenta con gli HOC che abbiamo creato
* Pensa ad un altro caso in cui un HOC avrebbe senso
  * Implementa l'HOC in questione

## Ordinamento avanzato

Abbiamo precedentemente implementato l'interazione di una ricerca client e server side. Dal momento che abbiamo un componente Table, avrebbe senso arricchirlo con altre interazioni avanzate. Con la prossima introdurremo una funzionalità di ordinamento per ogni colonna usando gli header delle colonne del componente Table.

È possibile scriversi da soli le funzioni di ordinamento ma qui preferiremo usare una libreria di utilità come [Lodash](https://lodash.com/) per questi casi. Ci sono altre opzioni ma noi installeremo Lodash per la nostra funzione di sorting:

{title="Command Line",lang="text"}
~~~~~~~~
npm install lodash
~~~~~~~~

Adesso importiamo la funzionalità di sorting di Lodash nel file *src/App.js*:

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Attualmente abbiamo svariate colonne in Table: titolo, autore, numero di commenti e punti. Possiamo definire funzioni di ordinamento dove ognuna prende una lista e restituisce una lista di oggetti ordinati da una specifica proprietà. Inoltre, avremo bisogno anche di una funzione di ordinamento di default che non ordina davvero, ma restituisce la lista non ordinata. Questa sarà il nostro stato iniziale.

{title="src/App.js",lang="javascript"}
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

Due delle funzioni di ordinamento restituiscono una lista al contrario. Questo è per mostrare gli oggetti con commenti e punti più alti, piuttosto che gli oggetti con conteggio più basso quando la lista è ordinata per la prima volta.

L'oggetto `SORTS` ti permette ora di referenziare qualsiasi funzione di sorting.

Il componente App è responsabile per salvare lo stato dell'ordinamento. Lo stato iniziale sarà la funzione di sorting di default, che non ordina nulla e restituisce la lista di input come output.

{title="src/App.js",lang="javascript"}
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

Una volta scelta una diversa `sortKey`, come per esempio la chiave `AUTHOR`, ordiniamo la lista con l'appropriata funzione di sorting dall'oggetto `SORTS`.

Adesso definiamo un nuovo metodo di classe nel componente App che setta una `sortKey` nello stato locale del componente, poi utilizza `sortKey` per recuperare la funzione di ordinamento da applicare alla lista:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {
  _isMounted = false;

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

  ...

# leanpub-start-insert
  onSort(sortKey) {
    this.setState({ sortKey });
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

Il prossimo step è passare il metodo e la `sortKey` al componente Table.

{title="src/App.js",lang="javascript"}
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

Il componente Table è responsabile per l'ordinamento della lista. Prende una delle funzioni da `SORT` in base a `sortKey` e gli passa la lista come input, dopodiché esegue il solito map ma sulla lista ordinata.

{title="src/App.js",lang="javascript"}
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

In teoria la lista dovrebbe essere ordinata da una delle funzioni, ma l'ordinamento di default è settato a `NONE`, quindi niente è ancora ordinato, visto che niente esegue il metodo `onSort()` per cambiare la `sortKey`. Estendiamo Table con una riga di header di colonna che usano componenti Sort nelle colonne per ordinare ogni colonna:

{title="src/App.js",lang="javascript"}
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

Ogni componente Sort prendere una specifica `sortKey` e la funzione generale `onSort()`. Internamente chiama il metodo con la `sortKey` settata alla specifica chiave.

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
~~~~~~~~

Come puoi notare il componente Sort riusa il componente Button. Al click di ogni bottone ogni `sortKey` è settato dal metodo `onSort()`, quindi la lista viene ordinata quando gli header di colonna sono selezionati.

Adesso rifiniremo l'aspetto del bottone nell'header di colonna. Assegniamoli uno specifico `className`:

{title="src/App.js",lang="javascript"}
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

Questo è stato fatto per migliorare la UI. Il primo obiettivo è implementare un sorting inverso. La lista dovrebbe eseguire un ordinamento inverso una volta che il componente Sort è cliccato per la seconda volta. Per prima cosa definiamo lo stato inverso con un booleano. L'ordinamento potrà essere inverso o non inverso.

{title="src/App.js",lang="javascript"}
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

Adesso nel metodo sort potremo valutare se la lista è in ordine inverso. Lo è se la `sortKey` nello stato è la stessa di quella chiamata e se lo stato inverso non è già settato a true.

{title="src/App.js",lang="javascript"}
~~~~~~~~
onSort(sortKey) {
# leanpub-start-insert
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
# leanpub-end-insert
}
~~~~~~~~

Passiamo la prop reverse al componente Table:

{title="src/App.js",lang="javascript"}
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

Table deve avere un blocco body per l'arrow function dove eseguire i dati adesso:

{title="src/App.js",lang="javascript"}
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

Infine, vogliamo dare all'utente un feedback visivo per distinguere per quale colonna è attivo il sorting. Ogni componente Sort ha già una specifica `sortKey`, che può essere usata per identificare l'ordinamento attivo. Passiamo la `sortKey` dallo stato interno al componente come chiave di ordinamento attiva al componente Sort:

{title="src/App.js",lang="javascript"}
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

Adesso l'utente saprà se l'ordinamento è attivo in base alla `sortKey` e ad `activeSortKey`. Diamo al componente Sort un attributo extra come `className` per dare un feedback visivo in caso sia ordinato:

{title="src/App.js",lang="javascript"}
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

Possiamo definire `sortClass` più efficientemente usando una libreria chiamata classnames, installata tramite npm:

{title="Command Line",lang="text"}
~~~~~~~~
npm install classnames
~~~~~~~~

Dopo l'installazione, importiamola in cima al file *src/App.js*.

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Adesso possiamo definire `className` con classi condizionali:

{title="src/App.js",lang="javascript"}
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

Noterai test falliti sia per snapshot che unit test per quanto riguarda il componente Table. Siccome abbiamo consapevolmente cambiato nuovamente la rappresentazione del componente, accetteremo la nuova snapshot, ma avremo comunque bisogno di fixare lo unit test. In *src/App.test.js*, forniamo una `sortKey` e il booleano `isSortReverse` al componente Table.

{title="src/App.test.js",lang="javascript"}
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

Potremmo dover accettare ancora una snapshot per un test che fallisce sul componente Table, poiché abbiamo fornito altre props. L'interazione di ordinamento avanzata è finalmente completa.

### Esercizi:

* Utilizza una libreria come [Font Awesome](https://fontawesome.com/) per indiciare l'ordinamento, inverso o no. Potrebbe essere l'icona di una freccia verso l'alto o il basso accanto all'header Sort
* Leggi sulla [libreria classnames](https://github.com/JedWatson/classnames)

{pagebreak}

Hai imparato delle tecniche avanzate per componenti in React! Ricapitoliamo cosa abbiamo visto:

* **React**
  * L'attributo `ref` per referenziare elementi del DOM
  * I componenti di ordine superiore come modo comune per costruire componenti avanzati
  * Implementazione di interazioni avanzate in React
  * Nomi di classe css condizionali con una libreria di utilità
* **ES6**
  * Rest destructuring per suddividere oggetti ed array

Puoi sempre trovare il codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.5).
