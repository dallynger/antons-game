# Handoff – Antons Weitflug

Stand 24.09.2026. Diese Datei in eine neue Unterhaltung einfügen, dann ist der
Kontext da. Sie ersetzt das alte Handoff zu Version 24 vollständig — dort
stimmen Physik, Grafik und Dateiliste nicht mehr.

---

## Worum es geht

Browserspiel als Weihnachtsgeschenk 2026 für Anton (8). Weitflug-Spiel im Stil
von *Learn to Fly*: mit einer Zwille abschießen, dann möglichst weit rutschen,
Münzen sammeln, in der Werkstatt ausbauen.

Die Strecke ist die echte Kasseler Achse: Herkules → Kaskaden → Bergpark →
Wilhelmshöher Allee → Goetheanlage → Innenstadt → Orangerie/Karlsaue → über die
Fulda → Buga-See.

Anton selbst ist die Figur: Laufkleidung, verspiegelte orange Brille,
dunkelblaue Weste über türkisem Trikot, wuscheliges braunes Haar. Er sitzt in
einem aufblasbaren Ring.

**Es ist genau EIN aktives Projekt. Dieses hier. Neue Ideen kommen in die
Warteschlange, nicht in die Umsetzung.**

---

## Wo es liegt

| Was | Wo |
|---|---|
| Spielbare Fassung | https://dallynger.github.io/antons-game/ |
| Quelle | `index.html`, eine einzige Datei, 732 KB |
| Repo | `github.com/dallynger/antons-game`, öffentlich |
| Entwicklungszweig | `claude/magical-edison-acxvx5`, wird nach `main` gespiegelt |

GitHub Pages liefert `main` aus dem Wurzelverzeichnis. Jeder Push nach `main`
ist nach ein bis zwei Minuten live.

Three.js r128 liegt inline in der Datei. Einzige Ausnahme von „lädt nichts
nach": die Schrift *Archivo* von Google Fonts, mit sauberem Rückfall auf Arial
Black. Spielstand über `localStorage`.

**`leicht.html` gibt es nicht mehr.** Sie war ein alter Stand mit der
Ausreißer-Physik und ist entfernt.

---

## Was als Nächstes dran ist

1. **Konzeptdokument** (Rev 34) ist weiterhin veraltet und beschreibt einen
   Stand von vor diesen Änderungen.
2. **Reste beim Instancing**: Pistenstangen und Hütten laufen noch als Gruppen.
3. **Grafik-Inhalt**, offen: Felsmodelle, lebendigere Stadt (fahrende
   Straßenbahnen, Schaufenster, Kirchtürme), Anton selbst (flatternder Schal,
   Schneefahne, Reaktion beim Aufprall), echtes Wetter (Schneetreiben,
   Nebelbänke). Tageszeit und Baummodelle sind erledigt.

Bewusst zurückgestellt: Töne, die Stadtabschnitte erreichbar machen.

---

## Spielstand fachlich

**Abschuss.** Zwille nach hinten ziehen: Zuglänge = Kraft, Richtung = Winkel
und seitliches Zielen. Beim Ziehen zeigen Punkte die Flugbahn und ein Ring die
Landestelle mit Meterangabe.

**In der Luft.** Tippen und halten gibt Schub, solange der Tank reicht.
Zusätzlich steuert die **senkrechte Fingerposition die Neigung**: unten heißt
Nase runter — kostet Höhe, bringt Tempo, und nur währenddessen darf das
Tempolimit um 35 % überschritten werden. Oben heißt Nase hoch — kostet Tempo,
verlängert den Gleitflug. Am Rechner Pfeil hoch/runter.

**Rutschen.** Rund 85 % der Zeit am Boden. Der Untergrund entscheidet stark:
Eis gleitet, Schotter bremst mittel, Wiese bremst hart. Lenken geht frei über
die ganze Breite.

**Booster** liegen flach als Leuchtfeld auf der Piste (4,5 % der Objekte) und
geben beim Überfahren 130 Bilder lang Vollgas mit 35 % Übertempo, in der Luft
wie am Boden, mit Sog am Bildrand.

