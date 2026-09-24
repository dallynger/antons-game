# Antons Weitflug

Browserspiel: mit der Zwille vom Herkules abschießen und so weit wie möglich
Richtung Buga-See rutschen. Münzen sammeln, in der Werkstatt ausbauen.
Die Strecke ist die echte Kasseler Achse — Herkules, Kaskaden, Bergpark,
Wilhelmshöher Allee, Goetheanlage, Innenstadt, Orangerie, über die Fulda.

## Spielen

**https://dallynger.github.io/antons-game/**

| Datei | Was |
|---|---|
| `index.html` | die volle Fassung mit 3D-Grafik (Three.js, 680 KB, alles inline) |
| `leicht.html` | schlanke Fassung ohne 3D-Bibliothek, falls das iPhone zäh läuft |

Die schlanke Fassung liegt unter `https://dallynger.github.io/antons-game/leicht.html`.

Beide Dateien teilen sich denselben Spielstand (`localStorage`, Schlüssel
`anton-weitflug-v1`), weil sie unter derselben Adresse liegen — man kann also
zwischen den Fassungen wechseln, ohne Münzen zu verlieren.

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

## Spielstand zurücksetzen

Entwicklerkonsole öffnen und eingeben:

```js
localStorage.removeItem("anton-weitflug-v1"); location.reload();
```
