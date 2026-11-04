# 3. Nessun dato nel codice: tutto nel database

## La regola
Nel codice non stanno **nomi di persone o aziende, conti, IBAN, importi, date, soglie, percentuali, elenchi di voci,
regole dell'azienda**. Stanno nel database e si cambiano da un'interfaccia (per il Core: la Console), con il registro
delle modifiche. Nel codice resta solo il **come**: come si legge un formato standard (FatturaPA, estratto conto, F24),
come si calcola, come si controlla.

Perché:
- un valore nel codice si cambia solo con uno sviluppatore e una pubblicazione; nel database lo cambia chi ne è responsabile;
- un'app white-label (Performance) o una seconda azienda non sono possibili se Southside è scritta nel codice;
- il codice va su GitHub: un dato lì è pubblicato per sempre nella storia.

## Dove vanno le cose
| Cosa | Dove | Esempio nel Core |
|---|---|---|
| Valori dell'azienda (una riga per tenant) | tabella `impostazioni` | conto principale, banca, società collegata, date di chiusura |
| Elenchi che cambiano | tabella dedicata, modificabile | piano dei conti (`piano_voci`), canali, regole di prima nota |
| Regole di classificazione | tabella di regole con priorità | `primanota_regole`, `fornitori_pagamento` |
| Testi delle pagine | nel codice dell'interfaccia (sono interfaccia, non dati) | titoli, etichette |
| Valori predefiniti di un'azienda nuova | migrazione o funzione «prima configurazione» | piano dei conti iniziale |
| Credenziali | Secret Manager (mai database, mai codice) | chiave della banca |
| Configurazione tecnica (URL, progetto) | variabili d'ambiente del servizio | `URL_CONSOLE` |

## Come si riconosce un dato nel codice
Claude si ferma e propone di spostarlo quando scrive:
- un nome proprio o una ragione sociale (`'Banca Etica'`, `'Mario'`);
- una data letterale (`'2026-08-01'`) che decide un comportamento;
- un importo o una soglia (`> 5000`, `0.22` se non è una regola di legge stabile);
- un `if` sul nome di qualcosa (`if (area === 'Marketing')`);
- un elenco di voci, categorie, persone.

## Il database
- Ogni tabella: `tenant_id`, Row-Level Security forzata, `creato_il`, `modificato_il`, chiusura (`archiviato_il` o
  `valido_dal`/`valido_al`), trigger del registro e del divieto di cancellare.
- Importi in **centesimi interi**; date in ISO; identificativi UUID.
- Migrazioni SQL numerate in ordine; quelle pubblicate **non si modificano mai**: ogni cambiamento è una migrazione nuova.
- Storico con date di validità dove conta «com'era a una certa data» (ruoli, responsabili, allocazioni).
- Chiavi tecniche già salvate nello storico non si rinominano: il nome mostrato viene dai dati.

## Dati e GitHub
- Nei test e negli esempi solo **dati inventati** (Cliente Uno, Progetto Alfa, `IT00X0000000000000000000000`).
- Nessun file con dati reali nel repository, nemmeno «temporaneo»; nessun nome di file che contenga dati.
- I dati veri stanno in Cloud SQL e nel bucket del servizio proprietario.