**Schanzen** wandeln Fahrt in Höhe: wer schnell ankommt, fliegt höher, und
verliert dabei 5,5 bzw. 10 % Tempo. Sie erzeugen *kein* Tempo mehr.

**Welt.** 220 m nach jeder Seite, fünf durchgehende Bahnen im Abstand von 84 m.
Die Bahnbreite wächst mit der Strecke (`pathW`): 21 m am Herkules, 38 m am Fuß
der Kaskaden, 48 m ab der Stadt. Nachgerechnet: geringster Abstand zwischen
zwei Bahnrändern über die ganze Strecke 20,4 m — sie berühren sich nirgends.

**Ausbauten.** Anlauf, Treibstoff, Schub, Flügel, Kufen, Lenkung, Glückskappe,
dazu der Ring in sechs Stufen. Die Flügel geben jetzt **Ruderwirkung beim
Neigen**, nicht mehr Endtempo.

**Wer spielt.** Im Startmenü eine Namensliste, bis sechs Namen. Jeder Name hat
einen eigenen Spielstand. Antons liegt unter `anton-weitflug-v1`, weitere unter
`anton-weitflug-v1__<Name>`, die Liste unter `anton-weitflug-namen`.

**Menü.** Startmenü und Endblatt liegen auf Reitern: Werkstatt, Anziehen, Wer
spielt. Während des Laufs gibt es oben rechts einen Neustart-Knopf.

**Versteckte Orte** (kein Bonus außer dem Spielplatz, keine Hausnummern):
Zuhause Papa und Das Schnucken in der Elfbuchenstraße, Spielplatz Goetheanlage,
Bei Mama in der Olgastraße. Beide Wohnungen gleichwertig dargestellt.

---

## Zahlen, die nicht aus dem Bauch kommen

Alles nachgerechnet mit einem Node-Skript, das die Physik ohne Browser
nachbildet, und im Browser gegengemessen.

Physik in Pixeleinheiten, `PPM = 2.2`, fester Zeitschritt 1/60 s:

```
G        = 0.0211815      Schwerkraft
AIR      = 0.999802993    Luftwiderstand je Schritt
VMAX     = 1.7531719      Tempolimit, je Schub-Stufe +0.061875
VSTOP    = 0.35           darunter ist Schluss (~9 m/s, gut 30 km/h)
FRIC     = [0.99955437, 0.99898688, 0.99675357]   Eis / Schotter / Wiese
STEER    = 0.007695       seitliche Beschleunigung
VZMAX    = 0.1215         seitliches Tempo
PITCH_V  = G * 0.95       Neigen, senkrecht
PITCH_H  = 0.01364502     Neigen, Tempogewinn im Sturzflug
DIVE_CAP = 0.35           Übertempo im Sturzflug
LIFT_CAP = G * 0.92       harte Auftriebsgrenze, siehe unten
TURBO_A  = 0.0165         Vollgas
TURBO_CAP= 0.35           Übertempo bei Vollgas
TURBO_T  = 130            Dauer in Bildern
Tank     = 391 + Sprit-Stufe * 113 Bilder
```

**Zeitdehnung** ist die Technik hinter dem Tempo: Geschwindigkeiten mal k,
Beschleunigungen mal k², Reibung hoch k, Bildzähler durch k. **Aktuell
k = 0.45** (vorher 0.32). Wer das Tempo erneut ändert, zieht diese Rechnung
durch alle 24 betroffenen Konstanten durch — auch durch die der Flugbahn-Vorschau
beim Zielen, sonst zeigt der Landering falsch an. Nicht einzelne Werte anfassen.

Gemessen, voll ausgebaut: nichts tun 710 m in 24,8 s, nur Schub 1768 m, Spitze
203 km/h, im Sturzflug 273 km/h.

---

## Die drei harten Grenzen

Ohne sie läuft die Balance davon. Alle drei sind konstruktiv, nicht gestimmt.

