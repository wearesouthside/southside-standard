# 8. Lavorare con Claude Code

## Come si parla con il titolare (e con chi non sviluppa)
- **Sempre in italiano**, sintesi da executive: cosa cambia, cosa deve fare lui. Niente racconto dei passaggi.
- Comandi **pronti da incollare, uno per blocco**, con cosa aspettarsi in output.
- Prima di costruire qualcosa di grosso o ambiguo, **una domanda breve**; sulle richieste chiare si parte subito.
- Le cose che solo il titolare può fare (permessi Google, Secret Manager, regole di GitHub) si elencano chiare alla fine.

## Memoria del progetto
- `CLAUDE.md`: istruzioni per Claude nel repository (modello in `modelli/CLAUDE.md`).
- `docs/stato.md`: **si legge all'inizio di ogni sessione** e si aggiorna a ogni passo importante (cosa è online,
  indirizzi, cosa resta aperto, decisioni con data).
- Le decisioni del titolare si scrivono con la data; quelle dell'ecosistema in `docs/impostazione.md`.

## Git
- Commit piccoli e descrittivi in italiano, con `npm run verifica` verde.
- Niente `git push --force`, niente riscritture della storia, niente tag spostati.
- Si lavora sul ramo indicato dalla sessione; si pubblica solo `main`.
- Repository nell'organizzazione **`wearesouthside`**, privati.

## Skill e plugin: i nostri e quelli già pronti
La skill `southside-standard` contiene **solo ciò che è di Southside** (un proprietario per ogni dato, tenant e RLS,
nessun dato nel codice, tema `@southside/ui`, documentazione generata, modo di lavorare con il titolare). Per i lavori
generici si usano i plugin ufficiali di Anthropic (marketplace `claude-plugins-official`), già attivati dal modello di
`.claude/settings.json`:

| Plugin | Cosa fa | Quando |
|---|---|---|
| `security-guidance` | Avvisi automatici mentre Claude scrive codice a rischio, revisione del diff a fine turno | Sempre attivo (agganci automatici) |
| `claude-security` | Scansione approfondita delle vulnerabilità del proprio codice, con verifica e proposta di correzione | Prima di ogni pubblicazione importante e a ogni revisione di fase (capitolo 2) |
| `code-review` | Revisione delle modifiche con più agenti, filtra i falsi allarmi | Prima di unire un ramo in `main` |
| `typescript-lsp` | Navigazione del codice TypeScript (definizioni, riferimenti, errori) | Sempre attivo; serve `typescript-language-server` installato |
| `claude-md-management` | Controlla e migliora `CLAUDE.md`, ci riporta ciò che si è imparato | A fine fase |

Già dentro Claude Code: `/security-review` (revisione di sicurezza delle modifiche) e `/simplify`.

**Da non usare** nei progetti Southside: `frontend-design`, perché spinge verso grafiche originali e diverse ogni volta,
il contrario del design system unico (capitolo 6). Ogni plugin nuovo si valuta prima: chi l'ha scritto, cosa può fare
(agganci, server MCP), quanto contesto occupa (`claude plugin details <nome>`).

## Le impostazioni del progetto (`.claude/settings.json`)
Il file del modello, nella radice di ogni repository:
- dichiara i marketplace `southside` (`wearesouthside/southside-standard`) e `claude-plugins-official`
  (`anthropics/claude-plugins-official`) e attiva i plugin qui sopra: chi apre il progetto se li vede proporre;
- vieta a Claude di leggere file di credenziali (`.env`, chiavi, `secrets/`), di fare `git push --force` e `git reset --hard`;
- consente senza chiedere i comandi di controllo (`npm run verifica`, `npm test`, `git status`, `git diff`, `git log`).

## Avvio di un progetto
1. Repository nuovo, privato, in `wearesouthside`.
2. Copiare i file del modello (cartella `modelli/` accanto a questo manuale, o `plugins/southside-standard/skills/southside-standard/modelli/` nel repository
   `southside-standard`): `.claude/settings.json`, `CLAUDE.md`, `.gitignore`, `.github/workflows/verifica.yml`,
   `docs/stato.md`, `docs/architettura-funzionale.md`, `docs/architettura.md`, `docs/api.md`, `docs/design.md`,
   `docs/sicurezza.md`; copiare `docs/impostazione.md` dal Core e aggiungere il progetto tra i pezzi (§2) e nella mappa
   dei dati (§3), poi chiedere al titolare di riportare la modifica negli altri repository.
3. Riempire `CLAUDE.md` (cosa fa il progetto, struttura) e `docs/architettura-funzionale.md` **prima** del codice:
   che dati custodisce, cosa legge dal Core, chi scrive.
4. Struttura minima con `npm run verifica` verde e il workflow su GitHub acceso.
5. Interfaccia con `@southside/ui` (React/Next) o con lo stile della Console (pagine dal server).
6. Prima pubblicazione con etichette `app` e `ambiente`, budget e avvisi; riga in `docs/stato.md`.

## Revisione di un lavoro (lista per chi controlla)
- [ ] Le regole del capitolo 1 sono rispettate (un proprietario per ogni dato, nessuna copia modificabile).
- [ ] Elenco di sicurezza: righe toccate con difesa e prova.
- [ ] Nessun dato nel codice né su GitHub.
- [ ] Calcoli nel backend, interfaccia che mostra.
- [ ] Verifica verde in locale e su GitHub.
- [ ] Documentazione funzionale e API aggiornata (test verde).
- [ ] Pagine con il tema Southside, guardate su computer e telefono, chiaro e scuro.
- [ ] `docs/stato.md` aggiornato.
