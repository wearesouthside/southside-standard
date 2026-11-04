# <Nome del progetto> — istruzioni per Claude Code

<Una frase: cosa fa il progetto e per chi.> Fa parte dell'ecosistema Southside.
Standard comune: plugin `southside-standard` (skill omonima, si attiva da `.claude/settings.json`): **applicarlo sempre**.
Riferimento dell'ecosistema: `docs/impostazione.md` (identico in tutti i repository).
**All'inizio di ogni sessione leggere `docs/stato.md`** e aggiornarlo a ogni passo importante.

## Come lavorare con Marco
- Sempre in italiano, sintesi da executive: cosa cambia, cosa deve fare lui.
- Marco non è uno sviluppatore: comandi pronti da incollare, uno per blocco, con l'output atteso.
- Prima di costruire qualcosa di grosso o ambiguo, una domanda breve; sulle richieste chiare si parte subito.

## Vincoli che non si negoziano
- Niente dati personali o aziendali su GitHub; nei test solo dati inventati.
- Credenziali solo in Secret Manager; tra servizi identità di servizio Google.
- Nessun dato nel codice: nomi, conti, date e regole dell'azienda stanno nel database.
- Non si cancella nulla: si chiude o si archivia; ogni modifica nel registro.
- Ogni tabella ha `tenant_id` con Row-Level Security.
- Dati del Core letti solo dall'API pubblica del Core (`docs/api.md` del Core), mai copiati in forma modificabile.
- Interfaccia: MUI con `@southside/ui` (o stile della Console), Material Design 3, chiaro e scuro, computer e telefono.
- Niente `git push --force`, niente riscritture della storia.

## Struttura
<Cartelle principali e cosa contengono. Dettaglio in `docs/architettura.md`; cosa fa il progetto in
`docs/architettura-funzionale.md`; API in `docs/api.md`.>

## Verifiche prima di ogni commit
```bash
npm run verifica     # tipi + test (sicurezza e documentazione comprese)
```
Commit piccoli e descrittivi in italiano.
