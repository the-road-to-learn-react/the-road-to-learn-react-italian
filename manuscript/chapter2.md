# Le basi di React

Questo capitolo ti guiderà attraverso le basi di React. Tratta lo stato e le interazioni fra componenti, necessarie quando andiamo oltre a semplici componenti statici. Tratteremo anche i differenti modi per dichiarare un componente e come mantenere i componenti componibili e riusabili.

## Stato locale ai componenti

Lo stato locale ai componenti, conosciuto anche come stato interno ai componenti, ti permette di salvare, modificare ed eliminare proprietà immagazzinate nei tuoi componenti. Un componente implementato con una classe ES6 utilizza un costruttore per inizializzare lo stato locale al componente. Il costruttore è chiamato una sola volta, quando il componente viene inizializzato:

Introduciamo il costruttore della classe.

{title="src/App.js",lang="javascript"}
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

Il componente App è una sottoclasse di `Component`, quindi `extends Component` è presente nella dichiarazione del componente App.

È obbligatorio chiamare `super(props);`. Questo setta `this.props` nel caso in cui tu voglia accedervi dal costruttore. Altrimenti `this.props` sarebbe `underfined` quando viene effettuato l'accesso nel costruttore. In questo caso, lo stato iniziale del componente dovrebbe essere la lista di oggetti di esempio:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://reactjs.org/',
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

Lo stato è legato alla classe usando l'oggetto `this`, così che sia possibile accedere allo stato locale dell'intero componente. Per esempio, può essere utilizzato nel metodo `render()`. Precedentemente abbiamo mappato una lista statica di oggetti nel metodo `render()` che era definita fuori dal componente. Adesso vediamo come utilizzare la lista dallo stato locale al componente.

{title="src/App.js",lang="javascript"}
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

La lista è adesso parte del componente, risiede nel suo stato locale. Potremmo aggiungere, cambiare o rimuovere elementi dalla lista. Ogni volta che tu cambi lo stato di un componente, il metodo `render()` del componente viene eseguito nuovamente. Ecco come si può modificare lo stato locale del componente e vedere il componente ri-renderizzare i dati corretti dallo stato locale.

Stai attento a non mutare lo stato direttamente. Dovresti invece usare un metodo chiamato `setState()` per modificare gli stati. Parleremo di questi concetti in modo più approfondito nel prossimo capitolo.

### Esercizi:

* Sperimenta con lo stato locale
  * Definisci più stato iniziale nel costruttore
  * Utilizza ed accedi allo stato nel metodo `render()`
