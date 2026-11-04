# 5. Batteria di test continui e blocco della pubblicazione

## Un solo comando
Ogni progetto ha `npm run verifica` = **controllo dei tipi + tutti i test** (con un Postgres usa-e-getta se serve).
Si esegue:
1. **prima di ogni commit** (Claude non fa commit con la verifica rossa);
2. **su GitHub a ogni push** (`.github/workflows/verifica.yml`, modello in `modelli/`);
3. **su Cloud Build prima di costruire l'immagine**: se fallisce, l'immagine non nasce.

## Cosa contiene la batteria
| Gruppo | Cosa prova | Esempio nel Core |
|---|---|---|
| Tipi | `tsc` senza errori, modalità `strict` | `npm run tipi` |
| Calcoli | ogni funzione pura di dominio con dati inventati; numeri identici prima e dopo un riordino | `backend/test/*.test.ts` |
| Database | migrazioni da zero, RLS su ogni tabella, divieto di cancellare, registro, ruolo di lavoro | `core.test.ts` |
| Sicurezza | l'elenco del capitolo 2: identità, isolamento tra aziende, matrice dei permessi, iniezioni, input ostili, CSRF, CSP | `sicurezza.test.ts`, `pagina.test.ts` |
| Contratto | ogni risposta pubblica rispetta il contratto OpenAPI; il catalogo corrisponde al database | `contratto.test.ts` |
| Documentazione | il riferimento delle API generato è aggiornato; la guida nomina ogni risorsa (capitolo 7) | `documentazione.test.ts` |
| Interfaccia | ogni pagina si apre, testo malevolo innocuo; per i temi: contrasto e regole del marchio | `pagina.test.ts`, test di `southside-ui` |
| Dipendenze | `npm audit` senza vulnerabilità alte | nella pipeline |

## Blocco della pubblicazione
- Si pubblica **solo `main`**, pulito e uguale a GitHub, con la batteria verde (lo script di pubblicazione lo controlla
  e si ferma, come `infra/c1.sh immagini` nel Core).
- Su GitHub: **regola di protezione di `main`** con il controllo «verifica» obbligatorio e niente push forzati
  (Settings → Branches → Add rule; lo imposta Marco).
- Le librerie (es. `southside-ui`) pubblicano un **tag di versione** solo dopo il controllo verde; i tag non si spostano.
- Prima di pubblicare un riordino: confronto delle pagine vere prima e dopo, devono essere identiche.
- Dopo ogni pubblicazione: una riga in `docs/stato.md` (cosa è online, da quando).

## Regole sui test
- Solo dati inventati.
- Un errore trovato a mano diventa un test, così non torna.
- Un test non si spegne, non si salta, non si «aggiusta» per farlo passare: si corregge il codice (o si discute con
  Marco se è il test a essere sbagliato).
- I test sono veloci: la verifica completa sta in pochi minuti.
