# Prompt di avvio

Da incollare come primo messaggio in una sessione di Claude Code aperta **nel repository del proprio progetto**
(nuovo o esistente). Porta il progetto dentro lo standard Southside e gli fa conoscere il Core.
Prima, una volta sul computer: installare il plugin (README, «Installare il plugin»).

---

Lavori su un progetto dell'ecosistema **Southside** (organizzazione GitHub `wearesouthside`). Rispondi sempre in
italiano, con sintesi chiare: cosa cambia e cosa devo fare io; comandi pronti da incollare, uno per blocco, con
l'output atteso. Prima di costruire qualcosa di grosso o ambiguo fammi una domanda breve.

**1. Lo standard.** Usa la skill `southside-standard` (plugin del marketplace `southside`,
repository `wearesouthside/southside-standard`) e leggi i capitoli del manuale che servono. Se la skill non è
disponibile, fermati e dimmi come installarla (README di `southside-standard`). Le regole che non si negoziano:
- un solo punto di verità: i dati dell'azienda (persone, organizzazione, accessi, aziende, commesse, fatture, banca,
  documenti) li custodisce **Southside Core**; questo progetto li legge dall'API pubblica del Core e non ne tiene copie
  modificabili; solo la Console del Core scrive nel Core, gli altri mandano proposte;
- sicurezza fin dal progetto (elenco delle vulnerabilità del capitolo 2, con prova per ciascuna);
- nessun dato nel codice e niente dati reali su GitHub; credenziali solo nel Secret Manager;
- regole e calcoli nel backend, interfaccia che mostra;
- `npm run verifica` (tipi + test) prima di ogni commit e su GitHub; si pubblica solo `main` verde;
- architettura funzionale e API descritte in modo completo, controllate dai test;
- interfaccia con i componenti Material Design 3 di MUI e il tema `@southside/ui` (`wearesouthside/southside-ui`,
  installato da git con il tag di versione); chiaro e scuro, computer e telefono;
- non si cancella nulla; `tenant_id` con Row-Level Security su ogni tabella; niente `git push --force`.

**2. Il Core.** Prima di scrivere codice che usa dati dell'azienda, leggi dal repository `wearesouthside/Southside-core`
(chiedimi di aggiungerlo alla sessione se non lo vedi):
- `docs/impostazione.md` — regole dell'ecosistema (va copiato identico anche qui);
- `docs/architettura-funzionale.md` — cosa fa il Core, area per area, chi scrive, i flussi;
- `docs/api.md` — come si legge: accesso con identità di servizio Google, ruoli, forma delle risposte, pagine,
  novità con `?modificati_dal=`, errori, indirizzi dedicati;
- `docs/api-risorse.md` — ogni risorsa con i campi e la tabella «chi legge cosa»;
- `docs/design.md` — regole visive comuni.
Usa solo gli indirizzi pubblici (`/api/v1/dati/<risorsa>`, `/contratto`, `/io`, `/stato`). Se ti manca un dato,
**non** usare gli indirizzi interni e non copiarlo da altre fonti: scrivimi quale risorsa o campo chiedere di aggiungere
al catalogo del Core.

**3. Cosa fare adesso.**
1. Leggi questo repository e dimmi in poche righe cosa fa, com'è fatto e cosa usa del Core.
2. Confrontalo con lo standard e fammi l'elenco di ciò che manca o va cambiato, in ordine di importanza
   (prima sicurezza e dati), con la gravità per i punti di sicurezza. Non cambiare nulla finché non ti rispondo.
3. Dopo il mio via, a piccoli passi, ognuno con `npm run verifica` verde e un commit:
   - copia i modelli dello standard che mancano (`.claude/settings.json`, `CLAUDE.md`, `.gitignore`,
     `.github/workflows/verifica.yml`, `docs/stato.md`, `docs/architettura-funzionale.md`, `docs/architettura.md`,
     `docs/api.md`, `docs/design.md`, `docs/sicurezza.md`) e riempili con i dati veri del progetto;
   - aggiungi questo progetto in `docs/impostazione.md` (§2 i pezzi, §3 la mappa dei dati) e dimmi il testo da far
     riportare negli altri repository;
   - porta l'interfaccia su MUI con `@southside/ui` (se è un'app React/Next);
   - aggiungi i test di sicurezza e di documentazione che mancano.
4. Alla fine aggiorna `docs/stato.md` e dimmi: cosa è cambiato, cosa devo fare io (permessi Google, identità di servizio
   e ruolo nel Core, regola di protezione di `main` su GitHub), cosa resta aperto.
