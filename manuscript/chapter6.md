# Gestione dello stato in React e alternative

Hai già imparato le basi della gestione dello stato in React nei capitoli precedenti. Questo capitolo approfondisce l'argomento. Imparerai le best practice, come applicarle e perché potresti considerare di usare una libreria di terze parti per la gestione dello stato.

## Spostare lo stato (lifting state)

Solo il component App è un componente ES6 contenente dello stato nella nostra applicazione. Gestisce molto dello stato dell'applicazione e della logica nei suoi metodi di classe. Forse hai notato che passiamo molte proprietà al componente Table. Molte di queste proprietà sono usato esclusivamente nel componente Table. In conclusione si potrebbe discutere sul senso che il componente App conosca queste proprietà.

L'intera funzionalità di ordinamento è usata solo nel componente Table. Si potrebbe spostare nel componente Table, perché il componente App non ha bisogno affatto di conoscerne il funzionamento. Il processo di refactoring parti di stato da un componente ad un altro è conosciuto come *lifting state*. Nel nostro caso, vogliamo spostare lo stato che non è utilizzato nel componente App al componente Table. Quindi spostarlo dal padre al componente figlio.

Per contenere dello stato e metodi di classe il componente Table deve diventare un componente implementato con una classe ES6. Il refactoring da componente funzionale privo di stato a componente classe ES6 è piuttosto semplice.

Il nostro componente Table come componente funzionale privo di stato:

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
    ...
  );
}
~~~~~~~~

Il componente Table come classe ES6:

