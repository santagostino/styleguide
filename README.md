# Style guide Santagostino DevOps

L'obiettivo è dare alcune indicazioni su come poter lavorare con la nuova Developer platform di Santagostino.

Daremo delel linee guida su:
- flusso di lavoro
- scrittura di commit e documentazione

## Utilizzo di GitHub
GitHub è il nostro punto d'inizio per tutti gli sviluppi.

**Perchè L'abbiamo scelto?**

- E' ben integrato con [Backstage](https://backstage.io/) (*lo strumento che abbiamo usato per creare endpoint principale per il catalogo servizi e per i nostri starterkit*)
- Le GitHub Actions sono strumento potente per le pipeline di CI/CD
- Ecosistema di servizi molto avanzata (Pages, Project, ...)

Consigliamo l'utilizzo delle **PullRequest** per strutturare meglio i rilasci e per avere un doppio check del codice

Consigliamo l'utilizzo dei test per migliorare la stabilità del codice per quelle parti **"critiche"** del codice.

**Alcune note sulle PR**

- tutte le PR (feature branch > stage / stage >master) devono usare l'opzione di github "create a merge commit" quando vengono approvate
- se i feature branch contengono più di un commit si può selezionare l'opzione **squash and merge**
- per le PR stage -> master non si deve mai usare l'opzione "squash and merge"
  
**Indicazioni di stile**

- Per ogni lavoro creiamo un branch (**non lavoriamo sui branch master e stage!**) usando questa sintassi `<feature|bug|fixed|>/[<numero_issue>_]<nome_branch>` 
(*esempio `feature/42_scopo_della_vita`*)
- Ogni pull request dovrà contenere un eventuale riferimento alla issue e il titolo della stessa. 
Utilizzare questa sintassi `[ref #<numero_issue> :] <titolo>`
- Ogni commit di sviluppo dovrebbe contenere il riferimento per la issue di sviluppo


## Workflows di lavoro
Di seguito disegnamo un flusso di sviluppo che è stato implmentato in tutti gli starterkit e che consigliamo anche per progetti che non partono dagli starterkit.

Da Backstage è possibile "istanziare" un nuovo servizio partendo dallo starterkit che soddisfa i requisiti base.

### Flusso per istanziare un nuovo servizio
1. Entrare su backstage
2. Scegliere lo starterkit desiderato
3. compilare gli step di creazione
4. Lanciare lo script di creazione/deploy

### Flusso d'inizio
1. Clonare il repository che è stato "istanziato" da backstage
2. fare il **checkout** del **branch di stage**
3. Aggiungere il file .env (c'è un file d'esempio nel repository) popolandolo con i dati necessari (API key ecc..) 
4. Lanciare la build per istanziare tutti i servizi del repository.
Usare il comando `make all`
5. Creare branch per lo sviluppo *(usare le linee guida descritte sopra)*

