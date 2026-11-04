# 6. Interfaccia: documento UI condiviso e Material Design 3

## Un documento solo
Le regole visive di tutte le app stanno in **un documento condiviso**: `docs/design.md` del Core
(`wearesouthside/Southside-core`), tratto dalla *Southside Brand Guideline* v1.0. Ogni progetto lo richiama dal suo
`docs/design.md` (modello in `modelli/docs/design.md`) e non ne fa copie con varianti: se una regola cambia, cambia lì.

In breve:
- **nero e bianco portano il peso**; ciano `#0DCCCC` e rosso `#FF0049` solo come accenti (forme, numeri, dettagli);
  su bianco il ciano da testo è `#087F80`;
- **titoli in ROC Grotesk Condensed, sempre maiuscolo**; tutto il resto in **Stack Sans Text**;
- **card, pannelli e campi con raggio 24 px**; **bottoni, tag, filtri e voci del menu a pillola**;
- su bianco **bordi da 1 px**, niente ombre; su nero superfici grafite e **bagliore teal** sui bottoni principali;
- **tema chiaro e scuro** dal dispositivo, con il pulsante per cambiarlo;
- ogni pagina funziona **sul telefono** senza scorrere di lato;
- testi in **italiano semplice**, frasi brevi, niente gergo.

## Material Design 3 per tutti i componenti
Tutte le app usano i **componenti di Material Design 3** (bottoni, campi, schede, scelte, interruttori, dialoghi,
menu, tabelle, avvisi, barre di avanzamento) con i loro **comportamenti, stati e regole di accessibilità**, e
l'**aspetto Southside**. M3 è fatto per essere personalizzato: i ruoli di colore, la scala dei caratteri e quella delle
forme diventano quelli del marchio.

- **App React/Next:** MUI (Material UI) con il tema `@southside/ui` (`wearesouthside/southside-ui`), installato da git
  con il tag di versione. Si usano i componenti MUI così come sono: niente colori, raggi o caratteri scritti nelle pagine.
  Se manca qualcosa, si aggiunge al tema e si pubblica una versione nuova. Nota: MUI implementa Material Design 2 con
  parti di M3; il tema Southside porta in stile M3 quello che serve (bottoni e scelte a pillola, interruttore M3,
  schede a pillola, scala delle forme).
- **Pagine generate dal server** (Console): stesse regole con il foglio di stile della Console
  (`frontend/src/design/stile.css`), componenti equivalenti a quelli M3.

| Componente M3 | MUI | Regola Southside |
|---|---|---|
| Bottone pieno / con bordo / di testo | `Button` `contained` / `outlined` / `text` | Pillola; principale nero (su scuro con bagliore teal) |
| Scelte segmentate | `ToggleButtonGroup` | Pillola, voce scelta piena |
| Schede | `Tabs` + `Tab` | A pillola, voce attiva piena |
| Chip | `Chip` | Pillola |
| Campo di testo | `TextField` (outlined) | Raggio 24 px, bordo ciano quando attivo |
| Interruttore | `Switch` | Forma M3 (binario 52×32) |
| Card | `Card` (outlined) | Raggio 24 px, bordo 1 px, niente ombra |
| Dialogo | `Dialog` | Raggio 24 px |
| Menu | `Menu` | Raggio 12 px |
| Navigazione | `List` + `ListItemButton` | Voci a pillola, voce attiva piena |
| Barra in alto | `AppBar` | Sul fondo pagina, bordo in basso |
| Tabella | `Table` | Intestazioni piccole in maiuscolo, numeri allineati |

## Prima di dire «fatto»
Ogni pagina si guarda **su computer e telefono, in chiaro e in scuro** (con Playwright si fanno le quattro foto), con
contrasto leggibile (almeno 4,5:1 per il testo; `southside-ui` lo controlla nei test del tema).