{title="src/App.js",lang=javascript}
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

    return(
      ...
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Dal momento che vogliamo gestire lo stato e aggiungere metodi al nostro componente, dobbiamo aggiungere un costruttore e uno stato iniziale.

{title="src/App.js",lang=javascript}
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

Ora possiamo spostare lo stato e i metodi riguardanti la funzionalità di ordinamento dal componente App giù al componente Table.

{title="src/App.js",lang=javascript}
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
    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Non dimenticarti di rimuovere la parte di stato spostata e il metodo `onSort()` dal tuo componente App.

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

Inoltre, possiamo rendere l'API del componente Table più leggera. Rimuoviamo le proprietà che gli sono passate dal componente App, perché adesso sono gestite internamente dal componente Table stesso.

{title="src/App.js",lang=javascript}
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
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

Ora nel componente Table possiamo usare il metodo `onSort()` interno e lo stato interno.

{title="src/App.js",lang=javascript}
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

La nostra applicazione dovrebbe funzionare ancora. Ma abbiamo applicato un refactoring cruciale. Abbiamo spostato funzionalità e parti di stato più vicini ad un componente che ne ha bisogno. Altri componenti sono stati resi più leggeri. L'API del componente Table è stata resa molto più snella perché adesso sfrutta la funzionalità di ordinamento interna al componente.

Il processo di spostare lo stato può prendere anche la via opposta, cioè dal figlio al componente padre. Immagina di avere a che fare con lo stato interno di un componente figlio e viene fatta una richiesta di mostrare parti di quello stato anche nel componente padre. In quel caso dovresti portare lo stato più in altro nella gerarchia al componente padre. Ma c'è altro. Immagina di voler mostrare quello stato a componenti fratelli del componente figlio. Di nuovo abbiamo la necessità di spostare lo stato più in alto ad un componente padre. Il componente padre si occuperà di gestire lo stato internamente e di esporlo ad entrambi i figli.

### Esercizi:

* leggi di più su [spostare lo stato in React](https://facebook.github.io/react/docs/lifting-state-up.html)
* leggi di più su spostare lo stato in [imparare React prima di usare Redux](https://www.robinwieruch.de/learn-react-before-using-redux/)

## Approfondimento setState()

Finora abbiamo usato il metodo `setState()` di React per gestire lo stato interno di un componente, passando un oggetto alla funzione aggiornando parzialmente (o completamente, in base all'oggetto passato) lo stato interno.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ foo: bar });
~~~~~~~~

Ma `setState()` non prendere in input necessariamente un oggetto. Nella sua seconda versione possiamo passare una funzione per aggiornare lo stato.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

Perché dovresti farlo? C'è un caso d'uso cruciale dove ha senso usare una funzione piuttosto che un oggetto. Cioè quando l'aggiornamento di stato dipende dal precedente stato o dalle props. Se in questi casi non usi una funzione, la gestione dello stato interno può causare bug.

Ma perché causerebbe errori l'uso di un oggetto al posto di una funzione quando l'aggiornamento dipende dallo stato precedente o da props? Il metodo `setState()` di React è asincrono. React raggruppa chiamate a `setState()` e le esegue ad un certo punto. Può succedere che lo stato precedente o che le props siano cambiate da quando avresti contato sul loro valore nella chiamata a `setState()`.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
~~~~~~~~

Immagina che `fooCount` e `barCount`, o lo stato o le pros, cambino da qualche altra parte, in modo asincrono, al momento della chiamata a `setState()`. In un'applicazione di discrete dimensioni, potresti avere più di una chiamata a `setState()` all'interno della tua applicazione. Siccome `setState()` viene eseguita in modo asincrono, potresti fare affidamento su dati ormai vecchi.

Con l'altro approccio, la funzione passata a `setState()` è una callback che lavora sui dati dello stato e delle props, al momento dell'esecuzione della funzione di callback. Anche se `setState()` è una chiamata asincrona, con la funzione come parametro prende lo stato e le props nel momento in cui viene eseguita.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
~~~~~~~~

Ora, torniamo al codice per aggiustare questo comportamento. Insieme modificheremo un posto dove `setState()` è chiamato e fa affidamento sui valori dello stato o delle props, dopodiché sarai in grado di sistemare lo stesso problema laddove dovesse essere necessario.

Il metodo `setSearchTopStories()` conta sullo stato precedente ed è quindi un perfetto esempio dove usare una funzione piuttosto che un oggetto in una chiamata a `setState()`. Per ora, il suo codice è quello che segue.

{title="src/App.js",lang=javascript}
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

Estraiamo valori dallo stato ma aggiorniamo lo affidandoci ai valori dello stato precedente in modo asincrono. Ora possiamo usare l'approccio con funzione per prevenire bug da dati non aggiornati.

{title="src/App.js",lang=javascript}
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

Possiamo spostare l'intero blocco che abbiamo già implementato dentro alla funzione di callback. Dobbiamo solo cambiare il fatto che operiamo sul `prevState`, lo stato precedente, piuttosto che da quello attuale, `this.state`.

{title="src/App.js",lang=javascript}
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

Questo aggiusta il problema di aggiornamento con dati non aggiornati. Possiamo fare un ulteriore miglioramento. Siccome è una funzione, possiamo estrarla per migliorare la leggibilità. Questo è un altro vantaggio nell'usare una funzione piuttosto che un oggetto. La funzione può vivere fuori dal componente ma dobbiamo usare una funzione di ordine superiore per passargli il risultato.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

La funzione `updateSearchTopStoriesState()` deve restituire una funzione. E' una funzione di ordine superiore. Possiamo definirla fuori dal componente App. Nota come la firma della funzione cambia leggermente adesso.

{title="src/App.js",lang=javascript}
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

Questo è quanto. L'approccio con funzione piuttosto che un oggetto in input al metodo `setState()` evita il verificarsi di eventuali bug e migliore la leggibilità e la manutenibilità del codice. Inoltre diventa testabile fuori dal componente App. Potresti esportarla e scrivere un test per testarla come esercizio.

### Esercizi:

* leggi di più su [React e l'utilizzo corretto dello stato](https://facebook.github.io/react/docs/state-and-lifecycle.html#using-state-correctly)
* rifattorizza tutti i metodi `setState()` usando una funzione come parametro
  * ma solo quando ha senso, cioè quando l'aggiornamento conta sullo stato precedente o sulle props
* esegui ancora i test e verifica che tutto sia aggiornato

## Domare lo stato

I capitoli precedenti hanno mostrato come gestire lo stato può essere un argomento cruciale in applicazioni di grandi dimensioni. In generale, non solo React ma anche gli altri framework per creare SPA (single page application) hanno difficoltà su questo argomento. Le applicazioni stanno diventando molto più complesse in questi ultimi anni. Una grande sfida per le web applications in questi giorni è come domare e controllare lo stato in grosse applicazioni.

In confronto ad altre soluzioni, React ha già fatto un grande passo avanti. Il fluire dei dati in modo unidirezionale e una semplice API per gestire lo stato in un componente sono indispensabili. Questi concetti rendono più semplice ragionare sullo stato e sui suoi aggiornamenti. Rende più facile ragionare a riguardo a livello di componente e fino ad un certo punto anche a livello applicativo.

In un'applicazione che tende a crescere, diventa difficile ragionare sui cambiamenti di stato. E' più facile introdurre bug da operazioni su dati non aggiornati quando si usa un oggetto piuttosto che una funzione nelle chiamate a `setState()`. Ed è necessario spostare lo stato in giro per condividere parti di stato comuni o nascondere parti di stato non necessarie tra componenti. Può succedere che un componente ha bisogno di spostare lo stato al padre, perché i componenti fratelli dipendono da quello stato. Magari il componente è lontano nell'albero dei componenti e quindi è necessario condividere lo stato attraverso l'intero sottoalbero di componenti. In conclusione i componenti vengono coinvolti maggiormente nella gestione dello stato. Ma dopotutto, la principale responsabilità dei componenti, dovrebbe essere quella di rappresentare la UI, no?

Per tutte queste ragioni, esistono soluzioni standalone che si preoccupano della gestione dello stato, e non sono limitate a React. Tuttavia, questo è quello che rende così potente l'ecosistema React. Possiamo usare differenti soluzioni per risolvere i nostri problemi. Per risolvere il problema della gestione dello stato in modo scalabile, potresti aver sentito parlare di alcune librerie tipo [Redux](http://redux.js.org/docs/introduction/) o [MobX](https://mobx.js.org/). Puoi usare una di queste soluzioni in un'applicazione React. Esistono estensioni, [react-redux](https://github.com/reactjs/react-redux) e [mobx-react](https://github.com/mobxjs/mobx-react) che permettono loro l'integrazione con le viste di React.

Redux e MobX sono esclusi dalla portata di questo libro. Alla fine di questo libro, riceverai consigli su come continuare a imparare circa React e il suo ecosistema. Una possibilità è quella di imparare Redux. Prima di buttarti nell'approfondimento dell'argomento delle soluzioni di gestione di stato di terze parti, ti invito a leggere questo [articolo](https://www.robinwieruch.de/redux-mobx-confusion/), che mira a darti una maggiore comprensione di come imparare la gestione di stato con soluzioni di terze parti.

### Esercizi:

* leggi di più su [gestione dello stato esterna a React e come impararla](https://www.robinwieruch.de/redux-mobx-confusion/)
* dai un'occhiata al mio secondo libro sulla [gestione dello stato in React](https://roadtoreact.com/)

{pagebreak}

Hai imparato tecniche di gestione dello stato avanzate in React! Ricapitoliamo quando visto negli ultimi capitoli:

* React
  * spostare la gestione dello stato più in alto e più in basso ai componenti più indicati
  * setState può prendere in input una funzione per prevenire bug da dati non aggiornati
  * soluzioni esterne esistenti che aiutano a domare lo stato

Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/rwieruch/hackernews-client/tree/4.6).
