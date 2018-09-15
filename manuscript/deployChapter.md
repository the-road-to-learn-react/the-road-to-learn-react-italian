# Ultimi passaggi verso la produzione

Gli ultimi capitoli mostreranno come effettuare il deploy della tua applicazione. Useremo il servizio di hosting gratuito Heroku. Durante questa fase imparerai di più su *create-react-app*.

## Eject

Quanto spiegato in questa sezione non è necessario per il deploy della tua applicazione in produzione, ma voglio spiegartelo lo stesso. *create-react-app* fornisce una funzionlità per renderlo estendibile e per prevenire una vendor lock-in. Vale a dire una situazione in cui adotti una tecnologia che poi non ti permette di abbandonarla in una fase successiva. Fortunatamente in *create-react-app* abbiamo una via di uscita con "eject".

Nel file *package.json* troviamo gli script *start*, *test* e *build*, rispettivamente per far partire, testare, ed effettuare una build della tua applicazione. L'ultimo script è *eject*. Puoi provarlo, ma non c'è modo di tornare indietro. **E' un'operazione irreversibile. Una volta che effettui l'eject, non puoi tornare indietro!**. Se stai semplicemente imparando React, non ha senso abbandonare il confortevole ambiente di *create-react-app*.

Se eseguissimo `npm run eject`, il comando copierebbe tutta la configurazione e le dipendenze di sviluppo nel file *package.json* e in una nuova directory *config/*. Praticamente convertirebbe l'intero progetto in uno con un setup custom con tool quali Babel e Webpack. Praticamente, il comando ti cederebbe il controllo di questi tool.

La documentazione ufficiale dice che *create-react-app* è adatta a progetti da piccole dimensioni fino a medie dimensioni. Non dovresti sentirti obbligato ad usare il comando "eject".

### Esercizi:

* leggi di più su [eject](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject)

## Il deploy della tua App

Alla fin fine, nessuna applicazione dovrebbe rimanere in localhost. Vogliamo invece andare live. Heroku è una piattaforma dove puoi mettere la tua applicazione che offre un'ottima integrazione con React. Per essere più specifici_ è possibile effettuare il deploy di un'applicazione realizzata con *create-react-app* in pochi minuti. E' praticamente un zero-configuration deployment che segue la stessa filosofia di *create-react-app*.

Ci sono due prerequisiti prima di poter effettuare il deploy di un'applicazione su Heroku:

* installare l'[Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* creare un [account gratuito su Heroku](https://www.heroku.com/)

Se hai installato Homebrew, puoi installare il client Heroku da linea di comando:

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Ora puoi usare git e l'Heroku CLI per effettuare il deploy della tua applicazione.

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

Questo è tutto. Spero che la tua applicazione sia live ora. Se ti sei imbattuto in qualche problema controlla le risorse seguenti:

* [Git e GitHub Essentials](https://www.robinwieruch.de/git-essential-commands/)
* [Effettuare il deploy di un'app React con zero configurazione](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack per create-react-app](https://github.com/mars/create-react-app-buildpack)
