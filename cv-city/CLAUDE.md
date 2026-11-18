# Progetto: CV interattivo 3D — "Connector City"

## Obiettivo
Costruisci un sito web statico che presenta il mio CV come una **cittadina 3D isometrica interattiva**, da condividere al posto del profilo LinkedIn.
- **Pubblico:** recruiter/aziende, partner/sponsor (eventi, talk, hackathon), network generale.
- **Messaggio chiave:** sono un **connettore**: metto in relazione persone, organizzazioni e competenze. La città deve *mostrarlo*, non solo dirlo.
- **Lingua dei testi del sito:** solo **inglese**.

## Concept
La città è un **diorama low-poly su un piedistallo**, come un plastico che fluttua nel vuoto, visto dall'alto in vista isometrica leggermente inclinata.

### Centro: la piazza con l'avatar
- Al centro c'è una piazza che rappresenta **me**. Tutte le strade principali partono da lì.
- Nella piazza c'è un **avatar stilizzato in stile cartoon/chibi** (testa grande, corpo piccolo, low-poly), **originale**: non deve copiare un Mii né altri personaggi esistenti.
- L'avatar **non cammina**. Resta fermo con un'animazione idle: oscilla leggermente su e giù, ondeggia piano, ogni tanto saluta con la mano.
- Per ora l'aspetto è un segnaposto generico. Più avanti lo personalizzo (capelli, barba, occhiali, colori dei vestiti) tramite parametri in un file di config.

### Edifici importanti (contenuto del CV)
| Edificio | Contenuto |
|---|---|
| Office tower | Work experience (un "piano" o una voce per ruolo) |
| University | Education |
| Hall of Fame / museo | Certifications |
| Lab / workshop | Skills & projects |
| Arena / convention center | Hackathons, talks, events organizzati |
| Airport / port | International collaborations (archi luminosi verso le città dei partner) |
| Post office | Contact |

### Edifici decorativi
- Aggiungi **altri edifici, alberi, lampioni, panchine e macchinine parcheggiate** per rendere la città viva e bella.
- Devono restare chiaramente **secondari**.

### Come si distinguono gli edifici importanti (sempre, giorno e notte)
- **Decorativi:** colori neutri e desaturati (bianco sporco, beige, grigio chiaro), altezza bassa o media.
- **Importanti:** colori pieni e saturi, un'**icona 3D che fluttua sopra** e ruota lentamente (es. tocco, valigetta, trofeo) e un'**etichetta col nome** (es. "Experience", "Education").
- **Di notte:** gli edifici importanti hanno le **finestre accese** con luce calda, i decorativi restano quasi spenti.

### Le strade sono i collegamenti (il cuore del concept)
- Ogni elemento del CV può essere collegato ad altri. Esempio: la skill "Public speaking" è collegata all'esperienza X e all'hackathon Y.
- **Clic su un edificio:** si illuminano (glow o colore animato che scorre lungo la strada) le strade che lo collegano agli edifici con cui ha relazioni, e quegli edifici si evidenziano. Tutto il resto si attenua leggermente.
- **Nel pannello dell'edificio** ogni voce (es. ogni singola skill del Lab) è cliccabile. Cliccandola si illuminano **solo** le strade e gli edifici collegati a quella voce.
- **Desktop:** passando col mouse sopra un edificio, l'edificio si evidenzia appena e compare il cursore pointer.

## Interazione
- **Camera:** rotazione con trascinamento e zoom con rotella o pinch, entrambi con limiti (niente vista da sotto, niente zoom estremo). Su mobile si usano le dita.
- **Clic su un edificio importante:**
  1. la camera fa uno **zoom leggero** animato verso l'edificio;
  2. si apre un **pannello vicino all'edificio** con tutto quello che lo riguarda (titoli, periodi, descrizioni, risultati, link);
  3. si illuminano i collegamenti, come descritto sopra.
- **Chiusura:** clic sul vuoto, tasto Esc o pulsante X. La camera torna alla vista generale.
- **Mobile:** il pannello diventa un **bottom sheet** (si apre dal basso) invece di stare accanto all'edificio.
- **Accessibilità minima:** gli edifici si raggiungono anche con il tasto Tab, si aprono con Invio e si chiudono con Esc.

## Ciclo giorno/notte
- Un ciclo completo giorno → tramonto → notte → alba dura **60 secondi**.
- Si **mette in pausa mentre un pannello è aperto**, così la luce non cambia durante la lettura.
- **Giorno:** cielo azzurro chiaro, luce calda, ombre morbide.
- **Notte:** cielo blu scuro, stelle, luna, lampioni accesi, finestre degli edifici importanti accese.
- Le transizioni tra i colori di cielo, luci e nebbia devono essere fluide.

## Stile visivo
Le immagini di riferimento sono nella cartella `/inspo`. Da ciascuna prendi questo:
- **Riferimento principale:** città isometrica low-poly pastello, con forme pulite, colori pieni, alberi semplici e macchinine. Asfalto lilla e grigio, marciapiedi chiari, strisce pedonali bianche.
- **Diorama su piedistallo** con bordo pulito e **pannelli UI in vetro smerigliato (glassmorphism)**: sfondo semitrasparente, blur, bordi arrotondati, ombra morbida.
- **Notte** in stile città voxel con finestre calde illuminate e cielo stellato.
- **Alberi tondi e "soffici"** (sfere o icosaedri raggruppati) mescolati ad alberi a cono.
- **Effetto tilt-shift** (sfocatura ai bordi, look da plastico): **solo su desktop**, disattivato su mobile.
- **NON** puntare al fotorealismo: niente texture realistiche né modelli dettagliati. Serve uno stile low-poly pulito che giri fluido nel browser.

