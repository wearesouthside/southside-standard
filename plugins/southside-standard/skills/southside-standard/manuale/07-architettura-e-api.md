# 7. Architettura e API descritte in modo completo

## La regola
Ogni progetto deve poter essere **integrato leggendo solo la sua documentazione**: chi costruisce un'altra app (o la
sessione di Claude Code di un altro repository) deve sapere cosa fa il progetto, quali dati custodisce, chi li scrive,
quali indirizzi può chiamare, con quale identità e permesso, in che forma arrivano le risposte e cosa è garantito.
Vale per tutti, **soprattutto per il Core**, da cui leggono tutte le app.

La documentazione **segue il codice**: ciò che può essere generato dal codice si genera, e un test ferma il commit se il
file nel repository non è aggiornato.

## I documenti obbligatori
| File | Cosa contiene | Come si tiene aggiornato |
|---|---|---|
| `docs/architettura-funzionale.md` | **Cosa fa** il progetto: le aree, per ognuna i dati custoditi, da dove arrivano, le regole, chi li scrive e chi li legge; chi parla con il servizio; i flussi principali; cosa è garantito a chi si integra | A mano, a ogni area o flusso nuovo; un test controlla che nomini ogni risorsa pubblica |
| `docs/architettura.md` | **Com'è fatto** il codice: cartelle, regole per cartella, «come si aggiunge un dato / un calcolo / una pagina» | A mano, a ogni cambio di struttura |
| `docs/api.md` | **Guida per chi si integra**: cosa è pubblico e cosa interno, accesso (identità, ruolo, intestazioni), forma delle risposte, pagine, novità, errori, esempio di codice, regole di compatibilità, indirizzi dedicati | A mano; un test controlla che nomini ogni risorsa |
| `docs/api-risorse.md` (o simile) | **Riferimento** di ogni risorsa: indirizzo, permesso, campi con tipo, obbligatorietà, riservatezza e significato; tabella «chi legge cosa» per ruolo | **Generato** dal catalogo del codice (`npm run documenta`); un test lo confronta |
| `GET /api/v1/contratto` | Contratto macchina OpenAPI 3.1 | Generato dal catalogo; test che ogni risposta lo rispetti |
| `docs/stato.md` | Cosa c'è online, indirizzi, cosa resta aperto | A ogni passo importante |
| `docs/impostazione.md` | Regole dell'ecosistema (identico ovunque) | Si copia dal Core quando cambia |

Esempio completo: Core (`docs/architettura-funzionale.md`, `docs/api.md`, `docs/api-risorse.md`,
`backend/src/api/catalogo.ts`, `backend/src/api/documentazione.ts`, `backend/test/documentazione.test.ts`).

## Come si descrive un'area (architettura funzionale)
Per ogni area, sempre le stesse voci:
- **Cosa:** i dati che custodisce, in parole semplici.
- **Da dove:** chi li inserisce o da quale sistema arrivano (e quando: a mano, caricamento, sincronizzazione pianificata).
- **Regole:** controlli, stati, calcoli, cosa è chiuso, cosa non si può fare.
- **Chi legge / chi scrive:** ruoli e applicazioni.
- **Risorse:** gli indirizzi pubblici che la espongono (nomi tra apici inversi, es. `persone`).

## Come si descrive un'API
- **Pubblico o interno**: gli indirizzi pubblici hanno un contratto stabile; quelli interni (viste per una sola
  interfaccia) sono marcati e non si usano da altre app.
- **Versione nel percorso** (`/api/v1/`); in `v1` i campi **si aggiungono soltanto**; togliere o cambiare = `v2`, tenendo
  la `v1` finché i client non sono passati. I client ignorano i campi che non conoscono.
- **Una forma sola** per gli elenchi: `{ letto_il, risorsa, dati, prossimo }`, ordine per modifica, pagine con
  `?limite=` e `?dopo=`, novità con `?modificati_dal=`; le righe chiuse restano con la loro data.
- **Accesso**: identità (account di servizio Google, IAP per le persone), ruolo da chiedere, intestazioni speciali.
- **Errori**: sempre JSON `{ errore }`, codici elencati, id della richiesta nell'intestazione per i log.
- **Formati**: importi in centesimi interi, date `AAAA-MM-GG`, istanti ISO UTC, UUID.
- **Esempio funzionante** di lettura (con pagine) nella guida.

## Integrare un progetto con un altro
1. Leggere `docs/architettura-funzionale.md` e `docs/api.md` del progetto da cui si legge (di solito il Core).
2. Usare solo gli indirizzi pubblici; se un dato manca, si chiede di aggiungerlo al catalogo del proprietario
   (nella sessione di quel repository), non si usano gli indirizzi interni e non si copia il dato da altre parti.
3. Chiedere al titolare l'identità di servizio e il ruolo nel proprietario dei dati.
4. Tenere una vista locale solo in lettura, aggiornata con `?modificati_dal=` (margine di 5 minuti, doppioni scartati per id).
5. Descrivere l'integrazione nella propria `docs/architettura-funzionale.md` (sezione «Chi parla con il servizio» e flussi).

## Per il proprietario dei dati (es. il Core)
- Ogni risorsa nuova nasce nel **catalogo** con descrizione di risorsa e di ogni campo: da lì vengono lettura, contratto,
  riferimento e test. Niente campo senza descrizione.
- Ogni area nuova → sezione in `docs/architettura-funzionale.md`.
- Ogni indirizzo dedicato a un'app (es. `/portale` per Southsiders) → sezione in `docs/api.md`.
