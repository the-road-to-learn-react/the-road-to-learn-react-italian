# Passaggi finali verso la produzione

Gli ultimi capitoli ti mostreranno come effettuare il deploy dell'applicazione in produzione. Per questo useremo il servizio di hosting gratuito Heroku. Facendo questo scopriremo molto altro su *create-react-app*.

## Eject

Le seguenti conoscenze non sono necessarie al fine di deployare la nostra applicazione in produzione, ma vale la pena menzionarle. *create-react-app* fornisce una funzionalità per mantenerlo estendibile ma anche per evitare un vendor lock-in. Un vendor lock-in si verifica quando si inizia ad utilizzare una tecnologia e non vi è possibilità di smettere di usarla in futuro. Fortunatamente *create-react-app* offre questa via di uscita con "eject".

Nel file *package.json* del nostro progetto troverai gli script *start*, *test*, *build*. Quello che rimane è *eject*. La prima cosa importante da sapere è che si tratta di **un'operazione a senso unico**. Questo significa che **una volta che effettui l'eject, non puoi tornare indietro!** Considera comunque che è molto consigliabile rimanere nell'ambiente sicuro di *create-react-app* se hai appena iniziato a creare applicazioni in React.

Se ti senti abbastanza confidente da eseguire `npm run eject`, il comando copia tutte le configurazioni e le dipendenze nel tuo *package.json* e nella cartella *config/*. Praticamente converte l'intero progetto in uno con un setup custom con tool quali Babel e Webpack, e offre l'intero controllo su questi.

La documentazione ufficiale dice che *create-react-app* è ideale per progetti da piccoli a media dimensioni, quindi non dovresti sentirti obbligato ad usare il comando "eject" fino a che non sei pronto.

### Esercizi

* Leggi su [eject](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#npm-run-eject)

## Il deploy della nostra app

Nessuna applicazione dovrebbe rimanere in localhost; infine dovrà andare live per vedere come si comporterebbe in uno scenario reale. Heroku è un PaaS (Platform as a Service) dove puoi mettere la tua applicazione online e offre un'integrazione molto snella con React. Nello specifico, è possibile effettuare il deploy di un'applcazione realizzata con *create-react-app* in pochi minuti, con zero configurazioni, il che segue la filosofia di *create-react-app*.

Ci sono due requisiti prima che un'applicazione possa essere deployata su Heroku:

* Installare l'[Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)
* Creare un [account gratuito su Heroku](https://www.heroku.com/)

Se hai installato Homebrew puoi installare la Heroku CLI dalla linea di comando:

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Adesso puoi usare git e l'Heroku CLI per deployare le tue applicazioni:

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

Questo è tutto. Se dovessi avere dei problemi controllare queste risorse per la risoluzione di questi:

* [Le basi di Git e GitHub](https://www.robinwieruch.de/git-essential-commands/)
* [Il deploy di applicazioni React con zero configurationi](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack per create-react-app](https://github.com/mars/create-react-app-buildpack)