## UI fissa (sempre visibile)
- In alto a sinistra: nome e titolo/ruolo, presi dal JSON.
- In alto a destra, sempre visibili:
  - **"Quick view"**: apre una versione testuale semplice e scorrevole di tutto il CV, pensata per i recruiter di fretta.
  - **"Download CV"**: scarica il PDF da `/public/cv.pdf`.
- Un piccolo indicatore o suggerimento iniziale: "Click a building to explore".
- **Schermata di caricamento** breve e in stile, ad esempio "Building the city…".

## Stack tecnico
- **Vite + React + TypeScript**
- **React Three Fiber + @react-three/drei** (e `@react-three/postprocessing` solo per il tilt-shift su desktop)
- **Geometria procedurale**: costruisci edifici, alberi, avatar e macchine con primitive Three.js nel codice, **senza scaricare asset esterni**.
- **Animazioni di camera e UI:** usa `maath/easing` oppure `framer-motion` / `@react-spring/three`. Scegli tu la soluzione più semplice.
- **Deploy:** GitHub Pages tramite una GitHub Action (`.github/workflows/deploy.yml`). Configura `base` in `vite.config.ts` in modo corretto.

### Performance
- Usa `InstancedMesh` per gli elementi ripetuti: alberi, lampioni, finestre, macchine.
- Limita il DPR (es. `dpr={[1, 2]}`) e abbassa la qualità delle ombre su mobile.
- Se WebGL non è disponibile o il dispositivo è troppo debole, mostra direttamente la **Quick view**.

## Dati: tutto il contenuto sta in un solo file
Tutto il contenuto deve stare in `src/data/cv.json`. Per aggiornare il CV non si deve mai toccare il codice. Per ora usa **contenuti segnaposto realistici in inglese**: li sostituisco io con i miei dati veri.

Schema suggerito, che puoi migliorare se serve:

```json
{
  "person": {
    "name": "Your Name",
    "title": "External Relations | Partnerships & Events",
    "tagline": "I connect people, companies and ideas.",
    "email": "you@example.com",
    "linkedin": "https://linkedin.com/in/...",
    "avatar": { "skin": "#f1c27d", "hair": "#3b2a20", "hairStyle": "short", "beard": false, "glasses": false, "shirt": "#4a6cf7", "pants": "#2d2d3a" }
  },
  "buildings": [
    { "id": "experience", "type": "office", "label": "Experience", "icon": "briefcase", "color": "#4a6cf7" },
    { "id": "education", "type": "university", "label": "Education", "icon": "graduation", "color": "#e5533d" },
    { "id": "certifications", "type": "museum", "label": "Certifications", "icon": "trophy", "color": "#f2b134" },
    { "id": "skills", "type": "lab", "label": "Skills & Projects", "icon": "flask", "color": "#21b07a" },
    { "id": "events", "type": "arena", "label": "Events & Hackathons", "icon": "mic", "color": "#a24bdb" },
    { "id": "international", "type": "airport", "label": "International", "icon": "plane", "color": "#1fa5c9" },
    { "id": "contact", "type": "postoffice", "label": "Contact", "icon": "mail", "color": "#ff7a59" }
  ],
  "items": [
    {
      "id": "exp-1",
      "building": "experience",
      "title": "External Relations",
      "org": "Organization",
      "period": "2025 – present",
      "description": "Short description of what I did.",
      "highlights": ["Result with a number", "Partner secured"],
      "links": [{ "label": "Website", "url": "https://..." }]
    },
    { "id": "skill-public-speaking", "building": "skills", "title": "Public speaking" }
  ],
  "connections": [
    ["skill-public-speaking", "exp-1"],
    ["skill-public-speaking", "event-hackathon-1"]
  ]
}
```

- Un collegamento fra due `items` illumina la strada fra i rispettivi edifici.
- Il clic su un edificio illumina l'unione di tutti i collegamenti dei suoi items.

## Fuori scope per la v1 (non farlo)
- Avatar realistico o che cammina
- Musica e suoni, mini-giochi
- Traffico animato (le macchine stanno parcheggiate o ferme)
- Più lingue
- Backend o CMS

## Come procedere
Lavora per step. Dopo ognuno fermati, fammi vedere il risultato con `npm run dev` e aspetta il mio ok prima di passare al successivo:
1. **Setup + scena base:** diorama su piedistallo, strade, edifici importanti e decorativi, alberi, avatar in piazza con l'animazione idle. Lo stile deve corrispondere alle inspo.
2. **Distinzione visiva:** colori, icone fluttuanti ed etichette degli edifici importanti.
3. **Interazione:** hover, clic, zoom leggero della camera, pannello glass (desktop) e bottom sheet (mobile), chiusura.
4. **Collegamenti:** illuminazione delle strade per edificio e per singola voce del pannello.
5. **Ciclo giorno/notte** di 60 secondi con pausa a pannello aperto, finestre accese, lampioni, stelle e luna.
6. **UI fissa:** nome, Quick view, Download CV, schermata di caricamento, fallback senza WebGL.
7. **Rifinitura e performance:** tilt-shift solo desktop, test su mobile.
8. **Deploy** su GitHub Pages.

Mantieni il codice semplice e leggibile: questo progetto lo devo poter modificare io. Niente over-engineering.
