# Organizzazione del codice e testing

Il capitolo tratta temi importanti per mantenere il codice manutenibile in applicazioni che tendono a crescere. Imparerai ad organizzare il codice seguendo le best practice per quanto riguarda l'organizzazione di file e cartelle. Un altro tema che imparerai è il testing, che è importante per mantenere il codice robusto. Infine, imparerai un tool molto utile per il debugging delle tue applicazioni React. La maggior parte del capitolo farà un passo indietro sull'applicazione pratica e ti spiegherà alcune tematiche importanti.

## Moduli in ES6: import ed export

In JavaScript ES6 puoi importare ed esportare funzionalità da moduli. Queste funzionalità possono essere funzioni, classi, componenti, costanti e altro. Praticamente qualsiasi cosa puoi assegnare ad una variabile. I moduli possono essere singoli file o intere cartelle che utilizzano un file "index" come entry point.

All'inizio del libro, dopo che hai impostato la tua applicazione con *create-react-app*, ti sei già imbatutto, nei primi file, in istruzioni di import ed export. Ora è il momento giusto per spiegarli.

Le istruzioni di `import` ed `export` ti aiutano a condividere il codice in diversi file. Prima c'erano più soluzioni a questo problema nell'ecosistema JavaScript ed era confusionario, perché sarebbe preferibile un solo metodo, standardizzato, piuttosto che varie soluzioni allo stesso problema. Adesso c'è un metodo nativo da JavaScript ES6.

Inoltre queste istruzioni incoraggiano la modularizzazione del codice. Distribuisci il codice in diversi file per mantenerlo riusabile e manutenibile. Riusabile perché puoi importarlo in vari pezzi di codice, manutenibile perché ogni singolo modulo risiede in un singolo posto (un file o una cartella).

Inoltre, ti aiuta a pensare al raggio di azione del codice. Non ogni funzionalità necessita di essere esportata da un file. Alcune di queste potrebbe essere usate solo nel file dove sono definite. Gli export in un file sono praticamente l'interfaccia publica (API) del file verso l'esterno. Solo le funzionalità esportate sono rese disponibili all'esterno per essere riutilizzate. Questo segue i principi dell'incapsulamento del codice.

Venendo alla parte pratica. Come funzionano le istruzioni di `import` ed `export`? Gli esempi che seguono mostrano queste funzionalità condividendo una o più variabili tra diversi file. Lo stesso approccio potrà essere utilizzato su più file per condividere più che semplici variabili.

Puoi esportare una o più variabili. E' chiamato export nominativo (named export).

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'Robin';
const lastname = 'Wieruch';

export { firstname, lastname };
~~~~~~~~

E importarle in un altro file con path relativo al primo file.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname, lastname } from './file1.js';

console.log(firstname);
// output: Robin
~~~~~~~~

Puoi anche importare tutte le variabili esportate da un altro fle come un unico oggetto.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import * as person from './file1.js';

console.log(person.firstname);
// output: Robin
~~~~~~~~

Gli import possono avere un alias. Può succedere che si importino funzionalità da diversi file che abbiano lo stesso nome, a questo servono gli alias.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname as username } from './file1.js';

console.log(username);
// output: Robin
~~~~~~~~

Un'altra possibilità è l'istruzione `default`. Può essere usata in alcuni scenari:

* per esportare o importare una singola funzionalità
* per evidenziare la funzionalità più importante di un modulo o API
* per avere un import di fallback

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const robin = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

export default robin;
~~~~~~~~

Si possono omettere le parentesi graffe per importare le funzionalità esportare come default.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer from './file1.js';

console.log(developer);
// output: { firstname: 'Robin', lastname: 'Wieruch' }
~~~~~~~~

Inoltre, il nome importato può essere diverso dal nome di default esportato. Ed è anche possibile usarlo insieme a degli import ed export nominativi.

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

E importare il default o l'export con nome in un altro file.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// output: { firstname: 'Robin', lastname: 'Wieruch' }
console.log(firstname, lastname);
// output: Robin Wieruch
~~~~~~~~

Puoi anche farlo su più righe ed esportare le variabili direttamente per export nominativi.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
export const firstname = 'Robin';
export const lastname = 'Wieruch';
~~~~~~~~

