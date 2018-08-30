# Le Basi in React

Questo capitolo ti guiderà attraverso le basi di React. Parleremo dello stato e delle interazioni con i componenti, perché i componenti statici sono un po' ostici, non è vero? Inoltre, imparerai differenti modi per dichiarare un componente e le tecniche per mantenere i componenti componibili e riutilizzabili. Fai un bel respiro ed entra nel mondo dei componenti.

## Internal Component State

Gli internal component state, anche conosciuti come local state, ti permettono di salvare, modificare e cancellare le proprietà che sono memorizzate nel tuo componente. La class component ES6 può usare un costruttore per inizializzare gli internal component state in un secondo momento. Il costruttore è chiamato solo una volta quando il componente è inizializzato.

Introduciamo una class constructor.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~


Quando abbiamo a che fare con un costruttore nella tua class component ES6, non abbiamo altro che un mandatario che chiameremo `super();` perché l'App component è una subclasse di `Component`. Ecco perché hai letto `extends Component` nella tua App component. Apprenderai di più su questo concetto più tardi.

Puoi anche chiamarla `super(props);`. Imposta `this.props` nel tuo costruttore nel caso tu voglia avere accesso ad essi nel tuo costruttore. Altrimenti, quando accedi a `this.props` nel tuo costruttore, riceveresti `undefined`. Anche questo argomento verrà ripreso più in là.

Adesso, ritornando all'esempio, lo stato iniziale nel tuo componente dovrebbe essere la lista di item qui sotto.

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

Lo stato è legato alla classe attraverso l'utilizzo dell'oggetto `this`. In questo modo puoi accedere al local state nel tuo intero componente. Per esempio, può essere usato nel metodo `render()`. Precedentemente hai mappato una lista statica di item nel tuo metodo `render()` che è stato definito fuori dal tuo componente. Adesso devi usare la lista dal tuo local state nel tuo componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

La lista è parte del componente adesso. Risiede nell'internal component state. Puoi aggiungere item, cambiare item o rimuovere item all'interno e dalla tua lista. Ogni volta che cambi il tuo component state, il metodo `render()` del tuo componente si avvierà di nuovo. E' in questo modo che puoi semplicemente cambiare il tuo internal component state ed essere sicuro che il componente effettuerà nuovamente il rendering e mostrerà i dati corretti che arrivano dal local state.

Fa attenzione. Non mutare lo stato direttamente. Devi usare un metodo chiamato `setState()` per modificare il tuo stato. Ne parleremo nel prossimo capitolo.

### Esercizi:

* sperimenta il local state
  * definisci più state iniziali nel tuo costruttore
  * utilizza e accedi al tuo stato nel metodo `render()`
