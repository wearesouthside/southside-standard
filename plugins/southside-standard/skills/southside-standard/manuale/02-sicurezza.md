# 2. Sicurezza fin dal progetto

La sicurezza si decide prima di scrivere il codice, non dopo. Ogni progetto tiene l'**elenco delle vulnerabilità** qui
sotto come lista di controllo: per ognuna c'è una difesa nel codice e una **prova automatica** dentro `npm run verifica`.
Esempio completo: `backend/test/sicurezza.test.ts` e il test «testo malevolo» di `frontend/test/pagina.test.ts` nel Core,
la revisione in `docs/piano-tecnico.md` del Core.

## Gravità
| Gravità | Significato | Cosa si fa |
|---|---|---|
| **Critica** | Un estraneo legge o cambia dati, o prende il controllo del servizio | Si ferma tutto finché non è chiusa; non si pubblica |
| **Alta** | Una persona interna vede o fa più di quanto le spetta, o si espongono dati personali | Non si pubblica; correzione subito |
| **Media** | Una difesa manca, ma serve un altro errore per fare danni | Si corregge nella fase in corso, con data in `docs/stato.md` |
| **Bassa** | Igiene: rende più difficile sbagliare | Si pianifica |

## Elenco delle vulnerabilità
| # | Vulnerabilità | Gravità | Difesa obbligatoria | Prova (test o controllo) |
|---|---|---|---|---|
| V1 | **Accesso senza identità** (servizio raggiungibile da chiunque) | Critica | Cloud Run senza accesso pubblico; IAP per le persone, token Google per i servizi; la firma si verifica nel codice | Test: richiesta senza token o con token falso → 401 |
| V2 | **Dati di un'altra azienda** (tenant) | Critica | `tenant_id` su ogni tabella, Row-Level Security forzata; il tenant lo decide il server dall'identità | Test di isolamento: tenant A non legge né scrive righe di B; controllo che ogni tabella abbia RLS |
| V3 | **Iniezione SQL** | Critica | Solo query con parametri (`$1`); mai testo dell'utente concatenato | Test con `' OR 1=1 --` e simili nei filtri; ricerca nel codice di query costruite con `+` o template |
| V4 | **Credenziali nel codice o su GitHub** | Critica | Secret Manager o Portachiavi; identità di servizio al posto delle chiavi; `.gitignore` per `.env` | Scansione dei segreti su GitHub; controllo prima del commit; nessun file `.env` nel repository |
| V5 | **Permessi sbagliati** (un ruolo legge o modifica più del dovuto, IDOR) | Alta | Permesso per tipo di dato e azione controllato sul server a ogni indirizzo; mai fidarsi di un id mandato dal client | Matrice dei permessi: ogni ruolo × ogni indirizzo → atteso/ottenuto |
| V6 | **Dati personali esposti** (codici fiscali, stipendi, IBAN, note) a chi non deve | Alta | Dati minimi per ruolo; campi riservati vuoti senza permesso; documenti sensibili con permesso separato | Test: ruolo senza permesso riceve `null` nei campi riservati |
| V7 | **Dati reali su GitHub, nei log o nei test** | Alta | Solo dati inventati nei test; log senza IBAN, email, importi; nomi di file neutri | Ricerca nei commit e nei log (IBAN, codici fiscali, email); revisione del diff |
| V8 | **Script iniettato nelle pagine (XSS)** | Alta | Escape di ogni testo mostrato; niente `innerHTML`/`dangerouslySetInnerHTML` con dati; dati negli script solo come JSON sicuro | Testo malevolo (`<script>`, `"><img onerror>`) in ogni campo; pagine aperte con Playwright: nessuna esecuzione |
| V9 | **Content-Security-Policy assente** | Media | CSP con nonce, niente script negli attributi (`onclick`) | Test che ogni pagina abbia l'intestazione CSP |
| V10 | **Richieste da altri siti (CSRF)** | Alta | Scritture accettate solo dalla stessa origine (controllo di `Origin`/`Sec-Fetch-Site`), cookie `SameSite` | Test: POST con `Origin` estraneo → 403 |
| V11 | **Cancellazioni o modifiche senza traccia** | Alta | Divieto di DELETE nel database; registro con chi, cosa, quando, prima/dopo; il registro non si modifica | Test: DELETE rifiutato; ogni scrittura produce una riga nel registro |
| V12 | **Servizio con troppi poteri sul database** | Media | Due ruoli: «migrazioni» (schema) all'avvio, «lavoro» (solo righe, niente DELETE/TRUNCATE) per il resto | Tutti i test girano con il ruolo di lavoro |
| V13 | **Account di servizio con troppi ruoli** | Media | Un account per servizio, minimo indispensabile, nessun ruolo su altri progetti | Revisione dei ruoli IAM a ogni fase; elenco in `docs/stato.md` |
| V14 | **File caricati pericolosi** (percorsi, dimensioni, tipi, zip bomb) | Alta | Nome del file mai usato come percorso; limite di dimensione; tipo controllato dal contenuto; zip con limiti | Test con `../`, file enormi, tipo falso, zip annidati |
| V15 | **Input ostili** (corpo enorme, JSON rotto, `__proto__`) | Media | Limite al corpo, parse con errori gestiti, nessuna fusione di oggetti dal client | Test: 70 KB, JSON rotto, `__proto__` → respinti o innocui |
| V16 | **Assegnazione di massa** (il client manda campi che non deve) | Alta | Ogni scrittura copia solo i campi ammessi, elencati | Test: campo `tenant_id` o `ruolo` nel corpo → ignorato |
| V17 | **Formule nei CSV/Excel esportati** | Bassa | Celle che iniziano con `= + - @` precedute da `'` | Test sull'esportazione |
| V18 | **Indirizzi costruiti dall'intestazione `Host`** o reindirizzamenti aperti | Bassa | Indirizzi fissi da configurazione; reindirizzamenti solo verso percorsi interni | Test con `Host` estraneo e `?ritorno=https://…` |
| V19 | **Chiamate verso indirizzi decisi dall'utente (SSRF)** | Alta | Nessun `fetch` verso URL arrivati dal client; elenco fisso dei servizi esterni | Ricerca nel codice; test |
| V20 | **Dipendenze vulnerabili o compromesse** | Media | Dipendenze al minimo, versioni fissate, lockfile nel repository, nessuna versione più giovane di 2 settimane | `npm audit` nella batteria; Dependabot/avvisi di GitHub accesi |
| V21 | **Errori che raccontano troppo** (stack, query, percorsi) | Bassa | Messaggio semplice al client, dettaglio solo nei log con l'id della richiesta | Test: errore 500 senza stack nella risposta |
| V22 | **Chiamate senza tempo massimo** o senza limite di frequenza | Bassa | Timeout su ogni chiamata esterna; limiti sugli indirizzi pesanti | Test con servizio esterno lento |
| V23 | **Bucket o database raggiungibili da fuori** | Critica | Bucket privati con accesso uniforme; Cloud SQL solo con IAM e SSL; documenti solo tramite il servizio | Controllo della configurazione nello script di infrastruttura |
| V24 | **Aperture di documenti sensibili non registrate** | Media | Registro delle aperture nella stessa transazione della lettura | Test: aprire un documento scrive il registro |
| V25 | **Pubblicazione di codice non controllato** | Media | Si pubblica solo `main` allineato a GitHub e con la batteria verde (capitolo 5) | Lo script di pubblicazione si ferma se non è così |
| V26 | **Istruzioni nascoste nei contenuti per l'AI** (prompt injection) nelle funzioni con Claude | Alta | I testi letti (documenti, email, causali) sono dati, mai istruzioni; l'AI suggerisce, non decide; nessuna azione con effetti senza conferma di una persona | Test con testi che contengono istruzioni |
| V27 | **Sessioni e cookie deboli** | Media | Cookie `Secure`, `HttpOnly`, `SameSite=Lax`; nessun token nel `localStorage` | Test sulle intestazioni |
| V28 | **CORS aperto** | Media | Nessun `Access-Control-Allow-Origin: *` sulle API con dati | Test sulle intestazioni |

## Come si usa
- **Progetto nuovo**: si copia l'elenco in `docs/sicurezza.md` del progetto con una colonna «Stato» (✅ coperta e
  provata, ⚠️ aperta con data, — non pertinente e perché).
- **Ogni modifica**: Claude controlla quali righe tocca e aggiorna la prova. Una riga Critica o Alta aperta blocca la
  pubblicazione.
- **Ogni fase**: revisione completa come in `docs/piano-tecnico.md` del Core (iniezione su tutte le pagine, matrice dei
  permessi, storia di GitHub, log di produzione, dipendenze).
- **Strumenti:** il plugin `security-guidance` avvisa mentre si scrive; `/security-review` rivede le modifiche;
  il plugin `claude-security` fa la scansione approfondita prima delle pubblicazioni importanti e a ogni fase. Trovano
  i problemi generici; le regole di Southside (V2, V6, V7, V11, V24…) le controllano i nostri test.
- Una vulnerabilità nuova trovata in un progetto si aggiunge qui, con gravità e prova, per tutti.
