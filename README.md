# Hausplan 2026 – Bliesweg 4a (PWA)

Vollwertige Progressive Web App (PWA) zur Verwaltung von Putzplan, Mülltonnenplan und Mietparteien. Installierbar auf iPhone/Android/Desktop, mit Admin-Login und Live-Sync über Firebase.

## Funktionen

- **Admin-Login** (E-Mail/Passwort via Firebase Authentication): nur Admin kann Mietparteien anlegen, bearbeiten, löschen und die Rotationsreihenfolge per Drag & Drop ändern.
- **Alle anderen (Viewer)** sehen nur die aktuellen Daten – ohne Bearbeitungsmöglichkeit.
- **PDF-Export**: kompletter Hausplan als PDF-Download (funktioniert für alle, auch ohne Login).
- **Kalender-Export**: Putzplan, Mülltonnenplan oder beides als `.ics`-Datei – direkt importierbar in Apple Kalender, Google Kalender oder Outlook.
- **Live-Sync**: Änderungen des Admins werden über Firestore in Echtzeit an alle offenen Geräte übertragen.
- **Offline-fähig**: Service Worker cached die App, funktioniert auch ohne Internetverbindung (zuletzt geladene Daten).
- **Installierbar**: "Zum Home-Bildschirm hinzufügen" auf iPhone/Android – App-Icon inklusive.

## 1. Setup – Firebase (kostenlos, ca. 10 Minuten)

1. Gehe zu [console.firebase.google.com](https://console.firebase.google.com) und erstelle ein neues Projekt (kostenloser "Spark"-Plan reicht völlig).
2. Klicke auf **Web-App hinzufügen** (</> Symbol), gib der App einen Namen (z. B. "Hausplan").
3. Kopiere das angezeigte `firebaseConfig`-Objekt.
4. Öffne `index.html` in diesem Projekt, suche nach `const firebaseConfig = {` (im `<script>`-Block ganz unten) und ersetze die Platzhalterwerte durch deine echten Werte.
5. Aktiviere in der Firebase-Konsole unter **Authentication → Sign-in method** die Methode **E-Mail/Passwort**.
6. Lege unter **Authentication → Users** einen Admin-Benutzer an (deine E-Mail + Passwort). Das ist dein Admin-Login für die App.
7. Aktiviere **Firestore Database** (im Produktionsmodus).
8. Kopiere den Inhalt von `firestore.rules` in die Firestore-Regeln (Firestore → Regeln) und veröffentliche sie. Diese Regeln erlauben allen das Lesen, aber nur angemeldeten Nutzern (= deinem Admin) das Schreiben.

## 2. Veröffentlichen über GitHub Pages (Link zum Teilen)

1. Erstelle ein neues GitHub-Repository (z. B. `hausplan-bliesweg4a`) und lade den kompletten Inhalt dieses Ordners hoch (inkl. `.github/workflows/deploy.yml`).
2. Gehe im Repository zu **Settings → Pages** und wähle als Quelle **GitHub Actions**.
3. Push auf den `main`-Branch löst automatisch das Deployment aus (Workflow ist bereits enthalten).
4. Nach ein bis zwei Minuten ist die App unter `https://DEIN-USERNAME.github.io/hausplan-bliesweg4a/` erreichbar.
5. Diesen Link kannst du an alle Mietparteien schicken. Sie sehen die App direkt im Browser und können sie optional auf dem Homescreen installieren.

## 3. Nutzung

- **Als Admin**: Auf "Admin" oben rechts klicken, mit E-Mail/Passwort anmelden. Danach erscheinen "Partei hinzufügen"-Buttons und Drag & Drop wird aktiv. Alle Änderungen werden automatisch in der Cloud gespeichert und an alle anderen Geräte live übertragen.
- **Als Mieter/Viewer**: Kein Login nötig. Alle Tabs (Plan, Übersicht, Putzplan, Mülltonnen, Parteien, Statistik) sind sichtbar, aber nicht bearbeitbar.
- **PDF sichern**: Erstellt ein mehrseitiges PDF mit dem kompletten Jahresplan zum Ausdrucken/Aufhängen im Treppenhaus.
- **Kalender**: Über den "Kalender"-Button in der Navigationsleiste eine `.ics`-Datei herunterladen und in die eigene Kalender-App importieren.

## Ohne Firebase nutzen (lokaler Testmodus)

Wird `firebaseConfig` nicht ausgefüllt, läuft die App im reinen Lokalmodus: Daten werden nur im Browser (localStorage) gespeichert, es gibt keinen Admin-Login und keine Cloud-Synchronisation – nützlich zum schnellen Testen, aber nicht für den produktiven Mehrbenutzer-Einsatz.

## Dateien

- `index.html` – die komplette App (HTML/CSS/JS in einer Datei)
- `manifest.json` – PWA-Manifest (App-Name, Icons, Startbildschirm)
- `sw.js` – Service Worker für Offline-Funktion
- `icons/` – App-Icons (192px, 512px)
- `firestore.rules` – Sicherheitsregeln für die Firestore-Datenbank
- `.github/workflows/deploy.yml` – automatisches Deployment auf GitHub Pages
