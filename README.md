# KawauchiCam web: guida (niente Xcode, niente Mac)

È una web app: la apri in Safari sull'iPhone, la aggiungi alla schermata Home e si comporta come un'app. Ti basta VS Code, su Windows, Mac o Linux.

## Pro e contro, senza giri di parole
- **Pro:** è gratis, non serve un Mac, non scade dopo 7 giorni e la modifichi e ricarichi quando vuoi.
- **Contro:** Safari passa alla pagina solo il flusso video, non la fotocamera completa. Quindi niente modalità notte e niente Deep Fusion, e la risoluzione è più bassa di quella della Fotocamera di Apple (di solito 2-8 MP invece di 12/48). Se vuoi stampare in grande, l'app nativa è meglio.
- Per salvare in Foto serve un tocco in più: "Salva" e poi "Salva immagine".

## File (cartella `web/`)
| File | Cosa fa |
|---|---|
| `index.html` | Tutta l'app: fotocamera, filtro WebGL e interfaccia. **I parametri del look sono in `LOOK`, all'inizio dello script** |
| `manifest.webmanifest` | Nome e icona quando la aggiungi alla Home |
| `sw.js` | Fa aprire l'app anche offline |
| `icon-180.png`, `icon-512.png` | Icone |

## Passo 1: prova sul computer (opzionale)
1. Apri la cartella `web/` in VS Code.
2. Installa l'estensione **Live Server**, poi tasto destro su `index.html` e "Open with Live Server".
3. Il browser usa la webcam e vedi subito il filtro.

## Passo 2: mettila online (serve https, altrimenti Safari blocca la fotocamera)
**GitHub Pages, gratis:**
1. Su github.com crea un **New repository** di nome `kawauchicam`, Public, e premi Create.
2. Clicca "uploading an existing file", trascina i 5 file della cartella `web/` e fai Commit.
3. Vai in Settings → **Pages**. Come Source scegli "Deploy from a branch", poi branch `main` e cartella `/ (root)`, e premi Save.
4. Dopo circa un minuto l'indirizzo è `https://<tuo-utente>.github.io/kawauchicam/`.

## Passo 3: sull'iPhone
1. Apri l'indirizzo in **Safari** e consenti la fotocamera.
2. Tasto Condividi → **Aggiungi alla schermata Home**.
3. Apri l'icona "Kawauchi" dalla Home: si apre a schermo intero, come un'app.
4. Scatta, poi "Salva" e "Salva immagine": la trovi in Foto.

## Con Claude Code in VS Code
Installa l'estensione **Claude Code** in VS Code. In alternativa, da terminale esegui `npm install -g @anthropic-ai/claude-code` e poi lancia `claude` nella cartella `web/`. Esempi di richieste:
> Rendi il look più freddo e più slavato, con meno grana.

> Aggiungi un timer di 3 secondi allo scatto.

Dopo ogni modifica ricarica i file su GitHub, poi sull'iPhone chiudi e riapri l'app.

## Filtri e strumenti
- **Kawauchi**: luce ariosa e sovraesposta, pastello ciano/rosa.
- **Soth** (Alec Soth): luce nordica, colori quieti e freddi.
- **Hido** (Todd Hido): crepuscolo, leggera foschia, ombre petrolio e luci calde al sodio.
- **Gruyaert** (Harry Gruyaert): colori pieni e profondi, ombre dense.
- **McGinley** (Ryan McGinley): estate, sole basso, toni dorati.

Sono interpretazioni libere, non copie: i parametri di ogni look stanno in `LOOKS` in `index.html`.
- **Griglia**: regola dei terzi.
- **Livella**: linea che diventa gialla quando il telefono è dritto. La prima volta chiede il permesso "Movimento e orientamento".
- **Raddrizza**: corregge in automatico l'inclinazione fino a 15°, con un leggero ritaglio. Se raddrizza nel verso sbagliato, in `index.html` metti `TILT_SIGN = -1`.

## Regolare il look (`LOOK` in index.html)
- Troppo bruciato: `exposureEV` a 0.3.
- Ancora troppo contrastato: primo valore di `curve` a 0.16 e `contrast` a 0.85.
- Colori troppo carichi: `saturation` a 0.65.
- Più rosa o più ciano: alza `highlightPink` o `shadowCyan`, ma resta sotto 0.06.
- Niente grana: `grain: 0`.

## Problemi tipici
- **"Fotocamera non disponibile"**: stai aprendo il file in locale o in http. Usa l'indirizzo https di GitHub Pages. Controlla anche Impostazioni → Safari → Fotocamera → Consenti.
- **Schermo nero quando torni nell'app**: tocca l'area dell'anteprima.
