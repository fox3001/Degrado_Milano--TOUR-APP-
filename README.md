# Vizi Nascosti — Tour Game

> **Scopo di questo repo: VERSIONE TOUR GAME, quella attiva su cui si lavora.**
> Usa il regolamento semplificato (con il Tabacco), descritto in **[REGOLAMENTO-TOUR.md](REGOLAMENTO-TOUR.md)**.
> L'archivio completo per il futuro gioco di carte è nel repo `fox3001/Degrado_Milano-`.

Web app di supporto a un gioco dal vivo a ruoli segreti (Nobile, Vizioso, Moralista, Cicisbeo) per un tour a piedi di 20–30 persone.

- **Sito:** https://degrado-milano.web.app
- **Area guida:** https://degrado-milano.web.app/#guida (non c'è più il link in home)
- **Hosting e database:** Firebase, progetto `degrado-milano`

## Cartelle
| Percorso | Cosa contiene |
|---|---|
| `public/` | **Solo questa cartella viene pubblicata sul sito** |
| `public/index.html` | L'app |
| `public/img/` | Tutte le immagini (vedi sotto) |
| `firebase.json` | Configurazione di pubblicazione |
| `database.rules.json` | Regole di sicurezza del database |
| `.github/workflows/deploy.yml` | Pubblicazione automatica a ogni modifica |
| `archivio/` | Versione originale (prima delle correzioni), schede PNG originali, vecchi backup. **Non viene pubblicata** |
| `ANALISI.md` | Analisi completa: bug, problemi delle schede e del regolamento |
| `REGOLAMENTO-TOUR.md` | **Il regolamento in uso in questa versione** |
| `REGOLE-E-IDEE.md` | Tutte le regole (v8.0, aggiunte, versione completa e semplificata) e le idee per il gioco di carte |

## Cambiare un'immagine
Tutte le immagini sono in `public/img/` e hanno nomi fissi:

| File | Cosa è |
|---|---|
| `nob-personaggi.jpg` | Illustrazioni del Nobile (in fondo alla sua scheda) |
| `viz-personaggi.jpg` | Illustrazioni del Vizioso |
| `mor-personaggi.jpg` | Illustrazioni del Moralista |
| `cic-personaggi.jpg` | Illustrazioni del Cicisbeo |
| `spillo.png` | Personaggio della home (PNG con sfondo trasparente) |
| `damask.jpg` | Motivo damascato dello sfondo, ripetuto a piastrelle |

Per sostituirne una:
1. su GitHub apri `public/img/`, clicca sul file e poi su **Download**;
2. modificala e salvala **con lo stesso nome e lo stesso formato**;
3. torna in `public/img/` e scegli **Add file → Upload files**, trascina il file e conferma con **Commit changes**. GitHub sostituisce quello vecchio.

Le dimensioni non devono essere identiche: le immagini si adattano sempre alla larghezza del telefono. Conviene però restare intorno ai **1080 px di larghezza** e sotto **500 KB**. Le schede vanno lette con la rete mobile.

Dopo il caricamento il sito si aggiorna da solo in 1–2 minuti, se hai attivato la pubblicazione automatica (vedi sotto). I telefoni vedono subito la nuova versione, perché le immagini non vengono tenute in cache.

## Cambiare i testi delle schede
I testi della scheda di ogni ruolo non sono più dentro un'immagine. Sono scritti nell'app, nel blocco `ROLES` all'inizio dello script in `public/index.html`:
- `motto`: la frase nel riquadro scuro;
- `rp1`: il riquadro "Chi sei";
- `rp2`: il riquadro "Come ti muovi";
- `op1` e `op2`: la pagina Obiettivi;
- `tok`: le risorse di partenza.

Si modificano direttamente da GitHub (icona della matita) e il sito si aggiorna da solo.

## Prima configurazione (una volta sola)

### 1. Account della guida
1. Firebase Console → progetto `degrado-milano` → **Authentication** → *Sign-in method* → attiva **Email/Password**.
2. *Users* → **Add user**: inserisci l'email e la password della guida.
3. Apri `database.rules.json` e sostituisci **tutte** le occorrenze di `EMAIL_DELLA_GUIDA@esempio.it` con quell'email.

### 2. Pubblicare regole e sito
Dal PC, nella cartella del repo:
```
firebase deploy
```
Questo comando pubblica sia il sito sia le regole del database. Da quel momento:
- l'elenco dei codici non è più leggibile da fuori;
- solo la guida, dopo il login, può generare codici;
- i 610 codici vecchi non funzionano più. La guida li può cancellare con "Elimina i codici delle partite chiuse".

### 3. Pubblicazione automatica da GitHub (facoltativa, ma serve per cambiare le immagini solo da GitHub)
Dal PC, nella cartella del repo:
```
firebase init hosting:github
```
Rispondi `fox3001/Degrado_Milano--TOUR-APP-` quando chiede il repository. Il comando crea da solo il segreto `FIREBASE_SERVICE_ACCOUNT` su GitHub.

Quando chiede di sovrascrivere i file del workflow, rispondi **no**, perché il file `deploy.yml` c'è già. Da lì in poi, ogni modifica in `public/` pubblica il sito da sola.

## Come funziona una partita
1. La guida apre `/#guida`, fa il login, sceglie il numero di giocatori e preme **Nuova partita**.
2. Manda a ogni giocatore il suo codice di 4 caratteri.
3. Il giocatore lo inserisce in home e vede:
   - la scheda del ruolo (pizzica per ingrandire);
   - chi è: personaggio, ruolo e risorse di partenza;
   - gli obiettivi;
   - le regole.
4. Alla fine la guida preme **Chiudi la partita**: i codici smettono di funzionare.

## Gesti segreti
Ogni ruolo ha un gesto, uguale per tutti i giocatori di quel ruolo, che va fatto dopo ogni scambio o guadagno di Monete. A ogni **Nuova partita** l'app pesca 4 gesti diversi dall'elenco, uno per ruolo, e li scrive nella scheda di ogni giocatore. La guida vede gli abbinamenti nel suo pannello.

L'elenco dei gesti si modifica nel pannello della guida, sezione **Gesti segreti**: aggiungi o togli con un tocco. Ne servono almeno 4. Il testo della regola si trova in `public/index.html`: cerca `GESTO_WHEN` e "IL GESTO SEGRETO".