### Flusso di sviluppo
1. Se non esiste ancora create il branch di sviluppo *(usare le linee guida descritte sopra)*
2. Sviluppare e fare i commit con un messaggio sempre riferito alla issue (se essite)
3. Prima di un commit è bene lanciare i test per la verifica del codice.
Usare il comando `make unit-test`
4. pusshare in remote il branch (anche per un backup)
5. Per chiudere il progetto (se si deve passare ad un'altro) usare `make stop` per stoppare il docker
   
### Flusso deploy in stage
1. Aggiornare la docs
2. **NON** fare mai il merge su stage, creare una pullrequest indicando un reviewer tra quelli del team
3. Tenere d'occhio la review per modifiche sul codice o altre segnalazioni
4. Alla chiusura della PR fare sempre **create a merge commit" *(questo permette a github di non mostrare delle diff sbagliate)*

### Flusso deploy in master
4. Aggiornare la docs
5. **NON** fare mai il merge su stage, creare una pullrequest indicando un reviewer tra quelli del team
6. Tenere d'occhio la review per modifiche sul codice o altre segnalazioni
7. Alla chiusura della PR fare sempre **create a merge commit" *(questo permette a github di non mostrare delle diff sbagliate)*

## Gli starterkit
Abbiamo creato alcuni starterkit che coprono una buona parte delle possibilità di servizi che possiamo creare.

Uno starterkit è la base di sviluppo che già fornisce alcune funzionalità per aiutare il programmatore e per farlo concentrare sulla business logic, accellerando i tempi di sviluppo (non deve gestire, se non in casi particolari, il deploy e la configurazione del progetto).

**Uno starterkit racchiude:**
- script di deploy
- script di sviluppo
- script per la generazione della docs
- scheletro di infrastruttura
- script per IaC
- readme interno
- configurazione dei test
- configurazione e docs sul sistema di log
- docs per l'utilizzo

### Il makefile
Questo file contiene una serie di script che possono essere utilizati per alcune operazioni.

Aiuta a creare "alias" di comandi.

I comandi che troverete saranno:
- `make all`: eseguirà una serire di comandi per inizializzare il progetto
- `make up`: accende i docker per il progetto
- `make cli`: attiva la shell del container docker
- `make logs`: essendo in docker attivati in detaxch mode questo comando attiva il tail sui logs
- `make unit-test`: lancia la suite di test
- `make lint`: lancia il lint
- `make stage-deploy`: istnaza le librerie necessari per lo sviluppo
- `make build`: istanzia i docker necessari
- `make check-env-file`:  verifica la presenza del env file

### Installare dipendenze/plugin/moduli
Indipendentemente dal linguaggio (JS o PHP), per instalalre delel dipendenze, dovete sempre entrare nei container e da li lanciare i comandi.
In questo modo non si crea incongruenza con i lockfile dei gestori di pacchetti e non darà errore in prod o stage.

### La docs
#### Docs generica

La documentazione (se si parte dagli starterkit) verrà deployata in automatico, **si dovrà solo scriverla, perchè non partira dai commenti nel codice**

Verrà deployata su backstage in modo tale che sarà disponibile nel catalogo servizi.

La docs usa anche [PLANTUML](https://plantuml.com/) un ottimo plugin per fare grafici in markdown e utile per avere una documentaizone più strututrata e di facile lettura.

**Come creare una nuova pagina nelal docs?**
1. Editare il file `mkdocs.yml` aggiungendo la voce di menu desiderata e il path del file MD desiderato
2. Creare ed editare il fiel di pagina in `/docs/`

#### Docs swagger per le API

Se il servizio avrà delle API che potranno essere consumate all'esterno, **è bene** predisporre anche il file swagger per la documentazione delle API.

Anche questo file verrà letto e gestito da backstage per la generazione della documentazione, nell'apposita sezione API.

**Come creare la documentazione per le api?**
1. creare la cartella `api-docs` nella folder principale del progetto.
2. creare il file `<nome-servizio>_api.yml`
3. usare come template questo file [swagger backstage template](https://github.com/santagostino/styleguide/blob/master/swagger-template.yml)
4. inserire nella parte `definition` lo swagger del servizio
## Come utilizzare i dati sensibili (dati di login, chiavi di crypt)
I dati per accedere ai servizi (database, token API, chiavi JWT) sono dati molto sensibili, e averli in chiaro su un repository, potrebbero minarne la sicurezza.

Si è scelto di utilizzare le secrets di GitHub per conservare questi dati e passarli sempre come variabili nei workflow e negli ambienti di stage/prod.

L'organizzazione ha già molte chiavi di default che possono essere utilizzate, in più ogni repository può avere le sue personali.

### Come utilizzarle?

**Localmente** : Utilizzare il file .env (nel repository è commentato e deve essere creato come il file `.env.template`)

**Per ambienti di stage e prod**

- riportare le variabili su `docker-compose.yml`
- riportare le variabili sulle gitHub actions `prod.yml` `stage.yml` nella sezione *env*


**Come utilizzarli**
- localmente 
  - creare pure un file `.env`
  - toglierlo dal GIT (usando gitignore)
- deploy in stage/prod
  - andare su GitHub e a livello di progetto o di organizzazione settare una secrets apposita (Settings > Secrets)

**Best practice**
- se la secrets sarà usata da più servizi allora è bene metterla a livello di organizzazione
- 
## Come scrivere una issue
Una issue di bug/sviluppo è molto impostante perchè ci aiuta ad avere riferimento alla funzionalità sviluppata e avere una descrizione dettagliata del lavoro da fare/fatto.

Una Issue dovrebbe contenere:
- **Titolo** che ci aiuti a capire cosa c'è da fare
- **Descrizione** del lavoro da fare
- **CheckList/List subtask** dei task da fare, verrà man mano aggiornata per flaggare i task fatti, cosi da aiutarci nel capire lo stato avanzamento
- **DoD** che mi idenfica cosa mi aspetto dalal funzionalità per capire se il lavoro è concluso o meno (da scrivere prima di iniziare a svluppuare)
- **Lista delle pullrequest** associate (verrà aggiornato a mano ad ogni PR, per tenere uno storico)

## Come gestire test sulle chiamate API
Consigliamo di utilizzar eil plugin di VSCode [REST API](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) perchè permette di avere dei file testuali all'interno del git (opportunamente formattati) per fare chiamate REST.

Il vantaggio sarà quello di condividere tramite GIT le nostre chiamate e di averne alcune di test utili per debug.

**NOTA**: utilizzare estensione .http per avere identazione, colorazione e autocompletamento

## Strumenti consigliati
Alcuni strumenti che consigliamo per gestire al meglio un progetto
- **ClickUP**: piattaforma per gestire TASK sfruttando le potenzialità di TRELLO e delle metodologie Kanban, Scrum
- **VSCode**: Editor molto leggero e customizzabile
- **Docker**: Ottimo strumento per sviluppare, senza dover configurare ogni volta il proprio pc.
Ci permette anche di essere cross-platform a costo zero
- **DBeaver**: piattaforma open-source per collegarsi a moltissimi tipi di database
- **Postman**: client per REST API
- **Miro**: per la creazione di schemi e mappe concettuali collaborative

## Plugin di VSCode

### Code
- [Auto close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
- [Auto complete Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-complete-tag)
- [Auto rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
- [Babel ES6/ES7](https://marketplace.visualstudio.com/items?itemName=dzannotti.vscode-babel-coloring)
- [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
- [Beautify blade](https://marketplace.visualstudio.com/items?itemName=apility.beautify-blade)
- [Better comment](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
- [Code spell check](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
- [Italian code spell check](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker-italian)
- [Color picker](https://marketplace.visualstudio.com/items?itemName=anseki.vscode-color)
- [CSS peek](https://marketplace.visualstudio.com/items?itemName=pranaygp.vscode-css-peek)
- [DOT Env](https://marketplace.visualstudio.com/items?itemName=mikestead.dotenv)
- [Editor config](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [GIT Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)
- [GIT History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)
- [GIT Lens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens-insiders)
- [HTML CSS Support](https://marketplace.visualstudio.com/items?itemName=ecmel.vscode-html-css)
- [Indent rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
- [Local history](https://marketplace.visualstudio.com/items?itemName=xyz.local-history)
- [Markdown all-in-one](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)
- [Markdown Preview](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)
- [Markdown Lint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
- [Path intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
- [PHP cs fixer](https://marketplace.visualstudio.com/items?itemName=junstyle.php-cs-fixer)
- [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)
- [PHP Extension pack](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-pack)
- [PHP Formatter](https://marketplace.visualstudio.com/items?itemName=Sophisticode.php-formatter)
- [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)
- [PHP Namespace resolver](https://marketplace.visualstudio.com/items?itemName=MehediDracula.php-namespace-resolver)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Remote SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)
- [Thunder client (es postman)](https://marketplace.visualstudio.com/items?itemName=rangav.vscode-thunder-client)
- [SFTP](https://marketplace.visualstudio.com/items?itemName=liximomo.sftp)
- [Terminal](https://marketplace.visualstudio.com/items?itemName=formulahendry.terminal)
- [Settings sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)
- [TODO highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight)
- [XML Tools](https://marketplace.visualstudio.com/items?itemName=DotJoshJohnson.xml)

### Temi e icone e varie
- [Material icon theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- [Darcula Theme](https://marketplace.visualstudio.com/items?itemName=rokoroku.vscode-theme-darcula)
- [Polacode](https://marketplace.visualstudio.com/items?itemName=pnp.polacode)
- [PDF](https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf)
- [Excel Viewer](https://marketplace.visualstudio.com/items?itemName=GrapeCity.gc-excelviewer)

## Link Utili
- [12 Factor app](https://12factor.net/it/)
