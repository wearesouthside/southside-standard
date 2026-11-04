# Architettura del codice di <progetto>

Cosa fa il progetto: `docs/architettura-funzionale.md`. Regole: `CLAUDE.md`, `docs/impostazione.md`.

## In una frase
<Database> ← <backend: calcola e scrive> ← <interfaccia: mostra>.

## Cartelle
| Cartella | Cosa contiene | Regola |
|---|---|---|
| | | |

## Come si aggiunge…
- **Un dato nuovo**: migrazione (con `tenant_id`, RLS, registro, divieto di cancellare) → dominio → indirizzo → catalogo se pubblico → `docs/architettura-funzionale.md`.
- **Un calcolo**: funzione pura con il suo test; lettura dal database separata.
- **Una pagina**: con i componenti MUI e il tema `@southside/ui`.

## Verifiche
`npm run verifica`.
