# 🎾 Tennis Freak – ATP & WTA in einer App

Eine kleine Web-App für das iPhone (optimiert für das **iPhone SE**,
375 × 667 pt), die die Live-Spielstände der **ATP-** und **WTA-Tour** in
einer Ansicht zusammenführt. Kein Backend, kein Build-Schritt: eine einzige
HTML-Datei plus App-Icon. Oberfläche auf **britischem Englisch**.

## Navigation

- Oben: **ATP / WTA** als markanter Segment-Schalter, darunter
  **Singles / Doubles** als dezente Text-Tabs (Mixed zählt zu Doubles)
- Unten: **Live / Finished / Upcoming / Rankings** in einer schwebenden
  „Liquid-Glass"-Leiste; der Live-Tab zeigt die Anzahl laufender
  Matches der aktuellen Auswahl
- **Rankings**: Weltrangliste (Top 150) der gewählten Tour mit Punkten
  und Trend-Pfeilen; Singles/Doubles blendet sich dort aus, da der Feed
  nur Einzel-Ranglisten liefert

Dazwischen die Matches: In Finished/Upcoming zuerst **nach Tagen**
(Today, Yesterday/Tomorrow, …), darin **nach Turnier** und innerhalb
des Turniers **nach Runden** mit kleinen Zwischenüberschriften.
Die zuletzt gewählte Ansicht wird gemerkt.

## Funktionen

- **Court** jedes Matches in der Infozeile (sofern der Feed ihn liefert)
- **Unterbrochene Matches** (Regen/Dunkelheit) bleiben unter **Live** stehen
  und sind orange als „Suspended" markiert – inkl. Fortsetzungszeit, sobald
  die Neuansetzung im Feed steht. (ESPN sortiert sie sonst fälschlich unter
  Finished bzw. Upcoming ein.)
- **Setzlisten-Nummer** in Klammern neben dem Namen
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
