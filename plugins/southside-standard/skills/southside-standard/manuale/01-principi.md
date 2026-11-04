# 1. Principi

## Perché questo manuale
A Southside il software si costruisce con Claude Code: chi decide (Marco e i responsabili) descrive cosa serve, Claude
scrive il codice. È veloce, ma senza regole comuni ogni progetto prende una strada diversa: dati copiati in più posti,
numeri diversi tra due app, password nel codice, pagine diverse una dall'altra. Questo manuale è il patto comune: Claude
lo applica in ogni repository, chi rivede il lavoro lo usa come lista di controllo.

## L'ecosistema
| Pezzo | Ruolo |
|---|---|
| **Southside Core** (`wearesouthside/Southside-core`) | Servizio senza interfaccia: unico proprietario dei dati dell'azienda, applica regole, storico e registro |
| **Console** (stesso repository del Core) | Unica interfaccia che scrive nel Core |
| **Southsiders**, **Planner**, **CRM**, **Performance**… | Applicazioni che leggono dal Core e custodiscono solo i propri dati |
| **southside-ui** | Tema Southside per MUI: l'aspetto comune di tutte le app React/Next |
| **southside-standard** | Questo manuale e il plugin di Claude Code che lo applica |

Documento vincolante sull'ecosistema: `docs/impostazione.md` del Core, identico in tutti i repository.

## I principi
1. **Un solo punto di verità.** Ogni dato ha un proprietario. Gli altri lo leggono da lì, sempre: niente copie
   modificabili, niente fogli paralleli. Una vista locale in sola lettura è ammessa (con `?modificati_dal=`).
2. **Le regole stanno nel backend.** Un numero dell'azienda si calcola in un posto solo, così due app non possono dare
   risultati diversi (capitolo 4).
3. **I dati stanno nel database, non nel codice** (capitolo 3).
4. **Sicurezza fin dal progetto**, non aggiunta dopo (capitolo 2).
5. **Si pubblica solo ciò che ha passato i controlli** (capitolo 5).
6. **Ogni progetto si spiega da solo**: chi si integra legge la documentazione e sa tutto (capitolo 7).
7. **Un'interfaccia sola** per tutte le app (capitolo 6).
8. **Non si cancella nulla**; ogni modifica lascia traccia.
9. **Semplicità**: dipendenze al minimo e con versione fissata, un file per area, nomi chiari in italiano.
10. **Si lavora a piccoli passi**: ogni passo verificato, pubblicato e scritto in `docs/stato.md`.

## Tecnologia di riferimento
- TypeScript su Node 22.18+ (eseguito direttamente: solo sintassi cancellabile, import relativi con `.ts`).
- Google Cloud, regione `europe-west3`: Cloud Run (a zero istanze quando nessuno lo usa), Cloud SQL Postgres,
  Cloud Storage, Secret Manager, IAP per il login delle app interne, identità di servizio tra servizi.
- Interfacce: pagine generate dal server (come la Console) oppure React/Next con MUI e `@southside/ui`.
- Ogni risorsa Google Cloud con le etichette `app` e `ambiente`.
- Cartelle principali in inglese (`backend/`, `frontend/`, `docs/`), nomi e commenti dentro in italiano.