1. **`LIFT_CAP`**: Auftrieb aus Flügeln und Nase-hoch zusammen bleibt unter der
   Schwerkraft. Ohne das stieg Anton mit voll ausgebauten Flügeln **unendlich** —
   gemessen 2.232.575 m Höhe, der Lauf endete nie.
2. **Übertempo lässt sich nicht ansparen**: das erhöhte Limit gilt nur, solange
   die Nase unten ist bzw. Vollgas läuft, und wird sofort gekappt.
3. **Auftrieb skaliert mit dem Tempo**: wer die Fahrt verliert, sackt weg. Das
   begrenzt das Segeln von selbst.

---

## Grafik

Three.js r128 als UMD, globales `THREE`. WebGLRenderer, Hemisphere- plus
Directional-Light, PCFSoftShadowMap, sRGB, Nebel, Gelände als Buffergeometrie
mit Vertexfarben, die der Kamera folgt.

**Fünf Baumarten**: Fichte, schlanke Tanne, runder Busch-Baum, kahler
Laubbaum (die auffälligste Winter-Silhouette) und breite alte Fichte. Die
Mischung hängt an der Zone — im Bergpark Nadelbäume, in Stadt und Karlsaue
viele kahle.

**Instancing.** Zwölf Einzel-Mesh-Pools über `poolInst()` und neun Gruppen
(fünf Baumarten, Gebäude, Laternen, Bänke, Booster) über `poolInstGroup()`. Die
`take()`-Schnittstelle ist identisch geblieben, deshalb musste keine
Aufrufstelle angefasst werden. Teile einer Gruppe dürfen eigene Lage, Größe und
Drehung haben, und `house()` setzt sie je Haus einzeln.

| Zone | Zeichenaufrufe vorher | nachher |
|---|---|---|
| Bergpark | 1154 | 121 |
| Allee | 733 | 123 |
| Innenstadt | 734 | 119 |
| Karlsaue | 624 | 132 |

**Tageszeit.** Der Himmel wandert über die Strecke: kalter Morgen am Herkules,
klarer Vormittag im Bergpark, heller Mittag über der Stadt, Abendlicht am
Buga-See. Vier Stützstellen in `TAG`, dazwischen wird weich gemischt. Betroffen
sind Kuppelhelligkeit, Dunstfarbe, Hintergrund, Sonnenfarbe und -stärke, beide
Farben des Himmelslichts, Sonnenscheibe und Bergketten. Kostet nur Farbwerte je
Bild, wirkt aber auf jedem Meter.

**Bergketten** am Horizont: drei gezackte Bänder auf der Himmelskuppel, die mit
der Kamera mitlaufen und nie näher kommen. Drei Meshes für die ganze Tiefe.

**Kaskaden**: Stufenband mit Höhe, Wasser und Randmauern, **neben** der Bahn.
Drei Anläufe gebraucht — flach am Boden lasen sie sich als blaue Bodenfliesen,
am Weltrand und mittig zwischen zwei Bahnen waren sie aus der Nähe unsichtbar.

**Herkules**: achteckiger Unterbau, 16 Säulen, Gesims, neunstufige Pyramide,
Statue. Hängt nicht am Pool, sondern wird einmal gebaut und ein-/ausgeblendet.
Steht bei z = −36, also klar neben der Bahn.

**Keine automatische Abstufung mehr.** Das Spiel läuft immer auf voller Stufe.
Auch die geräteabhängigen Drosseln sind entfernt: volles Terrainraster
(92 × 58), Schattenkarte 2048, weiche Schatten, Kantenglättung und
Pixelverhältnis bis 2 — auf allen Geräten. Als Handschalter bleiben
`?stufe=1` (Schatten aus), `?stufe=2` (zusätzlich gröber) und `?stufe=3` (Welt
ausgedünnt).

---

## Fallen, die schon mal zugeschnappt sind

**Zeichnen und Kollision dürfen nicht zweimal gerechnet werden.** Früher bildete
`treeAt()` die Baumstreuung ein zweites Mal nach. Als die Walddichte stieg,
drifteten beide auseinander und man flog durch Bäume. Jetzt trägt sich jedes
Objekt beim Zeichnen selbst über `addSolid()` in eine Hindernisliste ein, und
die Kollision liest nur diese. **Keine zweite Formel für dieselbe Sache.**

