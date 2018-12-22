# Organizzazione del codice e testing

Il capitolo tratterà di organizzazione del codice in un'applicazione che tende a crescere. Quindi vedremo le best practice per la struttura delle cartelle e dei file. Tratteremo anche il testing, una pratica importante per mantenere il codice robusto ed uno strumento importante per il debug di applicazioni React. Questo capitolo farà un passo indietro dall'applicazione pratica per il momento, così che possiamo concentrarci su questi argomenti in dettaglio prima di andare avanti.

## Moduli ES6: import ed export

In JavaScript ES6 si possono importare ed esportare funzionalità da moduli. Si può trattare di molte cose: funzioni, classi, componenti, costanti, essenzialmente qualsiasi cosa si possa assegnare ad una variabile. I moduli possono essere sia singoli file o intere cartelle con un file index come entry point.

Dopo che abbiamo inizializzato la nostra applicazione con *create-react-app* all'inizio, abbiamo già utilizzato svariate istruzioni di `import` ed `export` nei file iniziali. Le istruzioni di `import` ed `export` aiutano a condividere il codice tra più file. Storicamente c'erano già più soluzioni per questo compito in ambiente JavaScript, ma la cosa era confusionaria perché non c'era un metodo standardizzato per eseguire questo compito. JavaScript ES6 ha infine aggiunto  questo comportamento in modo nativo.

Queste istruzioni abbracciano la filosofia del code splitting, dove distribuiamo il codice su più file per renderlo riusabile e manutenibile. La prima caratteristica è ottenuta poiché possiamo importare un pezzo di codice in tutti i file in cui ne abbiamo bisogno, la seconda poiché esiste solo una fonte dove bisogna manutenere il pezzo di codice in questione.

Vogliamo pensare anche all'incapsulamento del codice, siccome non tutte le funzionalità hanno bisogno di essere esportate da un file. Alcune di queste potrebbero essere usate solo nei file dove sono state definite. Gli export di un file sono essenzialmente l'API pubblica del file, dove solo le funzionalità esportate saranno disponibili per essere riusate altrove. Questo segue la best practice dell'incapsulamento.

I seguenti esempi illustrano queste istruzioni condividendo una o più variabili in due file. Alla fine l'approccio può essere usato anche su più file e può essere condiviso più che semplici variabili.

L'azione dell'esportare una o più variabili è chiamata export:

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'Robin';
const lastname = 'Wieruch';

export { firstname, lastname };
~~~~~~~~

E importarle in un altro file con un path relativo al primo file.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname, lastname } from './file1.js';

console.log(firstname);
// output: Robin
~~~~~~~~

È anche possibile importare tutte le variabile esportate da un altro file come un unico oggetto.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import * as person from './file1.js';

console.log(person.firstname);
// output: Robin
~~~~~~~~

Gli import possono avere degli alias, che sono necessari quando importiamo funzionalità da più file che potrebbero avere esportato uno stesso nome.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname as username } from './file1.js';

console.log(username);
// output: Robin
~~~~~~~~

C'è anche l'istruzione `default`, che può essere usata per alcuni casi:

* esportare ed importare una singola funzionalità
* per fare risaltare la funzionalità principale dell'API esportata da un modulo
* per avere un funzionalità di import di fallback

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const robin = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

export default robin;
~~~~~~~~

Per importare un export di default è necessario non mettere le parentesi graffe.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer from './file1.js';

console.log(developer);
// output: { firstname: 'Robin', lastname: 'Wieruch' }
~~~~~~~~

Il nome dell'import può essere diverso da quello usato nell'export di default, e può essere usato con export nominali e istruzioni di import:

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'Robin';
const lastname = 'Wieruch';

const person = {
  firstname,
  lastname,
};

export {
  firstname,
  lastname,
};

export default person;
~~~~~~~~

Importare il default o gli export nominativi in un altro file:

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// output: { firstname: 'Robin', lastname: 'Wieruch' }
console.log(firstname, lastname);
// output: Robin Wieruch
~~~~~~~~

Puoi risparmiare delle righe di codice esportando le variabili direttamente con gli export nominativi.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
export const firstname = 'Robin';
export const lastname = 'Wieruch';
~~~~~~~~

