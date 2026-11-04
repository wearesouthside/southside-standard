# Sicurezza di <progetto>

Elenco delle vulnerabilità dello standard Southside (capitolo 2 del manuale) con lo stato in questo progetto.
Una riga Critica o Alta aperta blocca la pubblicazione.

| # | Vulnerabilità | Gravità | Stato | Prova nel progetto |
|---|---|---|---|---|
| V1 | Accesso senza identità | Critica | | |
| V2 | Dati di un'altra azienda | Critica | | |
| V3 | Iniezione SQL | Critica | | |
| V4 | Credenziali nel codice o su GitHub | Critica | | |
| V5 | Permessi sbagliati (IDOR) | Alta | | |
| V6 | Dati personali esposti | Alta | | |
| V7 | Dati reali su GitHub, nei log o nei test | Alta | | |
| V8 | Script iniettato nelle pagine (XSS) | Alta | | |
| V9 | Content-Security-Policy assente | Media | | |
| V10 | Richieste da altri siti (CSRF) | Alta | | |
| V11 | Cancellazioni o modifiche senza traccia | Alta | | |
| V12 | Servizio con troppi poteri sul database | Media | | |
| V13 | Account di servizio con troppi ruoli | Media | | |
| V14 | File caricati pericolosi | Alta | | |
| V15 | Input ostili | Media | | |
| V16 | Assegnazione di massa | Alta | | |
| V17 | Formule nei CSV/Excel esportati | Bassa | | |
| V18 | Indirizzi dall'intestazione Host, reindirizzamenti aperti | Bassa | | |
| V19 | Chiamate verso indirizzi decisi dall'utente (SSRF) | Alta | | |
| V20 | Dipendenze vulnerabili o compromesse | Media | | |
| V21 | Errori che raccontano troppo | Bassa | | |
| V22 | Chiamate senza tempo massimo o limite | Bassa | | |
| V23 | Bucket o database raggiungibili da fuori | Critica | | |
| V24 | Aperture di documenti sensibili non registrate | Media | | |
| V25 | Pubblicazione di codice non controllato | Media | | |
| V26 | Istruzioni nascoste per l'AI (prompt injection) | Alta | | |
| V27 | Sessioni e cookie deboli | Media | | |
| V28 | CORS aperto | Media | | |

Stato: ✅ coperta e provata · ⚠️ aperta (con data) · — non pertinente (perché).