* maggiori info su [the ES6 class constructor](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## ES6 Object Initializer

In JavaScript ES6 puoi utilizzare una sintassi short per inizializzare i tuoi oggetti in modo più consio. Dai un'occhiata alla seguente inizializzazione di oggetti:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

Quando il property name nel tuo oggetto ha lo stesso nome della variabile, puoi scriverlo in questo modo:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

Nella tua applicazione, puoi fare lo stesso. Il nome della variabile list e il suo state property name condividono lo stesso nome.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

Un altro piccolo suggerimento per i nomi abbreviati. In JavaScript ES6, puoi inizializzare i metodi nell'oggetto in modo più conciso.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

Ultimo, ma non ultimo, puoi permetterti di usare il computed property name in JavaScript ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

Forse i computed property name non hanno molto senso ancora. Perché dovresti averne bisogno? Nell'ultimo capitolo ti mostrerò come utilizzarli per allocare valori per chiave in un modo dinamico nell'oggetto. C'è un modo pulito per generare tabelle di ricerca in JavaScript.

### Esercizi:

* sperimenta l'inizializzazione degli oggetti in ES6
* approfondisci l'argomento su [ES6 object initializer](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Flusso unidirezionale dei dati

Mettiamo il caso che tu abbia alcuni internal state nel componente della tua App. Non hai ancora manipolato i local state. Un ottimo sistema per manipolare lo state è avere interazione con i componenti.

Aggiungiamo un bottone per ogni item nella lista mostrata. Il bottone dice "Dismiss" e ha il compito di rimuovere l'item dalla lista. Potrebbe essere utile eventualmente quando vuoi tenere una lista di item non letti e cancellare quelli a cui non sei interessato.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Il metodo della classe `onDismiss()` non è definito ancora. Lo faremo tra un momento, ma per adesso il focus dovrebbe essere sull'handler `onClick` dell'elemento del bottone. Come puoi vedere, il metodo `onDismiss()` nell'handler `onClick` è racchiuso all'interno di un'altra funzione. E' un'arrow function. Puoi controllare nella proprietà `objectID` dell'oggetto `item` per identificare l'item che sarà cancellato. Un modo alternativo potrebbe essere quello di definire la funzione fuori dall'handler `onClick` e passare solo la funzione definita all'handler. In un capitolo successivo spiegheremo l'argomento degli handler negli elementi con maggiore dettaglio.

Hai notato le diverse linee nell'elemento button? Questo per rendere più leggibile tutte le istruzioni. Non è obbligatorio. E' solo una raccomandazione di stile che suggerisco caldamente.

Adesso sei in grado di implementare la funzionalità `onDismiss()`. Ci vuole un id per identificare l'item da rimuovere. La funzione è vincolata alla classe e diventa un metodo della classe. Ecco perché accedi con `this.onDismiss()` e non con `onDismiss()`. L'oggetto `this` è l'istanza della tua classe. Per definire `onDismiss()` come metodo di classe, devi vincolarlo al costruttore. I vincoli saranno illustrati in un altro capitolo.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~

Nel prossimo step, dovrai definire le funzionalità, la logica di business, presente nella tua classe. I metodi della classe possono essere definiti nel modo seguente.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~


Adesso sei pronto per definire cosa accade all'interno dei metodi. Di base devi rimuovere l'item identificato con l'ID dalla lista e salvare una lista aggiornata al tuo local state. In seguito, la lista aggiornata sarà utilizzata per far ripartire il metodo `render()` che la mostrerà. L'item rimosso non dovrebbe apparire più.

Puoi rimuovere un item da una lista utilizzando una funzionalità built-in di JavaScript. Il filtro prende una funzione come input. La funzione ha accesso ad ogni valore nella lista, perché itera la lista. In questo modo puoi valutare ogni item nella lista basato sulla condizione dettata dal filtro. Le la valutazione di un item è vera, l'item resta nella lista. Altrimenti sarà filtrato dalla lista. Inoltre è bene sapere che la funzione restituisce una nuova lista e non muta la vecchia lista. Supporta la convenzione in React di avere i dati struttura immutabili.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

Nel prossimo step, puoi estrarre la funzione e passarla al filtro funzione.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Inoltre puoi renderlo più conciso utilizzando l'arrow function di JavaScript ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Puoi anche utilizzare l'inline mode, come hai fatto con l'handler `onClick` del bottone, ma risulterebbe meno leggibile.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

La lista rimuove l'item cliccato adesso. Comunque lo state non è aggiornato ancora. Quindi puoi finalmente usare il metodo della classe `setState()` per aggiornare la lista nell'internal component state.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Avvia ancora una volta la tua applicazione e prova il bottone "Dismiss". Dovrebbe funzionare. Adesso hai acquisito le competenze per comprendere l'**unidirectional data flow** di React. Inneschi un'azione nella tua view con `onClick()`, una funzione o un metodo della classe modifica l'internal component state e il metodo `render()` del componente avvia ancora una volta l'aggiornamento della view.

### Esercizi:

* approfondisci [lo state e il ciclo di vista in React](https://reactjs.org/docs/state-and-lifecycle.html)

## Bindings (vincoli)

E' importante comprendere i vincoli nelle classi JavaScript quando si usano i component class in React ES6. Nel precedente capitolo, hai vincolato il tuo metodo di classe `onDismiss()` all'interno del costruttore.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Perché devi metterlo in quel posto? Lo step per il binding è necessario, perché i metodi della classe non si vincolano automaticamente al `this` dell'istanza della classe. Te lo dimostro con l'aiuto della seguente class component ES6:

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Il componente renderizza bene, ma quando clicchi sul bottone, ti verrà restituito un `undefined` nel console log. Questa è una grande sorgente di errori in React, perché se vuoi accedere a `this.state` nel tuo metodo di classe, non può  essere recuperato perché `this` è `undefined`. Dunque per rendere accessibile `this` nei tuoi metodi di classe, devi vincolare i metodi di classe al `this`.

Nel seguente class component il metodo della classe è correttamente vincolato nel costruttore.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~


Questa volta quando proverai a premere il bottone, l'oggetto `this`, per essere più chiari l'istanza della classe, dovrebbe essere definita e tu dovresti accedere a `this.state`, oppure come più tardi imparerai `this.props`.

Il binding del metodo della classe può essere usato ovunque. Per esempio, può essere usato nel metodo della classe `render()`.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Ma ti consiglio di evitarlo, perché vincolerebbe il metodo di classe ogni volta che il metodo `render()` è richiamato. Di base si avvierebbe ogni volta che il tuo componente si aggiorna. Quando fai il binding del metodo della classe nel costruttore, lo vincoli una sola volta all'inizio quando il componente è istanziato. Questo è il miglior approccio per farlo.

Un altro argomento è definire la logica di business dei loro metodi di classe nel costruttore.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Evita anche questo modo di scrivere codice, perché intaserà il tuo costruttore in breve tempo. Il costruttore serve solo ad istanziare la tua classe con tutte le sue proprietà. Ecco perché la logica di business dei metodi di classe dovrebbe essere definite fuori dal costruttore.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // do something
  }

  doSomethingElse() {
    // do something else
  }

  ...
}
~~~~~~~~

