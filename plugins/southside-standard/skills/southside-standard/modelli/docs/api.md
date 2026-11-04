# API di <progetto>: guida per chi si integra

Contratto macchina: `GET /api/v1/contratto` (OpenAPI 3.1). Campi di ogni risorsa: `docs/api-risorse.md` (generato con
`npm run documenta`). Cosa fa il progetto: `docs/architettura-funzionale.md`. Esempio completo: `docs/api.md` del Core.

## 1. Cosa è pubblico e cosa no
| Indirizzo | Per chi | Garanzia |
|---|---|---|
| | | |

## 2. Accesso
Identità (account di servizio Google o IAP), ruolo da chiedere, intestazioni.

## 3. Leggere i dati
Forma unica `{ letto_il, risorsa, dati, prossimo }`, pagine (`?limite=`, `?dopo=`), novità (`?modificati_dal=`), righe chiuse.

### Risorse (v1)
`<risorsa>`, …

## 4. Errori
`{ "errore": "…" }`, codici, intestazione con l'id della richiesta.

## 5. Esempio
```ts
// lettura a pagine con l'identità di servizio
```

## 6. Come cambia il contratto
In `v1` i campi si aggiungono soltanto; togliere o cambiare = `v2`.
