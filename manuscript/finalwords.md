# Outline

Siamo quindi giunti in fondo al libro The Road to Learn React. Spero ti sia piaciuto leggerlo, e che ti abbia aiutato ad acquisire una certa familiarità con React. Se hai apprezzato il libro, condividilo come modo per imparare React con i tuoi amici. Dovrebbe essere usato come omaggio. Inoltre, una recensione su [Amazon](https://amzn.to/2JHlP42) o [Goodreads](https://www.goodreads.com/book/show/37503118-the-road-to-learn-react) può davvero aiutare a migliorare i progetti futuri.

**Quindi, come proseguire da qui?** Ti consiglio di estendere da solo l'applicazione che abbiamo creato, poi passare a creare i tuoi progetti in React. Valuta questa possibilità prima di buttarti su un altro libro, un corso o un tutorial. Fallo per una settimana, mettilo in produzione facendo il deploy e scrivi a [me](https://twitter.com/rwieruch) o ad altri per mostrarcelo. Sono sempre interessato a vedere quello che i miei lettori producono e voglio aiutarli.

Se stai cercando migliorie per l'applicazione, ti consiglio alcuni percorsi didattici da seguire, dopo che padroneggerai le basi:

* **Connettersi ad un database e/o autenticarsi:** In un'applicazione React che cresce potresti voler infine persistere dei dati. I dati dovrebbe essere salvati in un database così che possano sopravvivere ad una sessione del browser, e che possano essere condivisi tra utenti diversi che utilizzano l'applicazione. Il modo più semplice di introdurre un database è Firebase. In [questo tutorial](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial/) troverai una guida passo passo su come implementare un'autenticazione con Firebase in React. Oltre a questo potrai usare il database realtime di Firebase per salvare i tuoi utenti.

* **Gestione dellos tato:** Abbiamo usato le funzionalità di React `this.setState()` e `this.state` per gestire e accedere allo stato del componente. E questo è un ottimo inizio. In applicazioni più grandi però, capirai i [limiti dello stato locale ai componenti di React](https://www.robinwieruch.de/learn-react-before-using-redux/). È d'obbligo imparare ad usare una libreria di gestione dello stato come [Redux o MobX](https://www.robinwieruch.de/redux-mobx-confusion/). Sulla piattaforma [Road to React](https://roadtoreact.com/) troverai il corso "Taming the State in React" che insegna la gestione avanzatata dello stato usando Redux e MobX. Il corso offre anche un ebook, ma io consiglio di apprefondire anche con i codici sorgente e gli screencast.

* **I tool con Webpack e Babel:** Abbiamo usato *create-react-app* per fare il setup dell'applicazione che abbiamo creato per questo libro. Ad un certo punto potresti voler imparare i tool che lo compongono, il che ti permetterebbe di fare il setup di un tuo progetto senza *create-react-app*. Io consiglio un setup minimale con [Webpack e Babel](https://www.robinwieruch.de/minimal-react-webpack-babel-setup/), ai quali potrai poi aggiungere eventuali altri tool da solo. Per esempio, potresti voler [usare ESLint](https://www.robinwieruch.de/react-eslint-webpack-babel/) per seguire uno stile del codice omogeneo.

* **Organizzazione del codice:** Ricordati del capitolo sull'organizzazione del codice. Potresti applicare quei concetti adesso se non l'hai ancora fatto. Ti aiuterebbe ad organizzare i componenti in file e cartelle strutturati (moduli), e a capire i principi di divisione del codice, riusabilità, manutenibilità, e design dell'API di moduli. Le tue applicazione potrebbero crescere fino al punto di aver bisogno di essere strutturate in moduli, quindi meglio iniziare subito.

* **Testing:** Il libro ha trattato solo superficialmente la questione testing. Se non hai familiarità con l'argomento dovresti trattare più approfonditamente gli unit testing e i test d'integrazione, specialmente nell'ambito di applicazioni React. Io consiglierei di rimanere su Enzyme e Jest per l'implementazione e per rifinire il tuo approccio a test di unità e snapshot.

* **Sintassi dei componenti React:** Le best practice per l'implementazione di componenti React evolvono con il tempo. Troverai molti modi per scrivere componenti React, specialmente quelli implementati con classi, in altre risorse didattiche. Il repository GitHub chiamato [react-alternative-class-component-syntax](https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax) è un ottimo modo per imparare le modalità alternative per scrivere componenti React come classe. Utilizzando la dichiarazione di campi di classe si potranno scrivere in modo ancora più conciso in futuro.

* **Componenti UI:** Molti principianti fanno l'errore di introdurre librerie di componenti UI troppo presto nei loro progetti. È più pratico imparare come implementare ed usare dropdown, checkbox, dialog in React con elmementi HTML standard. Molti di questi componenti gestiranno il proprio stato locale. Una checkbox deve sapere se è spuntata o meno, quindi dovrebbe essere implementata come componente controllato (controlled component). Dopo aver implementato i componenti di base introdurre una libreria di componenti UI con checkbox e dialog come componenti React dovrebbe essere più semplice.

* **Routing:** Puoi implementare il routing nella tua applicazione con [react-router](https://github.com/ReactTraining/react-router). React Router aiuta a gestire pagine multiple attraverso multiple URL. Quando introduci il routing nella tua applicazione React, non viene effettuata nessuna chiamata al web server quando viene richiesta una nuova pagina. Il router effettuerà tutto il necessario per te client-side. Quindi prova a mettere mano al routing ed introducilo nella tua applicazione.

* **Controllo di tipi:** Prima abbiamo usato React PropTypes per definire le interfacce dei componenti. Questa è buona pratica per prevenire bug, ma PropTypes effettua solo controlli a runtime. Si può andare oltre introducendo un controllo di tipi statico durante la compilazione. [TypeScript](https://www.typescriptlang.org/) e [Flow](https://flowtype.org/) sono due popolari sistemi di tipi per React.

* **React Native:** [React Native](https://facebook.github.io/react-native/) porta la tua applicazione su dispositivi mobile come applicazione iOS e Android. Una volta padroneggiato React, la curva di apprendimento per imparare React Native non dovrebbe essere così ostica, visto che condividono molti principi. L'unica differenza con il mobile sono i layout di componenti.

* **Altri progetti:** Ci sono molti tutorial là fuori che utilizzano solo React per realizzare interessanti applicazioni, il che è una buona pratica per applicare quanto imparato in questo libro prima di passare a progetti intermedi. Ecco alcuni dei miei tutorial:
  * [Una lista paginata e a scrolling infinito](https://www.robinwieruch.de/react-paginated-list/)
  * [Vetrina di tweet su una bacheca di Twitter](https:/www.robinwieruch.de/react-svg-patterns/)
  * [Connettere la tua applicazione React a Stripe per addebitare denaro](https://www.robinwieruch.de/react-express-stripe-payment/).

Ti invito a visitare il mio [sito](https://www.robinwieruch.de) per trovare altri argomenti interessanti sullo sviluppo web e l'ingegneria del software. Puoi anche [iscriverti alla mia newsletter](https://www.getrevue.co/profile/rwieruch) per ricevere gli aggiornamenti circa articoli, libri e corsi. Inoltre la piattaforma di corsi [Road to React](https://roadtoreact.com) offre lezioni più avanzate sull'ecosistema React.

Infine, spero di trovare più [Patrons](https://www.patreon.com/rwieruch) che siano interessati a supportare i miei contentuti, visto che ci sono molti studenti che non possono permettersi di pagare per contenuti didattici. Ecco perché molti miei contenuti sono gratuiti. Supportarmi su Patreon permette di offrire educazione gratuita a chi non può permettersi di pagarla.

Ancora una volta, se hai apprezzato il libro, ti chiedo di prenderti un momento per pensare ad una persona che vorrebbe imparare React e di condividerlo con lei. Il libro è pensato per essere dato ad altri e migliorerà con il tempo se più persone lo leggono e condividono un feedback.

Grazie per aver letto The Road to learn React.

Cordiali saluti,

Robin Wieruch
