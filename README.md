# 🎾 Tennis Live – ATP & WTA in einer App

Eine kleine Web-App für das iPhone (optimiert für das **iPhone SE**,
375 × 667 pt), die die Live-Spielstände der **ATP-** und **WTA-Tour** in
einer Ansicht zusammenführt. Kein Backend, kein Build-Schritt: eine einzige
HTML-Datei plus App-Icon. Oberfläche auf **britischem Englisch**.

## Navigation

Drei Schalter-Reihen, von oben nach unten:

1. **ATP / WTA** – Tour wählen
2. **Singles / Doubles** – Disziplin (Mixed zählt zu Doubles)
3. **Live / Finished / Upcoming** – Status; der Live-Tab zeigt die Anzahl
   laufender Matches der aktuellen Auswahl

Darunter die Matches, **nach Turnier gruppiert**. Die zuletzt gewählte
Ansicht wird gemerkt.

## Funktionen

- Laufender Satz im Live-Match farblich hervorgehoben
- Satzergebnisse mit Tiebreak-Punkten, Siegerinnen/Sieger fett
- Automatische Aktualisierung jede Minute (nur solange die App sichtbar ist),
  zusätzlich manueller Aktualisieren-Knopf
- Alle Zeiten in deutscher Zeit (Europe/Berlin)

## Datenquelle

Das öffentliche ESPN-Scoreboard-JSON für beide Touren
(`site.api.espn.com/...//tennis/atp|wta/scoreboard`). Es sendet offene
CORS-Header und kann daher direkt aus dem Browser abgerufen werden.
**Hinweis:** Die Schnittstelle ist inoffiziell – sollte ESPN sie ändern,
zeigt die App eine sichtbare Fehlermeldung statt veralteter Stände.

## Aufs iPhone bringen

1. **Hosten** – die Dateien `index.html` und `icon.png` auf einen beliebigen
   statischen Host legen, z. B. GitHub Pages, Netlify Drop oder
   Cloudflare Pages (alle kostenlos, kein Server nötig).
2. Die URL auf dem iPhone in **Safari** öffnen.
3. **Teilen-Symbol → „Zum Home-Bildschirm"** – die App erscheint mit
   Tennisball-Icon und startet im Vollbild wie eine native App.

### Schneller Test im Heimnetz (ohne Hosting)

```bash
cd tennis-freak
python3 -m http.server 8123
```

Dann auf dem iPhone `http://<IP-des-Macs>:8123` öffnen (gleiches WLAN
vorausgesetzt; der Mac muss dafür laufen – für den Dauerbetrieb ist
statisches Hosting die bessere Lösung).

## Dateien

| Datei        | Zweck                                      |
|--------------|--------------------------------------------|
| `index.html` | Die komplette App (HTML, CSS, JavaScript)  |
| `icon.png`   | Home-Bildschirm-Icon (180 × 180 px)        |
