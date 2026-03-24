# KAMPLACON — Broodjes bestellen

Eenvoudige webapplicatie voor het intern bestellen van broodjes bij Kamplacon. Eén HTML-bestand met embedded CSS en JavaScript, Firebase Firestore als database en Excel-export via SheetJS.

---

## Inhoud

- `index.html` — de volledige applicatie (bestelformulier + beheerpagina)

---

## Firebase instellen

### 1. Firebase project aanmaken

1. Ga naar [https://console.firebase.google.com](https://console.firebase.google.com) en maak een nieuw project aan.
2. Schakel **Google Analytics** naar wens in of uit.

### 2. Firestore database aanmaken

1. Ga in de Firebase console naar **Firestore Database** → **Database aanmaken**.
2. Kies **Productie-modus** (of testmodus voor development).
3. Selecteer een regio (bijv. `europe-west4`).

### 3. Firestore beveiligingsregels

Pas de regels aan zodat iedereen kan schrijven (bestellingen plaatsen) maar alleen lezen via de beheerder-interface (client-side beveiligd met wachtwoord):

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /bestellingen/{doc} {
      allow write: if true;
      allow read: if true;
    }
  }
}
```

> **Let op:** voor productie is het aan te raden om Firebase Authentication toe te voegen en de leesregel te beperken tot ingelogde gebruikers.

### 4. Web-app registreren en config ophalen

1. Ga in de Firebase console naar **Projectinstellingen** (tandwiel-icoon) → **Algemeen**.
2. Scroll naar **Jouw apps** → klik op het web-icoon (`</>`).
3. Geef de app een naam en klik **App registreren**.
4. Kopieer het `firebaseConfig`-object.

### 5. Config invullen in index.html

Open `index.html` en vervang het placeholder-blok bovenaan het `<script>`-gedeelte:

```js
const firebaseConfig = {
  apiKey:            "JOUW_API_KEY",
  authDomain:        "JOUW_PROJECT.firebaseapp.com",
  projectId:         "JOUW_PROJECT_ID",
  storageBucket:     "JOUW_PROJECT.appspot.com",
  messagingSenderId: "JOUW_SENDER_ID",
  appId:             "JOUW_APP_ID"
};
```

Vervang elke waarde door de bijbehorende waarde uit jouw Firebase-console.

---

## Hosten

### Optie A — Netlify (aanbevolen, gratis)

1. Maak een account op [https://netlify.com](https://netlify.com).
2. Ga naar **Sites** → **Add new site** → **Deploy manually**.
3. Sleep de map met `index.html` naar het uploadvenster.
4. Netlify geeft direct een live URL.

Voor automatische deploys via Git:
1. Koppel je GitHub-repository aan Netlify.
2. Zet **Publish directory** op `/` (of de map waar `index.html` staat).
3. Bij elke push naar `main` wordt de site automatisch bijgewerkt.

### Optie B — GitHub Pages

1. Push `index.html` naar een GitHub-repository.
2. Ga naar **Settings** → **Pages**.
3. Kies bij **Source**: `Deploy from a branch` → branch `main` → map `/ (root)`.
4. De site is beschikbaar op `https://<gebruikersnaam>.github.io/<repo-naam>/`.

---

## Wachtwoord wijzigen

Het beheerwachtwoord staat hardcoded in `index.html`. Zoek de volgende regel en vervang de waarde:

```js
const ADMIN_PASSWORD = "kamplacon2024";
```

Verander `"kamplacon2024"` naar een wachtwoord naar keuze en sla het bestand op.

> **Tip:** voor een veiligere oplossing kun je Firebase Authentication gebruiken en het wachtwoord daar beheren, zodat het niet in de broncode zichtbaar is.

---

## Gebruik

| Functie | Beschrijving |
|---|---|
| Bestelformulier | Hoofdpagina — naam, broodje, broodsoort, saus, opmerkingen |
| Live prijsoverzicht | Wordt automatisch bijgewerkt bij elke selectie |
| Bevestiging | Verschijnt na succesvol plaatsen van de bestelling |
| Beheer | Klik op **Beheer** in de header, voer het wachtwoord in |
| Weekoverzicht | Toont alle bestellingen van maandag t/m zondag van de huidige week |
| Excel-export | Knop **Download Excel** — bestandsnaam: `Broodjes_week_[nr]_[jaar].xlsx` |

---

## Technische stack

- **HTML/CSS/JS** — één enkel bestand, geen build-stap nodig
- **Firebase Firestore** v9 (compat SDK via CDN)
- **SheetJS** v0.20 via CDN voor Excel-export
- **Google Fonts** — Inter
