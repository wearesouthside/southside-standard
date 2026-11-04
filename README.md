# Southside Standard

Il **manuale per il vibe coding di Southside** e il **plugin di Claude Code** che lo applica in ogni progetto.

- **Manuale:** `plugins/southside-standard/skills/southside-standard/manuale/`
  1. [Principi](plugins/southside-standard/skills/southside-standard/manuale/01-principi.md)
  2. [Sicurezza fin dal progetto](plugins/southside-standard/skills/southside-standard/manuale/02-sicurezza.md): elenco delle vulnerabilità con gravità, difesa e prova
  3. [Nessun dato nel codice: tutto nel database](plugins/southside-standard/skills/southside-standard/manuale/03-dati-nel-database.md)
  4. [Regole nel backend, interfaccia dinamica](plugins/southside-standard/skills/southside-standard/manuale/04-backend-e-frontend.md)
  5. [Test continui e blocco della pubblicazione](plugins/southside-standard/skills/southside-standard/manuale/05-test-e-pubblicazione.md)
  6. [Interfaccia: documento UI condiviso e Material Design 3](plugins/southside-standard/skills/southside-standard/manuale/06-interfaccia.md)
  7. [Architettura e API descritte in modo completo](plugins/southside-standard/skills/southside-standard/manuale/07-architettura-e-api.md)
  8. [Lavorare con Claude Code](plugins/southside-standard/skills/southside-standard/manuale/08-lavorare-con-claude.md): skill e plugin, impostazioni, avvio di un progetto
- **Skill** (riassunto e lista di controllo): `plugins/southside-standard/skills/southside-standard/SKILL.md`
- **Modelli per i progetti nuovi** (`.claude/settings.json`, `CLAUDE.md`, workflow di GitHub, documenti):
  `plugins/southside-standard/skills/southside-standard/modelli/`
- **Prompt per avviare una persona nuova o un progetto esistente:** [`docs/prompt-avvio.md`](docs/prompt-avvio.md)

Esempio di riferimento: `wearesouthside/Southside-core` (`docs/impostazione.md`, `docs/architettura-funzionale.md`,
`docs/architettura.md`, `docs/api.md`, `docs/design.md`, `docs/piano-tecnico.md`). Tema dell'interfaccia:
`wearesouthside/southside-ui`.

## Installare il plugin (una volta per computer)
Il repository è pubblico (contiene solo regole e modelli, nessun dato): non serve nessun accesso.
Nelle sessioni di Claude Code nel cloud non si installa nulla: basta il file `.claude/settings.json` nel repository
(vedi sotto). Sul computer, dentro Claude Code (in qualsiasi cartella), scrivi questi due comandi, uno alla volta:

```
/plugin marketplace add wearesouthside/southside-standard
```
Atteso: «Successfully added marketplace: southside».

```
/plugin install southside-standard@southside
```
Atteso: «Successfully installed plugin: southside-standard@southside». Da quel momento Claude applica lo standard
quando lavori sui progetti Southside.

In alternativa dal Terminale:

```bash
claude plugin marketplace add wearesouthside/southside-standard
```

```bash
claude plugin install southside-standard@southside
```

**Aggiornare** quando lo standard cambia:

```bash
claude plugin marketplace update southside
```

## Mettere lo standard in un progetto
Copia nella radice del progetto il file
[`modelli/.claude/settings.json`](plugins/southside-standard/skills/southside-standard/modelli/.claude/settings.json)
(come `.claude/settings.json`) e fai il commit. Chi apre il progetto con Claude Code si vede proporre:
- il plugin `southside-standard`;
- i plugin ufficiali di Anthropic che lo completano: `security-guidance`, `claude-security`, `code-review`,
  `typescript-lsp`, `claude-md-management` (perché questi e non altri: capitolo 8 del manuale);
- i divieti (leggere file di credenziali, `git push --force`, `git reset --hard`) e i comandi di controllo consentiti.

Per `typescript-lsp` serve una volta sul computer:

```bash
npm install -g typescript-language-server typescript
```

Per un progetto nuovo basta chiedere a Claude: «Avvia questo progetto con lo standard Southside». Copia i modelli
(`CLAUDE.md`, `.gitignore`, `.github/workflows/verifica.yml`, `docs/…`) e segue il capitolo 8.

## Cosa deve fare il titolare su GitHub (una volta per repository)
Settings → Branches → Add branch ruleset su `main`: richiedi il controllo «verifica» verde, blocca i push forzati e
le cancellazioni. Così niente di rosso arriva in `main`, e quindi niente di rosso si pubblica.

## Modificare lo standard
- Si modifica il manuale (e `SKILL.md` se cambia una regola principale), si alza `version` in
  `plugins/southside-standard/.claude-plugin/plugin.json` e in `.claude-plugin/marketplace.json`, si scrive in
  `CHANGELOG.md`.
- Controllo prima del commit:

```bash
claude plugin validate . && claude plugin validate plugins/southside-standard
```
Atteso: «Validation passed» due volte.
