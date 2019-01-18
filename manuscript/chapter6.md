# Gestione dello stato in React

Abbiamo già visto le basi sulla gestione dello stato in React nello scorso capitolo, usando lo stato locale dei componenti React. In questo capitolo approfondiremo l'argomento. Vedremo le best practice, come applicarle, e perché potresti considerare di usare una libreria esterna per la gestione dello stato.

## Spostare lo stato (lifting state)

Solo il componente App è uno stateful componente nella nostra applicazione. Gestisce molto dello stato dell'applicazione e delle logiche dei metodi della classe. Inoltre, passiamo molte proprietà al componente Table, la maggior parte delle quali sono usate solo lì. Non è importante che il componente App le conosca, quindi la funzionalità di sort può essere spostata nel componente Table.

Spostare parti di stato da un componente ad un altro è un'operazione conosciuta come *lifting state*. Vogliamo spostare lo stato che non è usato dal componente App nel componente Table, dal componente padre al componente figlio. Per poter avere uno stato e dei metodi di classe nel componente Table, questo deve prima diventare una classe ES6. Rifattorizzare un componente funzionale senza stato in un componente implementato come classe è un'operazione semplice.

Il componente Table come componente funzionale stateless:

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
    ...
  );
}
~~~~~~~~

Il componente Table come classe ES6:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
class Table extends Component {
  render() {
    const {
      list,
      sortKey,
      isSortReverse,
      onSort,
      onDismiss
    } = this.props;

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return (
      ...
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Siccome vogliamo gestire uno stato e dei metodi nel componente, aggiungeremo un costruttore ed uno stato iniziale.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {
# leanpub-start-insert
  constructor(props) {
    super(props);

    this.state = {};
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Adesso possiamo spostare la porzione di stato e i metodi di classe della funzionalità di sort dal componente App giù nel componente Table.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      sortKey: 'NONE',
      isSortReverse: false,
    };

    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    const isSortReverse = this.state.sortKey === sortKey &&
      !this.state.isSortReverse;

    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Ricordati di rimuovere la porzione di stato spostato e il metodo di classe `onSort()` dal componente App.

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
      isLoading: false,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
  }

  ...

}
~~~~~~~~

Possiamo anche rendere più leggero il componente Table. Per fare questo, spostiamo le props che gli vengono passate dal componente App, poiché sono gestite internamente al componente Table.

{title="src/App.js",lang="javascript"}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading
    } = this.state;
# leanpub-end-insert

    ...

    return (
      <div className="page">
        ...
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
# leanpub-start-insert
            list={list}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
        ...
      </div>
    );
  }
}
~~~~~~~~

Nel componente Table usiamo ora il metodo interno `onSort()` e anche lo stato locale al componente:

{title="src/App.js",lang="javascript"}
~~~~~~~~
class Table extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      list,
      onDismiss
    } = this.props;

    const {
      sortKey,
      isSortReverse,
    } = this.state;
# leanpub-end-insert

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
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Title
            </Sort>
          </span>
          <span style={{ width: '30%' }}>
            <Sort
              sortKey={'AUTHOR'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Author
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'COMMENTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Comments
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'POINTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Points
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            Archive
          </span>
        </div>
        { reverseSortedList.map((item) =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Abbiamo applicato un refactoring cruciale spostando la funzionalità e lo stato più vicino ad un altro componente, e reso più leggeri altri componenti. Inoltre anche l'API del componente Table è ora più leggera poiché gestisce internamente la funzionalità di sorting.

Spostare lo stato può essere utile anche nel verso opposto: dal componente figlio al componente padre. Immagina di stare lavorando con lo stato di un componente figlio ma di voler soddisfare un requisito di mostrare lo stato anche nel componente padre. In questo caso dovresti spostare in alto lo stato al componente padre. O ancora, immagina di voler mostrare lo stesso stato in un componente fratello del componente figlio in questione. Anche in questo caso bisogna spostare lo stato verso l'alto al componente padre. A questo punto il componente padre gestisce lo stato localmente e lo espone ad entrambi i componenti figli.

### Esercizi:

* Leggi di più su come [spostare lo stato in React](https://reactjs.org/docs/lifting-state-up.html)
* Leggi di più su come spostare lo stato in [imparare React prima di usare Redux](https://www.robinwieruch.de/learn-react-before-using-redux/)

## Rivisto: setState()

Finora abbiamo utilizzato `setState()` per gestire lo stato interno dei nostri componenti. Passiamo un oggetto alla funzione che aggiorna parzialmente lo stato locale.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ value: 'hello'});
~~~~~~~~

Ma `setState()` non prende in input solo un oggetto. Nella sua seconda versione è possibile passare una funzione per aggiornare lo stato.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

C'è un caso cruciale dove ha senso passare una funzione invece di un oggetto: quando si aggiorna lo stato in base a proprietà dello stato stesso o delle props precedenti. Se in questi casi non si usa una funzione si potrebbero verificare dei bug. Il metodo di React `setState()` è asincrono. React potrebbe raccogliere più chiamate a `setState()` ed eseguirle in batch. Alcune volte lo stato o le props precedenti cambiano tra successive chiamate e quindi non possiamo farci affidamento in una chiamata a `setState()`.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { oneCount } = this.state;
const { anotherCount } = this.props;
this.setState({ count: oneCount + anotherCount });
~~~~~~~~

Immagina che `oneCount` e `anotherCount`, quindi lo stato o le props, cambiano da qualche parte in modo asincrono quando chiami `setState()`. In un'applicazione di dimensioni non trascurabili potresti avere più di una chiamata a `setState()` in giro per l'applicazione. E siccome `setState()` viene eseguita in modo asincrono potresti affidarti nell'esempio a valori vecchi.

Con l'approccio con la funzione, la funzione in `setState()` è una callback che opera su stato e props nel momento  in cui la funzione callback viene eseguita. Sebbene `setState()` è asincrona con una funzione prende lo stato e le props nel momento in cui viene eseguita.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { oneCount } = prevState;
  const { anotherCount } = props;
  return { count: oneCount + anotherCount };
});
~~~~~~~~

Nel nostro codice, il metodo `setSearchTopStories()` si basa sullo stato precedente, quindi questo è un buon esempio dove usare una funzione al posto di un oggetto come parametro alla `setState()`. Adesso il codice è il seguente:

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  const { searchKey, results } = this.state;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  });
}
~~~~~~~~