* Leggi di più sul [costruttore nelle classi ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## L'oggetto inizializzatore in ES6

In JavaScript ES6 è possibile utilizzare un'abbreviazione, nella sintassi delle proprietà, per inizializzare gli oggetti in modo più conciso. Guardiamo la seguente inizializzazione di oggetto:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

Quando il nome della proprità dell'oggetto è lo stesso del nome della variabile, è possibile usare la seguente sintassi:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

Nelle tue applicazioni puoi fare lo stesso. Il nome della variabile lista e il nome della proprietà dello stato condividono lo stesso nome.

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

I nomi di metodo abbreviati sono un'altra feature utile di ES6. In JavaScript ES6, puoi definire un metodo in un oggetto o classe in modo più conciso:

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

Infine, è permesso l'utilizzo di nomi di proprietà calcolati (computed property) in JavaScript ES6:

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

In seguito sarai in grado di usare nomi tramite computed property per allocare valori da una chiave in un oggetto dinamicamente, un modo comodo per generare tabelle di lookup in JavaScript.

### Esercizi:

* Sperimenta con l'oggetto inizializzatore di ES6
* Leggi di più sull'[oggetto inizializzatore di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)

## Unidirectional Data Flow

Adesso abbiamo uno stato locale nel nostro componente App, ma non lo abbiamo ancora manipolato. Lo stato locale è statico, così come lo è il suo componente. Un buon modo per fare esperienza con la manipolazione dello stato è affrontare l'interazione tra componenti.

Faremo pratica con questo concetto aggiungendo un bottone per ogni oggetto nella lista visualizzata. Il bottone mostrerà "Dismiss", siccome il suo scopo sarà di rimuovere un oggetto dalla lista. In un client email, per esempio, sarebbe utile per contrassegnare alcuni email come 'lette', mantenendo quelle non lette separate.

{title="src/App.js",lang="javascript"}
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

Il metodo della classe `onDismiss()` non è ancora definito però. Lo faremo presto, ma per ora concentriamoci sul gestore `onClick` dell'elemento button. Come puoi vedere, il metodo `onDismiss()` nell'handler `onClick` è racchiuso in un arrow function. Puoi usarlo per dare un'occhiata alla proprietà `objectID` dell'oggetto `item` e identificare l'oggetto che verrà eliminato. Un modo alternativo sarebbe quello di definire la funzione all'esterno dell'handler `onClick` e passargli solo la funzione definita. Approfondiremo gli handlers più in dettaglio in seguito.

Notiamo l'uso degli elementi button su più righe e come gli elementi con svariati attributi possono facilmente diventare disorganizzati. L'elemento button è definito su più righe e indentato correttamente per mantenerlo leggibile. Anche se questa pratica non è specifica dello sviluppo React, è uno stile di programmazione che consiglio per ottenere una maggiore chiarezza.

Adesso implementeremo invece la funzionalità `onDismiss()`. Prenderà in input un id per identificare l'oggetto da eliminare. La funzione appartiene alla classe, quindi sarà un metodo di classe: ecco perché ci accediamo con `this.onDismiss()` e non `onDismiss()`. L'oggetto `this` si riferisce all'istanza della classe. Per definire `onDismiss()` come metodo di classe è necessario farne il binding nel costruttore:

{title="src/App.js",lang="javascript"}
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

Nel prossimo passaggio, definiremo la sua funzionalità, vale a dire la sua logica, nella classe:

{title="src/App.js",lang="javascript"}
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

Adesso possiamo definire cose succede all'interno del metodo della classe. Ricorda, l'obiettivo è rimuovere l'elemento identificato dal parametro id dalla lista, e memorizzare la lista aggiornata nello stato locale. La lista aggiornata sarà utilizzata quando verrà rieseguito il metodo `render()` per visualizzarla, dove non sarà più presente l'elemento eliminato.

Puoi rimuovere un elemento da una lista utilizzando la funzionalità [built-in di JavaScript filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), che prende una funzione come suo input. La funzione avrà accesso ad ogni valore della lista, poiché itera su ogni elemento, cosicché tu possa valutarne ognuno in base a determinate condizioni.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const filteredWords = words.filter(function (word) { return word.length > 6; });

console.log(filteredWords);
// expected output: Array ["exuberant", "destruction", "present"]
~~~~~~~~

La funzione restituisce una nuova lista, piuttosto che mutare l'originale, e supporta quindi la convenzione di React di usare strutture dati immutabili.

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

Nel prossimo passaggio estrarremo la funzione e la passeremo alla funzione filter:

{title="src/App.js",lang="javascript"}
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

**Ricorda**: Puoi filtrare più efficacemente utilizzando una arrow function ES6.

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Potresti addirittura renderla inline come abbiamo fatto con l'handler `onClick` del bottone, sebbene ciò potrebbe risultare meno leggibile:

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

La lista non contiene più l'oggetto cliccato, ma lo stato non è stato ancora aggiornato. Utilizziamo il metodo di classe `setState()` per aggiornare la lista nello stato locale del componente:

{title="src/App.js",lang="javascript"}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Esegui di nuovo l'applicazione e prova il bottone "Dismiss". Quello che stai sperimentando è ciò che viene chiamato l'**unidirectional data flow** di React. Un'azione è innescata nel layer di vista con `onClick()`, un'altra funzione o metodo di classe modifica lo stato locale al componente, e infine il metodo `render()` del componente viene eseguito di nuovo per aggiornare la vista.

### Esercizi:

* Leggi di più [sullo stato e i metodi di lifecycle dei componenti React](https://reactjs.org/docs/state-and-lifecycle.html)

## Bindings

È importante imparare alcune cose sul binding nelle classi JavaScript quando si scrivono componenti tramite classi ES6 in React. Nello scorso capitolo abbiamo collegato il metodo di classe `onDismiss()` nel costruttore.

{title="src/App.js",lang="javascript"}
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

Il passaggio di binding è necessario perché il metodo di classe non collega direttamente il `this` all'istanza dell'oggetto. Dimostriamolo con l'aiuto del seguente componente implementato con le classi di ES6:

{title="Code Playground",lang="javascript"}
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

Il componente viene visualizzato correttamente, ma quando si clicca il button, verrà visualizzato `undefined` come log nella console degli strumenti da sviluppatore. Questa è una delle principali fonte di bug che gli sviluppatori incontrano in React. Per rendere `this` accessibile nei metodi di classe, bisogna effettuare il binding dei metodi con `this`.

Nel seguente componente il metodo è correttamente collegato nel costruttore della classe:

{title="Code Playground",lang="javascript"}
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

Il binding dei metodi può avvenire ovunque. Per esempio, può essere fatto nel metodo `render()`:

{title="Code Playground",lang="javascript"}
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

Però evita questa pratica, dal momento che effettua il binding del metodo ogni volta che il metodo `render()` è eseguito. Vale a dire ogni volta che il componente è aggiornato, il che inciderebbe sulle performance della tua applicazione. Mentre effettuare il binding nel costruttore viene fatto una sola volta, quando il componente è istanziato.

Alcuni sviluppatori definiscono la logica dei loro metodi di classe nel costruttore:

{title="Code Playground",lang="javascript"}
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

Evita anche questo approccio, che tende a rendere il costruttore un casino con il tempo. Il costruttore serve ad istanziare una classe e le sue proprietà, quindi le logiche dei metodi dovrebbero essere definite fuori dal costruttore.

{title="Code Playground",lang="javascript"}
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

I metodi di classe possono essere auto-collegati utilizzando le arrow function di ES6:

{title="Code Playground",lang="javascript"}
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

Utilizza questo metodo se il binding ripetitivo ti infastidisce. La documentazione ufficiale di React fa uso del binding dei metodi nel costruttore, quindi questo libro farà altrettanto.

### Esercizi:

* Prova i diversi approcci al binding e fai il console log dell'oggetto `this`
* Leggi di più su [una sintassi alternativa per i componenti React]

## Gestori di eventi (event handlers)

Adesso tratteremo i gestori di eventi negli elementi. Nella nostra applicazione stiamo utilizzando il seguente elemento button per effettuare la cancellazione di un elemento dalla lista.

{title="src/App.js",lang="javascript"}
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

Questa funzione è già abbastanza complessa, dal momento che passa un valore al metodo di classe e dev'essere inclusa in un'altra funzione. Praticamente, è una funzione che deve essere passata ad un handler. Il codice seguente non funzionerebbe, perché il metodo sarebbe eseguito immediatamente quando l'applicazione viene aperta nel browser:

{title="src/App.js",lang="javascript"}
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

Nel codice `onClick={doSomething()}`, il `doSomething()` viene eseguito immediatamente quando l'applicazione viene aperta in un browser. Questo perché l'espressione nell'handler viene valutata. Dal momento che il valore restituito dalla funzione non è più una funzione, non succederà niente quando il bottone verrà cliccato. Mentre l'utilizzo `onClick={doSomething}` dove `doSomething` è una funzione, verrebbe eseguito se il bottone è cliccato. Le stesse regole valgono per il metodo di classe `onDismiss()`.

Inoltre, anche il codice `onClick={this.onDismiss}` non basterebbe, perché il valore `item.objectID` è necessario per identificare l'oggetto che dovrebbe essere cancellato. Lo racchiudiamo in un'altra funzione per passare la proprietà. Questo concetto è chiamato funzioni di ordine superiore (higher-order functions) in JavaScript, e sarà trattato brevemente più avanti.

{title="src/App.js",lang="javascript"}
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

Possiamo anche definire una funzione wrapping all'esterno del metodo, per passare solo la funzione definita al gestore dell'evento. Però, siccome necessita l'accesso all'elemento, è necessario dichiararla all'interno del blocco della funzione map.

{title="src/App.js",lang="javascript"}
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

Una funzione deve essere passata all'handler dell'elemento. Per esempio, prova invece questo codice:

{title="src/App.js",lang="javascript"}
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

Questo metodo sarà eseguito quando apri l'applicazione nel browser, ma non quando clicchi il bottone. Il codice seguente invece sarà eseguito solo quando clicchi il bottone, la funzione sarà scatenata quando il gestore dell'evento è innescato:

{title="src/App.js",lang="javascript"}
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

**Ricorda:** Puoi traformare funzioni in arrow function ES6 come abbiamo fatto con il metodo di classe `onDismiss()`:

{title="src/App.js",lang="javascript"}
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

I neofiti di React hanno spesso difficoltà ad usare funzioni negli event handler, quindi non scoraggiarti se dovessi avere problemi all'inizio. Dovremmo infine ottenere un arrow function ES6 inline con accesso alla property `objectID` dell'oggetto `item`:

{title="src/App.js",lang="javascript"}
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

Usare le arrow function nei gestori di eventi direttamente ha un impatto sulle performance di un'applicazione. Per esempio, l'handler `onClick` per il metodo `onDismiss()` include il metodo in un'altra arrow function per passare l'identificativo dell'elemento. Ogni volta che il metodo `render()` viene eseguito, l'handler instanzia una arrow function di ordine superiore. Questo può avere un impatto sulle performance di un'applicazione, ma nella maggior parte dei casi non succederà. Se dovessi avere una tabella di dati enorme, con 1000 righe, e ogni riga o colonna ha un arrow function in un event handler, in questo caso può aver senso pensare alle implicazioni a livello di performance, dunque implementare un componente Button dedicato che binda il metodo nel costruttore. Prima di questo però, sarebbe ottimizzazione prematura ed è più prudente imparare le basi di React prima di pensare alle ottimizzazioni.

### Esercizi:

* Prova i diversi approcci di utilizzo di funzioni nell'handler `onClick` di un bottone

## Interazioni con form ed eventi

Aggiungeremo un'altra interazione per vedere form ed eventi in React: una funzionalità di ricerca dove l'input del campo di ricerca filtra temporaneamente una lista in base alla property title dell'elemento.

Come primo step definiamo un form con un campo di input nel JSX:

{title="src/App.js",lang="javascript"}
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

Nel seguente scenario digiteremo nel campo di input e la lsta verrà filtra temporaneamente in base alla stringa di ricerca utilizzata nel campo di input. Per filtrare la lista in base al valore del campo di input salveremo il valore del campo di input nello stato locale. Utilizzeremo poi i **synthetic events** di React per accedere al valore nei dati dell'evento.

Definiamo un handler `onChange` per il campo di input:

{title="src/App.js",lang="javascript"}
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

La funzione è legata al componente, quindi è ancora un metodo di classe. Dobbiamo semplicemente effettuare il binding e definire il metodo:

{title="src/App.js",lang="javascript"}
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

Quando utilizzi un handler in un tuo elemento, ottieni l'accesso al synthetic event di React nella callback della firma della funzione.

{title="src/App.js",lang="javascript"}
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

L'evento avrà il valore del campo di input nel suo ogetto target, siamo quindi in grado di aggiornare lo stato locale con la query di ricerca utilizzando `this.setState()`.

{title="src/App.js",lang="javascript"}
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

Non dimenticarti di definire lo stato iniziale per la property `searchTerm` nel costruttore. Il campo di input dovrebbe essere vuoto inizialmente, quindi il suo valore una stringa vuota.

{title="src/App.js",lang="javascript"}
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

Salviamo il valore del campo di input nello stato locale ogni volta che il valore dell'input cambia.

Possiamo assumere che quando aggiorniamo `searchTerm` con `this.setState()`, abbiamo bisogno di passare anche la lista per mantenerne il valore. `this.setState()` di React effettua quello che viene chiamato uno shallow merge, tuttavia, quindi preserva da solo i fratelli delle proprietà dell'oggetto stato quando aggiorna una o più property. La lista dello stato rimarrà uguale dunque, dopo l'aggiornamento della property `searchTerm`.

Ritornando all'applicazione, possiamo notare come la lista non sia ancora aggiornata, in base al valore dell'input salvato nello stato locale. Abbiamo bisogno di filtrare la lista temporaneamente basandoci su `searchTerm`, e abbiamo tutto quello di cui abbiamo bisogno per effettuare questa operazione. Nel metodo `render()`, prima del map sugli elementi della lista, ci applichiamo un filtro. Il filtro sarà applicato solo se `searchTerm` è presente nella property title dell'elemento. Abbiamo già usato la funzionalità di filtro built-in di JavaScript filter, quindi usiamola nuovamente per inserire una funzione di filtro prima della funzione map. La funzione di filtro restituirà un nuovo array, così che la funzione map possa farne uso.

{title="src/App.js",lang="javascript"}
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

Approcciamo la funzione di filtro in un modo diverso questa volta. Vogliamo definire l'argomento filtro, che è la funzione passata alla funzione filter, fuori dalla classe del componente. Non avremo quindi accesso allo stato del componente, quindi non avremo accesso alla property `searchTerm` per valutare la condizione del filtro. Questo significa che avremo bisogno di passare `searchTerm` alla funzione filter, restituendo una nuova funzione per valutare la condizione. Questa funzione che prende un'altra funzione come parametro è chiamata funzione di ordine superiore (higher-order function).

Ha senso avere chiaro cosa siano le funzioni di ordine superiore, poiché React affronta un concetto chiamato componenti di ordine superiore. Imparerai a riguardo più avanti nel libro. Ora concentriamoci sulla funzionalità di filtro.

Prima di tutto, definiamo la funzione di ordine superiore fuori dal componente App.

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function (item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

La funzione prende in input `searchTerm` e restituisce un'altra funzione, poiché la funzione filter prende solo questo tipo come suo input. La funzione restituita ha accesso all'oggetto dell'elemento, poiché è quello passato alla funzione filter.

Sarà utilizzato per filtrare la lista in base alla condizione definita nella funzione, quindi definiamo la condizione:

{title="src/App.js",lang="javascript"}
~~~~~~~~
function isSearched(searchTerm) {
  return function (item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

La condizione confronta il `searchTerm` passato alla funzione con la property title dell'elemento della lista. Possiamo fare questo con la funzionalità built-in di JavaScript `includes`. Quando il pattern combacia, restituisce true e l'elemento rimarrà nella lista; quando il pattern non combacia, l'elemento sarà rimosso dalla lista. Non dimenticarti di ignorare il case su entrambe le stringhe, altrimenti non ci sarà matching tra la query di ricerca 'redux' e il titolo di un elemento 'Redux'. Siccome stiamo lavorando con una lsita immutabile e restituendo una nuova lista usando la funzione filter, la lista originale nello stato locale non sarà modificata.

Abbiamo un po' barato usando una feature di JavaScript ES6, non presente in ES5. Per ES5, utilizza la funzione `indexOf()` per ottenere l'indice dell'elemento nella lista. Quando l'elemento è nella lista `indexOf()` restituirà il suo indice nell'array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Possiamo ulteriormente rifattorizzare utilizzando una arrow function. Renderà la funzione più concisa:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function (item) {
    return item.title.toLowerCase().indexOf(searchTerm.toLowerCase()) !== -1;
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

L'ecosistema React abbraccia molti concetti della programmazione funzionale, spesso utilizzando funzioni che restituiscono funzioni (il concetto è chiamato funzioni di ordine superiore) per passare informazioni. JavaScript ES6 ci permette di esprimere questo in modo ancora più conciso con le arrow function.

Infine, utilizziamo la funzione che abbiamo definito `isSearched` per filtrare la lista. Passeremo la property `searchTerm` dallo stato locale, così restituirà la funzione in input alla funzione filter e filtrerà la lista in base alla condizione del filtro. Dopodiché, mapperà ogni elemento della lista filtrata visualizzando ogni elemento.

{title="src/App.js",lang="javascript"}
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

La funzionalità di ricerca dovrebbe funzionare ora, provala nel browser.

### Esercizi:

* Leggi di più sugli [eventi in React](https://reactjs.org/docs/handling-events.html)
* Leggi di più sulle [funzioni di ordine superiore](https://en.wikipedia.org/wiki/Higher-order_function)

## Destructuring ES6

Il destructuring in JavaScript ES6 fornisce un modo più semplice per accedere alle proprietà di oggetti e array. Confronta il seguente codice rispettivamente in ES5 e ES6:

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

Mentre dobbiamo aggiungere una linea extra ogni volta che accediamo ad una proprietà dell'oggetto in JavaScript ES5, ci vuole solo una riga per JavaScript ES6. Per una migliore leggibilità si consiglia di formattare il codice su più righe quando si effettua il destructuring di un oggetto in più di una proprietà.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

Lo stesso concetto si applica agli array. Si possono destrutturare, anche usando più righe per tenere il codice più facilmente leggibile.

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

Notare come l'oggetto dello stato locale nel componente App può essere destrutturato allo stesso modo. Si può per esempio accorciare la riga di codice con filter e map.

{title="src/App.js",lang="javascript"}
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

Si può fare sia in ES5 che ES6:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

Ma siccome il libro utilizza principalmente ES6, dovresti fare lo stesso.

### Esercizi:

* Leggi di più sul [destructuring di ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Componenti controlled

Abbiamo in precedenza parlato dell'unidirectional data flow di React, la stessa legge si applica al campo di input, che aggiorna lo stato con il `searchTerm` per filtrare la lista. Non appena lo stato cambia, il metodo `render()` viene eseguito di nuovo e utilizza il nuovo `searchTerm` dello stato locale per applicare la condizione di filtro.

Ma non abbiamo dimenticato qualcosa nell'elemento input? Un tag HTML input può avere un attributo `value`. L'attributo value di solito contiene il valore mostrato nel campo di input. In questo caso questo è la property `searchTerm`. Gli elementi dei form come `<input>`, `<textarea>` e `<select>` mantengono il loro stato in plain HTML. Questi modificano il valore internamente quando qualcuno li modifica dall'esterno. In React questi sono chiamati **componenti non controllati** (uncontrolled component), perché gestiscono il proprio stato. Noi invece vogliamo essere sicuri che questi elementi siano **componenti controllati** (controlled components).

Per fare questo setteremo l'altro value dei campi di input, che è già salvato nella property `searchTerm` dello stato, quindi possiamo accedervi da lì:

{title="src/App.js",lang="javascript"}
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

Il ciclo di unidirectional data flow per il campo di input è autocontenuto, e lo stato locale del componente è l'unica fonte di verità per il campo di input.

La gestione dello stato locale e l'unidirectional data flow potrebbero essere nuovi per te, ma una volta che ci farai l'abitudine diverranno un metodo naturale da seguire per le implementazioni in React. React introduce pattern innovativi con l'unidirectional data flow, infatti è stato adottata da diversi framework e librerie per la creazione di applicazioni single page.

### Esercizi:

* Leggi di più sui [form in React](https://reactjs.org/docs/forms.html)
* Leggi di più su [differenti controlled components](https://github.com/the-road-to-learn-react/react-controlled-components-examples)

## Suddivisione in componenti

In questo momento abbiamo un corposo componente App che continua a crescere e potrebbe diventare troppo complesso per essere gestito efficentemente. Abbiamo bisogno di suddividerlo in parti più piccole, più gestibili, creando componenti separati per l'input di ricerca e la lista di elementi.

{title="src/App.js",lang="javascript"}
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

Passeremo ai componenti le proprietà che questi useranno. Il componente App passerà le proprietà gestite nello stato locale e i metodi di classe.

{title="src/App.js",lang="javascript"}
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

Adesso definiremo i componenti assieme al componente App, lo faremo utilizzando le classi di JavaScript ES6. Questi renderizzano gli stessi elementi di prima.

Il primo sarà il componente Search:

{title="src/App.js",lang="javascript"}
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

{title="src/App.js",lang="javascript"}
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

Adesso abbiamo tre componenti sotto forma di classi ES6. Nota come l'oggetto `props` è accessibile attraverso l'istanza della classe utilizzando `this`. Props, abbreviazione per properties, conterrà tutti i valori passati ai componenti quando abbiamo usato il componente App. In questo modo i componenti possono passare proprietà giù nell'albero dei componenti.

Estraendo questi componenti dal componente App questi diventano anche riutilizzabili. Visto che i componenti prendono i loro valori usando l'oggetto `props`, puoi passare props diverse ai tuoi componenti ogni volta che li usi da qualche altra parte.

### Esercizi:

* Scopri più componenti che possono essere suddivisi come i componenti Search e Table, ma aspetta finché non abbiamo visto più concetti prima di implementarli da solo.

## Componenti componibili

La prop `children` è usata per passare elementi ai componenti da un punto più alto dell'albero, questi sono sconosciuti al componente ma rende possibile comporre componenti insieme. Vediamo come ciò funziona passando una stringa di testo come figlio al componente Search.

{title="src/App.js",lang="javascript"}
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

Ora il componente Search può destrutturare la property `children` dall'oggetto `props`, e specificare dove dovrebbe essere visualizzato.

{title="src/App.js",lang="javascript"}
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

Il testo "Search" dovrebbe ora essere visibile a fianco del campo di input. Quando userai il componente Search altrove, potrai specificare differenti entità, siccome non è solo il testo che può essere passato come figlio. Puoi anche passare un elemento, o un albero di elementi, che può essere incapsulato dai componenti come figli. La proprietà children rende possibile intrecciare componenti tra loro.

### Esercizi:

* Leggi di più sul [modello a composizione di React](https://reactjs.org/docs/composition-vs-inheritance.html)

## Componenti riutilizzabili

Componenti riutilizzabili e componibili ti danno il potere di realizzare gerarchie di componenti efficaci, le basi del layer di vista di React. Le ultime sezioni hanno menzionato la riusabilità e adesso possiamo vedere come funziona la riusabilità di componenti come Table e Search nel nostro caso. Anche il componente App è riutilizzabile, visto che può essere istanziato altrove anche lui.

Definiamo un nuovo componente riusabile, un componente Button che può essere riutilizzato spesso:

{title="src/App.js",lang="javascript"}
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

Potrebbe sembrare ridondante dichiarare componenti come questo, ma non lo è. Usiamo un componente `Button` invece di un elemento `button`, che risparmia solo il `type="button"`. Potrebbe non sembrare una grande vittoria, ma queste cose si vedono sulla lunga distanza. Immagina di avere diversi bottoni nella tua applicazione, e di voler cambiare un attributo, uno stile, o il comportamento per solo uno. Senza il componente, dovresti cambiare (rifattorizzare) ognuno. Il componente Button assicura che l'operazione abbia un singolo punto di verità, o un Button per rifattorizzare tutti gli altri in una volta.

Visto che abbiamo già un elemento button, possiamo già usare il componente Button al suo posto. Ometteremo l'attributo type, perché il componente Button lo specifica da sé.

{title="src/App.js",lang="javascript"}
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

Il componente Button si aspetta un proprietà `className` tra le `props`. Questo è un altro attributo derivato di React per l'attributo classe dell'HTML. Non abbiamo passato nessun `className` quando il bottone è stato usato però. Dovrebbe essere più esplicito nel nostro componente Button che il `className` è opzionale, quindi assegneremo un valore di default nel destructuring dell'oggetto.

{title="src/App.js",lang="javascript"}
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

Adesso, ovunque non ci sia una proprietà `className` specificata per il componente Button, il valore sarà una stringa vuota invece di `undefined`.

### Esercizi:

* Leggi di più su [come passare le props in React](https://www.robinwieruch.de/react-pass-props-to-component/)

## Dichiarazione di componenti

Adesso abbiamo quattro componenti come classi ES6, ma l'applicazione può ancora essere migliorata usando componenti funzionali senza stato (functional stateless components) in alternativa ai componenti realizzati tramite classe. Prima di rifattorizzare i nostri componenti, introduciamo i diversi tipi di implementazione per componenti.

* **Functional Stateless Components** sono funzioni che prendono un input e restituiscono un output. Gli input sono le props e l'output è l'istanza del componente in plain JSX. Fin qui, è piuttosto simile ad un componente implementato come classe ES6. Però, i functional stateless components sono funzioni (functional) e non hanno stato locale (stateless). Non puoi accedere o aggiornare lo stato con `this.state` o `this.setState()` perché non c'è un oggetto `this`. Inoltre, non hanno metodi di lifecycle eccetto per il metodo `render()` che sarà applicato implicitamente nel caso dei functional stateless components. Non abbiamo ancora imparato niente sui metodi di lifecycle, ma ne abbiamo già usati due: `constructor()` e `render()`. Il costruttore viene eseguito solo una volta nella vita di un componente, mentre il metodo di classe `render()` viene eseguito una volta inizialmente e in seguito ogni volta che il componente viene aggiornato. Tieni a mente che i functional stateless components non hanno metodi di lifecycle quando arriveremo a trattarli in un capitolo successivo.

* **Componenti come classi ES6** si estendono dalla classe Component di React. L'`extend` aggancia tutti i metodi di lifecycle, disponibili nell'API del Component React, al componente definito. Ecco come eravamo in grado di usare il metodo di classe `render()`. Puoi anche salvare e manipolare lo stato nei componenti implementati tramite classe ES6 usando `this.state` e `this.setState()`.

* **React.createClass** era utilizzato in vesioni vecchie di React ed è ancora utilizzato in applicazioni React che fanno uso di JavaScript ES5. In ogni caso [Facebook l'ha dichiarato deprecato](https://reactjs.org/blog/2015/03/10/react-v0.13.html) in favore di JavaScript ES6. È anche stato aggiunto un [warning di deprecazione dalla versione 15.5](https://reactjs.org/blog/2017/04/07/react-v15.5.0.html), quindi non useremo questa modalità nel libro.

Quando bisogna decidere se utilizzare componenti funzionali senza stato al posto di componenti tramite classi ES6, una buona regola da seguire è di usare i componenti funzionali senza stato quando non si ha bisogno di uno stato locale o di utilizzare metodi di lifecycle. Di solito si usa implementare i componenti come functional stateless componentes, e quando è necessario accedere allo stato o utilizzare metodi di lifecycle, rifattorizzarli in classi ES6. Abbiamo fatto l'opposto finora ma solo per motivi didattici.

Ritornando all'applicazione, vediamo come il componente App faccia uso dello stato locale, quindi deve rimanere una classe ES6. Gli altri tre componenti non hanno stato, quindi non hanno bisogno di accedere a `this.state` o `this.setState()`, e non hanno metodi di lifecycle. Rifattorizzeremo ora il componente Search in uno stateless functional component. Il refactoring dei componenti Table e Button sarà lasciato come esercizio.

{title="src/App.js",lang="javascript"}
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

Le `props` sono accessibili dalla firma della funzione, mentre il valore che viene restituito è un JSX; ma possiamo fare di più con il codice in un componente funzionale senza stato utilizzando il destructuring di ES6. La best practice è di utilizzarlo nella firma della funzione per destrutturare le `props`:

{title="src/App.js",lang="javascript"}
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

Ricorda che le arrow function ES6 ti permettono di mantenere le funzioni concise e di rimuovere il blocco body della funzione. In un body conciso viene aggiunto un return implicito, permettendoci di rimuovere l'istruzione di `return`. Siccome un functional stateless component è una funzione, può essere reso più conciso:

{title="src/App.js",lang="javascript"}
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

L'ultimo passaggio è utile specialmente per applicare le props come input e il JSX come output. Possiamo tuttavia *fare qualcosa* nel mezzo utilizzando un blocco body nell'arrow function ES6:

{title="Code Playground",lang="javascript"}
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

Adesso abbiamo un componente funzionale senza stato molto leggero. Quando avremo bisogno di accedere allo stato locale o a dei metodi di lifecycle potremo rifattorizzarlo in una classe ES6. Quando utilizzano il blocco del body i programmatori tendono a rendere le proprie funzioni più complesse, mentre ometterlo permette di concentrarsi su input e output. Il JavaScript ES6 nei componenti React rende i componenti più leggibili ed eleganti.

### Esercizi:

* Rifattorizza i componenti Table e Button in componenti funzionali senza stato
* Leggi di più su [componenti come classi ES6 e componenti funzionali senza stato](https://reactjs.org/docs/components-and-props.html)

## Lo stile dei componenti

In questa sezione aggiungeremo dello stile base alla nostra applicazione e ai componenti utilizzando i file *src/App.css* e *src/index.css*. Questi file dovrebbero già essere nel tuo progetto, visto che abbiamo effettuato il bootstrap con *create-react-app*. Dovrebbero anche essere già imporati nei file *src/App.js* e *src/index.js*. Il seguente è CSS che può essere copiato e incollato in questi file, ma sentiti libero di usare del tuo stile se sei a tuo agio con CSS.

Prima lo stile generale dell'applicazione:

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial, sans-serif;
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

Secondo lo stile per i componenti nel file App:

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

Ora possiamo usare questo stile con alcuni dei nostri componenti. Ricorda di usare `className` in React invece di `class` come attributo HTML.

Prima cosa applichiamolo al nostro componente App:

{title="src/App.js",lang="javascript"}
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

Adesso al componente Table:

{title="src/App.js",lang="javascript"}
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

Ora l'applicazione ed i componenit sono stati stilizzati con del CSS di base. Inoltre, noi sappiamo che JSX mescola HTML e JavaScript, ora possiamo aggiungere CSS al mix probabilmente. Questo è chiamato inline style, dove si può definire oggetti JavaScript e passarli come attributo style ad un elemento.

Manteniamo la larghezza delle colonne di Table flessibile utilizzando dello stile inline.

{title="src/App.js",lang="javascript"}
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

Lo stile è inline adesso. Definiamo gli oggetti di stile fuori dai nostri elementi per renderli più chiari.

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

Dopodiché possiamo usarli nelle colonne: `<span style={smallColumn}>`. Ci sono diverse opinioni e soluzioni circa lo stile in React, ma il puro CSS inline che abbiamo utilizzato è sufficiente per questo tutorial. Non voglio prendere una decisione in particolare qui, ma voglio offrirti altre opzioni. Puoi leggerle e applicarle da solo:

* [styled-components](https://github.com/styled-components/styled-components)
* [Moduli CSS](https://github.com/css-modules/css-modules) (leggi il mio breve articolo su [come usare moduli CSS in create-react-app](https://www.robinwieruch.de/create-react-app-css-modules/))
* [Sass](https://sass-lang.com/) (leggi il mio breve articolo su [come usare Sass in create-react-app](https://www.robinwieruch.de/create-react-app-with-sass-support/))

Ma se sei nuovo di React ti consiglio di attenerti al puro CSS inline per ora.

{pagebreak}

Hai imparato le basi su come scrivere un'applicazione React! Ricapoliamo l'ultimo capitolo:

* **React**
  * Usa `this.state` e `setState()` per gestire lo stato locale dei tuoi componenti
  * Passaggio di funzioni o metodi di classe all'handler degli elementi
  * Utilizzo di form ed eventi in React per aggiungere interazione
  * L'unidirectional data flow è un concetto importante in React
  * Fare uso di controlled components
  * Comporre componenti con figli e componenti riutilizzabili
  * Utilizzo ed implementazione di componenti come classe ES6 e come funzioni prive di stato
  * Approccio allo stile dei componenti
* **ES6**
  * Le funzioni che sono collegate ad una classe sono metodi di classe
  * Destructuring di oggetti e array
  * Parametri di default
* **Concetti generali**
  * Funzioni di ordine superiore

Di nuovo ha senso fermarsi per una pausa ora, interiorizzare le lezioni e applicarle per conto proprio. Sperimenta con il codice che abbiamo scritto fin qui. Il codice sorgente per questo progetto si trova nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.2).