**Das Geländeraster ist grob.** Quer gibt es nur alle ~3,7 m einen Stützpunkt.
Muster oder Übergänge, die feiner sind, zerfallen in harte Kanten — ein
Rillenmuster mit 4,3 m Periode ergab einen senkrechten Riss quer durchs Bild.
Feines gehört in die Bodentextur, Übergänge brauchen mindestens 12 m.

**`setColorAt()` legt den Farbpuffer in der Größe von `count` an.** Wer `count`
beim Bauen auf 0 setzt, bekommt einen leeren Puffer und schwarze Objekte. Puffer
ausdrücklich in voller Größe anlegen.

**`frustumCulled = false` ist bei `InstancedMesh` Pflicht.** Die Hülle wird nur
aus der Geometrie berechnet, nicht aus den Instanzpositionen — sonst
verschwindet alles, sobald der Ursprung aus dem Bild läuft.

**Pools, die ausgehen.** `take()` zählt jetzt mit (`poolMiss`), unter `?debug`
auslesbar. Bei Dichteänderungen prüfen.

**Blickfeldwerte sind senkrecht gemeint.** 58° senkrecht werden auf einem
breiten Fenster über 90° waagerecht, und alles am Bildrand zieht sich lang.
`fovFit()` deckelt waagerecht auf 78°. Im Hochformat ändert das nichts.

**Objekte am Boden.** Das Gelände hat Wellen, `groundM()` kennt die nicht.
Deshalb ist die Bahn glatt gewalzt. Der Schneewulst an der Pistenkante ist rein
optisch.

**Abstand zur Bahn prüfen.** Häuser taten das nie — bei höherer Dichte stand
Anton plötzlich in einem Haus. Alle Platzierungen halten `pathW(x) + 7`.

**Vereinbarungen.** Echte Vereinsnamen, Wappen und Trikotdesigns gehen nicht.
Erfundene Vereine in denselben Farben. Zugangsdaten gehören nicht in den Chat.

---

## Wie geprüft wird

Playwright mit dem vorinstallierten Chromium unter
`/opt/pw-browsers/chromium-1194/chrome-linux/chrome`,
`PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD=1`, kein Nachinstallieren. Läuft über einen
Software-Renderer — die Bildraten dort sagen nichts über echte Geräte, die
Zeichenaufrufe und Poolzahlen schon.

Bewährte Messungen:

- Lauf abschießen, Weite und Dauer mitschreiben, daraus Schnitt und Spitze
- Poolauslastung und `poolMiss` ausgeben, um Erschöpfung zu finden
- Zeichenaufrufe und Dreiecke aus `renderer.info` je Zone
- Bildschirmfotos ansehen statt auf Zahlen zu vertrauen

**Hilfen in der Adresse**, alle nur mit `?debug` wirksam:

| Anhängsel | Wirkung |
|---|---|
| `?debug` | legt `window.__dbg` offen: Pools, `poolMiss`, `renderer.info`, Spielzustand, Kamera, Stufe |
| `?debug&at=3600` | startet weiter hinten auf der Strecke — Stadt und Aue liegen jenseits jeder Wurfweite |
| `?stufe=0..3` | erzwingt eine Leistungsstufe |
| `?voll` | ohne `?debug` nutzbar, lässt jede Abstufung aus |

Balance wird nicht nach Gefühl gedreht, sondern mit einem Node-Skript
gerechnet, das dieselben Formeln ohne Browser durchläuft. Für Videos vom Gerät:
`@ffmpeg-installer/ffmpeg` über npm holen — das mitgelieferte ffmpeg von
Playwright kann kein HEVC.

---

## Spielstand zurücksetzen

```js
Object.keys(localStorage)
  .filter(k => k.startsWith("anton-weitflug"))
  .forEach(k => localStorage.removeItem(k));
location.reload();
```