Ultimo, ma non ultimo vale la pena menzionare che i metodi di classe possono essere autovincolati automaticamente senza il binding utilizzando le arrow function di JavaScript ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Se ripetere continuamente il binding nel tuo costruttore ti annoia, puoi andare avanti con questo approccio. La documentazione ufficiale di React suggerisce il metodo di classe vincolato nel costruttore. Ecco perché questo libro farà lo stesso.

### Esercizi:

* prova i differenti approcci di binding ed effettual il console log dell'oggetto `this`

## Event Handler

Il capitolo dovrebbe darti una comprensione approfondita degli event handler negli elementi. Nella tua applicazione, stai usando il seguente elemento bottone per cancellare un item dalla lista.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~


Questo è un caso d'uso complesso, perché devi passare un valore al metodo di classe e quindi devi racchiuderlo in un'altra arrow function. Dunque di base deve essere una funzione che è passata ad un event handler. Il seguente codice non dovrebbe funzionare, perché il metodo di classe dovrebbe essere eseguito immediatamente tutte le volte in cui apri l'applicazione nel browser.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Quando si usa `onClick={doSomething()}`, la funzione `doSomething()` dovrebbe innescarsi immediatamente quando si apre l'applicazione nel browser. L'espressione nell'handler è presa in considerazione. Dal momento in cui il valore restituito della funzione non è più una funzione, niente accadrà quando cliccherai sul bottone. Ma quando userai `onClick={doSomething}` nel momento in cui `doSomething` è una funzione, sarà eseguito quando si cliccherà sul bottone. Le stesse regole saranno applicate per il metodo di classe `onDismiss()` che è utilizzato nella tua applicazione.

Dunque, l'utilizzo di `onClick={this.onDismiss}` non sarebbe sufficiente, perché in qualche modo la proprietà `item.objectID` ha bisogno di essere passata al metodo di classe per identificare l'item che sta per essere cancellato. Ecco perché deve essere racchiuso in un'altra funzione di nascosto nella proprietà. Il concetto è chiamato higher order function in JavaScript e sarà spiegato a breve.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Un workaround sarebbe definire la funzione che funge da involucro da qualche parte fuori e passare solo la funzione definita all'handler. Dal momento in cui ha bisogno di accedere all'item individuale, deve vivere all'interno della map function block.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

Dopo tutto, deve essere una funzione che è passata all'handler dell'elemento. Come esempio, prova questo codice:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Partirà quando aprirai l'applicazione nel browser ma non quando cliccherai sul bottone. Mentre il seguente codice partirà solo quando cliccherai sul bottone. E' una funzione che è eseguita quando inneschi l'handler.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Per mantenerlo conciso, puoi trasformarlo in un JavaScript ES6 arrow function.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Spesso chi è alle prime armi con React ha difficoltà con l'argomento legato all'utilizzo delle funzioni negli event handler. Ecco perché ho provato a spiegarlo con più dettagli. Alla fine, dovresti terminare con il seguente codice nel tuo bottone per avere una JavaScript ES6 arrow function che ha accesso alla proprietà `objectID` dell'oggetto `item`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Un altro argomento utile per le performance, che è spesso menzionato, riguarda le implicazioni dell'utilizzo delle arrow function negli event handler. Per esempio, l'handler `onClick` per il metodo `onDismiss()` racchiude il metodo in un altra arrow function per essere adatta a passare l'item identifier. Quindi ogni volta che il metodo `rendere()` si innesca, l'handler istanzia l'higher order arrow function. Questo processo *può* avere un impatto sulla performance della tua applicazione, ma nella maggior parte dei casi non lo noterai. Immagina di avere una grande tabella di dati con 1000 item e ogni riga o colonna gestiscono una arrow function in un event handler. In questo caso sarebbe giusto pensare alle implicazioni sulla performance e quindi dovresti implementare un componente Button dedicato per vincolare il metodo al costruttore. Ma prima che ciò accada è prematuro ottimizzare. E' meglio focalizzarsi su React.

