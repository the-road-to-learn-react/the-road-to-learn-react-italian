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

## Revisited: setState()

So far, you have used React `setState()` to manage your internal component state. You can pass an object to the function where you can update partially the internal state.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ foo: bar });
~~~~~~~~

But `setState()` doesn't take only an object. In its second version, you can pass a function to update the state.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

Why should you want to do that? There is one crucial use case where it makes sense to use a function over an object. It is when you update the state depending on the previous state or props. If you don't use a function, the internal state management can cause bugs.

But why does it cause bugs to use an object over a function when the update depends on the previous state or props? The React `setState()` method is asynchronous. React batches `setState()` calls and executes them eventually. It can happen that the previous state or props changed in between when you would rely on it in your `setState()` call.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
~~~~~~~~

Imagine that `fooCount` and `barCount`, thus the state or the props, change somewhere else asynchronously when you call `setState()`. In a growing application, you have more than one 'setState()' call across your application. Since `setState()` executes asynchronously, you could rely in the example on stale values.

With the function approach, the function in `setState()` is a callback that operates on the state and props at the time of executing the callback function. Even though `setState()` is asynchronous, with a function it takes the state and props at the time when it is executed.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
~~~~~~~~

Now, lets get back to your code to fix this behavior. Together we will fix it for one place where `setState()` is used and relies on the state or props. Afterward, you are able to fix it at other places too.

The `setSearchTopStories()` method relies on the previous state and thus is a perfect example to use a function over an object in `setState()`. Right now, it looks like the following code snippet.

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

You extract values from the state, but update the state depending on the previous state asynchronously. Now you can use the functional approach to prevent bugs because of a stale state.

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

You can move the whole block that you have already implemented into the function. You only have to exchange that you operate on the `prevState` rather than `this.state`.

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

That will fix the issue with a stale state. There is one more improvement. Since it is a function, you can extract the function for an improved readability. That's one more advantage to use a function over an object. The function can live outside of the component. But you have to use a higher order function to pass the result to it. After all, you want to update the state based on the fetched result from the API.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

The `updateSearchTopStoriesState()` function has to return a function. It is a higher order function. You can define this higher order function outside of your App component. Note how the function signature changes slightly now.

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

That's it. The function over an object approach in `setState()` fixes potential bugs yet increases readability and maintainability of your code. Furthermore, it becomes testable outside of the App component. You could export it and write a test for it as exercise.

### Exercise:

* read more about [React using state correctly](https://facebook.github.io/react/docs/state-and-lifecycle.html#using-state-correctly)
* refactor all `setState()` methods to use a function
  * but only when it makes sense, because it relies on props or state
* run your tests again and verify that everything is up to date

## Taming the State

The previous chapters have shown you that state management can be a crucial topic in larger applications. In general, not only React but a lot of SPA frameworks struggle with it. Applications got more complex in the recent years. One big challenge in web applications nowadays is to tame and control the state.

Compared to other solutions, React already made a big step forward. The unidirectional data flow and a simple API to manage state in a component are indispensable. These concepts make it easier to reason about your state and your state changes. It makes it easier to reason about it on a component level and to a certain degree on a application level.

In a growing application, it gets harder to reason about state changes. You can introduce bugs by operating on stale state when using an object over a function in `setState()`. You have to lift state around to share necessary or hide unnecessary state across components. It can happen that a component needs to lift up state, because its sibling component depends on it. Perhaps the component is far away in the component tree and thus you have to share the state across the whole component tree. In conclusion components get involved to a greater extent in state management. But after all, the main responsibility of components should be representing the UI, shouldn't it?

Because of all these reasons, there exist standalone solutions to take care of the state management. These solutions are not only used in React. However, that's what makes the React ecosystem such a powerful place. You can use different solutions to solve your problems. To address the problem of scaling state management, you might have heard of the libraries [Redux](http://redux.js.org/docs/introduction/) or [MobX](https://mobx.js.org/). You can use either of these solutions in a React application. They come with extensions, [react-redux](https://github.com/reactjs/react-redux) and [mobx-react](https://github.com/mobxjs/mobx-react), to integrate them into the React view layer.

Redux and MobX are outside of the scope of this book. When you have finished the book, you will get guidance on how you can continue to learn React and its ecosystem. One learning path could be to learn Redux. Before you dive into the topic of external state management, I can recommend to read this [article](https://www.robinwieruch.de/redux-mobx-confusion/). It aims to give you a better understanding of how to learn external state management.

### Exercises:

* read more about [external state management and how to learn it](https://www.robinwieruch.de/redux-mobx-confusion/)
* check out my second ebook about [state management in React](https://roadtoreact.com/)

{pagebreak}

You have learned advanced state management in React! Let's recap the last chapters:

* React
  * lift state management up and down to suitable components
  * setState can use a function to prevent stale state bugs
  * existing external solutions that help you to tame the state

You can find the source code in the [official repository](https://github.com/rwieruch/hackernews-client/tree/4.6).