Queste sono le funzionalità principali dei moduli ES6. Questi aiutano ad organizzare il codice, manutenerlo, e a realizzare API di moduli riutilizzabili. Puoi anche esportare ed importare funzionalità per testarle e lo faremo in un capitolo successivo.

### Esercizi:

* Leggi sugli [import ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* Leggi sugli [export ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## Organizzazione del codice con i moduli ES6

Potresti chiederti perché non abbiamo seguito la buona pratica di dividere i componenti in file differenti nel file *src/App.js*. Nel file abbiamo già diversi componenti che potrebbero essere definiti in loro file/cartelle (moduli). Per lo scopo di imparare React è più pratico mantenere questi componenti in un unico posto. Una volta che la tua applicazione React raggiunge certe dimensioni dovresti considerare di dividere questi componenti in diversi moduli così che possa crescere in modo produttivo.

Di seguito propongo alcune strutture dei moduli che potrebbe essere applicate. Puoi provarle più avanti come esercizio ma continueremo a sviluppare la nostra app con il solo file *src/App.js* per mantenere le cose semplici.

Questa è una possibile struttura dei moduli:

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App.js
  App.test.js
  App.css
  Button.js
  Button.test.js
  Button.css
  Table.js
  Table.test.js
  Table.css
  Search.js
  Search.test.js
  Search.css
~~~~~~~~

Questa separa ogni componenti in dei file propri ma non sembra troppo promettente. Possiamo notare molta duplicazione nei nomi, dove solo l'estensione del file differisce. Un'altra struttura potrebbe essere:

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App/
    index.js
    test.js
    index.css
  Button/
    index.js
    test.js
    index.css
  Table/
    index.js
    test.js
    index.css
  Search/
    index.js
    test.js
    index.css
~~~~~~~~

Adesso sembra più chiara di prima. Il nome index per i file li descrive come file entry point della cartella. Ma questa è solo una naming convention, puoi decidere tu quali nomi utilizzare. In questa struttura un componente è definito della sua dichiarazione nel file JavaScript, ma anche del suo file di stile e dai test. Se dovessi aver problemi a trovare i componenti in questo modo, cercandoli nel tuo editor/IDE, prova a cercare i path piuttosto che i file (es. cerca "Table index").

Un altro passaggio pure essere quello di estrarre le variabili costanti dal componente App, quelle usate per comporre le URL dell'API di Hacker News.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
# leanpub-start-insert
  constants/
    index.js
  components/
# leanpub-end-insert
    App/
      index.js
      test.js
      index.css
    Button/
      index.js
      test.js
      index.css
    ...
~~~~~~~~

Natualmente i moduli sarebbe divisi in *src/constants/* e *src/components/*. Il file *src/constants/index.js* potrebbe essere come il seguente:

{title="Code Playground: src/constants/index.js",lang="javascript"}
~~~~~~~~
export const DEFAULT_QUERY = 'redux';
export const DEFAULT_HPP = '100';
export const PATH_BASE = 'https://hn.algolia.com/api/v1';
export const PATH_SEARCH = '/search';
export const PARAM_SEARCH = 'query=';
export const PARAM_PAGE = 'page=';
export const PARAM_HPP = 'hitsPerPage=';
~~~~~~~~

Il file *App/index.js* può a questo punto importare le variabili per utilizzarle.

{title="Code Playground: src/components/App/index.js",lang="javascript"}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
} from '../../constants/index.js';

...
~~~~~~~~

Quando utilizzi la naming convention  *index.js* puoi omettere il nome del file dal path relativo.

{title="Code Playground: src/components/App/index.js",lang="javascript"}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
# leanpub-start-insert
} from '../../constants';
# leanpub-end-insert

...
~~~~~~~~

La naming convention del file *index.js* è una convenzione introdotta nel mondo Node.js, dove il file index è l'entry point di un modulo. Questo descrive l'API pubblica del modulo. I moduli esterni possono utilizzare solo il file *index.js* per importare codice condiviso dal modulo. Considera la seguente struttura di moduli per dimostrazione:

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  App/
    index.js
  Buttons/
    index.js
    SubmitButton.js
    SaveButton.js
    CancelButton.js
~~~~~~~~

La cartella *Buttons/* ha diversi componenti Button definiti in file distinti. Ogni file può fare un `export default` dello specifico componente, renderlo disponibile in *Buttons/index.js*. Il file *Buttons/index.js* tutte le differenti rappresentazioni di bottoni e le esporta come API pubblica del modulo.

{title="Code Playground: src/Buttons/index.js",lang="javascript"}
~~~~~~~~
import SubmitButton from './SubmitButton';
import SaveButton from './SaveButton';
import CancelButton from './CancelButton';

export {
  SubmitButton,
  SaveButton,
  CancelButton,
};
~~~~~~~~

Adesso il file *src/App/index.js* può importare i bottoni dall'API pubblica del modulo dal file *index.js*.

{title="Code Playground: src/App/index.js",lang="javascript"}
~~~~~~~~
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
~~~~~~~~

Invece può essere visto come una bad practice raggiunge file diversi da *index.js* nel modulo, poiché viola le regole dell'incapsulamento.

{title="Code Playground: src/App/index.js",lang="javascript"}
~~~~~~~~
// bad practice, don't do it
import SubmitButton from '../Buttons/SubmitButton';
~~~~~~~~

Abbiamo rifattorizzato il nostro codice in un modulo con i vincoli di incapsulamento. Ricordo che manterremo le cose semplici nel libro, ma dovresti sempre rifattorizzare il tuo codice per tenerlo pulito.

### Esercizi:

* Quando avrai finito il libro rifattorizza il file *src/App.js* in componenti divisi in moduli

## Snapshot test con Jest

Testare il codice è essenziale nella programmazione e dovrebbe essere visto come obbligatorio. Vogliamo mantenere la qualità del codice alta ed essegre sicuri che tutto funzioni prima di usarlo in produzione. Useremo la piramide del testing come guida.

La piramide del testing include test end-to-end, test d'integrazione e test d'unità. Un test d'unità (unit test) è per piccoli ed isolati blocchi di codice, come una singola funzione. Tuttavia alcune volte le unità funzionano bene in isolamento ma non in combinazione con altre unità, quindi necessitano di essere testate come un gruppo. Qui è dove i test d'integrazione possono aiutarci a capire se le unità funzionano bene insieme. Un test end-to-end è una simulazione di uno scenario reale, come per esempio il setup automatico di un browser che simula il processo di login in un'applicazione web. Laddove gli unit test sono veloci e facili da scrivere e mantenere, i test end-to-end sono all'opposto dello spettro.

Quello che vogliamo sono molti test d'unità che coprono le funzioni isolate. Dopodiché, potremmo avvalerci di alcuni test d'integrazione per assicurarci che la maggior parte delle funzioni funzionino insieme come ci si aspetta. Infine, potremmo aver bisogno di pochi test end-to-end per simulare gli scenari critici.

La base del testing in React sono i test dei componenti, in parte come unit test e in parte come test di snapshot (snapshot tests). I test d'unità per i nostri componenti saranno trattati nel prossimo capitolo con una libreria di nome Enzyme. In questa sezione ci focalizzeremo invece sugli snapshot test usando [Jest](https://jestjs.io/).

Jest è un framework di testing JavaScript usato da Facebook. È utilizzato per il test di componenti dalla community React. Fortunatamente *create-react-app* contiene già Jest, quindi non dovremo fare niente per farlo funzionare.

Prima di testare il primo componente, notiamo come bisogna esportare il componente interessato, un questo caso il file *src/App.js*. Dopodiché sarà possibile testarlo in un altro file.

{title="src/App.js",lang="javascript"}
~~~~~~~~
...

class App extends Component {
  ...
}

...

export default App;

# leanpub-start-insert
export {
  Button,
  Search,
  Table,
};
# leanpub-end-insert
~~~~~~~~

Nel file *App.test.js* troverai il nostro primo test creato da *create-react-app*. È un semplice test che verifica che il componente App venga renderizzato senza errori.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
~~~~~~~~

Il blocco "it" descrive un caso di test. È formato da una descrizione del test e quando viene testato può avere successo o fallire. È inoltre possibile racchiuderlo in un blocco "describe" che definisce la suite di test. Una test di suite include svariati blocchi "it" per uno specifico componente. Vedremo il blocco "describe" più avanti. Entrambi i blocchi sono usati per separare ed organizzare i casi di test.

Nota come la funzione `it` è riconosciuta nella community JavaScript come la funzione dove viene eseguito un singolo test. Tuttavia in Jest è più spesso trovato con la funzione alias `test`.

Puoi eseguire i casi di test usando lo script test interattivo di *create-react-app* dalla linea di comando. Otterrai l'output per tutti i test nell'intefaccia della linea di comando.

{title="Command Line",lang="text"}
~~~~~~~~
npm test
~~~~~~~~

Jest ti permette di scrivere snapshot test. Questi test effettuano una snapshot del componente renderizzato e la eseguono per differenza con le snapshot future. Quando una snapshot futura cambia, il test te lo segnalerà. A questo punto ci sono due possibilità: accettare le modifiche alla snapshot, magari perché hai cambiato di proposito l'implementazione del componente, o rifiutare le modifiche e verificare il perché della differenza. Questo è un ottimo complemento agli unit test, poiché si testano solo le differenze degli output renderizzati. Questo non aggiunge grossi costi di manutenzione, considerato che si possono accettare le nuove snapshot a seguito di modifiche intenzionali.

Jest salva le snapshot in una cartella così che possa validare le differenze rispetto a future snapshot. Questo permette anche di condividere le snapshot con un team se si usa un sistema di versionamento del codice come git.

Prima di scrivere il nostro primo test di snapshot con Jest bisogna installare una libreria:

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev react-test-renderer
~~~~~~~~

Adesso possiamo estendere i test del componente App con il nostro primo snapshot test. Per prima cosa importiamo la nuova funzionalità dal pacchetto node che abbiamo installato e racchiudiamo il precedente bloccato "it" in un descrittivo blocco "describe". In questo caso la test di suite è solo per il componente App.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
# leanpub-start-insert
import renderer from 'react-test-renderer';
# leanpub-end-insert
import App from './App';

# leanpub-start-insert
describe('App', () => {

# leanpub-end-insert
  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
    ReactDOM.unmountComponentAtNode(div);
  });
# leanpub-start-insert

});
# leanpub-end-insert
~~~~~~~~

Adesso possiamo implementare il nostro primo snapshot test usando un blocco "test":

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App from './App';

describe('App', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
    ReactDOM.unmountComponentAtNode(div);
  });

# leanpub-start-insert
  test('has a valid snapshot', () => {
    const component = renderer.create(
      <App />
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

# leanpub-end-insert
});
~~~~~~~~

Esegui di nuovo i test e osserva se hanno successo o falliscono. Una volta che cambi l'output del blocco render nel componente App, lo snapshot test dovrebbe fallire. Puoi quindi decidere se aggiornare la snapshot o se investigare sull'output del componente App.

La funzione `renderer.create()` crea una snapshot del componente App, lo renderizza virtualmente e poi salva il DOM nella snapshot. In seguito ci si aspetta che le snapshot create siano uguali alla versione precedente dell'ultimo test. Con questo meccanismo ci assicuriamo che il DOM resti lo stesso e che non cambi per sbaglio.

Aggiungiamo dei test anche per gli altri componenti. Primo, il componente Search:

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import App, { Search } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Search', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Search>Search</Search>, div);
    ReactDOM.unmountComponentAtNode(div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Search>Search</Search>
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Il componente Search ha due test simili a quelli del componente App. Il primo renderizza semplicemente il componente Search nel DOM e verifica che non ci siano errori durante il processo di rendering. Se si verificasse un errore il test fallirebbe nonostante non ci siano assertion (es. expect, match, equal) nel blocco di test. Il secondo è un test di snapshot ed è utilizzato per salvare una snapshot del componente renderizzato e confrontarla con la versione precedente. Questo fallisce quando la snapshot cambia.

Secondo testiamo il componente Button visto che si applicano le stesso logiche del componente Search.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Button', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Button>Give Me More</Button>, div);
    ReactDOM.unmountComponentAtNode(div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Button>Give Me More</Button>
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Infine il componente Table, al quale passiamo del props iniziali per renderizzarlo con una lista di esempio.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
...
# leanpub-start-insert
import App, { Search, Button, Table } from './App';
# leanpub-end-insert

...

# leanpub-start-insert
describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Table { ...props } />, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Table { ...props } />
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Gli snapshot test di solito sono piuttosto semplici. Ci vogliamo solo assicurare che l'output del componente non cambi. Quando cambia dobbiamo decidere se accettare i cambiamenti o sistemare il componente.

### Esercizi:

* Guarda come fallisce un test di snapshot una volta che cambi il valore di ritorno nel metodo `render()`
  * Accetta o rifiuta i cambiamenti alla snapshot
* Tieni aggorni gli snapshot test quando l'implementazione dei componenti cambierà nei prossimi capitoli
* Leggi su [Jest in React](https://jestjs.io/docs/en/tutorial-react)

## Unit test con Enzyme

[Enzyme](https://github.com/airbnb/enzyme) è una libreria di testing sviluppata da Airbnb per manipolare, attraversare ed eseguire assert su componenti. È utilizzata per realizzare unit test in modo complementare ai test di snapshot in React. Per prima cosa dobbiamo installarla insieme alle sue estensioni, visto che non è presente in *create-react-app*.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16
~~~~~~~~

Seconda cosa, la importiamo nel file di test e inizializziamo il suo adapter.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
# leanpub-end-insert
import App, { Search, Button, Table } from './App';

# leanpub-start-insert
Enzyme.configure({ adapter: new Adapter() });
# leanpub-end-insert
~~~~~~~~

Adesso possiamo scrivere il nostro primo test nel blocco "describe" per il componente Table. Useremo `shallow()` per renderizzare il componente ed effettuare un assert che gli siano stati passati due elementi. La verifica controlla semplicemente che l'elemento abbia due elementi con classe `table-row`.

{title="src/App.test.js",lang="javascript"}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme, { shallow } from 'enzyme';
# leanpub-end-insert
import Adapter from 'enzyme-adapter-react-16';
import App, { Search, Button, Table } from './App';

...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  ...

# leanpub-start-insert
  it('shows two items in list', () => {
    const element = shallow(
      <Table { ...props } />
    );

    expect(element.find('.table-row').length).toBe(2);
  });
# leanpub-end-insert

});
~~~~~~~~

Shallow renderizza il componente senza i suoi componenti figli, così che possiamo dedicare il test ad un singolo componente.

Enzyme ha tre meccanismi di rendering nelle sue API. Abbiamo già visto `shallow()` ma ci sono anche `mount()` e `render()`. Entrambi istanziano le istanze del componente padre e di tutti i componenti figli. `mount()` ti da' accesso ai metodi di lifecycle del componente. Queste sono le regole su quando usare questi meccanismi:

* Iniziare sempre con un test shallow
* Se devono essere testati `componentDidMount()` o `componentDidUpdate()` usare `mount()`
* Se si vuole testare il ciclo di vita del componente o il comportamento dei figli usare `mount()`
* Se si vuole testare il rendering dei figli di un componente con meno overhead di `mount()` e non si è interessati ai metodi di lifecycle usare `render()`

Continua ad applicare lo unit testing dei componenti ma assicurati che i test siano semplici e manutenibili. Altrimenti dovrai rifattorizzarli ogni volta che cambi i componenti. Questa è una delle ragioni principali per cui Facebook ha introdotto gli snapshot test con Jest.

### Esercizi:

* Scrivi uno unit test con Enzyme per il componente Button
* Mantieni gli unit test aggiornati durante i prossimi capitoli
* Leggi su [Enzyme e la sua API di rendering](https://github.com/airbnb/enzyme)
* Leggi sul [testing di applicazioni in React](https://www.robinwieruch.de/react-testing-tutorial)

## L'interfaccia dei componenti con PropTypes

[TypeScript](https://www.typescriptlang.org/) e [Flow](https://flowtype.org/) sono utilizzati per introdurre un sistema di tipi in JavaScript. Un linguaggio tipizzato è meno soggetto ad errori poiché il codice viene validato dal testo del programma. Editor e altri strumenti possono catturare gli errori prima che il programma venga eseguito.

In questo libro non tratteremo Flow e TypeScript ma un altro utile strumento per controllare i tipi nei componenti: React è dotato di un type checker built-in per prevenire bug. È possibile utilizzare PropTypes per descrivere l'interfaccia dei componenti. Tutte le props passate da un componente padre al figlio vengono validate in base all'interfaccia PropTypes assegnata al figlio.

Questa sezione ti illustrerà come realizzare componenti type safe con PropTypes. Ometterò i cambiamenti nei prossimi capitoli, dal momento che richiederebbe refactoring non necessario, ma tu dovresti aggiornarli per mantere l'interfaccia dei tuoi componenti type safe.

Per prima cosa installiamo un pacchetto separato per React:

{title="Command Line",lang="text"}
~~~~~~~~
npm install prop-types
~~~~~~~~

Importiamolo.

{title="src/App.js",lang="javascript"}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import PropTypes from 'prop-types';
# leanpub-end-insert
~~~~~~~~

Iniziamo a definire un'interfaccia di props per i nostri componenti:

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

# leanpub-start-insert
Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node,
};
# leanpub-end-insert
~~~~~~~~

Praticamente vogliamo prendere ogni argomento dalla firma della funzione e assegnarli un PropType. I tipi base per primitivi e oggetti complessi sono:

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

Ci sono altre due PropTypes che possono definire un frammento renderizzabile (nodo), ad esempio una stringa e un elemento React:

* PropTypes.node
* PropTypes.element

Abbiamo già usato la PropType `node` nel componente Button. Ci sono altre definizioni PropType nella documentazione ufficiale di React.

Al momento, tutte le PropTypes per Button sono opzionali. I parametri possono essere `null` o `undefined`. Ma per alcune props vorremmo poter forzare il fatto che siano definiti. Per fare questo rendiamo un requisito il fatto che siano passate al componente.

{title="src/App.js",lang="javascript"}
~~~~~~~~
Button.propTypes = {
# leanpub-start-insert
  onClick: PropTypes.func.isRequired,
# leanpub-end-insert
  className: PropTypes.string,
# leanpub-start-insert
  children: PropTypes.node.isRequired,
# leanpub-end-insert
};
~~~~~~~~

L'attributo `className` non è obbligatorio poiché può avere una stringa vuota come default. Definiamo adesso un'interfaccia PropType per il componente Table:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
# leanpub-end-insert
~~~~~~~~

È possibile definire il contenuto di un array PropType in modo più esplicito:

{title="src/App.js",lang="javascript"}
~~~~~~~~
Table.propTypes = {
  list: PropTypes.arrayOf(
    PropTypes.shape({
      objectID: PropTypes.string.isRequired,
      author: PropTypes.string,
      url: PropTypes.string,
      num_comments: PropTypes.number,
      points: PropTypes.number,
    })
  ).isRequired,
  onDismiss: PropTypes.func.isRequired,
};
~~~~~~~~

Solo `objectID` è obbligatorio, perché parte del codice dipende da lui. Le altre proprietà sono solo visualizzate, quindi non lo sono. Inoltre non possiamo essere sicuri che l'API di Hacker News definisca sempre queste proprietà per ogni oggetto nell'array.

Si possono definire anche props di default per il componente. La proprietà `className` ha un parametro di default ES6 nella firma del componente:

{title="src/App.js",lang="javascript"}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children
}) =>
  ...
~~~~~~~~

Rimpiazziamolo con una prop di default interna a React:

{title="src/App.js",lang="javascript"}
~~~~~~~~
# leanpub-start-insert
const Button = ({
  onClick,
  className,
  children
}) =>
# leanpub-end-insert
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.defaultProps = {
  className: '',
};
# leanpub-end-insert
~~~~~~~~

Come il parametro di default di ES6 la default prop assicura che la proprietà sia settata ad un valore di default quando il componente padre non la speicifca. Il controllo di tipo PropType avviene dopo che la default prop è valutata.

Se esegui nuovamente i test potresti vedere degli error PropType per i tuoi componenti nella riga di comando. Succede quando non definiamo tutte le props per i componenti nei test che sono richieste nella definizione PropType. I test passano correttamente in ogni caso. Assicurati solo di passare tutte le props richieste ai componenti nei tuoi test per evitare questi errori.

### Esercizi:

* Definisci l'interfaccia PropType per il componente Search
* Aggiungi e modifica l'interfaccia PropType quando aggiungerai e modificherai componenti nei prossimi capitoli
* Leggi su [React PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)

## Debugging con React Developer Tools

[React Developer Tools](https://github.com/facebook/react-devtools) ti permette di fare l'inspect nella gerarchia dei componenti React, delle props e degli stati. È fornita come estensione del browser per Chrome e Firefox e come applicazione standalone per altri ambienti. Una volta installata, l'icona dell'estensione si accenderà nei siti che usano React. In queste pagine vedrai una tab chiamata "React" negli strumenti per sviluppatori del browser.

Proviamola con la nostra applicazione Hacker News. Nella maggior parte dei browser un modo veloce per attivare i *dev tools* è fare un click con il destro nella pagina e cliccare "Inspect". Fallo quando l'applicazione è caricata, poi clicca la tab "React". Dovrsti vedre la gerarchia dei suoi elementi, con `<App>` come elemento radice. Se la espandi dovresti trovare anche le istanze dei componenti `<Search>`, `<Table>` e `<Button>`.

L'estensione mostra nel pannello a lato lo stato e le props del componente per l'elemento selezionato. Per esempio, se clicchi su `<App>` vedrai che non ha props ma ha uno stato. Una tecnica molto semplice di debugging consiste nel monitorare i cambiamenti di stato nell'applicazione all'interazione dell'utente.

Per prima cosa, spunta l'opzione "Highlight Updates", di solito sopra all'albero degli elementi. Secondo, digita una differente query di ricerca nel campo di input dell'applicazione. Solo `searchTerm` verrà cambiato nello stato del componente. Sapevamo già che sarebbe successo ma adesso possiamo vederlo funzionare come ci aspettavamo.

Infine premi il bottone "Search". Lo proprietà `searchKey` dello stato immediatamente cambierà allo stato valore di `searchTerm` e l'oggetto di risposta sarà aggiunto a `results`. La natura asincrona del nostro codice è adesso visibile.

Se fai click destro su un elemento un menu a cascata mostrerà alcune opzioni utili. Per esempio, è possibile copiare le props o il nome di un elemento, trovare il corrispondente nodo del DOM o saltare al sorgente del codice nel browser. Quest'ultima opzione è molto utile per inserire breakpoint e debuggare le funzioni JavaScript.

### Esercizi:

* Installa l'estensione [React Developer Tools](https://github.com/facebook/react-devtools) sul tuo browser preferito
  * Esegui la nostra applicazione clone di Hacker News ed effettua l'inspect usando l'estensione
  * Sperimenta con cambiamenti di stato e props
  * Guarda cosa succede quando inneschi una richiesta asincrona
  * Esegui diverse richieste, anche ripetute. Osserva il meccanismo di caching in azione
* Leggi su [come debuggare le tue funzioni JavaScript nel browser](https://developers.google.com/web/tools/chrome-devtools/javascript/)
* Leggi (e usa) su [il profiler di React](https://reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html)

Hai imparato come organizzare e testare il tuo codice React! Ricapitoliamo quest'ultimo capitolo:

* **React**
  * Le PropTypes ti permettono di definire controllo dei tipi per i componenti
  * Jest ti permette di scrivere snapshot test per i componenti
  * Eznyme ti permette di scrivere unit test per i componenti
  * I React Developer Tools sono uno strumenti di debugging molto utile
* **ES6**
  * le istruzioni di import ed export ti aiutano ad organizzare il codice
* **Generale**
  * l'organizzazione del codice ti permette di scalare più facilmente la tua applciazione grazie alle best practice

Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.4).
