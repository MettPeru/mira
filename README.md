<img src="icons/icon-512.png" width="96" alt="Icona di Mira">

# Mira

**Punta il tuo router 5G verso il ripetitore giusto.**

Mira è una web app per orientare un router 5G/4G da esterno (es. TP-Link NE200-Outdoor) verso un ripetitore preciso, usando GPS e bussola dello smartphone. Le coordinate dei ripetitori si prendono da [LTE Italy](https://lteitaly.it).

Niente installazione: gira nel browser del telefono e si aggiunge alla schermata Home come un'app.

```
index.html             l'app
manifest.webmanifest   installazione come app
icons/                 icona, favicon, apple-touch-icon
```

## Le schede

- **Punta**: bussola con l'azimut del router, il bersaglio in giallo e l'arco di correzione. L'indicazione "Ruota a destra di 12°" diventa verde con **Allineato**.
- **Siti**: elenco e mappa dei ripetitori, più la posizione del router (GPS dal vivo o salvata).
- **Segnale**: registro di RSRP, SINR e RSRQ per azimut, con il controllo del Cell ID.
- **Impostazioni**: declinazione, offset, tolleranza, backup.

## Funzioni

- **Bussola di puntamento**: mostra dove punta il router e dove si trova il ripetitore, con l'indicazione "Ruota a destra/sinistra di X°" fino a **✓ Allineato**.
- **Azimut, distanza e tilt** verso il ripetitore, con la curvatura terrestre (k = 4/3) e la quota del tetto e del sito.
- **Due modi di appoggio**: telefono in piano sul router (riferimento: bordo superiore) o in verticale contro il pannello (riferimento: retro del telefono). L'app li distingue da sola.
- **Suono "geiger" e vibrazione**: i bip si fanno più fitti e acuti man mano che ti avvicini al bersaglio, così puoi lavorare senza guardare lo schermo.
- **Registro segnale**: salvi RSRP, SINR e RSRQ insieme all'azimut. La lettura migliore viene evidenziata e con "Punta" ci torni.
- **Declinazione magnetica automatica** per l'Italia (≈3–5° Est), modificabile.
- **Mappa** (OpenStreetMap) con il router, i ripetitori e la linea di puntamento.
- **Backup** dei dati da copiare e incollare.

## Come si usa

1. Aggiungi il ripetitore in **Siti → +**. Hai tre modi:
   - **Link del navigatore**: su LTE Italy, invece di toccare "Avvia navigatore", tienilo premuto, scegli **Copia link** e poi tocca **Incolla** in Mira.
   - **Da Mappe**: se si è già aperta Apple Mappe, tocca il segnaposto e usa Condividi → Copia, oppure copia le coordinate dalla scheda in basso.
   - **Scegli sulla mappa**: con la vista satellite sposti la mappa finché il mirino non è sul traliccio che vedi su LTE Italy.

   Formati accettati:
   - `41.902782, 12.496366`, oppure `42,9493° N, 12,3995° E`
   - link di Apple Mappe (`ll=`, `daddr=`, `coordinate=`), di Google Maps (`@…`, `destination=`, `!3d…!4d…`) e `geo:`
   - gradi, primi e secondi: `45°04'12.3"N 7°41'10"E`
   - link con `lat=…&lon=…`

   I link brevi (`maps.apple/p/…`, `maps.app.goo.gl/…`) non contengono le coordinate: per quelli usa la mappa.
   Puoi anche aprire `…/mira/?add=<link o coordinate>&enb=58112` per avere il sito già compilato.
2. Sul tetto premi **Avvia sensori** e consenti l'accesso a posizione e sensori di movimento.
3. Prima di iniziare, calibra la bussola muovendo il telefono a "8".
4. Appoggia il telefono sul router e ruotalo finché non compare **✓ Allineato**.
5. Rifinisci il puntamento con il **registro segnale**, leggendo i valori dalla pagina di stato del router.

### Verificare di essere sul ripetitore giusto

Nella pagina di stato del router leggi il **Cell ID** (ECI) e dividilo per 256, tenendo solo la parte intera: ottieni l'**eNB ID**, cioè il numero del sito su LTE Italy.

```
Cell ID 14876693 ÷ 256 = 58112  →  sito 58112
```

Se il router si aggancia a un altro sito vicino:

- **Blocco di banda**: blocca le bande che ha solo il ripetitore che vuoi. Con il 5G DSS (NSA) conta soprattutto la banda **LTE** di appoggio.
- **Puntamento**: metti il ripetitore che non vuoi alle spalle del pannello.
- Il NE200-Outdoor non ha il blocco di cella (PCI/EARFCN), ma solo il blocco di banda.

## Consigli

- Tieni il telefono lontano da staffe, pali e cavi metallici: la bussola può sbagliare di parecchi gradi. Usala per arrivare a ±5° e rifinisci con il SINR.
- Se il telefono non è allineato al pannello, imposta un **offset** in Impostazioni (es. telefono sul fianco destro del router = 90).
- Mentre misuri, tieni lo schermo in verticale (portrait).

## Pubblicazione

GPS e bussola funzionano solo in **HTTPS**, quindi aprendo il file in locale sono bloccati. L'app è pensata per GitHub Pages: Settings → Pages → Branch `main` → Save.

Sul telefono, usa **Aggiungi a schermata Home** per aprirla come un'app.

## Privacy

Ripetitori, posizione del tetto e letture del segnale restano salvati **solo nel browser del telefono** (localStorage). Il sito pubblicato contiene solo il codice. Per spostare i dati su un altro dispositivo usa **Impostazioni → Copia backup / Importa backup**.

## Compatibilità

| Piattaforma | Bussola | Note |
|---|---|---|
| Android (Chrome) | `deviceorientationabsolute` | Calcolo compensato per l'inclinazione |
| iPhone (Safari) | `webkitCompassHeading` | Bisogna toccare "Avvia sensori" per dare il permesso |

Mappa: [Leaflet](https://leafletjs.com) 1.9.4 con i dati di © OpenStreetMap. Serve la connessione.