### Esercizi:

* cerca i differenti approcci sull'utilizzo delle funzioni nell'handler `onClick` del tuo bottone.

## Interazione con Form ed Eventi

Aggiungiammo un'altra interazione per la nostra applicazione dedicandoci ai Form ed Eventi in React. L'interazione riguarderà un motore di ricerca. L'input del campo di ricerca dovrebbe essere utilizzato per filtrare temporaneamente la tua lista basata sulla proprietà titolo di un item.

Nel primo step, dovrai definire un form con un campo input nel tuo JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Nel seguente scenario scriverai qualcosa nel tuo campo e filtrerai la lista attraverso il termine di ricerca che è stato scritto nel campo. Per abilitare il filtro attraverso il campo di ricerca, avrai bisogno di salvare il valore del campo nel tuo local state. Ma come avrai accesso al valore? Puoi usare i **synthetic event**.

Definiamo un handler `onChange` per il campo input.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

La funzione è vincolata al componente e poi ad un metodo di classe. Devi vincolare e definire il metodo.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Quando utilizzi un handler nel tuo elemento, hai accesso al synthetic event di React nella tua callback.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

L'evento ha il valore del campo input nel suo oggetto. Quindi tu avrai la possibilità di aggiornare il local state con il termine di ricerca utilizzando `this.setState()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

Inoltre, non dovresti dimenticare di definire l'initial state per la proprietà `searchTerm` nel costruttore. Il campo input dovrebbe essere vuoto all'inizio e quindi il valore dovrebbe essere una stringa vuota.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Adesso il valore viene intercettato ogni volta che il valore nel campo input cambia.

Qualche considerazione sull'aggiornamento del local state nel componente React. Sarebbe giusto che quando si aggiorna il `searchTerm` con `this.setState()` la lista debba essere passata per preservarla. Ma non è questo il caso. `this.setState()` preserva la proprietà dei fratelli nello state object quando si aggiorna una sola proprietà di esso. In questo modo la list state, anche se hai già disattivato un item, resterà la stessa quando aggiorni la proprietà `searchTerm`.

Torniamo alla nostra applicazione. La lista non è filtrata sulla base del valore del campo input che è salvato nel local state. Di base devi filtrare la lista temporaneamente basata su `searchTerm`. Hai tutto ciò che occorre per filtrare la lista. Come filtrare la lista adesso? Nel tuo metodo `render()`, prima che tu effettui la mappatura sulla lista, puoi applicare un filtro. Il filtro dovrebbe solo valutare se `searchTerm` associa la proprietà title dell'item. Hai già utilizzato la funzionalità built-in di JavaScript a proposito dei filtri, dunque usiamola ancora. La funzione filter restituisce un nuovo array e quindi la funzione map può essere utilizzata in modo corretto.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Approcciamo alla funzione filter in un modo diverso questa volta. Vogliamo definire l'argomento filter fuori dalla classe component ES6. Lì non abbiamo accesso allo state del componente e quindi non abbiamo accesso alla proprietà `searchTerm` per valutare la condizione del filtro. Dobbiamo passare `searchTerm` alla funzione filter e ricevere una nuova funzione per valutare la condizione. Questo processo è chiamato higher order function.

Di solito non avrei menzionato gli higher order functions, ma nel libro React ha senso. Ha senso conoscere le higher order functions perché React tratta un concetto chiamato higher order component. Ne parleremo poi. Adesso focalizziamoci sulla funzionalità filter.

Prima di tutto, devi definire l'higher order function fuori dal tuo componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

La funzione prende `searchTerm` e restituisce un'altra funzione, perché dopo tutto la funzione filter prende una funzione come suo input. La funzione restituita ha accesso all'item perché è la funzione che è passata alla funzione filter. Inoltre la funzione restituita sarà utilizzata per filtrare la lista basata sulla condizione definita nella funzione. Definiamo la condizione.

