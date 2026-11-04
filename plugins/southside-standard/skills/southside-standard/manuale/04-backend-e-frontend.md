# 4. Regole nel backend, interfaccia dinamica

## Divisione dei compiti
| | Backend (servizio) | Frontend (interfaccia) |
|---|---|---|
| Fa | Legge e scrive i dati, applica permessi e controlli, **calcola** ogni numero e stato, decide cosa può fare l'utente | **Mostra** quello che riceve, raccoglie l'input, chiama il backend |
| Non fa | Disegnare pagine per una sola app (le viste per la Console sono marcate «interne») | Ricalcolare totali, stati, scadenze, permessi; tenere regole dell'azienda |
| Si prova | Funzioni pure con test, letture dal database separate | Test delle pagine (rendering, testo malevolo, accessibilità) |

Perché: se un numero si calcola nel frontend, un'altra app non può riottenerlo identico; se un permesso si controlla
solo nel frontend, basta chiamare l'API a mano per scavalcarlo.

## Backend: com'è fatto (esempio: il Core)
- `api/app.ts`: ingresso, identità, tabella delle aree (primo pezzo del percorso → file dell'area). Niente logica.
- `api/rotte/<area>.ts`: indirizzi dell'area: permesso richiesto, lettura dei parametri, chiamata al dominio. Niente calcoli.
- `dominio/<area>.ts`: regole e calcoli, divisi in **lettura** (`leggi…`, dentro una transazione con tenant) e
  **calcolo puro** (`calcola…`, testabile senza database).
- `dati/`: connessione, transazioni con tenant e autore, migrazioni.
- `integrazioni/`: servizi esterni e formati dei file.
- Ogni scrittura: permesso → controlli di forma (obbligatori, formati, doppioni) → transazione → registro.

## Frontend dinamico
L'interfaccia si adatta ai dati e ai permessi senza essere riscritta:
- **menu, schede e azioni** dipendono da ciò che il backend dice che l'utente può fare (es. `/io` con ruolo e permessi);
- **elenchi, etichette di voci, stati, colori di categoria** arrivano dai dati, non sono scritti nella pagina;
- **moduli** costruiti dalle definizioni del backend dove possibile (campi, obbligatori, formati, scelte ammesse);
- **nessun valore dell'azienda** nella pagina: se serve, è un campo della risposta.
- La pagina funziona anche se il backend aggiunge campi (si ignorano quelli sconosciuti).

## Interfaccia: due modi ammessi
1. **Pagine generate dal server** (Console del Core): HTML dal server, piccoli script dove serve, CSP con nonce.
2. **React/Next con MUI** e `@southside/ui` (app nuove): componenti Material, dati dal backend dell'app che a sua volta
   legge dal Core; nessuna chiamata dal browser direttamente al Core (le identità di servizio stanno sul server).

## Prestazioni
- Chiamate al backend in parallelo; risultati costosi tenuti in memoria per impronta dei dati (cambia un dato → si ricalcola).
- Letture solo delle novità (`?modificati_dal=`), a pagine.
- Servizi a zero istanze quando nessuno li usa; risorse dimensionate sull'uso reale.