Queste sono le funzionalità principali dei moduli in ES6. Sono utili ad organizzare il codice, a manutenerlo e a progettare moduli riusabili (API). Puoi anche importare o esportare funzionalità a scopo di test ed è quello che faremo in uno dei prossimi capitoli.

### Esercizi:

* leggi di più su [import ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* leggi di più su [export ES6](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## Organizzazione del codice con moduli ES6

Potresti chiederti: perché non abbiamo seguito le best practice di suddivisione del codice in moduli per il file *src/App.js*? In quel file avevamo più componenti che potevano essere definiti in rispettivi file e cartelle (moduli). allo scopo di imparare React è più pratico tenere tutto in un'unica vista ma una volta che la tua applicazione cresce, dovresti considerare di dividere i componenti in diversi moduli. Solo in quel modo è possibile scalare l'applicazione.

A seguire, proporrò diverse strutture di moduli che *potresti* applicare. Consiglierei di provarli come esercizio alla fine del libro. Per mantenere il libro semplice non eseguirò la suddivisione del codice in moduli ma continuerò nei prossimi capitoli ad avere tutto il codice nel file *src/App.js*.

Una possibile struttura dei moduli potrebbe essere:

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

Separa i componenti in rispettivi file ma non sembra molto promettente. C'è molta duplicazione nel naming e differenza solo nelle estensioni dei file. Un altro modo potrebbe essere:

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

E' più chiara della precedente. I file index sono entry point delle rispettive cartelle (moduli). E' solo naming convention, puoi usare il nome che preferisci. In questa struttura dei moduli un componente è definito nel suo file JavaScript ma ha anche i rispettivi file per lo stile e per i test.

Un altro passo potrebbe essere quello di estrarre le variabili costanti dal component App. Queste costanti erano usate per comporre la URL dell'API di Hacker News.

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

Si ottiene una divisione naturale dei moduli tra *src/constants/* e *src/components/*. Ora il file *src/constants/index.js* potrebbe essere qualcosa del genere:

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

Il file *App/index.js* potrebbe importare queste variabili per utilizzarle.

{title="Code Playground: src/components/App/index.js",lang=javascript}
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

Quando usi la naming convention di *index.js*, puoi omettere il nome del file dal path relativo.

{title="Code Playground: src/components/App/index.js",lang=javascript}
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

Cosa c'è dietro il naming *index.js*? La convenzione arriva dal mondo node.js. Il file index è l'entry point del modulo e descrive la sua API pubblica. I moduli esterni sono tenuti ad usare solo il file *index.js* per importare codice. Considera la seguente struttura di moduli come dimostrazione:

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

La cartella *Buttons/* ha diversi componenti bottone in file distinti. Ogni file può esportare con `export default` il proprio componente rendendolo disponibile a *Buttons/index.js*. Il file *Buttons/index.js* importa tutti i diversi tipi di bottone e gli esporta come API pubblica del modulo *Buttons*.

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

Ora il file *src/App/index.js* può importare i bottoni dall'API pubblica del modulo definita nel file *index.js*.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
~~~~~~~~

Seguendo questa convenzione sarebbe una bad practice raggiungere un altro file oltre all'*index.js* in un modulo. Violerebbe le regole di incapsulamento del codice.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
// bad practice, don't do it
import SubmitButton from '../Buttons/SubmitButton';
~~~~~~~~

Ora sai come rifattorizzare il tuo codice in moduli seguendo le regole dell'incapsulamento. Come già detto, per mantenere il libro semplice non applicherò quanto mostrato ma tu dovresti farlo quando finisci di leggere il libro.

### Esercizi:

* rifattorizza il tuo file *src/App.js* in componenti in file distinti quando finirai il libro

## Snapshot Tests con Jest

Il libro non tratterà a fondo l'argomento testing, ma non dovrebbe rimanere tralasciato. Testare il codice nella programmazione è essenziale e dovrebbe essere visto come obbligatorio. Vogliamo mantenere la qualità del codice alta e assicurarci che tutto funzioni.

Potresti aver sentito parlare della piramide del testing. Ci sono i test codsiddetti "end-to-end", gli integration test e gli unit test. Se non hai famigliarità con l'argomento, il libro ti darà una rapida panoramica. Uno unit test è usato per testare un frammento di codice isolato. Potrebbe essere una singola funzione ad essere testata da uno unit test. Gli uni test funzionano bene in isolamento che in combinazione con altre unità. Tendono ad essere testati come gruppi di unità. Qui è dove i test d'integeazione trovano la loro utilità, assicurando che le unità di lavoro funzionino bene insieme. Infine, i test end-to-end sono una simulazione di uno scenario reale che potrebbe eseguire un utente. Per esempio un'esecuzione autiomatica di una simulazione nel browser di un utente che intergisce con il sistema di login di un'applicazione web. Mentre gli unit test sono veloci e facili da scrivere e mantenere, i test end-to-end sono l'esatto opposto.

Di quanti test per ogni tipo ho bisogno? Solitamente si vuole avere molti unit test per coprire le funzioni in modo isolato. Dopodiché, potremmo avere diversi test d'integrazione per assicurarsi che le più importanti funzioni funzionino come ci aspettiamo quando messe insieme. Infine, potremmo avere giusto alcuni test end-to-end per simulare gli scenari critici dell'applicazione. Questo è quanto per quanto riguarda le tipologie di test generali.

Quindi, come applichiamo questo conoscenze del testing alla nostra applicazione React? Le basi del testing in React sono i test dei componenti che può essere generalizzato come unit testing e una parte di essi come snapshot test. Applicherai lo unit testing sui tuoi componenti nel prossimo capitolo usando una libreria chiamata Enzyme. In questa sezione, invece, ci concentreremo su un altro tipo di test: gli snapshot test. Qui è dove entra in gioco Jest.

[Jest](https://jestjs.io/) è un framework di testing JavaScript che è usato in Facebook. Nella comunità React è usato per testare i componenti React. Fortunatamente *create-react-app* contiene già Jest, quindi non dobbiamo preoccuparci di installarlo e configurarlo.

Iniziamo a testare i tuoi primi componenti. Prima di tutto però, è necessario esportare i componenti che vogliamo testare, dal tuo file *src/App.js*. A questo punto, è possibile testarli in un altro file. Dovresti aver imparato questi concetti nel capitolo sull'organizzazione del codice.

{title="src/App.js",lang=javascript}
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

Nel file *App.test.js* troverai un primo test generato da *create-react-app*. Semplicemente verifica che il component App venga renderizzato senza nessun errore.

{title="src/App.test.js",lang=javascript}
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

Il blocco "it" descrive un caso di test. E' composto da una descrizione del test e quando eseguito può finire con successo o fallire. Inoltre, è possible raggruppare i test in un blocco "describe", che definisce una suite di test. Una suite di test include una serie di blocchi "it" per uno specifico componente. Vedrai questi blocchi "describe" più avanti. Entrambi i blocchi sono usati per separare ed organizzare i casi di test.

Da notare che la funzione `it` è conosciuta nella community JavaScript come funzione dove viene eseguito un singolo test, ma in Jest potresti anche trovarlo con l'alias `test`.

Puoi eseguire i test interattivamente usando lo script `test` di *create-react-app* dalla riga di comando. L'output sarà composto dall'output di tutti i casi di test nel tuo terminale.

{title="Command Line",lang="text"}
~~~~~~~~
npm test
~~~~~~~~

Ora Jest dovrebbe permetterti di scrivere snapshot test. Questi test generano una snaphot del componente renderizzato e confrontano questa snapshot con future snapshot. Quando una futura snaposhet cambierà, verrai notificato dal test. A questo punto puoi accettare il fatto che la snaphot è cambiata, magari perché hai cambiato l'implementazione del componente di proposito, o rifiutare la modifica e investigare le cause dell'errore. Questo si coniuga molto bene con gli unit test, perché testa solo le differenze degli output renderizzati, non aggiunge grossi costi di maintenance, perché puoi semplicemente accettare i cambiamenti delle snaphot quando cambi qualcosa di proposito nell'output del tuo componente.

Jest salva le snaphot in una cartella. Grazie a questo può validare le differenze rispetto a future snaphot. Inoltre, le snapshot possono essere facilmente essere condivise tra team avendole in una sola cartella.

Prima di scrivere il tuo primo snaphot test con Jest, devi installare un pacchetto di utility.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev react-test-renderer
~~~~~~~~

Ora puoi estendere i test del tuo componente App con il tuo primo snaphot test. Prima cosa, importiamo le nuove funzionalità dal pacchetto e inseriamo il precedente blocco "it" per il componente App in un descrittivo blocco "describe". In questo caso la suite di test è solo per il componente App.

{title="src/App.test.js",lang=javascript}
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

Ora puoi implementare il tuo primo snaphot test usando un blocco "test".

{title="src/App.test.js",lang=javascript}
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

Esegui di nuovo i test e guarda se passano o falliscono. Dovrebbe passare. Se cambi l'output del metodo render nel tuo componente App, lo snaphot test dovrebbe fallire. A quel punto puoi decidere se aggiornare la snaphot o investigare il codice del tuo componente.

Praticamente la chiamata a funzione `renderer.create()` crea una snapshot per il tuo componente App. Lo renderizza virtualmente e salva il DOM dentro una snaphot. Dopodiché, si aspetta che la snaphot sia uguale a quella generata in precedenza. In questo modo puoi assicurarti che il DOM rimango lo stesso e non cambi per sbaglio.

Aggiungiamo altri test per i nostri componenti indipendenti. Primo, il componente Search:

{title="src/App.test.js",lang=javascript}
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

Il componente Search ha due test simili al componente App. Il primo test semplicemente renderizza il componente Search nel DOM e verifica che non ci sono errori durante il processo di rendering. Se ci fosse un errore, il test fallirebbe anche se non ci sono assertion (esempio: expect, match, equal) nel blocco di test. Il secondo test, quello di snaphot, è utilizzato per salvare una snaphot del componente renderizzato ed eseguirlo confrontandolo con la snaphot precedente. Fallisce se la snaphot è cambiata.

Secondo, puoi testare il componente Button dove le regole di testing applicate sono le stesse del componente Search.

{title="src/App.test.js",lang=javascript}
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

Infine, il componente Table al quale puoi passare alcune props per renderizzarlo con una lista di esempio.

{title="src/App.test.js",lang=javascript}
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

Gli snaphot test solitamente sono piuttosto semplici. Il senso è quello di assicurarsi che l'output del componente non cambi. Se cambia, devi decidere se accettare i cambiamenti. Altrimenti devi aggiustare il componente affinché il suo output non sia quello che ti aspetti.

### Esercizi:

* guarda come lo snaphot test fallisce se cambi il valore di ritorno del metodo `render()` del tuo commponente
  * accetta o rifiuta i cambiamenti della snapshot
* mantini i tuoi snaphot test aggiornati quando l'implementazione dei componenti cambierà nei prossimi capitoli
* leggi di più su [Jest in React](https://jestjs.io/docs/en/tutorial-react)

## Unit test con Enzyme

[Enzyme](https://github.com/airbnb/enzyme) è una utility di test sviluppata da Airbnb per fare asserzioni, manipolare e navigare sui tuoi componenti React. Puoi usarla per condurre unit test da affiancare agli snaphot test in React.

Vediamo come si usa Enzyme. Prima di tutto devi installarlo, perché non è installato di default con *create-react-app*. E' distriubuito con un'estensione per usarlo con React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16
~~~~~~~~

Secondo, devi incuderlo nel setup dei tuoi test e inizializzare il suo Adapter per usarlo in React.

{title="src/App.test.js",lang=javascript}
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

Ora puoi scrivere il tuo primo uni test nel blocco "describe" di Table. Userai `shallow()` per renderizzare il tuo componente e asserire che ha due oggi, perché passeremo una lista di due elementi. L'assertion semplicemente controlla che l'elemento ha due elementi con classe `table-row`.

{title="src/App.test.js",lang=javascript}
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

Shallow renderizza il componente senza i componenti figli. In questo modo puoi mantenere il focus del test su un componente.

Enzyme ha in totale tre meccanismi di rendering nella sua API. Conosci già `shallow()`, ma esistono anche `mount()` e `render()`. Entrambi istanzione instanze del componente padre e di tutti i componenti figli. Inoltre `mount()` ti dà accesso ai metodi di lifecycle del componente. Ma quando usare ogni meccanismo di rendering? Ecco alcuni spunti:

* Inizia sempre con uno shallow test
* Se `componentDidMount()` o `componentDidUpdate()` necessitano di essere testate usa `mount()`
* Se vuoi testare i metodi di lifecycle del componente e dei suoi figli usa `mount()`
* Se vuoi testare il rendering dei figli del componente con meno overhead che con `mount()` e non sei interessato ai metodi di lifecycle usa `render()`

Potresti continuare facendo unit test sui tuoi componenti ma ricorda di mantenere i test semplici e manutenibili, altrimenti dovrai rifatorizzarli ogni volta che cambi i tuoi componenti, ed ecco perché Facebook ha introdotto gli snaphot test con Jest.

### Esercizi:

* Scrivi uno unit test con Enzyme per il tuo componente Button
* Mantieni i tuoi unit test aggiornati durante i prossimi capitoli
* Leggi di più su [Enzyme e le sue API di rendering](https://github.com/airbnb/enzyme)
* Leggi di più sul [testing di applicazioni React](https://www.robinwieruch.de/react-testing-tutorial)

## L'interfaccia dei componenti con PropTypes

Potresti essere a conoscenza di [TypeScript](https://www.typescriptlang.org/) o [Flow](https://flowtype.org/) per aggiungere un controllo sui tipi in JavaScript. Un linguaggio tipizzato è meno soggetto ad errori perché il codice viene validato in base al codice del programma. Editor di testo ed altre utility possono segnalare questi errori prima che il programma venga eseguito e questo rendere il tuo programma più robusto.

In questo non introdurremo Flow o TypeScript ma un'altra efficace alternativa per controllare i tipi di dato dei tuoi componenti. React contiene di default un modo per controllare i tipi di dato e prevenire errori. Puoi usare PropTypes per descrivere l'interfaccia dei tuoi componenti. Tutte le props che vengono passate da un componente padre a un componente figlio possono essere validate assegnando delle specifiche PropTypes al componente figlio.

Questa sezione ti mostrerà come rendere tutti i tuoi componenti sicuri con PropTypes. Non riporterò queste modifiche nei capitoli successivi perché richiederebbero dei refactoring non necessari al codice ma tu dovresti applicarle e mantenerle aggiornate nel corso del libro per mantenere l'interfaccia dei tuoi componenti type safe.

Prima cosa, è necessario installare un pacchetto per React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install prop-types
~~~~~~~~

Ora è possibile importare le funzionalità di PropTypes.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import PropTypes from 'prop-types';
# leanpub-end-insert
~~~~~~~~

Iniziamo ad assegnare un'interfaccia alle props dei componenti:

{title="src/App.js",lang=javascript}
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

Tutto qui. Prendi ogni proprietà in input al componente e le assegni un PropType. I PropTypes di base per i tipi primitivi e gli oggetti più complessi sono:

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

Inoltre abbiamo altri due PropTypes per definire frammenti renderizzabili (nodi), ad esempio una stringa, e un elemento React:

* PropTypes.node
* PropTypes.element

Abbiamo già usato il PropType `node` per il componente Button. In generale ci sono altre definizioni di PropType di cui puoi leggere sulla documentazione ufficiale di React.

Al momento tutte le PropTypes che abbiamo definito per Button sono opzionali. I parametri possono essere `null` o `undefined`. Ma in molti casi si vuole ottenere che queste proprietà siano definite. Puoi rendere un requisito che queste props siano passate al componente.

{title="src/App.js",lang=javascript}
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

L'attributo `className` non è obbligatorio perché ha un default come stringa vuota. Di seguito definiamo un'interfaccia PropType per il componente Table:

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
# leanpub-end-insert
~~~~~~~~

Puoi anche definire più nel dettaglio il contenuto di un array:

{title="src/App.js",lang=javascript}
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

Solo `objectID` è obbligatorio, perché sai che parte del tuo codice dipende da quello, le altre proprietà sono solo visualizzate, quindi non devono essere necessariamente obbligatorie. Inoltre non puoi essere certo che l'API di Hacker News ha sempre una proprietà definita per ogni oggetto nell'array.

Questo è quanto per quanto riguarda PropTypes ma c'è un altro aspetto. Puoi definire props di default nel tuo componente. Prendiamo di nuovo il componente Button. La proprietà `className` è un parametro che ha un default (ES6) nella firma del componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children
}) =>
  ...
~~~~~~~~

Possiamo sostituirlo assegnando un default con PropTypes:

{title="src/App.js",lang=javascript}
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

Stessa cosa del parametro di default in ES6, la default prop assicura che alla proprietà venga assegnato il valore di default se il componente padre non lo specifica. Il controllo di tipo di PropTypes avviene dopo che il default prop è eseguito.

Se esegui ancora i tuoi test, potresti notare errori PropTypes sui tuoi componenti nel terminale. Può succedere perché non hai rispettato tutte le definizioni PropTypes nei test dei tuoi componenti. I test passano tutti correttamente comunque. Puoi passare tutte le props obbligatorie ai componenti nei tuoi test per non vedere questi errori.

### Esercizi:

* definire l'interfaccia PropType per il componente Search
* aggiungere e modificare interfacce PropTypes quando aggiungi o modifichi componenti nei prossimi capitoli
* leggi di più su [React PropTypes](https://reactjs.org/docs/typechecking-with-proptypes.html)

## Il debugging con i React Developer Tools

Quest'ultima sezione ti introduce un utile strumento, generalmente usato per ispezionare e fare il debugging delle applicazione React. [React Developer Tools](https://github.com/facebook/react-devtools) ti permettono di ispezionare la gerarchia dei componenti React, le loro props e i loro stati. Puoi installarla come estensione per il browser (per Chrome e Firefor al momento) o come applicazione standalone (che funziona in tutti gli ambienti). Una volta installata, l'icona dell'estensione sarà illuminata nei siti che usano React. In quelle pagine vedrai una tab chiamata "React" negli strumenti per sviluppatori del browser.

Proviamo con la nostra applicazione Hacker News. Con la maggior parte dei browser, un modo veloce per scatenare i _dev tools_ è fare click destro sulla pagina e cliccare "Inspect". Fallo quando la tua applicazione ha caricato e clicca la tab "React". Dovrsti vedere la sua gerarchia degli elementi, con `<App>` come elemento root. Se lo espandi troverai anche le istanze dei componenti `<Search>`, `<Table>` e `<Button>`.

L'estensione mostra nel pannello a lato lo stato e le props dell'elemento selezionato. Per esempio, se clicchi su `<App>`, vedrai che non ha props ma ha uno stato. Una tecnica di debugging molto semplice consiste nel monitorare i cambiamenti di stato dell'applicazione a seguito dell'interazione utente.

Per prima cosa, dovresti mettere la spunta sull'opzione "Evidenzia aggiornamenti" (highlight updates). Poi, potresti provare a digitare qualcosa nel campo di ricerca dell'applicazione. Come vedrai, solo `searchTerm` verrà modificato nello stato del componente. Tu già sapevi che sarebbe successo, ma adesso puoi veramente vederlo accadere come pianificato.

Infine puoi premere il bottone "Search". La proprietà `searchKey` dello stato sarà immediatamente cambiata con lo stesso valore di `searchTerm` e l'oggetto di risposta sarà aggiunto a `results` poco dopo. La natura asincrona del nostro codice è adesso visibile ai tuoi occhi.

Inoltre, se fai click destro su qualsiasi elemento, un menu ti mostrerà delle opzioni utili. Per esempio, potresti copiare le props dell'elemento o il nome, trovare il corrispondente nodo del DOM o saltare al codice dell'applicazione nel browser. Quest'ultima opzione è molto utile per inserire breakpoint e fare il debugging di funzioni JavaScript.

### Esercizi:

* installare l'estensione [React Developer Tools](https://github.com/facebook/react-devtools) per il tuo browser preferito
  * eseguire la nostra applciazione clone di Hacker News e ispezionarla usando l'estensione
  * sperimentare con cambiamenti di stato e di props
  * vedere cosa succede quando viene scatenata una richiesta asincrona
  * eseguire più richieste, anche ripetute. Vedere il meccanismo di caching funzionare
* leggi di più su [Come fare il debugging di funzioni JavaScript nel browser](https://developers.google.com/web/tools/chrome-devtools/javascript/)

{pagebreak}

Hai imparato come organizzare il codice e come testarlo! Ricapitoliamo gli ultimi capitoli:

* React
  * PropTypes ti permette di definire i tipi di dato dell'interfaccia dei tuoi componenti
  * Jest ti permette di scrivere snapshot test per i tuoi componenti
  * Enzyme ti permette di scrivere unit test per i tuoi componenti
  * L'utile strumento di debugging React Developer Tools
* ES6
  * le istruzioni di import ed export ti aiutano ad organizzare il codice
* Generale
  * l'organizzazione del codice ti permette di scalare la tua applicazione seguendo delle best practice

Puoi trovare il codice sorgente nel [repository ufficiale](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.4).
