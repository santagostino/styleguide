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

Consigliamo l'utilizzo delle **PullRequest** per strutturare meglio i rilasci e per avre un doppio check del codice


**Indicazioni di stile**

- Per ogni lavoro creiamo un branch (**non lavoriamo sui branch master e stage!**) usando questa sintassi `<feature|bug|fixed|>/[<numero_issue>_]<nome_branch>` esempio `feature/42_scopo_della_vita`
- Ogni pull request dovrà contenere un eventuale riferimento alla issue e il titolo della stessa. Utilizzare questa sintassi `[ref #<numero_issue> :] <titolo>`


## Flow di sviluppo
@TODO
### Gli starterkit
@TODO
#### Il make file
@TODO
#### La docs
@TODO

## Come scrivere una issue
@TODO
## Strumenti consigliati
Alcuni strumenti che consigliamo per gestire al meglio un progetto
- **ClikUP**: piattaforma per gestire TASK sfruttando le potenzialità di TRELLO e delle metodologie Kanban, Scrum
- **VSCode**: Editor molto leggero e customizzabile
- **Docker**: Ottimo strumento per sviluppare, senza dover configurare ogni volta il proprio pc. Ci permette anche di essere cross-platform a costo zero
- **DBeaver**: piattaforma open-source per collegarsi a moltissimi tipi di database
- **Postman**: client per REST API