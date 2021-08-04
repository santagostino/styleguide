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

### Flusso deploy in master
4. Aggiornare la docs
5. **NON** fare mai il merge su stage, creare una pullrequest indicando un reviewer tra quelli del team
6. Tenere d'occhio la review per modifiche sul codice o altre segnalazioni

### Gli starterkit
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

#### Il makefile
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

#### La docs
La documentazione (se si parte dagli starterkit) verrà deployata in automatico, **si dovrà solo scriverla, perchè non partira dai commenti nel codice**

Verrà deployata su backstage in modo tale che sarà disponibile nel catalogo servizi.

La docs usa anche [PLANTUML](https://plantuml.com/) un ottimo plugin per fare grafici in markdown e utile per avere una documentaizone più strututrata e di facile lettura.

**Come creare una nuova pagina nelal docs?**
1. Editare il file `mkdocs.yml` aggiungendo la voce di menu desiderata e il path del file MD desiderato
2. Creare ed editare il fiel di pagina in `/docs/`

## Come scrivere una issue
Una issue di bug/sviluppo è molto impostante perchè ci aiuta ad avere riferimento alla funzionalità sviluppata e avere una descrizione dettagliata del lavoro da fare/fatto.

Una Issue dovrebbe contenere:
- **Titolo** che ci aiuti a capire cosa c'è da fare
- **Descrizione** del lavoro da fare
- **CheckList/List subtask** dei task da fare, verrà man mano aggiornata per flaggare i task fatti, cosi da aiutarci nel capire lo stato avanzamento
- **DoD** che mi idenfica cosa mi aspetto dalal funzionalità per capire se il lavoro è concluso o meno (da scrivere prima di iniziare a svluppuare)
- **Lista delle pullrequest** associate (verrà aggiornato a mano ad ogni PR, per tenere uno storico)
## Strumenti consigliati
Alcuni strumenti che consigliamo per gestire al meglio un progetto
- **ClickUP**: piattaforma per gestire TASK sfruttando le potenzialità di TRELLO e delle metodologie Kanban, Scrum
- **VSCode**: Editor molto leggero e customizzabile
- **Docker**: Ottimo strumento per sviluppare, senza dover configurare ogni volta il proprio pc.
Ci permette anche di essere cross-platform a costo zero
- **DBeaver**: piattaforma open-source per collegarsi a moltissimi tipi di database
- **Postman**: client per REST API