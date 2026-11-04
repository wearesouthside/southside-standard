---
name: southside-standard
description: Standard di sviluppo Southside. Da usare SEMPRE quando si lavora in un repository dell'ecosistema Southside (Core, Console, Southsiders, Planner, CRM, Performance o un progetto nuovo) per progettare, scrivere, rivedere o pubblicare codice; quando si avvia un progetto nuovo; quando si integra un'app con il Core; quando si disegnano pagine o componenti; quando si parla di sicurezza, test, pubblicazione, documentazione dell'architettura o delle API.
---

# Standard Southside

Regole comuni a tutti i progetti di Southside, scritte per lavorare in «vibe coding» con Claude Code senza perdere
sicurezza, ordine e coerenza. Esempio di riferimento: `wearesouthside/Southside-core`.
Il dettaglio sta nei capitoli di `manuale/`: leggi quello che serve al compito prima di scrivere codice.

| Capitolo | Quando leggerlo |
|---|---|
| `manuale/01-principi.md` | sempre, all'inizio di un progetto o di una sessione |
| `manuale/02-sicurezza.md` | ogni modifica che tocca dati, accessi, input, file, dipendenze, pubblicazione |
| `manuale/03-dati-nel-database.md` | ogni volta che compare un nome, un importo, una data, una soglia o una regola dell'azienda |
| `manuale/04-backend-e-frontend.md` | quando si decide dove mettere una logica o si costruisce una pagina |
| `manuale/05-test-e-pubblicazione.md` | prima di ogni commit, ogni pubblicazione, ogni test nuovo |
| `manuale/06-interfaccia.md` | ogni pagina, componente, colore, carattere |
| `manuale/07-architettura-e-api.md` | ogni area, dato, indirizzo nuovo; ogni integrazione tra progetti |
| `manuale/08-lavorare-con-claude.md` | come si parla con Marco, come si tiene lo stato, git, avvio di un progetto nuovo |

## Le regole in una pagina (non si negoziano)
1. **Un solo punto di verità.** Ogni dato ha un solo proprietario (il Core per anagrafiche, organizzazione, aziende,
   commesse, fatture, banca, documenti). Le altre app leggono dall'API e non tengono copie modificabili. Solo la
   Console scrive nel Core; le altre app mandano proposte.
2. **Sicurezza fin dal progetto.** Ogni vulnerabilità dell'elenco (`02-sicurezza.md`) ha una difesa nel codice e un test.
   Critica o Alta aperta = non si pubblica.
3. **Nessun dato nel codice e niente dati su GitHub.** Nomi, conti, importi, date, soglie e regole stanno nel database e
   si cambiano da un'interfaccia; credenziali solo nel Secret Manager; nei test solo dati inventati.
4. **Regole nel backend, interfaccia dinamica.** Il backend calcola e decide; il frontend mostra quello che riceve e non
   ricalcola. Menu, etichette, scelte e permessi arrivano dai dati.
5. **Test continui che bloccano la pubblicazione.** `npm run verifica` (tipi + test) prima di ogni commit; la stessa
   batteria gira su GitHub e su Cloud Build; si pubblica solo `main` verde.
6. **Architettura e API descritte in modo completo.** Ogni progetto ha `docs/architettura-funzionale.md`,
   `docs/architettura.md`, `docs/api.md` e un riferimento delle risorse generato dal codice; un test ferma il commit se la
   documentazione non corrisponde al codice. Chi si integra deve poter lavorare leggendo solo questi file.
7. **Un'interfaccia sola.** Material Design 3 per tutti i componenti, con MUI e il tema `@southside/ui`
   (`wearesouthside/southside-ui`); regole visive in `docs/design.md` del Core. Chiaro e scuro, computer e telefono.
8. **Non si cancella nulla.** Si chiude o si archivia; ogni modifica nel registro (chi, cosa, quando, prima/dopo).
9. **Ogni tabella ha `tenant_id`** con Row-Level Security; nessuna query senza azienda.
10. **Italiano semplice** nei testi per le persone; Marco non è uno sviluppatore: comandi pronti, uno per blocco, con
    l'output atteso.

## Prima di dire «fatto»
- [ ] `npm run verifica` verde (e la batteria di sicurezza dentro).
- [ ] Nessun dato reale, nome di persona o credenziale nei file del commit (`git diff --cached`).
- [ ] Nessun valore dell'azienda scritto nel codice: è in tabella o in impostazioni.
- [ ] Documentazione aggiornata: `docs/architettura-funzionale.md`, `docs/api.md`, riferimento generato, `docs/stato.md`.
- [ ] Pagine guardate su computer e telefono, chiaro e scuro, con il tema Southside.
- [ ] Ogni vulnerabilità toccata ha il suo test (`02-sicurezza.md`, colonna «Prova»); prima di pubblicare,
      `/security-review` sulle modifiche (e il plugin `claude-security` per le pubblicazioni importanti).
- [ ] Sintesi per Marco: cosa cambia, cosa deve fare lui.

## Cosa non fa questa skill
Solo le regole di Southside. Per sicurezza generica, revisione del codice e `CLAUDE.md` si usano i plugin ufficiali
attivati dal modello di `.claude/settings.json` (`manuale/08-lavorare-con-claude.md`). Non usare `frontend-design`:
l'aspetto è quello del tema Southside.

## Progetto nuovo
Copia i file della cartella `modelli/` accanto a questo file (`.claude/settings.json`, `CLAUDE.md`, `.gitignore`,
`.github/workflows/verifica.yml`, `docs/`), poi segui `manuale/08-lavorare-con-claude.md` § «Avvio di un progetto».
