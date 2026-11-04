# Architettura funzionale di <progetto>

Per chi deve integrarsi con questo progetto o lavorarci dentro: cosa fa, area per area. Mappa del codice in
`docs/architettura.md`; API in `docs/api.md`; campi delle risorse in `docs/api-risorse.md` (generato).
Esempio completo: `docs/architettura-funzionale.md` di `wearesouthside/Southside-core`.

## 1. In breve
- Cosa fa e per chi.
- Quali dati custodisce (ne è il proprietario) e quali legge da altri (di solito dal Core, in sola lettura).
- Chi scrive.

## 2. Chi parla con il servizio
| Chi | Identità | Cosa fa | Indirizzi |
|---|---|---|---|
| | | | |

## 3. Le aree
### 3.1 <Area>
- **Cosa:**
- **Da dove:**
- **Regole:**
- **Chi legge / chi scrive:**
- **Risorse:** `<risorsa>`

## 4. Flussi principali
1. <Modifica di un dato: da dove parte, quali controlli, dove finisce, come lo vedono gli altri>

## 5. Cosa garantisce a chi si integra
- Contratto `v1`: i campi si aggiungono soltanto.