{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

La condizione dice che puoi associare il pattern in entrata `searchTerm` con la proprietà title dell'item dalla tua lista. Puoi fare questo con la funzionalità built-in `includes` (JavaScript). Solo quando il pattern è associato, restituisci true e l'item resterà nella lista. Quando il pattern non è associato l'item sarà rimosso dalla lista. Ma fa attenzione all'associazione del pattern: non dimenticare il lower case per entrambe le stringhe. Altrimenti potrebbero esserci problemi tra una ricerca di 'redux' e un title dell'item chiamato 'Redux'. Siccome stiamo lavorando su una lista immutabile e sulla restituzione di una nuova lista con l'utilizzo della funzione filter, la lista originale nel local state non sarà modificata.

Ancora una cosa: abbiamo imbrogliato un po utilizzando la funzionalità built-in includes di JavaScript. E' già una feature ES6. Come si lavora nel caso di JavaScript ES5? Dovresti usare la funzione `indexOf()` per recuperare l'indice dell'item nella lista. Quando l'item è nella lista, `indexOf()` restituirà il suo indice nell'array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Un altro po' di refactoring può essere fatto con le arrow function ES6. Per rendere il tutto più conciso:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return item.title.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1;
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~


Qualcuno potrebbe argomentare che la funzione è più leggibile. Personalmente preferisco il secondo tipo. L'ecosistema React utilizza un sacco di concetti di programmazione funzionale. Accadrà spesso che utilizzerai una funzione che restituisce una funzione (higher order functions). In JavaScript ES6, puoi esprimere queste cose in modo più conciso con le arrow function.


Ultimo, ma non ultimo puoi usare la funzione `isSearched()` per filtrare la tua lista. Passi la proprietà `searchTerm` dal tuo local state, restituisce la funzione e filtra la tua lista basata sulla condizione filter. Poi mappa sulla lista filtrata per mostrare un elemento di ogni item della lista.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

La funzionalità di ricerca dovrebbe funzionare ora. Provala nel browser.

### Esercizi:

* approfondisci sugli [eventi React](https://reactjs.org/docs/handling-events.html)
* approfondisci sugli [higher order functions](https://en.wikipedia.org/wiki/Higher-order_function)

## ES6 Destrutturato

C'è un'alternativa in JavaScript ES6 che permette di accedere con più facilità alle proprietà negli oggetti e negli array. E' chiamata destrutturazione. Paragona il seguente snippet come viene gestito in ES5 e ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

Mentre ti tocca aggiungere una linea extra ogni volta che vuoi accedere alla proprietà di un oggetto in JavaScript ES5, puoi farlo in una linea in JavaScript ES6. Una best practice che aumenta la leggibilità è utilizzare più linee quando destrutturi un oggetto in proprietà multiple.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

Stesso discorso vale per gli array. Puoi destrutturarli allo stesso modo. Le linee multiple aiuteranno il tuo codice ad essere più manutenibile e leggibile.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

Forse avrai notato che l'oggetto local state nel componente App può essere destrutturato allo stesso modo. Puoi accorciare il filtro e mappare la linea del codice.

{title="src/App.js",lang=javascript}
~~~~~~~~
  render() {
# leanpub-start-insert
    const { searchTerm, list } = this.state;
# leanpub-end-insert
    return (
      <div className="App">
        ...
# leanpub-start-insert
        {list.filter(isSearched(searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
~~~~~~~~

Ecco come fare in ES5 e ES6:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

Dal momento in cui questo libro utilizza la sintassi JavaScript ES6, ti consiglio di approfondire l'argomento.

### Esercizi:

* approfondisci [la destrutturazione ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Componenti controllati

Hai già affrontato il flusso di dati unidirezionali in React. La stessa legge si applica per i campi input, che aggiornano il local state con `searchTerm` per filtrare la lista. Quando lo state cambia, il metodo `render()` riparte e utilizza `searchTerm` dal local state per applicare il filtro.

Ma non abbiamo dimenticato qualcosa nell'elemento input? Un input tag HTML ha un attributo `value`. L'attributo `value` di solito ha un valore che è mostrato nel campo input. In questo caso sarebbe la proprietà `searchTerm`. Comunque, sembra che non ne abbiamo bisogno in React.

Sbagliato. Gli elementi del form come `<input>` `<textarea>` e `<select>` mantengono il loro stato in plain HTML. Modificano il loro valore internamente nel momento in cui qualcuno li cambia dall'esterno. In React questo è chiamato come **componente incontrollato** perché gestisce il suo stesso state. In React, dovresti assicurarti di utilizzare i **componenti controllati**.

Come fare? Devi solo impostare il value del campo input. Il value è già salvato nella proprietà state `searchTerm`. Dunque perché non accedere da lì?

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
# leanpub-start-insert
            value={searchTerm}
# leanpub-end-insert
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~


Ecco. Il ciclo di flusso dati unidirezionale per il campo input è autosufficiente adesso. L'internal component state è la singola sorgente di verità del campo input.

L'intera gestione dell'internal state e il flusso dati unidirezionali dovrebbe essere un argomento nuovo per te. Ma appena lo utilizzerai, sarà un processo naturale per implementare le attività in React. In generale, React ha portato la novità del flusso di dati unidirezionali nelle applicazioni single page. E' adottato da tantissimi framework e libreria oggi.

### Esercizi:

* approfondisci i [form in React](https://reactjs.org/docs/forms.html)
* scopri di più su [diversi controlled components](https://github.com/the-road-to-learn-react/react-controlled-components-examples)

## Dividere i componenti

Sei alle prese con un'applicazione grande in questo momento. Tenderà ad aumentare e può generare ulteriore confusione. Puoi iniziare a dividerla in componenti più piccoli.

Iniziamo ad utilizzare un componente per l'input di ricerca e un componente per la lista di item.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search />
        <Table />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Puoi passare queste proprietà dei componenti che possono essere usate anche da sole. Nel caso della nostra applicazione c'è bisogno di passare le proprietà gestite nel local state e nei suoi metodi di classe.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Adesso puoi definire i componenti nella tua applicazione. Questi componenti saranno componenti di classe ES6. Renderizzeranno gli stessi elementi come al solito.

Il primo è il componente Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...
}

# leanpub-start-insert
class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
# leanpub-end-insert
~~~~~~~~


Il secondo è il componente Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
# leanpub-end-insert
~~~~~~~~


Adesso hai tre classi di componenti ES6. Forse avrai notato l'oggetto `props` che è accessibile attraverso l'istanza della classe utilizzando `this`. I props, piccoli moduli per le proprietà, hanno tutti i valori che hai passato ai componenti quando li utilizzavi nella tua App. In questo modo, i componenti possono passare le proprietà lungo l'albero del componente.

Estraeondo questi componenti dall'App, avrai la possibilità di riutilizzarli altrove. Dal momento in cui i componenti ricevono i valori utilizzando l'oggetto props, puoi passare ogni volta differenti props ai tuoi componenti quando li utilizzi per qualcos'altro. Questi componenti diventano riutilizzabili.

### Esercizi:

* risolvi ulteriroi componenti che puoi dividere come hai fatto con i componenti Search e Table.
    * ma non farlo adesso, altimenti ti troverai in una serie di conflitti presenti nei prossimi capitoli

## Componenti componibilit

C'è più di una piccola proprietà che è accessibile nei props object: il prop `children`. Puoi utilizzarlo per passare elementi ai tuoi componenti da sopra, che sono sconosciuti al componente stesso, ma permetti la creazione di componenti annidati. Vediamo cosa accade quando passi solo una stringa di testo come `child` al componente "Search".

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Adesso il componente Search può destrutturare le proprietà `children` dai props object. Quindi può specificare dove i `children` saranno mostrati.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-insert
    return (
      <form>
# leanpub-start-insert
        {children} <input
# leanpub-end-insert
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

Il testo "Search" dovrebbe essere visibile nel tuo campo input adesso. Quando utilizzi il componente "Search" da qualche parte, puoi scegliere un testo differente se ti va. Dopo tutto, non passerai solo il testo come `children`. Puoi passare un elemento o alberi di elementi (che saranno incapsulati dai componenti) come `children`. La proprietà `children` rende possibile intreccare componenti all'interno di altri.

### Esercizi:

* approfondisci il [model di composizione di React](https://reactjs.org/docs/composition-vs-inheritance.html)

## Componenti riusabili

I componenti riusabili e componibili ti autorizzano a gestire le gerarchie del componente. Rappresentano il fondamento del view layer in React. Nell'ultimo capitolo si è parlato del termine riutilizzabilità. Puoi riutilizzare i componenti Table e Search. Anche il componente App è riutilizzabile, perché puoi istanziarlo ovunque.

Definiamo un ulteriore componente riutilizzabile, un componente Button, che può essere riutilizzato spesso, in effetti.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

Potrebbe sembrare ridondante dichiarare ancora una volta un componente. Userai un componente `Button` invece di un elemento `button`. Si risparmi solo `type="button"`. Ad eccezione del tipo di attributo devi definire qualcos'altro quando vuoi utilizzare il componente Button. Ma devi pensare a lungo termine. Immagina di avere diversi bottoni nella tua applicazione, ma vuoi cambiarne un attributo, stile o comportamento. Senza un componente dovresti occuparti del refactoring di ogni bottone. Invece il componente Button ti garantisce  di avere solo una singola sorgente di verità. Un componente Button per il refactoring di tutti i bottoni in una volta. Un Bottone regola tutto.

Dal momento in cui hai già un elemento button, devi utilizzare il componente Button. Omette l'attributo type, perché il componente Button lo specifica.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
# leanpub-start-insert
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Il componente Button si aspetta una proprietà `className` nei props. L'attributo `className` è un altro derivato React per l'attributo HTML class. Ma non abbiamo passato alcun `className` quando il Button è stato utilizzato. Nel codice dovrebbe essere più esplicito nel componente Button che `className` è opzionale. Pertanto potremmo assegnare il valore di default durante il destructuring dell'oggetto.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
# leanpub-start-insert
      className = '',
# leanpub-end-insert
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

Adesso, ogni volta che non è specificata la proprietà `className` quando si utilizza il componente Button, il valore sarà una stringa vuota invece di `undefined`.

## Dichiarazioni dei componenti

Da adesso hai quattro classi di componenti ES6. Ma puoi fare di meglio. Permettimi di introdurre i componenti functional stateless come alternativa ai componenti ES6. Prima che tu faccia il refactoring dei tuoi componenti, ti introdurrò a queste differenti tipologie di componenti in React.

* **Componenti Functional Stateless:** Questi componenti sono funzioni che recuperano un input e restituiscono un output. L'input è rappresentato dai props. L'output è una istanza del componente in JSX plain. Finora è abbastanza simile ad una classe di componente ES6. Però, i componenti functional stateless sono funzioni (funzionali) e non hanno local state (stateless). Non puoi accedere o aggiornare lo state con `this.state` o `this.setState()` perché non esiste un oggetto `this`. Inoltre, non hanno metodi lifecycle. Non hai imparato nulla a proposito dei metodi lifecycle, ma ne utilizzi già due: `constructor()` e `render()`. Mentre il costruttore si avvia una sola volta durante il lifetime di un componente, il metodo di classe `render()` si avvia una sola volta all'inizio e ogni volta che il componente si aggiorna. Tieni a mente che il componente function stateless non ha metodi lifecyle, quando te ne occuperai nei capitoli successivi.

* **ES6 Class Components:** Hai già utilizzato questo tipo di componente nei tuoi quattro componenti. Nella definizione di classe, estendono il componente React. `extend` si aggancia a tutti i metodi lifecycle disponibili nel componente API di React. Inoltre puoi salvare e manipolare lo state in nei componenti di classe ES6 utilizzando `this.state` e `this.setState()`.

* **React.createClass:** La dichiarazione del componente era utilizzata in vecchie versioni di React ed è ancora presente nelle applicazioni React in JavaScript ES5. Ma [Facebook li considera deprecati](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html), favorendo JavaScript ES6. Se ne parla anche in un [warning nella versione 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). Non si utilizzeranno in questo libro.

Quindi restano solo due dichiarazioni di componenti. Ma quando utilizzare i componenti functional stateless al posto dei componenti di classe ES6? Una regola è quella di utilizzare i componenti functional stateless quando non hai bisogno di local state o metodi lifecycle. Di solito inizi ad implementare i tuoi componenti come functional stateless. Nel momento in cui hai bisogno di accedere allo state o ai metodi lifecycle, ti occuperai del refactoring e lo trasformerai in componente di classe ES6. Nella nostra applicazione, abbiamo iniziato nell'altro modo per comprendere meglio React.

Torniamo sulla nostra applicazione. Il componente App utilizza l'internal state. Ecco perché deve restare come componente di classe ES6. Ma gli altri tre dei tuoi componenti di classe sono stateless. Non hanno bisogno di accedere a `this.state` o `this.setState()`. Ancora, non hanno metodi lifecycle. Facciamo il refactoring insieme del componente Search e transformiamolo in un componente stateless functional. Il refactoring dei componenti Table e Button saranno il tuo esercizio.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
# leanpub-end-insert
~~~~~~~~

I props sono accessibili nella funzione e il valore restituito compare nel JSX. Ma puoi fare di più in un componente functional stateless. Sei già a conoscenza della destrutturazione in ES6. La migliore pratica è utilizzarla nella funzione per destrutturare i props.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search({ value, onChange, children }) {
# leanpub-end-insert
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Possiamo fare ancora di più. Sei a conoscenza delle arrow function che ti permettono di avere il codice più conciso. Puoi rimuovere il corpo della funzione.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
# leanpub-end-insert
~~~~~~~~

L'ultimo step è stato particolarmente utile per applicare i props come input e JSX come output. Niente in mezzo. Ancora, puoi *fare qualcosa* tra di loro utilizzando un body block nell'arrow function di ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Ma non ne hai bisogno per adesso. Ecco perché puoi mantenere la precedente versione senza il block body. Quando si utilizzano i block body, la gente tende spesso a fare troppe cose nella funzione. Lascia il block body fuori, focalizzati sull'input e sull'output della tua funzione.

Adesso hai un componente functional stateless leggero. Nel momento in cui dovessi avere accesso al suo internal component state o ai metodi lifecycle, dovresti effettuare un refactoring e trasformare tutto in un componente di classe ES6. Inoltre hai visto come JavaScript ES6 può essere utilizzato nei componenti React per renderli più concisi ed eleganti.

### Esercizi:

* fai il refactoring dei componenti Table e Button e trasformali in componenti functional stateless
* approfondisci le [componenti di classe ES6 e componenti functional stateless](https://reactjs.org/docs/components-and-props.html)

## Decorare con stile i componenti

Aggiungiamo un po' di stile alla tua applicazione e ai tuoi componenti. Puoi riutilizzare i file *src/App.css* e *src/index.css*. Questi file dovrebbero essere già presenti nel tuo progetto sin dall'inizio con *create-react-app*. Ho preparato alcuni CSS che puoi semplicemente copiare ed incollare, ma sentiti libero di utilizzare il tuo stile personale a questo punto.

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~

In seconda battuta, un po' di stile per i tuoi componenti

{title="src/App.css",lang="css"}
~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

Adesso puoi utilizzare lo stile in alcuni dei tuoi componenti. Non dimenticare di utilizzare `className` invece di `class` come attributo HTML.

Prima di tutto applica tutto nella tua classe App.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
# leanpub-start-insert
      <div className="page">
        <div className="interactions">
# leanpub-end-insert
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
# leanpub-start-insert
        </div>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-start-insert
      </div>
# leanpub-end-insert
    );
  }
}
~~~~~~~~

Poi nella componente functional stateless Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
# leanpub-start-insert
  <div className="table">
# leanpub-end-insert
    {list.filter(isSearched(pattern)).map(item =>
# leanpub-start-insert
      <div key={item.objectID} className="table-row">
# leanpub-end-insert
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
# leanpub-start-insert
            className="button-inline"
# leanpub-end-insert
          >
            Dismiss
          </Button>
        </span>
# leanpub-start-insert
      </div>
# leanpub-end-insert
    )}
# leanpub-start-insert
  </div>
# leanpub-end-insert
~~~~~~~~

Adesso che hai dato stile alla tua applicazione e ai tuoi componenti con CSS dovrebbe sembrare tutto più decente. Come già sai, JSX miscela HTML e JavaScript. Nessuno ha intenzione di aggiungere CSS al mix. Puoi invece definire oggetti JavaScript e passarli all'attributo style di un elemento.

Diamo un'ampiezza flessibile alla colonna Table usando la tecnica dell'inline style.


{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    {list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
# leanpub-start-insert
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
# leanpub-end-insert
      </div>
    )}
  </div>
~~~~~~~~

Lo style è inline adesso. Puoi definire l'oggetto style fuori dai tuoi elementi per renderli più puliti e leggibili.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

Dopo aver fatto questo puoi utilizzarli nelle tue colonne `<span style={smallColumn}>`.

In genelera, troverai differenti opinioni e soluzioni a riguardo circa lo stile in React. Hai usato CSS puro e l'inline style. E' sufficiente per iniziare.

Non voglio darti un'idea predominante, ma mi piace lasciarti alcune opzioni. Puoi dare un'occhiata e applicare queste opzioni in autonomia. Ma se sei nuovo in React, ti raccomando l'inline style per adesso.

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)

{pagebreak}

Hai appreso le basi per scrivere la tua applicazione React! Facciamo un riassuno:

* React
  * utilizza `this.state` e `setState()` per gestire il tuo internal component state
  * passa le funzioni o i metodi di classe al tuo element handler
  * utilizza form ed eventi in React per aggiungere interazione
  * il flusso di dati unidirezionali è un importante concetto in React
  * sfrutta i componenti controllati
  * componi componenti con i children e componenti riutilizzabili
  * utilizza e implementa componenti di classe ES6 e componenti functional stateless
  * dai uno stile ai tuoi componenti
* ES6
  * le funzioni che sono vincolate ad una classe sono metodi di classe
  * destrutturazione di oggetti e di array
  * parametri di default
* General
  * higher order functions

Adesso ha un senso fare un break. Internalizza l'apprendimento e applicalo ai tuoi progetti. Puoi verificare il codice sorgente scritto un po' di tempo fa. Puoi dare un'occhiata al codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.2).
