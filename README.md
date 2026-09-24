# Antons Weitflug

Browserspiel: mit der Zwille vom Herkules abschießen und so weit wie möglich
Richtung Buga-See rutschen. Münzen sammeln, in der Werkstatt ausbauen.
Die Strecke ist die echte Kasseler Achse — Herkules, Kaskaden, Bergpark,
Wilhelmshöher Allee, Goetheanlage, Innenstadt, Orangerie, über die Fulda.

## Spielen

**https://dallynger.github.io/antons-game/**

Das ganze Spiel steckt in `index.html` — eine einzige Datei mit 3D-Grafik
(Three.js inline), ohne Bauwerkzeug und ohne Server.

## Wer spielt?

Im Startmenü steht oben eine Namensliste. Jeder Name hat seinen **eigenen**
Spielstand: eigene Münzen, Ausbauten, Trikots und Bestweite. Antons Stand
liegt unter `anton-weitflug-v1`, weitere Namen bekommen den Namen angehängt.
Besuch kann also spielen, ohne Antons Rekord zu überschreiben.

## GitHub Pages einschalten

Einmalig, danach ist die Adresse dauerhaft erreichbar:

1. Im Repo auf **Settings** → links **Pages**
2. Unter *Build and deployment*: Source **Deploy from a branch**
3. Branch **main**, Ordner **/ (root)**, dann **Save**
4. Eine bis zwei Minuten warten, dann die Adresse oben aufrufen

## Steuerung

- **Zwille nach hinten ziehen** und loslassen. Zuglänge = Kraft,
  Richtung = Winkel und seitliches Zielen.
- **In der Luft tippen und halten** gibt Schub, solange der orange Balken unten reicht.
- **Beim Rutschen links oder rechts** antippen, um seitlich zu lenken.
  Rampen bringen wieder Höhe.
- Am Rechner: Leertaste schießt und gibt Schub, Pfeiltasten lenken.

## Technisch

- Eine einzelne HTML-Datei je Fassung, keine Bauwerkzeuge, kein Server nötig.
  Zum Testen reicht ein Doppelklick auf die Datei.
- Three.js r128 liegt in `index.html` inline — nichts wird nachgeladen.
- Einzige Ausnahme: die Schrift *Archivo* kommt von Google Fonts. Ohne Netz
  fällt die Seite sauber auf Arial Black / System-Schrift zurück, das Spiel
  läuft normal weiter.
- Sparmodus: Nach 2,2 Sekunden wird die Bildrate gemessen. Unter 26 fps gehen
  Sichtweite, Schatten und Pixelverhältnis herunter. Mit `?voll` an der Adresse
  bleibt die volle Fassung erzwungen:
  `https://dallynger.github.io/antons-game/?voll`
- Zum Prüfen: `?debug` legt Messwerte offen, `?debug&at=3600` startet weiter
  hinten auf der Strecke. Ohne `?debug` wirkungslos.

## Spielstand zurücksetzen

Entwicklerkonsole öffnen und eingeben:

```js
localStorage.removeItem("anton-weitflug-v1"); location.reload();
```
