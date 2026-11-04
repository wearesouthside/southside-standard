# Southside Standard — istruzioni per Claude Code

Manuale del vibe coding di Southside e plugin di Claude Code (marketplace `southside`, plugin `southside-standard`).

- Sempre in italiano, sintesi da executive; il titolare non è uno sviluppatore: comandi pronti, uno per blocco, con l'output atteso.
- Il manuale sta **solo** in `plugins/southside-standard/skills/southside-standard/manuale/` (il plugin installato vede
  solo la sua cartella): niente copie altrove, il README rimanda lì.
- `SKILL.md` resta corto (si carica a ogni uso): regole principali e lista di controllo; il dettaglio va nei capitoli.
- La skill contiene solo ciò che è di Southside; per il generico si usano plugin ufficiali (capitolo 8).
- Esempi presi dal Core (`wearesouthside/Southside-core`): citare file reali, non inventarne.
- Niente dati personali o aziendali (nomi, importi, IBAN, email): negli esempi solo dati inventati.
- A ogni modifica: versione in `plugin.json` e `marketplace.json`, `CHANGELOG.md`, poi
  `claude plugin validate .` e `claude plugin validate plugins/southside-standard` verdi.
- Niente `git push --force`, niente tag spostati.