Qui abbiamo estratto i valori dallo stato, ma aggiornato lo stato in base al valore precedente dello stato in modo asincrono. Adesso useremo l'approccio funzionale per prevenire bug da stato non aggiornato:

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

Possiamo spostare l'intero blocco che abbiamo implementato nella funzione operando direttamente su `prevState` invece che su `this.state`.

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

  this.setState(prevState => {
# leanpub-start-insert
    const { searchKey, results } = prevState;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    return {
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    };
# leanpub-end-insert
  });
}
~~~~~~~~

Questo risolverà eventuali problemi da stato non aggiornato, ma c'è ancora un miglioramento che si può fare. Siccome è una funzione, si può estrarre un'altra funzione per migliorarne la leggibilità. Un altro vantaggio di utilizzare una funzione al posto di un oggetto è che la funzione può vivere anche fuori dal componente. Dobbiamo ancora usare una funzione di ordine superiore per passarle il risultato, siccome vogliamo aggiornare lo stato in base ai risultati recuperati dall'API.

{title="src/App.js",lang="javascript"}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

La funzione `updateSearchTopStoriesState()` deve restituire una funzione. È una funzione di ordine superiore che può essere definita fuori dal componente App. Nota come la firma della funzione cambia leggermente adesso.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const updateSearchTopStoriesState = (hits, page) => (prevState) => {
  const { searchKey, results } = prevState;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  return {
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  };
};
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

L'approccio con una funzione invece dell'oggetto, come parametro alla `setState()`, risolve potenziali bug e aumenta la leggibilità e manutenibilità del codice. Inoltre, diventa testabile fuori dal componente App. Consiglio di esportarla e testarla come esercizio.

### Esercizi:

* Leggi di più su come [usare lo stato correttamente in React](https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly)
* Esporta updateSearchTopStoriesState dal file
  * Scrivici un test che passa un payload (hits e page) e inventa uno stato precedente e infine aspettati un nuovo stato
* Rifattorizza i metodi `setState()` per usare una funzione ma solo quando ha senso, quindi quando l'aggiornamento si basa su props o stato precedente
* Esegui di nuovo i test e verifica che tutto è aggiornato

## Domare lo stato

I precedenti capitoli ti hanno mostrato come la gestione dello stato può essere un argomento cruciale in applicazioni complesse, e come React e molti altri framework per SPA debbano battersi per affrontarlo. Quando le applicazioni diventano complesse, la grossa sfida nelle applicazioni web è domare e controllare lo stato.

In confronto ad altre soluzioni React ha già fatto un grande passo avanti. L'unidirectional data flow e una semplice API per gestire lo stato nei componenti sono indispensabili. Questi concetti facilitano il ragionare sullo stato e sui cambiamenti di stato. Rende anche più facile ragionare a livello di componente e a livello di applicazione.

È possibile introdurre bug operando su uno stato non aggiornato quando si usa un oggetto al posto di una funzione come parametro alla `setState()`. Spostiamo lo stato in giro per condividere o nascondere porzioni di stato tra i componenti come conveniente. Alcune volte un componente ha bisogno di spostarlo in alto, per renderlo visibile anche ad un componente fratello. Oppure il componente è molto lontano nell'albero dei componenti, quindi è necessario condividerlo per l'intero albero di componenti. I componenti sono più coinvolti nella gestione dello stato, siccome la responsabilità principale dei componenti è rappresentare la UI.

Per questo motivo esistono soluzioni standalone per prendersi cura della gestione dello stato. Librerie come [Redux](https://redux.js.org/introduction) o [MobX](https://mobx.js.org/) sono entrambe soluzioni praticabili in applicazioni React. Queste forniscono estensioni, [react-redux](https://github.com/reactjs/react-redux) e [mobx-react](https://github.com/mobxjs/mobx-react) per integrarle nel layer di vista di React. Redux e MobX esulano dallo scopo di questo libro, ma ti incoraggio a studiare i diversi modi di gestire la complessità della gestione dello stato quando le tue applicazioni React diventeranno più complesse.

### Esercizi:

* Leggi di più sulla [gestione esterna dello stato e come impararla](https://www.robinwieruch.de/redux-mobx-confusion/)
* Dai un'occhiata al mio secondo libro sulla gestione dello stato in React, si chiama [Taming the State in React](https://roadtoreact.com/)

{pagebreak}

Hai imparato a gestire lo stato in React in modo avanzato. Ricapitoliamo l'ultimo capitolo:

* **React**
  * Sposta la gestione dello stato su e giù nei componenti adeguati
  * `setState()` può prendere una funzione come parametro per evitare bug da stato non aggiornato
  * Le soluzioni esterne esistenti che aiutano a gestire lo stato

Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.6).
