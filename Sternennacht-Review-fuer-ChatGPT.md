# Sternennacht – Nachtfahrt · Review und Übergabe an ChatGPT

| | |
|---|---|
| **Prüfgegenstand** | `Sternennacht-Nachtfahrt.html` aus `Sternennacht-Nachtfahrt.zip` (Google Drive), Stand 24.11.2026 |
| **ZIP** | 91 381 895 Byte, SHA-256 `52ee7afd46c9312fae32c47f6116a36d24063b479dbb5c6767f231cd215aec93` |
| **HTML** | 122 815 176 Byte, SHA-256 `f128a58618f6ebb0055a40d5f71ea8b7ae307e6da54ea59da8f525e6e2045953` |
| **Referenz** | https://starrynightrider.com/ (index.html zuletzt geändert 22.11.2026, Bundle `assets/index-CQrjeIkV.js`) |
| **Prüfdatum** | 24.11.2026 |
| **Rolle** | unabhängiges Review; die Website und das ZIP wurden nicht verändert |

---

## Lesehilfe

### Herkunftsmarken

Jede Aussage trägt eine Herkunftsmarke:

- **[Beobachtet]** – im Browser selbst gesehen (Screenshot liegt beim Prüfer).
- **[Gemessen]** – per Skript gemessener Wert.
- **[Berechnet]** – aus Messwerten oder Code-Konstanten berechnet.
- **[Code]** – aus dem Quellcode abgeleitet.
- **[Nicht geprüft]** – mangels Zugriff offen.
- **[Annahme]** – begründete Vermutung, vor der Umsetzung zu bestätigen.

### Teilprüfungen

Drei Teilbereiche wurden zusätzlich von unabhängigen Prüfagenten mit frischem Kontext untersucht: Bild und Rendering, Fahrgefühl, Audio und Offline. Ich habe ihre Ergebnisse zusammengeführt. Jede zitierte Codestelle habe ich selbst nachgeschlagen; alle im Bericht genannten Suchtexte kommen genau einmal im jeweiligen Bundle vor, sofern nicht anders vermerkt.

### Prüfumgebung und ihre Grenzen

- **Browser:** Linux-Container mit headless Chromium 158 (Playwright). WebGL lief nur per Software (SwiftShader), mit 0,4–0,5 Bildern pro Sekunde bei 1280 × 720.
- **Fahrsequenzen:** Sie wurden mit einer virtuellen Uhr abgespielt: genau 1/60 s Simulationszeit pro Schritt, unabhängig von der Software-Grafik.
- **Belastbar sind:** Bildinhalte, Kameraführung, Layout, Startzeiten, Codepfade, Asset-Maße und die Audio-Messwerte.
- **Nicht belastbar sind:** echte Bildrate, Ruckeln und Eingabelatenz auf einer GPU.
- **Klang:** Ich konnte die Musik nicht hören. Alle Audioaussagen beruhen auf Messungen.
- **Handy:** Es wurde nur emuliert (844 × 390 CSS-px, Touch). Das ist ein Layouttest, kein Gerätetest.
- **Offline-Test:** Er lief echt offline. Chromium lief in einem Netzwerk-Namensraum ohne jede Verbindung (`unshare -n`), die HTML wurde direkt per `file://` geöffnet, ohne lokalen Server. Einzelne Bildmessungen der Teilprüfung „Bild“ liefen dagegen über einen lokalen HTTP-Server mit den entpackten Dateien; das ist kein Offline-Test.

### Suchtexte statt Zeilennummern

Die Bundles sind minifiziert und einzeilig. Code-Stellen werden deshalb über eindeutige Suchtexte benannt, zum Beispiel `else if(ie.directEntry)` in `bundles/original.mjs`. Die minifizierten Bezeichner (`ie`, `jt`, `Re`, `J` …) gelten für genau diese Fassung. Baut ChatGPT aus Quelltext neu, findet es die Stellen über Suchtext und Logik wieder.

---

## So ist die Datei aufgebaut

Die äußere HTML enthält 553 eingebettete Dateien als `<script type="application/octet-stream" data-file="…" data-type="…">`. Das sind 122,75 MB Base64, dekodiert 92,06 MB.

Ein Router im letzten, nicht eingebetteten `<script>` stellt `window.Nachtfahrt` mit den Funktionen `asset()`, `storage()` und `open()` bereit. Je Ansicht baut er ein `iframe` per `srcdoc`; beim Wechsel wird das alte `iframe` entfernt. Unter `file://` werden Bilder und Schriften zu `data:`-URLs, alles andere zu `blob:`-URLs. Die CSP lautet `default-src 'none'`.

| Route (`#…`) | Seite und Bundle | Rolle |
|---|---|---|
| `start` | `index.html` + `bundles/home.mjs` (776 KB) | Startseite; Gemäldebetrachter („Gemälde erkunden“, Dialog `#art`) |
| `fahrt` | `vorlage/index.html?fahrt=1` + `bundles/original.mjs` (934 KB) | **die Fahrt** („Fahrt beginnen“) |
| `dorf` | `index.html?fahrt=1&classic=1` | ältere eigene Dorffahrt; Ersatz ohne WebGL 2; sonst nur über einen Werkinfo-Link erreichbar |
| `reise`, `vergleich` | `gemaelde.html` + `bundles/journey.mjs` (627 KB) | ältere Gemäldereise; nur über einen Werkinfo-Link erreichbar |

| Asset | Pixel | Größe | Verwendung |
|---|---|---|---|
| `ride/art/1/*.jpg` (30 Kacheln) | 5 571 × 4 412 | 5,6 MB | Betrachter, Stufe 1 |
| `ride/art/2/*.jpg` (99 Kacheln) | 11 142 × 8 823 | 17,9 MB | Betrachter, Stufe 2 |
| `ride/art/3/*.webp` (396 Kacheln) | 22 284 × 17 646 | 41,9 MB | Betrachter, Stufe 3 |
| `ride/art/cover.webp` | 2 300 × 1 821 | 1,4 MB | Startseite, Betrachter-Gesamtansicht, Ladebildschirm der Fahrt |
| `vorlage/painting.webp`, `vorlage/sky.webp` | je 4 096 × 3 244 | 5,6 und 4,3 MB | Fahrt (Nicht-Touch-Geräte) |
| `vorlage/painting-s.webp`, `vorlage/sky-s.webp` | je 1 800 × 1 426 | je 0,7 MB | Fahrt (alle Touch-Geräte) |
| `vorlage/flow.png`, `cypress.png`, `fill.png` | 500 × 396, 1 500 × 1 188, 375 × 297 | 0,5 MB | Fahrt: Strichrichtung, Zypressenmaske, Farbfüllung |
| `ride/sky-panorama.webp`, `ride/sky-mobile.webp` | 6 144 × 1 280, 4 096 × 853 | 2,8 und 1,1 MB | nur ältere Dorffahrt |
| `kunst/audio/sternennacht-piano.mp3` | 257,5 s | 6,2 MB | Klavier |
| `kunst/audio/sternennacht-piano-preview.mp3` | 28 s | 0,7 MB | nur „Piano anhören“ in der Gemäldereise |

---

## A. Kurzurteil für ChatGPT

**Kernaussage:** Die Fahrt ist technisch und künstlerisch stark, weil sie die Engine der Referenz nutzt. Die Portierung hat drei Dinge geschwächt:

- Den Einstieg hat sie gehetzt.
- Das Klavier bricht an jedem Übergang ab.
- In Kurven schwimmt der Fernraum mit, statt stehen zu bleiben.

Mehr Quellpixel helfen nirgends. Mehr Ruhe hilft: im Einstieg, in der Kamera, an den Kanten und in der Oberfläche.

**Vorbedingung vor jeder Weitergabe der Datei:** Befund L1 (Herkunft der Engine). Für den privaten Gebrauch ändert er nichts an der Umsetzung.

### Die drei Änderungen, die man bei der nächsten Fahrt am deutlichsten merkt

**1. Der Einstieg zieht ins Bild, und die Fahrt beginnt mit freiem Bild (E1 und der Kern von UI1).**
- **Heute:**
  - Der Ladebildschirm zeigt das Gemälde bildschirmfüllend beschnitten.
  - Sobald das Dorf geladen ist, wechselt das Bild binnen 0,22 s auf einen kleinen gerahmten Scan. 0,42 s nach Ladeende ist der Scan aufgelöst.
  - 2,70 s nach Ladeende steht man in der Gasse; die Kamera erreicht dabei bis zu 45 m/s, in der Referenz 14,6 m/s.
  - Jeder Tipp aufs Display beendet den Einstieg sofort, ebenso jede Fahr-, Lenk- oder Sprungeingabe.
  - Danach verdecken Spielelemente 24,7 % der Bildfläche.
- **Künftig:**
  - Der gerahmte Scan steht schon unter dem Ladehinweis und bleibt 1 s stehen.
  - Dann folgt ein Flug von 6,5 s über die Dächer in die Gasse; Berührungen stören ihn nicht; W oder S überspringen erst ab 3 s.
  - Die Fahrt beginnt im vorhandenen Modus „Bild pur“, also ohne eingeblendete Anzeigen (das „HUD“) und ohne Spielobjekte.
- **Code:** drei Ersetzungen, an einer Kopie getestet (C1); dazu zwei CSS-Zeilen und eine HTML-Klasse.

**2. Das Klavier trägt die ganze Fahrt (AU1, AU2).**
- **Heute:**
  - Jeder Ansichtswechsel schneidet die Musik hart ab.
  - In der Fahrt setzt sie erst mit der ersten echten Eingabe ein, mitten in der Phrase und mit nur 0,2 s Einblendung.
  - Diese erste Eingabe beendet zugleich den Einstieg.
- **Künftig:**
  - Ein einziger Spieler im äußeren Dokument startet mit dem Klick auf „Fahrt beginnen“ und blendet über 4 s ein.
  - Er läuft über alle Ansichten weiter.
  - Aus- und Wiedereinblenden dauern 0,5 bzw. 1,2 s; der Lautstärkeregler arbeitet quadratisch.
- **Code:** Entwurf, an einer Kopie über fünf Ansichtswechsel getestet (C2).

**3. In der Kurve bleibt der Fernraum stehen, und das Sichtfeld atmet nicht mehr (R1, R2).**
- **Heute:**
  - Die Kamera hinkt bis zu 32° hinterher und dreht nach der Kurve noch 38° nach.
  - Der Kirchturm wischt mit bis zu 1000 px/s über das Bild, der Horizont kippt um 3°.
  - Jedes W vergrößert das Sichtfeld von 73,5° auf 83,9°, Kirchturm, Hügel und Himmel schrumpfen dabei um 17 %.
- **Künftig:**
  - Der Fernraum dreht nur, solange das Rad dreht.
  - Der Horizont bleibt fast gerade, die nahen Hausecken tragen die Bewegung.
- **Code:** drei Zahlen, in der Fahrgefühl-Teilprüfung simuliert (C6).

### Warum diese Reihenfolge

1. **Einstieg:** Er betrifft die ersten Sekunden jeder Fahrt, Ihr erstes Ziel. Er ist mit drei Zeilen der billigste große Gewinn und bereits getestet. Er schließt den ruhigen Start ohne Anzeigen ein, weil dieselbe Änderung ihn mitliefert.
2. **Klavier:** Es ist die zweite Hälfte des Grundwunsches („dabei ruhiges Klavier hören“), und heute bricht es genau an den Übergängen.
3. **Kurve:** Sie ist Ihr ausdrücklich beschriebenes Raumgefühl. Sie steht an dritter Stelle, weil sie erst nach dem Einstieg wirkt und weil ihre Werte auf echter GPU noch abzuschmecken sind.
4. **Danach:** Das Paket Bildruhe und Schärfe (BQ1–BQ4: Kantenglättung, Renderauflösung, Dachtupfer, Himmel) und das Anhalten (R3). Beide sind für „weich und unruhig“ und „Pinselstriche beim Anhalten“ entscheidend. BQ braucht aber GPU-Messungen auf echter Hardware.

### Was ChatGPT unbedingt bewahren soll

- **Engine und Bildsprache:**
  - der Scan als Himmel aus dem Blickpunkt des Malers,
  - Strichrichtung aus `flow.png`, Mondlicht, Laternen, warme Fenster,
  - `uPaint` = 0, also keine am Bildschirm klebenden Muster,
  - der Szenenpuffer in halber Fließkomma-Genauigkeit mit manueller sRGB-Kodierung.
  Die Portierung hat hier nichts verändert; so bleibt es.
- **Raumaufbau:** Himmel im Unendlichen, Hügelring um die Kamera, Nebel ab 170 m. Auf Geraden wandert der Kirchturm nur etwa 1°/s, während Fassaden in 5 m Abstand mit etwa 94°/s vorbeiziehen. Genau diese Parallaxe ist gewünscht.
- **Das erste Bild der Fahrt:** der exakte Scan im Rahmen, ringsum fortgesetzt durch die 3D-Welt. Es ist das stärkste Motiv der Datei.
- **Fahrmechanik:** Das Rad rollt von selbst mit 8,2 m/s, gut für eine ruhige Fahrt. Dazu die geglättete Lenkung, die bildratenunabhängige Dämpfung `st=(e,t,i,a)=>e+(t-e)*(1-Math.exp(-i*a))` und die Kamerakollision.
- **Der Gemäldebetrachter:** echte Kachelpyramide bis 22 284 px, nahtlos, progressiv geladen, echtes Pinselrelief bei 1:1.
- **Der Offline-Aufbau:**
  - CSP `default-src 'none'`, null Netzanfragen,
  - Startseite in 1,3 s,
  - Speicher-Ersatz ohne `localStorage`,
  - 2D-Ersatzfahrt ohne WebGL 2.
- **Die Startseite:** Typografie (Instrument Serif, DM Sans), deutsche Texte, zurückhaltendes Gold, Rücksicht auf `prefers-reduced-motion`.
- **Das Klavierstück:**
  - −19,2 LUFS, Lautheitsumfang 4,5 LU,
  - natürliche Atempause an der Schleifenstelle, ohne Klick,
  - D-Dur, passend zur D-Dur-Pentatonik der Fahrgeräusche,
  - saubere Lizenzdokumentation.
  - `<audio loop>` genügt.
- **Touch:** Der Stick erscheint erst beim Berühren. Die Fahransicht im Handy-Querformat ist bereits aufgeräumt.

---

## B. Befunde mit Umsetzungsvorschlägen

### Übersicht

| Kennung | Priorität | Befund | Aufwand | Code |
|---|---|---|---|---|
| E1 | P1 | Einstieg gehetzt, Maßstabssprung, Abbruch bei jeder Berührung | klein | C1 (getestet) |
| UI1 | P1 | Spieloberfläche verdeckt 24,7 % des Bildes, gemischte Sprache | klein | C1 (Kern getestet) |
| AU1 | P1 | Klavier reißt bei jedem Ansichtswechsel ab, startet erst mit der ersten Eingabe | mittel | C2 (isoliert getestet) |
| AU2 | P2 | Ein- und Ausblenden zu kurz, Regler linear | klein | C2 |
| AU3 | – | Klang-Messwerte: bewahren | – | – |
| AU4 | P2 (bei Weitergabe P1) | Klavier-Nennung unsichtbar, Textdateien falsch kodiert | klein | C5 |
| R0 | – | Kamera- und Fahrkennwerte (Referenzwerte) | – | – |
| R1 | P1 | Kurve: Fernraum schwimmt nach, Horizont kippt | klein | C6 (simuliert) |
| R2 | P1 | Sichtfeld atmet mit W; W heißt „fahren“, obwohl das Rad von selbst rollt | klein | C6 (simuliert) |
| R3 | P1 | Anhalten geht nur mit gehaltener Bremse | klein | C6 (Entwurf, Syntax geprüft) |
| R4 | P2 | Touch-Stick: große Totzone, steile Kennlinie | klein | C6 (Entwurf, Syntax geprüft) |
| R5 | P3 | Tastatur-Lenkung rastet zurück und wirkt binär | klein | Beschreibung |
| R6 | P3 | Ohne WebGL 2: Ersatz funktioniert, aber ohne Hinweis | klein | Beschreibung |
| BQ0 | – | Diagnose „weich, flach, unruhig“ | – | – |
| BQ1 | P1 | Renderauflösung unter nativ, Untergrenze 55 % | klein | C3 (Entwurf, Syntax geprüft) |
| BQ2 | P1 | keinerlei Kantenglättung | mittel | C3 (Entwurf) |
| BQ3 | P1 | Dachtupfer flirren bei streifendem Blick | klein | C3 (Entwurf) |
| BQ4 | P2 | Himmel „atmet“, Gelb flackert | mittel | C3 (Entwurf) |
| BQ5 | P2 | Texturwahl: 4096 px ohne Nutzen, Tablets nur 1800 px | klein | Beschreibung |
| GB1 | P3 | Betrachter: Maximalzoom überzieht, Zeichenfläche auf DPR 2 begrenzt | klein | C4 (Entwurf, Syntax geprüft) |
| UI2 | P3 | Startseite: Dorf abgeschnitten, Kontrast oben rechts knapp; Handy-Layout | klein | Beschreibung |
| OF1 | P1 (Klärung), sonst P3 | iPhone-Start unklar; Speicherspitzen | Test, klein | Beschreibung |
| L1 | P0 bei Weitergabe | Engine und fünf Texturen stammen von starrynightrider.com, ohne Lizenz | Klärung | – |

---

### B0 · Erlebnisvergleich mit der Referenz

**Grundlage des Vergleichs**
- Eine interaktive Live-Fahrt auf starrynightrider.com fand nicht statt; der Prüflauf wurde vom Auftraggeber abgebrochen. Sein Hinweis lautete „Meine Fassung ist identisch“, und der Code bestätigt das für die Engine.
- Verglichen wurden deshalb das Referenz-Bundle und die Referenz-Texturen (heruntergeladen am 24.11.2026) mit Ihrer Fassung (eigene Fahrt, Screenshots, virtuelle Uhr).
- Alle Aussagen über das Laufzeitverhalten der Referenz sind aus ihrem Code abgeleitet.

| Moment | Referenz | Ihre Fassung | Grundlage |
|---|---|---|---|
| Erste Sekunden | Gemälde gerahmt an einer Galeriewand mit Schild; „Step into the painting“; 8,35 s Kamerafahrt; Scan aufgelöst nach 1,27 s; Spitzentempo der Kamera 14,6 m/s | eigene Startseite, dann bildschirmfüllendes Titelbild als Ladebildschirm; nach 0,22 s Sprung auf den kleinen gerahmten Scan; Scan nach 0,42 s weg; Fahrt nach 2,70 s; Kamera bis 45 m/s; jeder Tipp bricht ab | Referenz: [Code]; Ihre Fassung: [Beobachtet] und [Gemessen] |
| Gasse, Kurve, Raum | Kamera, Physik und Szenenparameter | identisch (siehe R0) | [Code], diff-geprüft |
| Himmel und Zypresse | 2 400-px-Scan auf Desktop **und** Tablet; 1 800 px nur auf kleinen Touch-Bildschirmen | 4 096 px auf Desktop (in der Fahrt unsichtbar, +93 MB Grafikspeicher, andere Strichfärbung); 1 800 px auf **allen** Touch-Geräten, auch Tablets | [Code] und [Gemessen] |
| Flüssigkeit | Pixelbudget- und Qualitätsregelung | gleich; durch +93 MB Texturen auf schwachen GPUs eher früheres Herunterregeln | [Code]; Bildrate [Nicht geprüft] |
| Klang | geschwindigkeitsabhängiges, gefiltertes Rauschen (Fahrtwind, Rollen), Töne beim Einsammeln, Stimmen der Dorfbewohner (englische Sprachclips), keine Hintergrundmusik | eigenes Klavierstück; Fahrgeräusche standardmäßig aus; keine Stimmen | [Code]; Hören [Nicht geprüft] |
| Oberfläche | konsequent englisch; Mehrspieler und Bestenliste online | deutsch mit englischen Resten; funktionslose Mehrspieler-Reste; lokale Liste „Meine Fahrten“ | [Beobachtet] und [Code] |
| Gemälde betrachten | kein Zoom-Betrachter im Bundle gefunden | eigener Betrachter mit 22-k-Kachelpyramide und echtem Pinselrelief | [Beobachtet] |
| Offline | nein (Server, Analytics, Mehrspieler) | ja: eine Datei, null Netzanfragen | [Gemessen] |

**Wo die Referenz heute überzeugender ist**
- Vor allem in den ersten Sekunden: Der gerahmte Scan steht lang genug, um ihn als Gemälde zu erkennen, bevor er zur Welt wird.
- Bei Tablets, durch die schärfere Himmelstextur.

**Wo Ihre Fassung schon weiter ist:** Gemäldebetrachter, Offline-Betrieb, Musik, deutsche Startseite.

---

### E1 · Der Einstieg ist gehetzt und bricht bei jeder Berührung ab · Priorität P1

**Reproduktion:** Startseite → „Fahrt beginnen“. Am Handy in den ersten Sekunden aufs Bild tippen; am Desktop W oder eine Pfeiltaste drücken.

**Beobachtung und Belege**
- **[Code]** Die Referenz zeigt das Gemälde zuerst gerahmt an einer Galeriewand. Nach „Step into the painting“ löst sich der Scan in einer Kamerafahrt von 8,35 s in die lebendige Szene auf; Im Referenz-Bundle lautet die Stelle `Z.diveT+=i?-e*fd:e`; in `original.mjs` entspricht ihr der Zweig `else ie.diveT+=e`.
- **[Code]** Die Portierung fügt für `?fahrt=1` diesen Zweig ein:
  ```js
  else if(ie.directEntry){let x=ie.reduceMotion||jt.any||jt.throttle||jt.brake||jt.jumpPressed||Math.abs(jt.steer)>.1;ie.diveT=x?8.35:ie.diveT+8.35/2.7*Math.min(t,.45),…}
  ```
  Damit läuft die Kamerafahrt 3,1-mal schneller als in der Referenz.
- **[Code]** Wann die Eingabe den Einstieg beendet:
  - `jt.any` wird bei jeder Berührung gesetzt (`touchstart … i.any=!0`) und bei jeder Taste (`t.add(z.code),i.any=!0`).
  - Ausgenommen ist M: Der Klavier-Handler stoppt die Weitergabe mit `e.stopImmediatePropagation(),Ep.toggle()`.
  - Die erste echte Eingabe startet zusätzlich das Klavier (Ausweichlösung `function _(b){…f(!0)}`). Musikbeginn und Abbruch des Einstiegs fallen also zusammen.
- **[Code]** Der Ladebildschirm `#loading` zeigt in der Portierung das Titelbild bildschirmfüllend und beschnitten: `url('@@FILE:ride/art/cover.webp@@') center center/cover`, Überblendung `transition:opacity .22s`. Danach erscheint der Scan klein und gerahmt (602 × 477 px bei 1280 × 720). Das ist ein Maßstabssprung, laut Fahrgefühl-Teilprüfung nach etwa 0,2 s.
- **[Gemessen]** Zeitpunkte des Einstiegs, mit der exakten `Ms`-Funktion des Bundles (smootherstep) bei 60 fps nachgerechnet:

| | Scan aufgelöst | Szene lebendig | Dorf enthüllt | Fahrt beginnt |
|---|---|---|---|---|
| Referenz | 1,27 s | 2,17 s | 4,18 s | 8,37 s |
| Portierung heute | **0,42 s** | 0,70 s | 1,35 s | **2,70 s** |
| Vorschlag (Halt 1,0 s, dann 6,5 s) | 2,00 s | 2,70 s | 4,27 s | 7,52 s |

- **[Gemessen]** Isolierter Test mit virtueller Uhr, ohne Änderung:
  - Ein Tipp aufs Display nach 0,5 s beendet den Einstieg bei 0,50 s.
  - W nach 1,0 s beendet ihn bei 1,00 s.
- **[Beobachtet]** Die Bildfolge des Einstiegs, Simulationszeit (die Bilder sind stark):

| Zeit | Bild |
|---|---|
| 0 s | gerahmter Scan, ringsum die 3D-Welt |
| 0,67 s | 3D-Zypresse vor dem Dorf |
| 1,33 s | Flug über die Dächer auf den Kirchturm zu |
| 2,0 s | Absenken in die Gasse |
| 2,67 s | Verfolgerkamera hinter dem Fahrer |

**Eingriff**
- `bundles/original.mjs`: Suchtexte `else if(ie.directEntry)`, `document.documentElement.classList.add("direct-entry"),Mo())` und `document.documentElement.classList.remove("direct-entry")),Q2())`.
- `vorlage/index.html`: CSS-Regel `#loading {` mit `center center/cover` sowie `<html lang="de">`.

**Änderung**
1. Der Direkteinstieg dauert 6,5 s statt 2,7 s; davor steht rund 1 s Halt auf dem gerahmten Scan. Code in C1.
2. Überspringen geht nur mit Fahren oder Bremsen (W/S, Pfeil hoch/runter, Stick hoch/runter) und erst nach 3,05 s, wenn der Scan aufgelöst ist. Mit `prefers-reduced-motion` beginnt die Fahrt weiterhin sofort.
3. `Math.min(t,.45)` wird zu `Math.min(t,.1)`, damit ein Ruckler nicht 1,4 s Kamerafahrt überspringt.
4. Kein Maßstabssprung:
   - In `#loading` das Titelbild entfernen und nur einen Verlauf lassen: `background:linear-gradient(0deg,#0b1424cc,transparent 40%)`.
   - `<html lang="de" class="direct-entry">` statisch setzen, damit Schild und Handle (`.direct-entry #plaque,.direct-entry #handle`) von Anfang an verborgen sind.
   - Dann sieht man schon während „Das Dorf entsteht …“ den gerahmten Scan. Die Fahrgefühl-Teilprüfung hat das visuell bestätigt: Der Rahmen mit dem Gemälde ist bei 1,1 s unter dem Ladehinweis sichtbar.
   - Die Überblendung von `#loading` auf `.9s` verlängern (statt `.22s`).
5. Das Klavier beginnt mit dem Klick auf „Fahrt beginnen“ und blendet über 4 s ein (AU1).

**Abnahme**
- Vom Klick bis zur Kontrolle über das Rad vergehen etwa 7,5 s plus Ladezeit.
- Das Gemälde ist ohne Maßstabssprung durchgehend zu sehen.
- Ein Tipp aufs Display oder Lenken während des Flugs bricht nichts ab; W oder S überspringen erst ab etwa 3 s.
- Mit `prefers-reduced-motion: reduce` beginnt die Fahrt sofort.

**Stärkstes Gegenargument:** Wer zum zehnten Mal fährt, will nicht jedes Mal 7,5 s warten. Die Fahrgefühl-Teilprüfung hielt das Überspringen per beliebiger Eingabe deshalb für erhaltenswert.

**Abwägung:** Die beliebige Eingabe zerstört den Einstieg gerade für den ersten, wichtigsten Eindruck, und am Handy zerstört ihn schon die erste Berührung. Ausgleich:
- W oder S überspringen ab 3 s.
- Optional zählt ein Merker in `Night.storage("localStorage")` die Fahrten; ab der dritten Fahrt verkürzt sich der Flug auf 4 s.

---

### UI1 · Die Spieloberfläche verdeckt das Bild, die Sprache ist gemischt · Priorität P1

**Reproduktion:** Desktop 1280 × 720, „Fahrt beginnen“, die ersten 16 Sekunden der Fahrt.

**Beobachtung und Belege**
- **[Gemessen]** Rund 10 s nach Fahrtbeginn bedecken die Anzeigen (HUD) **24,7 % der Bildfläche** (Vereinigung der Element-Rechtecke). Darin enthalten ist der Kartencontainer `#map` (296 × 417 px), der auch die leere Fläche zwischen Liste und Minikarte umfasst; ohne ihn sind es rund 22 %. Nach 4 s waren es 10,5 %.

| Element | Größe | Lage |
|---|---|---|
| Liste „Highlights“ (`#map-list`) | 296 × 197 px | links |
| Minikarte (`#map-disc`) | 208 × 208 px | links unten |
| Auftragskasten (`#brief`) | 520 × 95 px | mitten im Himmel |
| Tastenhilfe (`#hint`) | 271 × 142 px | unten mittig |
| Zielanzeige (`#guide`) | 197 × 38 px | oben |
| Knopfleiste (`#corner`) 130 × 38 px und „Menü“ 71 × 42 px | – | oben rechts |

- **[Gemessen]** Am Handy (844 × 390) liegt der Auftragskasten (480 × 77 px) über dem Kirchturm.
- **[Code]** `function Q2()` zeigt den Auftrag 11 s lang (`ot.briefT=11`) und die Tastenhilfe 16 s lang (`setTimeout(()=>{Re("hint").classList.add("faded")},16e3)`). Liste und Minikarte bleiben dauerhaft.
- **[Code]** Der Modus „Bild pur“ existiert bereits und ist gut gebaut: `function yp(e)` blendet das HUD **und** die Spielobjekte aus (`bt.mesh.visible=fc.group.visible=mc.group.visible=!e`). Er lässt 0,3 % (Desktop) bzw. etwa 3 % (Touch) der Fläche bedeckt.
- **[Beobachtet]** Englische Reste:
  - in der Liste: „Highlights“, „Tab next“, „DESTINATION“, „VIEWPOINT“, „NIGHT CAFÉ“, „GALLERY“, „The Painter’s Knoll“;
  - im Code außerdem: `"rider in the painting"`, `title="Say something to the riders nearby (Enter)"`, der Platzhalter des Chats und `talk` in `vorlage/index.html`.
- **[Code]** Die Mehrspieler-Reste (`#chat`, `#btn-chat`, `#riders`) sind offline ohne Funktion.

**Eingriff**
- `bundles/original.mjs`: `function Q2()`, `function yp(e)` sowie `function E2(` mit `<span>Highlights</span>`; die Ortsbezeichnungen `label:"viewpoint"` und `label:"night caf\xE9"`.
- `vorlage/index.html`: Chat- und Mitfahrer-Elemente.

**Änderung**
1. Wer über „Fahrt beginnen“ kommt, fährt im ruhigen Modus. Am Ende des Einstiegs wird nach `Q2()` zusätzlich `yp(!0)` aufgerufen; Code in C1, getestet. Der vorhandene Hinweis „Bild pur · Mit T blenden Sie die Anzeigen ein.“ erklärt den Weg zurück.
2. „Lichter sammeln“ wird ein bewusster Menüpunkt in `#ride-menu`, der `yp(!1)` und `Q2()` aufruft. Die Wahl wird in `Night.storage("localStorage")` gespeichert.
3. Im ruhigen Modus bleibt höchstens eine dezente Zielanzeige „Kirchturm · 95 m“ (`#guide`, 60 % Deckkraft). Sie blendet nach 6 s ohne Richtungswechsel aus.
4. Alle sichtbaren Texte stehen auf Deutsch:

| Englisch | Deutsch |
|---|---|
| Highlights | Orte |
| Tab next | Tab · weiter |
| destination | Ziel |
| viewpoint | Aussicht |
| night café | Nachtcafé |
| gallery | Galerie |
| The Painter’s Knoll | Malerhügel |

5. Chat, Sprechtipp „talk“ und die Mitfahrer-Anzeige entfernen oder per CSS verbergen.
6. In „Bild pur“ bleiben ☾ und ♪ gedämpft sichtbar. Heute verbirgt `#hud.theater #corner button:not(#btn-theater){opacity:0;pointer-events:none}` den ♪-Knopf, und am Desktop ist `#hud.theater #corner` ganz ausgeblendet. Neu in `vorlage/index.html`:
   ```css
   #hud.theater #corner{opacity:1}
   #hud.theater #corner button:not(#btn-theater):not([data-piano-toggle]){opacity:0;pointer-events:none}
   #hud.theater #corner #btn-theater,#hud.theater #corner [data-piano-toggle]{opacity:.55}
   ```
   Am Touch-Gerät führt das Antippen des Mondes (☾) zurück, nicht T; so steht es auch im vorhandenen Hinweis „Tippen Sie auf den Mond“.

**Abnahme**
- In den ersten 20 s der Fahrt sind nur „Menü“ sowie, gedämpft, ☾ und ♪ zu sehen (nach Punkt 6).
- T (am Touch-Gerät ☾) blendet alles ein.
- In der Oberfläche steht kein englisches Wort mehr.
- Im Spielmodus funktionieren Zähler und Abgabe wie bisher.

**Nebenwirkung:** Ohne Auftragstext fehlt Erstnutzern die Orientierung. Deshalb bleibt die Zielanzeige (Punkt 3), und der Menütext wird auf „Fahren Sie zum Kirchturm.“ gekürzt (heute: „Sammeln Sie Lichter und fahren Sie zum Kirchturm, um sie abzugeben.“).

---

### AU1 · Jeder Ansichtswechsel schneidet das Klavier ab; in der Fahrt kommt es erst mit der ersten Eingabe · Priorität P1

**Reproduktion:** Auf der Startseite „Piano aus“ anklicken (die Beschriftung wechselt zu „Piano an“), dann „Fahrt beginnen“. Später im Menü „Zur Startseite“ wählen und erneut „Fahrt beginnen“.

**Beobachtung und Belege**
- **[Code]** Der Router entfernt bei jedem Wechsel das ganze `iframe` (Suchtext im äußeren Loader `if(frame){frame.remove();frame=null;}`). Damit verschwindet auch das `<audio id="piano">` der alten Seite.
- **[Code]** Jede Seite hat eine eigene Kopie der Klavier-Engine:
  - `home.mjs`, erkennbar an `p.setVolume(o/100)`,
  - `original.mjs`, erkennbar an `p.setVolume(n/100)`,
  - `journey.mjs`, erkennbar an `V.ambient.setMuted(!0),V.soundWanted=!1`.
  Die beiden `setVolume`-Texte kommen je viermal vor. Eindeutige Anker sind `function bc(` in `original.mjs` und `function ff(` in `home.mjs`.
- **[Gemessen, Audio-Teilprüfung, Chromium]**
  - Der letzte hörbare Block liegt 13 ms vor `pagehide`; die Musik bricht ohne Ausblendung ab.
  - Mit der Standard-Autoplay-Regel bleibt die Fahrt stumm, bis man darin zum ersten Mal klickt oder eine Taste drückt. Dann setzt das Klavier mitten in der Phrase an der gespeicherten Stelle ein.
- **[Gemessen, eigener Lauf]** Mit frischem Profil war das Klavier 3 s nach dem Verschwinden des Ladehinweises noch pausiert (`paused:true, currentTime:0`).
- **[Code]** Die Startseite startet das Klavier nie von selbst (`c=s&&a&&o>0` in `home.mjs`). Die Gemäldereise beginnt immer stumm.

**Eingriff:** äußerer Loader (`window.Nachtfahrt={`), die drei Klavier-Engines und das Element `<audio id="piano"` (mit `src="@@FILE:kunst/audio/sternennacht-piano.mp3@@"`) in `index.html` und `vorlage/index.html`.

**Änderung**
1. Ein einziger Klavierspieler im äußeren Dokument, das alle Ansichtswechsel überdauert:
   - ein `Audio`-Element mit `loop`,
   - ein `AudioContext` mit `GainNode` über `createMediaElementSource`,
   - ein MP3-Blob, einmal erzeugt und nie widerrufen.
2. Die Schnittstelle heißt `Nachtfahrt.piano` mit `play()`, `pause()`, `setEnabled()`, `setVolume()`, `state`, `subscribe()` und `unlock()`. Die drei Bundles benutzen sie, statt eigene Engines zu bauen. Code und Anbindung in C2.
3. Der Klick auf „Fahrt beginnen“ ist die Benutzergeste. Er startet das Klavier mit 4 s Einblendung, synchron zum Halt vor dem Flug ins Bild (E1).
   - Standard ist „an“. Wer ausdrücklich abschaltet (M oder ♪), behält diese Wahl gespeichert.
   - Die Startseite zeigt daher „Piano an“, solange nichts anderes gespeichert ist.
4. Die `<audio>`-Elemente der inneren Seiten entfallen. Die Vorschau-MP3 wird erst bei „Piano anhören“ dekodiert.

**Abnahme**
- Ablauf start → fahrt → start → reise → fahrt: Das Klavier pausiert nie, die Wiedergabeposition steigt stetig, außerhalb der Schleifenstelle gibt es keine Lücke über 50 ms.
- Hörtest: Nach dem Klick setzt das Klavier binnen 1 s leise ein und ist nach etwa 4 s voll da. Die erste Taste in der Fahrt ändert am Klang nichts.

**Nebenwirkung und Gerätevorbehalt**
- Die Musik läuft auch auf der Startseite weiter. Das ist gewollt; der ♪-Knopf bleibt sichtbar.
- Rund 6,2 MB bleiben im Speicher.
- **[Nicht geprüft]** Ob unter iOS ein Tipp im `iframe` den Audiokontext des äußeren Dokuments freischaltet, braucht ein echtes Gerät. Vorsorge: Bei jedem `pointerdown`/`touchstart` in der Fahrt `parent.Nachtfahrt.piano.unlock()` aufrufen.

---

### AU2 · Ein- und Ausblenden zu kurz, Regler linear · Priorität P2

**Reproduktion:** In der Fahrt M drücken (aus), 3 s warten, erneut M (an). Im Fahrtmenü den Regler „Pianolautstärke“ von 100 % auf 50 % ziehen.

**Belege [Code + Gemessen]**
- **Einblenden:** `gain.setTargetAtTime(…,.075)` (in `home.mjs`: `l.gain.setTargetAtTime(ve,le,.075)`) erreicht 95 % in 225 ms. Gemessen stieg der Pegel beim Wiedereinschalten in rund 200 ms von −41 auf −28 dBFS.
- **Ausblenden:** Die Pause kommt 220 ms nach Beginn des Ausblendens, bei noch −26 dB Verstärkung. Gemessen fällt der Pegel von −53 dBFS schlagartig auf Stille. Ob das hörbar ist, ist **[Nicht geprüft]**.
- **Regler:** `setVolume(o/100)` arbeitet linear, die obere Reglerhälfte deckt nur 6 dB ab. Die Gemäldereise nutzt eine andere Skala (0–1, Standard 0,35) und speichert sie nicht.

**Änderung (Startwerte, in C2 umgesetzt)**
- **Einblenden** linear:

| Anlass | Dauer |
|---|---|
| Fortsetzen mitten im Stück | 1,2 s |
| Fahrtbeginn | 4 s |
| Stückanfang | 0,3 s |

- **Ausblenden** linear auf 0 in 0,5 s, Pause bei 560 ms.
- **Regler:** quadratisch, `(v/100)**2`. Neuer Standard 74 %; das ergibt 0,548, also die heutige Lautstärke.
- **Speicher:** Schlüssel `nachtfahrt-piano-v2`; Altwerte werden mit `Math.round(Math.sqrt(v1/100)*100)` übernommen.

**Abnahme**
- 300 ms nach dem Einschalten liegt die Verstärkung bei höchstens 40 % des Ziels, nach 1,2 s am Ziel.
- Im Moment der Pause ist die Verstärkung ≤ 0,001.
- Jede Halbierung des Reglers senkt den Pegel um etwa 12 dB.

---

### AU3 · Klang-Messwerte: bewahren

Ich konnte die Musik nicht hören; alle Werte sind gemessen.

**Datei**

| Größe | Wert |
|---|---|
| Länge | 257,5 s |
| Format | MP3 192 kbit/s, 44,1 kHz, Stereo |
| Integrierte Lautheit | −19,2 LUFS |
| Lautheitsumfang | 4,5 LU |
| Spitzenpegel | −4,0 dBTP |
| Effektiver Pegel bei Standardeinstellung 55 % | rund −24,4 LUFS |

Die letzten rund 45 s sind leiser (integriert −21,2 LUFS; kurzzeitig −19 bis −27 LUFS).

**Schleifenstelle**
- Der Schlussakkord klingt ab 253 s aus, dann folgt rund 1 s Atempause, danach beginnt das Stück neu.
- Die letzten rund 0,3 s sind exakt Null; der Anfang bleibt rund 36–38 ms unter −90 dBFS. Der Sprung beim Umbruch ist 0, es gibt also keinen Klick.
- `<audio loop>` fügt in Chromium nur 81–104 ms zur vorhandenen Pause hinzu. **Beibehalten**; ein dekodierter Puffer (rund 91 MB) bringt keinen hörbaren Gewinn.
- Encoder-Verzögerung und Auffüllung sind hier unerheblich.

**Tonart:** geschätzt D-Dur. Die Fahrgeräusche der Engine nutzen D-Dur-Pentatonik (`Nn=[62,64,66,69,71,74,76,78,81,83,86]`); beides passt harmonisch.

**[Nicht geprüft]** Ob das Stück auf Dauer ruhig trägt und wie laut die Fahrgeräusche neben dem Klavier wirken, entscheidet nur das Hören.

---

### AU4 · Namensnennung des Klaviers unsichtbar, Textdateien falsch kodiert · Priorität P2 (bei Weitergabe P1)

**Reproduktion:** Werkinfo (Startseite) und Fahrtmenü nach einer Nennung der Klavieraufnahme durchsuchen. In der Gemäldereise den Link auf die Hinweisdatei öffnen und Gedankenstriche und Anführungszeichen prüfen.

**Belege**
- **[Code]** Die CC-BY-Nennung erscheint nur in der Gemäldereise. Startseite und Fahrt spielen ohne Nennung.
- **[Gemessen]** Die eingebetteten Textdateien haben `data-type="text/plain"` ohne `charset`. Ein geöffneter Tab zeigt „â€““ statt „–“.
- **[Code]** Die Lizenztexte der Schriften (`ride/fonts/*-LICENSE.txt`) sind eingebettet, aber nirgends verlinkt.

**Änderung**
1. In `asset()` Textdateien als `text/plain;charset=utf-8` ausliefern; Code in C5.
2. Eine Zeile in `#info` (Werkinfo) und im Fahrtmenü: „Klavier: eigene Komposition, gespielt mit ‚Salamander Grand Piano V3‘ von Alexander Holm, CC BY 3.0“. Dazu Links auf `PIANO-NOTICE.txt` und `PIANO-LICENSE.txt` sowie auf die Schriftlizenzen.

**Abnahme:** Die Nennung ist sichtbar, und die Hinweisdatei öffnet sich mit korrekten Gedankenstrichen und Anführungszeichen.

---

### R0 · Kamera- und Fahrkennwerte (Referenzwerte für alle R-Befunde)

**[Code]** Gegen die Referenz diff-geprüft: Kameraklasse, Fahrphysik, Eingabe und Shader sind unverändert.

| Größe | Wert | Suchtext in `original.mjs` |
|---|---|---|
| Nah- und Fernebene | 0,25 / 2 600 m | `Rt=new da(60,1,.25,2600)` |
| Vertikales Sichtfeld | 72° in Ruhe, 73,5° bei 8,2 m/s, 83,9° bei 15,5 m/s | `this.fov=72+12*r+5*this.air+15*n` |
| Horizontales Sichtfeld bei 1280 × 720 | 104,5° / 106° / 116° | berechnet |
| Horizontales Sichtfeld bei 844 × 390 | 115° / 116,5° / 125,6° | berechnet |
| Drehpunkt | Rad + 1,35 m | `f=t.y+1.35` |
| Auslegerlänge | 4,9 m in Ruhe | `h=(4.9+1.3*r+5.5*this.air-1.7*n)*l*(t.walking?.74:1)` |
| Augenhöhe | 2,45 m über Grund | `d=((t.onRoof?3:2.45)+2.6*this.air-.32*n)*(.45+.55*l)` |
| Vorausblick | 3,6 m | `let T=3.6,k=t.gx+Math.cos(a)*T` |
| Gierfolge | lineare Annäherung, 3,4/s | `this.yaw+=lo(this.yaw,t.heading)*Math.min(1,3.4*e)` |
| Rollen | Neigung × 0,18, bis 5,6° | `this.roll=st(this.roll,-t.lean*(.18+.4*n)*(t.grounded?1:.3),5,e)` |
| Weltkrümmung | 3·10⁻⁴, um die Kamera, wölbt nach oben | `bend:3e-4` |
| Nebel | ab 170 m, voll bei 432 m | – |
| Hügelring | 440/466 m um die Kamera | – |
| Tempo | Rollen 8,2 m/s ohne Eingabe; W 15,5 m/s; S bremst auf 0 | `Yn=8.2,$x=15.5` |
| Beschleunigung | 5,2 m/s² | – |
| Bremsen | 11 m/s² | – |
| Wenderate | 2,2 rad/s bei 8,2 m/s; 1,46 rad/s bei 15,5 m/s | `(2.5-1.05*ga(4,16,a))` |
| Lenkglättung | 7/s beim Loslassen, 5,5/s beim Lenken | `this.steer=st(this.steer,i,i===0?7:5.5,e)` |
| Einrasten auf die Straßenachse | unter ±0,5 rad | `Math.abs(k)<.5&&(this.heading+=k*Math.min(1,1.6*e))` |
| Simulationsschritt | variabel, höchstens 50 ms; unter 20 fps läuft die Simulation in Zeitlupe | `gc(Math.min(t,.05),t)` |

**[Gemessen, Fahrgefühl-Teilprüfung]**
- Die Weltkrümmung hebt den Kirchturm beim Start um 2,6 m (1,6°), bei 30 m Abstand nur noch um 0,3 m. Das Absinken bei der Annäherung ist kleiner als 1,1° und damit nicht wahrnehmbar.
- Die Hügel stehen konstant +58/+65 m hoch.
- Die Kamerakollision griff an drei getesteten Ecken nie.
- Der Fernraum ist richtig aufgebaut.

---

### R1 · In der Kurve schwimmt der Fernraum nach, der Horizont kippt · Priorität P1

**Reproduktion**
- **Stelle:** der Kirchplatz, rund 60 m nach dem Start. Nach dem Einstieg nichts drücken; das Rad rollt etwa 7 s geradeaus, bis die Gasse auf den Platz mündet. Die helle Kirche mit dunklem Turm steht dann vorn rechts, am rechten Bildrand eine Zypresse.
- **Manöver:** D (Rechtskurve) 0,6 s halten. Die Kurve führt am Platzrand entlang in die nächste Gasse. Die rechte Hausecke zieht in 5,3–6 m Abstand vorbei; die Kirche wandert nach links und steht danach in rund 31 m Abstand links.

**Beobachtung [Gemessen, Fahrgefühl-Teilprüfung]** bei 8,2 m/s, Simulation mit 60 Hz:

| Größe | Wert |
|---|---|
| Drehung des Rads | 90° in 0,9 s, Spitze 140°/s |
| Nachlauf der Kamera | bis 32° |
| Spitzendrehrate der Kamera | 118°/s |
| Kirchturm am Bildschirm (1280 px) | wischt mit bis zu 1000 px/s |
| Kameradrehung, nachdem das Rad 90 % der Kurve hinter sich hat | 38° |
| Beruhigung | erst nach 1,8 s |
| Horizontneigung | 3,1° (beim Sprint 5,1°) |

Himmel, Hügel und Kirchturm schwingen also noch etwa 0,9 s nach, während das Rad schon geradeaus fährt. Die nahe Hausecke zieht mit etwa 90°/s vorbei, der Fernraum aber ebenso schnell. Damit fehlt genau der Kontrast zwischen Nah und Fern, den Sie suchen.

**Wie es wirken soll:** Der Fernraum dreht nur, solange das Rad dreht, und steht danach sofort. Der Horizont bleibt nahezu waagerecht. Die Bewegung liegt in den nahen Fassaden, Laternen und Bäumen.

**Eingriff:** `original.mjs`, Suchtexte `this.yaw+=lo(this.yaw,t.heading)*Math.min(1,3.4*e)` und `this.roll=st(this.roll,-t.lean*(.18+.4*n)*(t.grounded?1:.3),5,e)`.

**Änderung:** Code in C6.
- Gierfolge exponentiell mit 8/s: `(1-Math.exp(-8*e))` statt `Math.min(1,3.4*e)`.
- Rollanteil der Neigung von 0,18 auf 0,06.

**Simuliertes Ergebnis (Fahrgefühl-Teilprüfung, Variante „RP“)**

| Größe | heute | mit Änderung |
|---|---|---|
| Nachlauf | 32° | 16° |
| Nachdrehen nach der Kurve | 38° | 21° |
| Beruhigung | 1,8 s | 1,2 s |
| Horizontneigung | 3,1° | 1,0° |
| Spitzendrehrate des Fernraums | 118°/s | 137°/s, aber auf 0,68 s statt 0,87 s zusammengedrängt |

**Abnahme:** In derselben Kurve dreht die Kamera nach 90 % der Raddrehung höchstens noch 22° nach, und die Neigung bleibt bei höchstens 1,2°. Sichtprüfung: Nach dem Einlenken kommt der Kirchturm in unter 1,2 s zur Ruhe, die Hausecke rechts zieht deutlich vorbei.

**Nebenwirkungen**
- Man sieht das Rad in Kurven weniger von der Seite; die seitliche Drift sinkt von 4,3 auf etwa 2,2 m/s.
- Über k ≈ 10 erreichen kleine Richtungskorrekturen beim Entlanggleiten an Wänden die Kamera; deshalb 8, nicht mehr.
- **Gestalterische Ergänzung [Annahme, nicht getestet]:** eine Laterne oder ein kleiner Baum 2–4 m innen an den ersten Kreuzungsecken. So hätte das Auge in der Kurve ein nahes Objekt, das sichtbar vorbeizieht.

---

### R2 · Das Sichtfeld „atmet“ mit W; W heißt „fahren“, obwohl das Rad von selbst rollt · Priorität P1

**Reproduktion:** Nach dem Einstieg in der Gasse W 3 s halten und loslassen, dabei die Größe von Kirchturm und Hügeln beobachten.

**Belege**
- **[Code]** Das Rad rollt ohne Eingabe mit 8,2 m/s; W beschleunigt auf 15,5 m/s. Die Tastenhilfe nennt W trotzdem „fahren“ (`<kbd data-code="KeyW"><b>W</b><i>fahren</i></kbd>`), im Menü steht „Fahren und lenken“.
- **[Gemessen]** Mit W wächst das Sichtfeld von 73,5° auf 83,9°. Kirchturm, Hügel und Himmel schrumpfen bei jedem Druck um 17 % und wachsen beim Loslassen zurück.

**Änderung:** Code in C6.
- `this.fov=72+4*r+5*this.air+15*n`. [Berechnet] Bei 15,5 m/s ergibt das höchstens 76° statt 84°. Simuliert wurde die Änderung nur bei 8,2 m/s (72,3–72,5°).
- Beschriftung: W heißt „schneller“; im Menü „Lenken: A/D · schneller: W · bremsen: S“.
- **Optional [Annahme, nicht getestet]:** Auf sehr breiten Bildschirmen (Handy quer, Seitenverhältnis über 2) das horizontale Sichtfeld auf etwa 100° begrenzen. Heute sind es bis 126°, was an den Rändern stark verzerrt. Formel, nur für `aspect > 2`: `vfov = min(72°, 2·atan(tan(50°)/aspect))`. Ohne diese Bedingung griffe sie schon ab Seitenverhältnis 1,64 und verengte auch 16:9 (1280 × 720: 72° → 67,7°).

**Abnahme:** Zwischen 0 und 15,5 m/s ändert sich das Sichtfeld um höchstens 4°. Beim Drücken von W bleibt der Kirchturm gleich groß.

**Nebenwirkung:** Der Sprint wirkt weniger rasant. Die nahen Fassaden ziehen bei 15,5 m/s aber weiterhin mit etwa 180°/s quer vorbei.

---

### R3 · Anhalten geht nur mit gehaltener Bremse · Priorität P1

**Reproduktion:** In der Fahrt S halten bis zum Stillstand, dann loslassen. Danach mit der Maus (oder rechts mit dem Finger) den Blick zur Seite ziehen und loslassen.

**Belege**
- **[Code]** Die Bremse setzt das Zieltempo nur, solange sie gehalten wird (`t.brake>0&&!n&&(l=o?7:0)`). Nach dem Loslassen beschleunigt das Rad wieder auf 8,2 m/s.
- **[Code]** Ein mit Maus oder Finger verschobener Blick zentriert sich 1,2 s nach dem Loslassen von selbst (`z-i.lastLook>1.2&&(i.lookYaw*=.94`). Diese Glättung wirkt pro Bild statt pro Zeit: bei 144 Hz mit 0,11 s Zeitkonstante, bei 30 Hz mit 0,54 s.

**Folge:** In Ruhe auf die Pinselstriche zu schauen, verlangt einen festgehaltenen Finger. Am Handy liegt dieser Finger auf dem Bild.

**Änderung**
1. „Parken“: Bremsen bis unter 0,3 m/s rastet ein, bis W, Lenken oder Sprung kommt. Code-Entwurf in C6.
2. Solange geparkt ist, kehrt der Blick nicht zur Mitte zurück.
3. Die Zentrierung bildratenunabhängig machen. In `i.update=z=>{…}` ist `z` die absolute Zeit; daraus `let dt=z-(i.lastZ??z);i.lastZ=z;` bilden und für `lookYaw` und `lookPitch` `*=Math.exp(-3.7*dt)` statt `*=.94` verwenden. Das entspricht der heutigen Zeitkonstante bei 60 fps (0,27 s).
4. Optional [Annahme, nicht getestet]: Ab 0,6 s Stillstand das Pixelbudget verdoppeln (bis DPR 2). Im Stand zählt Schärfe, nicht Bildrate.

**Abnahme**
- Ein kurzes Halten von S bringt das Rad zum Stehen, und es bleibt stehen.
- Den Blick mit Maus oder Finger auf den Sternring richten: Er bleibt dort.
- W fährt wieder los.

---

### R4 · Touch-Stick: große Totzone, steile Kennlinie · Priorität P2

**Reproduktion:** Handy quer; die linke Bildhälfte berühren und den Daumen etwa um den halben Ringradius zur Seite ziehen.

**Belege [Gemessen bei 844 × 390 + Code]**
- **Stick:** Ring 116 px, Knopf 50 px, Weg 70 px. Er erscheint dort, wo der Daumen die linke Bildhälfte berührt.
- **Kennlinie:** `O=H<.22?0:(H-.22)/.78;_=Math.sign(F)*O**2.2`, also 22 % Totzone und Exponent 2,2:

| Auslenkung | Lenkwert |
|---|---|
| 42 % | 0,05 |
| 50 % | 0,105 |
| 71 % | 0,36 |

  Kleine Ausschläge macht das Einrasten auf die Straßenachse (R5) wieder rückgängig.
- **Gas und Bremse:** schalten bei ±40 % senkrechter Auslenkung (`x=B<-.4,S=B>.4`).
- **Weitere Bedienelemente:**
  - Sprungknopf 72 × 72 px rechts unten.
  - Rechte Bildhälfte: Umschauen; zwei Finger: Zoom.
  - Bei den ersten drei Fahrten zeigt ein Geist-Stick (116 × 116 px) 7 s lang die Bedienung.

**Änderung (Entwurf, ungetestet, Code in C6):** Totzone 0,12, Exponent 1,5.

**Abnahme:** Bei 50 % Auslenkung lenkt das Rad spürbar (Lenkwert ≥ 0,25), ohne bei 15 % zu zittern.

---

### R5 · Tastatur-Lenkung rastet zurück und wirkt binär · Priorität P3

**Reproduktion:** Am Desktop in einer geraden Gasse D kurz antippen (etwa 0,1 s, 0,25 s und 0,45 s) und die Fahrtrichtung beobachten.

**Beleg [Gemessen]**

| Tippdauer | Ergebnis |
|---|---|
| 0,10 s | 6,8° Drehung, rastet in 1,65 s zurück auf die Straßenachse |
| 0,25 s | 19°, rastet in 2,4 s zurück |
| 0,45 s | 52°, bleibt |

Das Fenster des Einrastens beträgt ±28,6° (0,5 rad).

**Änderung (ungetestet):**
- Fenster auf 0,25 rad verkleinern (`Math.abs(k)<.5` → `Math.abs(k)<.25`).
- 0,6 s nach jeder Lenkeingabe nicht einrasten. Dazu einen Zeitzähler in der Rad-Klasse führen, der bei `Math.abs(i)>.05` auf 0,6 gesetzt und mit `e` heruntergezählt wird.

**Abnahme:** Ein Tipp von 0,25 s ergibt eine bleibende, kleine Richtungsänderung.

---

### R6 · Ohne WebGL 2 funktioniert der Ersatz, aber ohne Hinweis · Priorität P3

**Reproduktion:** Chromium mit `--disable-webgl --disable-webgl2` starten, die Datei öffnen und „Fahrt beginnen“ wählen.

**Belege [Gemessen]**
- Der Renderer-Konstruktor wirft. Die Meldung „Dieser Browser kann WebGL 2 nicht starten.“ ist nie lesbar, weil `catch{al()}` sofort `Night.open("dorf",!0)` ruft.
- Die Dorffahrt fällt auf eine 2D-Canvas-Fahrt mit Fahrknöpfen zurück; Gesamtdauer etwa 4 s. Die Fläche passt sich von 1000 × 563 auf 520 × 293 px an.
- **[Code]** Bei `webglcontextlost` wechselt die Fahrt dauerhaft in die Dorffahrt, ohne neuen Versuch.

**Änderung:**
- Vor dem Wechsel einen Merker in `sessionStorage` setzen; die Dorffahrt zeigt dann 4 s lang „Vereinfachte Fahrt – dieses Gerät unterstützt die 3D-Fahrt nicht.“
- Bei Kontextverlust einmal `#fahrt` neu laden, erst beim zweiten Verlust auf die Dorffahrt wechseln.

**Abnahme:** Mit abgeschaltetem WebGL erscheint der Hinweis; ein einzelner Kontextverlust führt zurück in die 3D-Fahrt.

---

### BQ0 · Diagnose: Woran es beim Fahren weich, flach oder unruhig wirkt

**[Gemessen]** Die Portierung hat keinen Rendering-Code geändert, nur die Texturen. Alle 75 Shader-Texte der Portierung stimmen mit der Referenz überein: 63 byte-genau, die übrigen bis auf vom Minifier umbenannte Variablen. Die Portierung bündelt three.js mit; das Referenz-Hauptbundle enthält 73 Shader-Blöcke (siehe L1). Dasselbe gilt für Renderer-Einstellungen, Pixelbudget, Qualitätsregelung und Postprocessing. **Die Ausgangsbilder sind nicht der Engpass.**

| Was | Ursache | Beleg | Hilft mehr Quellauflösung? | Was stattdessen hilft |
|---|---|---|---|---|
| Dach weich und flirrend | Dächer nutzen das Gemälde gar nicht, sondern prozedurale Pinseltupfer. Deren Größe hängt nur von der Entfernung ab; bei streifendem Blick schrumpfen sie auf 1–3 px. Dazu fehlt jede Kantenglättung. | [Code] `float lod = log2(max(vDist / uStrokeDist, 0.5));`, `uStrokeDist:{value:7}`, `antialias:!1`; [Gemessen] harte Treppen an Traufen | nein | BQ2, BQ3 |
| Gesamtbild weich auf HiDPI-Bildschirmen | Das Pixelbudget begrenzt die Renderauflösung, der Browser skaliert hoch. Die Qualitätsregelung kann bis 55 % absenken. | [Code] `Math.sqrt((Di.touch?17e5:33e5)/(e*t))`, `ie.quality=Math.max(.55,ie.quality-.12)`; [Gemessen] siehe Tabelle unten | nein | BQ1 |
| Zypresse flach, Ränder hart | Relief aus der Maske, 17 m vor dem Malerauge (`cypressDepth:17`), laut Bild-Teilprüfung nur rund 2,35 m tief bei 9 × 15 m Baum. Von der Seite verschmiert die Projektion. Harter Alpha-Test. | [Code] `if (texture2D(tCypress, uvc).r < 0.5) discard;` | nur unter etwa 6 m Abstand sichtbar | BQ2 (Alpha-to-Coverage); optional die Flanken nach dem Blickwinkel abdunkeln |
| Sternring und Himmel „atmen“ | Zwei Strömungsproben werden im 5,9-s-Takt überblendet. Bei 50/50 bleiben 73 % der Feinstruktur, gemittelt gehen rund 19 % verloren, fleckweise phasenversetzt. Gelbtöne flackern um ±12 %. Trilineare Mipmaps behalten 60 % des Details. | [Code] `vec3 pc = mix(c1, c0, 1.0 - abs(2.0 * p0 - 1.0));`, `float flick = 0.88 + 0.12 * sin(uTime * 1.7`; [Gemessen] Simulationen der Bild-Teilprüfung | nein: schon 2 400 px liefern 2,8–4,5 Texel je Bildpunkt | BQ4 |

Gemessene Zeichenflächen zur Zeile „Gesamtbild weich“:

| Fenster (CSS-px) | DPR | Zeichenfläche | Hochskalierung |
|---|---|---|---|
| 1920 × 1080 | 2 | 2422 × 1362 | × 1,59 |
| 1180 × 820, Touch | 2 | 1564 × 1086 | × 1,51 |
| 1512 × 982 (MacBook, berechnet) | 2 | – | × 1,34 |

### BQ1 · Renderauflösung: Budget anheben, Untergrenze 0,7 · Priorität P1 (HiDPI-Desktop)

- **Reproduktion:** Laptop mit DPR 2, Fenster 1920 × 1080 CSS-px. Breite und Höhe von `#gl` mit `innerWidth × devicePixelRatio` vergleichen.
- **Eingriff:** `original.mjs`, Suchtexte `Math.sqrt((Di.touch?17e5:33e5)/(e*t))`, `ie.quality=Math.max(.55,ie.quality-.12)`, `ie.quality=Math.max(.55,ie.quality-.06)` und `i>.02&&ie.quality>.55?(`.
- **Änderung:** Desktop-Budget `33e5` → `52e5` Pixel; Untergrenze überall `.55` → `.7`. Touch bleibt bei `17e5`. Code in C3.
- **Abnahme:**
  - Bei DPR 2 und 1920 × 1080 CSS ist `#gl` mindestens 3040 × 1710 px groß.
  - `ie.quality` (vorübergehend protokollieren) fällt auf einem Mittelklasse-Laptop nach 60 s Fahrt nicht unter 0,82.
  - Im A/B-Ausschnitt einer Dachkante ist sie sichtbar schärfer.
- **Nebenwirkung:** Bei DPR 2 sind rund 58 % mehr Pixel zu schattieren. Auf schwachen GPUs regelt die Schleife wieder herunter, dann aber nicht unter 70 %. Bei Ruckeln das Budget auf `44e5` senken.

### BQ2 · Kantenglättung fehlt völlig · Priorität P1

- **Reproduktion:** In der Startgasse (1280 × 720) die Dachkanten rechts per Screenshot zweifach vergrößern. Beim langsamen Lenken die Traufen und Fensterrahmen beobachten.
- **Belege:**
  - [Code] Der Kontext entsteht mit `antialias:!1,alpha:!1,depth:!1`.
  - [Code] Das Szenenziel `this.sceneRT=new Qi(4,4,{type:Aa` hat keine `samples`; FXAA oder TAA gibt es nicht.
  - [Code] Die Konturlinien stammen aus einem Tiefenpuffer mit Nearest-Abtastung.
  - [Gemessen] Harte Treppenstufen an Traufen und Firsten.
  - [Annahme] Diese Kanten krabbeln beim Fahren.
- **Änderung (Entwurf, Code in C3):**
  - `samples:4` für `sceneRT` auf Nicht-Touch-Geräten. Die gebündelte three.js-Version (r187) löst unter WebGL2 Farbe und Tiefe auf; das Composite liest `tDepth` weiter.
  - Für die Zypresse Alpha-to-Coverage statt `discard`.
  - Ausweichlösung, falls die Tiefenauflösung Probleme macht: ein FXAA-Pass nach dem Composite-Durchgang (dem Pass, der Szene, Tiefe und Bloom zum Endbild zusammensetzt).
- **Abnahme:**
  - Eine Dachkante, zweifach vergrößert, zeigt keine Treppen.
  - Bei langsamem Schwenk krabbeln Traufen und Fensterrahmen nicht.
  - Im Differenzbild aufeinanderfolgender Bilder liegt entlang der Kanten deutlich weniger Energie.
- **Nebenwirkung:** mehr Füllrate und mehr Grafikspeicher. 4-fach-MSAA mit RGBA16F-Farbe und 32-bit-Tiefe kostet rund 48 Byte je Pixel. Das sind etwa 100 MB bei 1920 × 1080 (DPR 1), rund 158 MB beim heutigen Budget von 3,3 MP und rund 250 MB mit BQ1 (5,2 MP). Deshalb BQ2 vor BQ1 einführen und messen. MSAA ist FXAA vorzuziehen, weil FXAA feine Pinselstruktur weichzeichnet.

### BQ3 · Dachtupfer passen ihre Größe nicht an die Verkürzung an · Priorität P1

- **Reproduktion:** Durch die Startgasse rollen und die Dachflächen rechts beobachten, die man flach von unten sieht. Entlang der Traufe flimmert ein Band aus pixelkleinen Tupfern ([Gemessen] im Ausschnitt der Bild-Teilprüfung; die zeitliche Wirkung ist [Annahme]).
- **Eingriff:** Hausshader in `original.mjs`, Suchtext `float lod = log2(max(vDist / uStrokeDist, 0.5));`. Die Zeile `vec2 jx = dFdx(p), jy = dFdy(p);` steht heute wenige Zeilen später.
- **Änderung (Entwurf, Code in C3):** Die Tupfergröße richtet sich zusätzlich nach der Verkürzung, gemessen über die Bildschirmableitungen. Frontal gesehene Flächen bleiben unverändert; erst ab etwa 4,5-facher Verkürzung werden die Tupfer gröber statt pixelklein.
- **Abnahme:** Beim Geradeausfahren durch die Startgasse flimmern die Dächer rechts nicht mehr in einem Band entlang der Traufe. Frontale Wände sehen im A/B-Screenshot an derselben Stelle aus wie vorher.

### BQ4 · Himmel ruhiger und detailreicher, ohne neue Quelldaten · Priorität P2

- **Reproduktion:** Anhalten (S halten) und 10 s auf den Sternring neben dem Mond schauen.
- **Belege:** siehe BQ0. Zusätzlich `uScan:{value:.8}`: 20 % des gemalten Himmelsausschnitts sind eine prozedurale Strichschicht.
- **Änderung (Entwurf, Code in C3):**
  - varianzerhaltende Überblendung der beiden Strömungsproben,
  - LOD-Bias −0,5 für die Scan-Abfragen,
  - Flackern auf ±6 % halbieren,
  - optional `uScan` auf 0,9.
- **Abnahme:** Beim Anhalten 10 s auf den Sternring neben dem Mond schauen: Er pulsiert nicht mehr im Rhythmus von rund 3 s, und seine Striche bleiben gleich scharf.
- **Nebenwirkung:** Der Look entfernt sich minimal von der Referenz; er wirkt etwas „gemalter“ und weniger „fließend“.

### BQ5 · Texturwahl: 4096 px bringen in der Fahrt nichts, Tablets bekommen weniger · Priorität P2

- **Reproduktion:** Auf einem Tablet die Fahrt starten und in der Konsole des äußeren Dokuments `Nachtfahrt.asset` mit einer Protokollfunktion umhüllen. Geladen wird `vorlage/painting-s.webp`. Am Desktop wird `vorlage/painting.webp` (4 096 px) geladen.
- **Belege:**
  - [Code] Die Portierung lädt `mo(Di.touch||qa.capabilities.maxTextureSize<4096?"painting-s.webp":"painting.webp")`. Die Referenz lädt 1 800 px nur bei `bo.small=bo.touch&&Math.min(window.screen.width…)<820`, sonst 2 400 px.
  - [Gemessen] `vorlage/painting.webp` (4 096 px) enthält echtes zusätzliches Detail: Gegenüber einer hochskalierten Referenz hat es das 5,3–6,6-Fache an Hochfrequenzenergie in Kirchturm, Zypressenspitze, Stern und Mond, im Median das 7,2-Fache.
  - [Gemessen] `vorlage/sky.webp` hat echtes Detail nur im oberen Teil; der untere Teil und der Zypressenstreifen sind hochskaliert.
  - [Gemessen] In der Fahrt ist das Detail unsichtbar, sichtbar nur auf der Zypresse unter etwa 6 m Abstand.
  - [Berechnet] Kosten: rund 93 MB mehr Grafikspeicher und 9,8 MB mehr Base64.
  - [Code] Der feste Mip-Level in `textureLod(tPainting, clamp(mid, 0.012, 0.988), 3.5` liest bei 4 096 px die Strichfarben aus einer 362 px breiten Stufe statt aus 212 px wie in der Referenz; auf Tablets aus 159 px. Damit ändert sich die Färbung der prozeduralen Striche.
- **Änderung:**
  - Aus den eigenen 4 096-px-Bildern 2 400-px-Fassungen erzeugen (Lanczos) und als `painting.webp` und `sky.webp` für die Fahrt einbetten.
  - In beiden `mo(…)`-Aufrufen (Gemälde und Himmel) wie in der Referenz nach `Di.small` statt `Di.touch` wählen und den Texturgrößen-Schutz behalten: `mo(Di.small||qa.capabilities.maxTextureSize<4096?…)`. Die Referenz prüft nur `bo.small`.
  - Alternative: 4 096 behalten und den festen LOD auf `3.5 + log2(float(textureSize(tPainting,0).x)/2400.0)` setzen.
- **Abnahme:**
  - Ein iPad (1180 × 820) lädt die 2 400-px-Fassung.
  - Die Himmelsfarben des Titelbilds entsprechen im A/B-Vergleich der Referenz.
  - Der Grafikspeicher sinkt am Desktop um rund 90 MB.

---

### GB1 · Gemäldebetrachter: hervorragend, nur der Maximalzoom überzieht · Priorität P3

**Reproduktion:** Startseite → „Gemälde erkunden“ → mit + oder dem Mausrad auf die Kirchturmspitze zoomen, bis nichts mehr geht.

**Belege**
- **[Gemessen]** Drei echte Stufen: 5 571 × 4 412, 11 142 × 8 823 und 22 284 × 17 646 px.
  - Kacheln 1024 px, Raster nahtlos; die Gesamtansicht kommt zuerst aus `cover.webp`.
  - Stufe 1 ist JPEG mit Qualität ≈ 72, Stufe 2 JPEG mit ≈ 62, Stufe 3 WebP.
- **[Beobachtet]** Zoom auf Kirchturm und Mond: echtes Pinselrelief, sehr überzeugend.
- **[Gemessen]** Der Maximalzoom erlaubt 2,2 CSS-px je Bildpixel (`m=si(m,g,Math.max(g,2.2))`), bei DPR 2 also 4,4 Geräte-Pixel.
  - Die Laplace-Varianz fällt dort von 1147 (bei 1:1) auf 87.
  - Sichtbar werden 8-px-Kompressionsblöcke und Ringing.
- **[Code]** Eine Stufe wird bis zu 12 % hochskaliert genutzt (`S=yu.findIndex(R=>R.width*1.12>=H)`).
- **[Code]** Die Zeichenfläche ist auf DPR 2 und 4 096 px Kantenlänge begrenzt (`f=Math.min(window.devicePixelRatio||1,2,4096/Math.max(d,p))`). DPR-3-Handys skalieren deshalb 1,5-fach hoch.

**Änderung:** Code in C4.
- Maximalzoom auf 2 Geräte-Pixel je Bildpixel, an beiden Zoomstellen.
- Stufenwahl ohne 12 % Toleranz.
- Zeichenfläche bis DPR 3 mit einer Flächengrenze von 16 MP.
- Optional Stufe 2 mit JPEG-Qualität ≈ 80 neu kodieren (etwa 40 % größer).

**Abnahme**
- Beim stärksten Zoom auf die Kirchturmspitze bleiben die Striche plastisch, ohne Blockraster.
- Auf einem DPR-3-Handy ist die Gesamtansicht so scharf wie am Desktop.

**Nicht verändern:** Kachelpyramide, progressives Laden, nahtloses Raster, `imageSmoothingQuality="high"`.

---

### UI2 · Startseite und Handy-Querformat · Priorität P3

**Reproduktion:** Die Startseite bei 1280 × 720 und im Handy-Querformat (844 × 390) ansehen; im Querformat auch „Gemälde erkunden“ öffnen.

**Belege**
- **[Beobachtet]** Bei 1280 × 720 zeigt das Titelbild (`.cover`, `object-fit:cover;object-position:50% 48%`) vor allem Himmel und Zypresse. Kirchturm und Dorf liegen am unteren Rand unter dem dunklen Verlauf `.shade` – dabei ist das Dorf das Ziel der Fahrt.
- **[Gemessen am Screenshot]** Kontraste:

| Text | Median | lokal über hellen Strichen |
|---|---|---|
| Fließtext #fbf7e9 | 14,6 : 1 | etwa 3 : 1 |
| „Piano aus“ und „Werkinfo“ oben rechts (über dem Mond) | 5,2 : 1 | 2,7 : 1 |

- **[Gemessen, Emulation 844 × 390]** Die Startseite ist 464 px hoch, die Fußzeile liegt unter dem Falz. Im Betrachter liegen Titel und Zoomknöpfe auf dem Gemälde, die Zoomknöpfe auf dem Dorf.

**Änderung**
- Breitbild: `object-position:50% 72%`, damit Dorf und Kirchturm sichtbar werden. Auf schmalen Schirmen bleibt `60% 50%`.
- Oben rechts ein weicher radialer Schatten hinter der Leiste (`rgba(6,15,23,.55)` → transparent, etwa 220 px) oder `text-shadow:0 1px 8px rgba(0,0,0,.6)`.
- Betrachter im Handy-Querformat: Zoomknöpfe senkrecht an den rechten Rand, den Titel beim Zoomen ausblenden.

**Abnahme**
- Kirchturmspitze und Dorfdächer sind bei 16 : 9 auf der Startseite sichtbar.
- Oben rechts liegt der Kontrast lokal bei mindestens 4,5 : 1.
- Bei 844 × 390 verdeckt nichts das Dorf, und es gibt keine Fußzeile unter dem Falz.

---

### OF1 · Offline funktioniert; auf dem iPhone ist der Start unklar; Speicherspitzen · Priorität P1 (Klärung), sonst P3

**Reproduktion:** Netzwerk ausschalten, die Datei per Doppelklick öffnen und alle Ansichten aufrufen. Am iPhone die Datei über die Dateien-App öffnen.

**Belege**
- **[Gemessen, echt offline]** Chromium 158 ohne jede Netzverbindung, HTML per `file://`:
  - Alle fünf Ansichten laden, mit 0 externen Anfragen und 0 Konsolenfehlern.
  - Äußeres `load`-Event nach 1,26 s; Startseite samt Titelbild nach 1,3 s.
  - Ansichtswechsel synchron in 43–74 ms (`Uint8Array.fromBase64` vorhanden; ältere Browser nutzen den langsameren `atob`-Weg).
  - Ladehinweis der Fahrt verschwunden nach 4,2–5,9 s (Software-Grafik, 960 × 540; auf GPU [Nicht geprüft]).
- **[Gemessen]** Speicher:
  - RSS aller Chromium-Prozesse rund 0,75 GB auf der Startseite und rund 1,35 GB in der Fahrt. Mit Software-Grafik liegt der GPU-Speicher im RAM; die Zahlen geben nur die Größenordnung.
  - JS-Heap 5 bzw. 31 MB.
  - Der Renderer wächst über vier Wechsel Fahrt ↔ Start von 312 auf 526 MB und bleibt dann stabil.
- **[Code]** CSP `default-src 'none'`: keine Netzzugriffe möglich. Externe Adressen gibt es nur als Quellen-Links, die beim Klick öffnen.
- **[Berechnet]** GPU-Texturen der Fahrt am Desktop: 2 × 4096 × 3244 RGBA mit Mipmaps ≈ 2 × 67,6 MiB.
- **[Gemessen]** Das `srcdoc` der Startseite ist 5,0 MB groß, weil `cover.webp` zweimal als `data:`-URL darin steht (je 1,85 MB Base64). Einmal genügt; dann wären es rund 3,2 MB.
- **[Annahme, nicht am Gerät geprüft]**
  - iOS: Die Dateien-App zeigt HTML per Quick Look ohne JavaScript, und Safari öffnet keine `file://`-Adressen aus „Dateien“. Dann wäre die Einzeldatei auf dem iPhone ohne Zusatz-App nicht startbar.
  - Android: Chrome öffnet lokale HTML je nach Dateimanager über `content://`; das Verhalten ist uneinheitlich.

**Änderung**
1. **Zuerst klären:** Auf einem echten iPhone und einem Android-Gerät prüfen, ob und wie die Datei startet. Das Ergebnis als kurze Anleitung in die Werkinfo schreiben.
2. Ist das iPhone Zielgerät und startet die Datei dort nicht, gibt es zwei Wege. Beides ist eine Produktentscheidung, keine Code-Korrektur.
   - eine kleine Begleit-Variante als Web-Adresse mit Offline-Cache (einmal online laden),
   - eine Viewer-App mit JavaScript-Unterstützung.
3. Speicherspitzen senken:
   - `cover.webp` nur einmal je Seite einbetten; das Werkinfo-Bild setzt seine `src` erst beim Öffnen.
   - Klavier nur im äußeren Dokument dekodieren (AU1), Vorschau-MP3 erst bei Bedarf.
   - Nach Abnahme der Schritte 1–3 aus Abschnitt D die alten Routen `reise` und `vergleich` mit `journey.mjs`, `gemaelde.html` und der Vorschau-MP3 entfernen; das spart rund 1,3 MB. Die Dorffahrt `dorf` bleibt als 2D-Ersatz ohne WebGL 2 (R6), bis ein schlanker Ersatz gebaut ist, denn sie braucht die Himmelspanoramen. Danach sind weitere rund 3,8 MB frei.
   - **Nicht** die Kachelstufe 3 opfern. Sie ist 45 % der Datei, aber der Kern des Betrachters.

**Abnahme**
- Der Start auf beiden Geräten ist dokumentiert: geht oder geht nicht, und auf welchem Weg.
- Das `srcdoc` der Startseite ist höchstens 3,3 MB groß.
- Die Messung „0 externe Anfragen“ ist wiederholt.

---

### L1 · Herkunft der Fahr-Engine (vor jeder Weitergabe klären) · Priorität P0 für Weitergabe

**Belege**
- **[Gemessen]** `bundles/original.mjs`, die Engine hinter „Fahrt beginnen“, ist eine eingedeutschte Offline-Bearbeitung des Bundles von starrynightrider.com:
  - 61 von 73 GLSL-Shaderblöcken und 277 von 326 Shader-Kommentarzeilen der Referenz sind byte-identisch; die übrigen unterscheiden sich nur in Variablennamen.
  - Fünf Texturen sind byte-identisch (SHA-256 verglichen): `vorlage/cypress.png`, `fill.png`, `flow.png`, `painting-s.webp`, `sky-s.webp`.
  - Das Fahrtmenü nennt „Spielvorlage von Ebby Amir“.
- **[Gemessen]** Die Referenz veröffentlicht keine Lizenz: `/LICENSE` und `/license.txt` liefern 404, und im Bundle steht nur der MIT-Hinweis von three.js.

**Konsequenz**
- Für den privaten Gebrauch ändert sich an Qualität und Umsetzung nichts.
- Bevor die Datei an Rehabilitanden, Kollegen oder auf eine Website geht, braucht es eine schriftliche Erlaubnis des Urhebers. Ohne sie bleibt die Datei privat, oder Engine und abgeleitete Texturen werden durch eigene ersetzt.
- Dies ist ein technischer Herkunftsbefund, keine Rechtsberatung.

**Für ChatGPT**
- Alle Vorschläge zu `original.mjs` setzen diese Klärung voraus.
- Keine weiteren Dateien, Stimmen oder Codeteile von starrynightrider.com übernehmen.
- Allgemeine Techniken (Projektionstexturierung, Kamerafahrten, Kantenglättung, Nebel) sind frei nutzbar und gesondert umzusetzen.

---

## C. Gezielte Codevorschläge

**Für alle Ausschnitte gilt:** Jeder Suchtext kommt im genannten Bundle genau einmal vor (geprüft). Die eingebetteten Bundles liegen als Base64 in `<script type="application/octet-stream" data-file="bundles/….mjs">`; nach der Änderung neu kodieren. Zum Testen eignet sich ein Skript, das eine **Kopie** der HTML erzeugt: Base64 dekodieren, ersetzen, neu kodieren.

### C1 · Ruhiger, ungestörter Einstieg ins freie Bild (zu E1 und UI1) · isoliert getestet

**Datei:** `bundles/original.mjs`

```js
// (1) Tempo und Überspringen.
// ALT:
let x=ie.reduceMotion||jt.any||jt.throttle||jt.brake||jt.jumpPressed||Math.abs(jt.steer)>.1;ie.diveT=x?8.35:ie.diveT+8.35/2.7*Math.min(t,.45)
// NEU: 6,5 s statt 2,7 s; überspringen nur mit Fahren/Bremsen und erst nach Auflösung des Scans; Ruckler-Schutz
let x=ie.reduceMotion||ie.diveT>2.6&&(jt.throttle||jt.brake);ie.diveT=x?8.35:ie.diveT+8.35/6.5*Math.min(t,.1)

// (2) Rund 1 s Halt auf dem gerahmten Scan (bei 8,35/6,5 Dive-Sekunden je Sekunde entspricht −1,3 etwa 1,02 s).
// ALT:
document.documentElement.classList.add("direct-entry"),Mo())
// NEU:
document.documentElement.classList.add("direct-entry"),Mo(),ie.reduceMotion||(ie.diveT=-1.3))

// (3) Die Fahrt beginnt im Modus „Bild pur“ (HUD und Spielobjekte aus; T blendet ein).
// ALT:
document.documentElement.classList.remove("direct-entry")),Q2())
// NEU:
document.documentElement.classList.remove("direct-entry"),ie.calmStart=!0),Q2(),ie.calmStart&&(ie.calmStart=!1,yp(!0)))
```

Dazu in `vorlage/index.html`, ungetestet, aber von der Fahrgefühl-Teilprüfung visuell bestätigt:

```css
/* in der Regel  #loading { … }  mit  center center/cover  ersetzen: */
background:linear-gradient(0deg,#0b1424cc,transparent 40%);
transition:opacity .9s ease;
```

```html
<!-- statt <html lang="de"> -->
<html lang="de" class="direct-entry">
```

**Teststatus:** isoliert getestet, nur auf Zustandsebene.
- Getestet wurde eine gepatchte **Kopie** der HTML in headless Chromium, offline.
- Die virtuelle Uhr lief mit 1/60 s je Schritt, das GPU-Zeichnen wurde übersprungen; es gab keine Seitenfehler.
- Bildwirkung und Ruckelfreiheit auf echter GPU sind nicht geprüft.
- Die Messung prüft bis 2,9 s in Schritten von 0,1 s, danach jedes Bild; der Sollwert für das Original ist 2,70 s.

| Szenario | Original: Fahrt beginnt bei | Mit Änderung: Fahrt beginnt bei | Mit Änderung: „Bild pur“ aktiv |
|---|---|---|---|
| keine Eingabe | 2,80 s | 7,52 s | ja |
| M bei 0,5 s | 2,80 s | 7,52 s | ja |
| Tipp aufs Display bei 0,5 s | **0,50 s** | 7,52 s | ja |
| W ab 1,0 s gehalten | **1,00 s** | 3,07 s | ja |

### C2 · Ein Klavier für alle Ansichten (zu AU1 und AU2) · isoliert getestet

**Einfügestelle:** äußerer Loader, also das letzte `<script>` der HTML, nicht eingebettet.
- Den folgenden Block direkt vor `  function bytes(el){` einfügen.
- In `window.Nachtfahrt={` die Eigenschaft ergänzen: `asset,storage,piano,`.
- Der Block nutzt die vorhandenen Bezeichner `entries`, `bytes()` und `storage()`. Die CSP des äußeren Dokuments erlaubt `media-src blob:`.

```js
const piano = (() => {
  const KEY = 'nachtfahrt-piano-v2', FILE = 'kunst/audio/sternennacht-piano.mp3';
  let el = null, ctx = null, gain = null, pauseTimer = 0;
  const listeners = new Set();
  const prefs = { enabled: true, volume: 74 };                       // 74 % quadratisch = 0,548 = heutige 55 % linear
  try {
    const v2 = JSON.parse(storage('localStorage').getItem(KEY) || 'null');
    const v1 = JSON.parse(storage('localStorage').getItem('nachtfahrt-piano-v1') || 'null');
    if (v2) Object.assign(prefs, v2);
    else if (v1) { prefs.enabled = v1.enabled !== false; if (Number.isFinite(v1.volume)) prefs.volume = Math.round(Math.sqrt(v1.volume / 100) * 100); }
  } catch {}
  const target = () => (prefs.enabled ? (prefs.volume / 100) ** 2 : 0);
  const save = () => { try { storage('localStorage').setItem(KEY, JSON.stringify(prefs)); } catch {} };
  const emit = () => listeners.forEach(fn => { try { fn(api.state); } catch {} });
  function ensure() {
    if (el) return;
    const url = URL.createObjectURL(new Blob([bytes(entries.get(FILE))], { type: 'audio/mpeg' })); // einmal, nie widerrufen
    el = new Audio(url); el.loop = true; el.preload = 'auto'; el.setAttribute('playsinline', '');
    const AC = window.AudioContext || window.webkitAudioContext;
    if (AC) { ctx = new AC(); gain = ctx.createGain(); gain.gain.value = 0; ctx.createMediaElementSource(el).connect(gain).connect(ctx.destination); }
    try { if (navigator.audioSession) navigator.audioSession.type = 'playback'; } catch {}
    el.addEventListener('playing', emit); el.addEventListener('pause', emit);
  }
  function ramp(to, seconds) {
    if (!gain) { el.volume = Math.min(1, to); return; }               // ohne Web Audio (unter iOS ist volume schreibgeschützt)
    const t = ctx.currentTime;
    gain.gain.cancelScheduledValues(t); gain.gain.setValueAtTime(gain.gain.value, t); gain.gain.linearRampToValueAtTime(to, t + seconds);
  }
  const api = {
    get state() { return { enabled: prefs.enabled, volume: prefs.volume, playing: !!el && !el.paused }; },
    get currentTime() { return el ? el.currentTime : 0; },
    async play(fadeSeconds = 1.2) {                                     // erster Aufruf innerhalb einer Benutzergeste
      if (!prefs.enabled) return;
      ensure(); clearTimeout(pauseTimer);
      if (ctx && ctx.state !== 'running') { try { await ctx.resume(); } catch {} }
      if (el.paused) { if (gain) gain.gain.value = 0; try { await el.play(); } catch { return; } }
      ramp(target(), fadeSeconds);
    },
    pause(fadeSeconds = 0.5) {
      if (!el || el.paused) return;
      ramp(0, fadeSeconds); clearTimeout(pauseTimer);
      pauseTimer = setTimeout(() => el.pause(), fadeSeconds * 1000 + 60);
    },
    setEnabled(on) { prefs.enabled = !!on; save(); on ? api.play(1.2) : api.pause(0.5); emit(); },
    setVolume(v) { prefs.volume = Math.max(0, Math.min(100, Math.round(+v || 0))); save(); if (el && !el.paused) ramp(target(), 0.15); emit(); },
    subscribe(fn) { listeners.add(fn); return () => listeners.delete(fn); },
    unlock() { if (ctx && ctx.state !== 'running') ctx.resume().catch(() => {}); }  // bei jedem Tipp im iframe aufrufen (iOS)
  };
  let resumeAfterHide = false;                                        // nach dem Ausblenden des Tabs nur fortsetzen, wenn vorher gespielt wurde
  document.addEventListener('visibilitychange', () => {
    if (!el) return;
    if (document.hidden) { resumeAfterHide = !el.paused; api.pause(0.3); }
    else if (prefs.enabled && resumeAfterHide) { resumeAfterHide = false; api.play(1.2); }
  });
  return api;
})();
```

**Anbindung in den Seiten** (Beschreibung; nicht getestet)
- **`home.mjs`**
  - Im Klick-Handler `Ye("ride-open").addEventListener("click",()=>{Night.open("fahrt")})` zuerst `Night.piano.play(4)` aufrufen, dann `Night.open("fahrt")`. Das Klavier spielt nur, wenn es nicht ausdrücklich abgeschaltet wurde; Standard ist „an“.
  - Die Knöpfe `[data-piano-toggle]` rufen `Night.piano.setEnabled(!Night.piano.state.enabled)` auf; der Regler `[data-piano-volume]` ruft `Night.piano.setVolume(value)` auf (Standard 74).
  - Die Beschriftungen kommen aus `Night.piano.subscribe(…)`.
  - Die eigene Klavierlogik (`function ff(`) entfällt.
- **`original.mjs`**
  - `Ep=bc({inRide:…})` und `function bc(` auf dieselbe Schnittstelle umstellen. Die Taste M (Suchtext `if(e.code==="KeyM"&&!e.repeat`) ruft `Night.piano.setEnabled(…)` auf.
  - Bei `pointerdown` und `touchstart` zusätzlich `Night.piano.unlock()` aufrufen.
  - Die Ausweichlösung `function _(b){…f(!0)}` (Start bei der ersten Eingabe) behalten, aber umstellen: Sie ruft `Night.piano.play(1.2)` auf, wenn das Klavier eingeschaltet ist und nicht spielt. Sonst bliebe die Fahrt stumm, wenn `#fahrt` ohne Klick auf der Startseite geöffnet wird (Neuladen, Zurück/Vor, R6).
- **`journey.mjs`:** Den Regler `starry-volume` auf 0–100 umstellen und an dieselbe Schnittstelle hängen.
- **HTML:** `<audio id="piano" …>` in `index.html` und `vorlage/index.html` entfernen.

**Teststatus:** isoliert getestet.
- Der Block wurde in eine **Kopie** des äußeren Loaders eingesetzt. Headless Chromium lief offline mit der Standard-Autoplay-Regel; vor `play()` gab es einen echten Mausklick.
- Nicht getestet sind die Anbindung in den drei Bundles, Safari und iOS (Freischalten des äußeren Audiokontexts durch einen Tipp im `iframe`) sowie Firefox.

| Zeitpunkt | spielt | Position | Verstärkung |
|---|---|---|---|
| +0,3 s nach `play(1.2)` | ja | 0,31 s | 0,142 (Ziel 0,548) |
| +1,3 s | ja | 1,31 s | 0,548 |
| nach Wechsel zu `fahrt` | ja | 6,55 s | 0,548 |
| nach `start` | ja | 10,46 s | 0,548 |
| nach `reise` | ja | 13,65 s | 0,548 |
| nach `fahrt` | ja | 17,97 s | 0,548 |
| nach `start` | ja | 19,62 s | 0,548 |
| Tab verborgen, +0,7 s | **pausiert** | 19,98 s | 0 |
| Tab wieder sichtbar, +0,3 s | ja | 20,28 s | 0,136 |
| +1,4 s | ja | 21,38 s | 0,548 |
| `setEnabled(false)` +0,25 s | ja | 21,64 s | 0,271 |
| +0,65 s | **pausiert** | 21,95 s | 0 |

Der erste Entwurf setzte nach einem Tab-Wechsel nicht fort. Die Gegenprüfung hat das gefunden; der Fehler ist behoben (`resumeAfterHide`) und mit den Zeilen „Tab verborgen/sichtbar“ nachgetestet. Der getestete Block entsprach dem obigen bis auf einen zusätzlichen Mess-Getter `get gainValue()`, der nur dem Test diente.

### C3 · Bildruhe und Schärfe der Fahrt (zu BQ1–BQ4) · Entwurf, nicht getestet

**Datei:** `bundles/original.mjs`

```js
// BQ1 – Pixelbudget (Desktop) und Qualitäts-Untergrenze
// ALT: Math.sqrt((Di.touch?17e5:33e5)/(e*t))          NEU: Math.sqrt((Di.touch?17e5:52e5)/(e*t))
// ALT: ie.quality=Math.max(.55,ie.quality-.12)         NEU: ie.quality=Math.max(.7,ie.quality-.12)
// ALT: ie.quality=Math.max(.55,ie.quality-.06)         NEU: ie.quality=Math.max(.7,ie.quality-.06)
// ALT: i>.02&&ie.quality>.55?(                         NEU: i>.02&&ie.quality>.7?(
```

```js
// BQ2 – Multisampling für das Szenenziel (nur Nicht-Touch). Suchtext: this.sceneRT=new Qi(4,4,{type:Aa
// Im bestehenden Options-Objekt ergänzen:  samples: Di.touch ? 0 : 4
// Ist Di an dieser Stelle nicht erreichbar, den Wert über den Konstruktor der Postprocessing-Klasse durchreichen.
// Danach prüfen, dass tDepth weiterhin gültige Tiefen liefert (Konturen, Nebel im Composite-Durchgang).
// Zypresse: Material mit alphaToCoverage:true; im Shader statt  if (texture2D(tCypress, uvc).r < 0.5) discard;
// die Deckkraft weich ausgeben:  float a = smoothstep(0.35, 0.65, texture2D(tCypress, uvc).r); if (a < 0.01) discard;  … gl_FragColor.a = a;
// Ausweichlösung: samples weglassen und einen FXAA-Pass hinter den Composite-Durchgang hängen.
```

```glsl
// BQ3 – Tupfergröße nach Verkürzung (Hausshader). Suchtext: float lod = log2(max(vDist / uStrokeDist, 0.5));
// Die spätere Zeile  vec2 jx = dFdx(p), jy = dFdy(p);  entfällt, weil sie hierher wandert. p ist an dieser Stelle bereits final.
vec2 jx = dFdx(p), jy = dFdy(p);
float pix = max(length(vec2(jx.y, jy.y)) / size.y, length(vec2(jx.x, jy.x)) / size.x);
float lod = max(log2(max(vDist / uStrokeDist, 0.5)), log2(max(4.0 * pix, 1e-6)));
```

```glsl
// BQ4 – Gelb-Flackern halbieren. Suchtext: float flick = 0.88 + 0.12 * sin(uTime * 1.7
//        NEU:                            float flick = 0.94 + 0.06 * sin(uTime * 1.7      (Rest der Zeile unverändert)
// BQ4 – varianzerhaltende Überblendung. Suchtext: vec3 pc = mix(c1, c0, 1.0 - abs(2.0 * p0 - 1.0));
// m = zusätzliche grobe Probe am selben Ort (textureGrad mit 6-fachen Gradienten der vorhandenen Abfragen) – neu anzulegen.
float w0 = 1.0 - abs(2.0 * p0 - 1.0), w1 = 1.0 - w0;
vec3 pc = m + (w0 * (c0 - m) + w1 * (c1 - m)) / max(sqrt(w0 * w0 + w1 * w1), 1e-3);
// BQ4 – LOD-Bias −0,5 für die Scan-Abfragen c0 und c1: deren Gradienten (cgx, cgy) mit 0.7 multiplizieren.
```

**Teststatus:** Die JavaScript-Ersetzungen aus BQ1 und die Flacker-Zeile aus BQ4 lassen sich eindeutig anwenden; das geänderte Bundle besteht `node --check`. GLSL und Wirkung sind nicht getestet.

**Hinweis für ChatGPT:** Vor jedem GLSL-Eingriff den Kontext der Zeile im Shader lesen. Je Änderung einen A/B-Screenshot an derselben Stelle der Vergleichsfahrt (D, Punkte 4 und 6) machen und die Bildrate messen.

### C4 · Gemäldebetrachter (zu GB1) · Entwurf, nicht getestet

**Datei:** `bundles/home.mjs`

```js
// Zeichenfläche bis DPR 3, Flächengrenze 16 MP statt 4096-px-Kante
// ALT: f=Math.min(window.devicePixelRatio||1,2,4096/Math.max(d,p))
// NEU: f=Math.min(window.devicePixelRatio||1,3,Math.sqrt(16e6/(d*p)))
// Maximalzoom 2 Geräte-Pixel je Bildpixel – beide Stellen
// ALT: m=si(m,g,Math.max(g,2.2))       NEU: m=si(m,g,Math.max(g,2/f))
// ALT: m=si(m*q,g,Math.max(g,2.2))     NEU: m=si(m*q,g,Math.max(g,2/f))
// Stufenwahl ohne 12 % Hochskalierung
// ALT: S=yu.findIndex(R=>R.width*1.12>=H)   NEU: S=yu.findIndex(R=>R.width>=H)
```

**Geprüft:** `f` ist im selben Closure deklariert wie `F()` und `G()` (`…,d=0,p=0,f=1,g=1,m=1,…`), also an beiden Zoomstellen verfügbar. `m` ist der Zoom in CSS-px je Bildpixel der Stufe 3; `2/f` entspricht 2 Geräte-Pixeln je Bildpixel. Teststatus: Ersetzungen eindeutig, das geänderte Bundle besteht `node --check`; die Wirkung ist nicht getestet.

### C5 · Zeichensatz der eingebetteten Textdateien (zu AU4) · in der Audio-Teilprüfung getestet

**Ort:** äußerer Loader, Funktion `asset(path)`

```js
// ALT:
else urls.set(path,URL.createObjectURL(new Blob([bytes(el)],{type:el.dataset.type})));
// NEU:
else urls.set(path,URL.createObjectURL(new Blob([bytes(el)],{type:el.dataset.type==='text/plain'?'text/plain;charset=utf-8':el.dataset.type})));
```

### C6 · Kamera, Sichtfeld, Parken, Stick (zu R1–R4)

R1 und R2 wurden in der Fahrgefühl-Teilprüfung an einer **Testkopie** mit Messpunkten simuliert (60 Hz, ohne GPU-Bewertung). R3 und R4 sind Entwürfe, in ihrer Wirkung nicht getestet. Alle vier Ersetzungen lassen sich eindeutig anwenden, und das geänderte Bundle besteht `node --check`. Für R3 ist geprüft, dass `a` im selben Update `this.speed` ist: Zwischen `let a=this.speed` und der Bremszeile liegt keine Neuzuweisung und keine Funktionsgrenze.

**Datei:** `bundles/original.mjs`

```js
// R1 – Kamera folgt der Fahrtrichtung exponentiell; weniger Rollen
// ALT: this.yaw+=lo(this.yaw,t.heading)*Math.min(1,3.4*e)
// NEU: this.yaw+=lo(this.yaw,t.heading)*(1-Math.exp(-8*e))
// ALT: this.roll=st(this.roll,-t.lean*(.18+.4*n)*(t.grounded?1:.3),5,e)
// NEU: this.roll=st(this.roll,-t.lean*(.06+.4*n)*(t.grounded?1:.3),5,e)

// R2 – Sichtfeld atmet nicht mehr mit dem Tempo
// ALT: this.fov=72+12*r+5*this.air+15*n
// NEU: this.fov=72+4*r+5*this.air+15*n

// R3 – Anhalten rastet ein (Entwurf). a ist im selben Update die aktuelle Geschwindigkeit (let a=this.speed).
// ALT: t.brake>0&&!n&&(l=o?7:0)
// NEU: t.brake>0&&!n&&a<.3&&(this.parked=!0),(t.throttle>0||Math.abs(t.steer)>.2||t.jumpPressed)&&(this.parked=!1),t.brake>0&&!n&&(l=o?7:0),this.parked&&!n&&!this.auto&&(l=0)
// Zusätzlich this.parked=!1 in der reset()-Methode der Rad-Klasse.
// Blick nicht zurückzentrieren, solange geparkt (Suchtext z-i.lastLook>1.2&&(i.lookYaw*=.94):
// Bedingung um !J.parked ergänzen. Ist J dort nicht erreichbar, im Fahrt-Update jt.parked=J.parked setzen und i.parked abfragen.

// R4 – Touch-Stick: kleinere Totzone, weichere Kennlinie (Entwurf)
// ALT: O=H<.22?0:(H-.22)/.78;_=Math.sign(F)*O**2.2
// NEU: O=H<.12?0:(H-.12)/.88;_=Math.sign(F)*O**1.5
```

**Beschriftungen zu R2** in `vorlage/index.html`:
- `<kbd data-code="KeyW"><b>W</b><i>fahren</i></kbd>` → `<i>schneller</i>`
- `<dt>Fahren und lenken</dt><dd>WASD · Pfeiltasten</dd>` → `<dt>Lenken · schneller · bremsen</dt><dd>A/D · W · S (oder Pfeiltasten)</dd>`

---

## D. Empfohlene Reihenfolge und Abnahme

### Reihenfolge für ChatGPT

Jeder Schritt ist für sich abnehmbar. Nach jedem Schritt die Datei neu bauen, eine Sicherungskopie der Vorversion behalten und die zugehörigen Punkte der Vergleichsfahrt prüfen.

1. **Einstieg und freies Bild:** C1 (drei Ersetzungen), dazu `#loading` und `class="direct-entry"`. Danach die übrigen Teile von UI1 (Übersetzungen, Menüpunkt „Lichter sammeln“). Prüfen: Punkte 1–3.
2. **Klavier:** C2 im äußeren Loader, Anbindung der drei Bundles, `<audio id="piano">` aus den inneren Seiten entfernen; dazu AU4 mit C5 und der sichtbaren Nennung. Prüfen: Punkte 1, 8, 9, 10.
3. **Kurve und Sichtfeld:** R1 und R2 mit C6, dazu die neue W-Beschriftung. Prüfen: Punkte 5 und 11.
4. **Anhalten:** R3. Prüfen: Punkt 6.
5. **Bildruhe und Schärfe**, in dieser Reihenfolge: BQ2, BQ1, BQ3, BQ4.
   - Jeweils mit A/B-Screenshot an derselben Stelle.
   - Jeweils mit Messung von `ie.quality` und Bildrate auf echter GPU.
   - Was die Bildrate am Desktop unter 50 fps oder am Tablet unter 30 fps drückt, wird zurückgenommen oder abgeschwächt.
   - Prüfen: Punkte 4 und 6.
6. **Texturwahl:** BQ5 (2 400-px-Fassungen, `Di.small`).
7. **Bedienung und Ränder:** R4, R5, R6, GB1 (C4), UI2. Prüfen: Punkt 7.
8. **Geräte und Aufräumen:** OF1. Der Gerätetest (OF1, Änderung 1) kann jederzeit parallel laufen. Die alten Routen erst entfernen, wenn die Schritte 1–3 abgenommen sind.

### Vergleichsfahrt (vorher und nachher unter gleichen Bedingungen)

**Bedingungen**
- Gleiches Gerät, gleicher Browser, gleicher Bildschirm, Fenster im Vollbild.
- Systemlautstärke fest (zum Beispiel 50 %), Kopfhörer.
- Privates Fenster, damit keine gespeicherten Einstellungen wirken.
- Netzwerk aus (Flugmodus); die Datei per Doppelklick öffnen.
- Jeden Punkt von 1 bis 5 bewerten und eine Zeile notieren.

| Nr. | Moment | So herstellen | Darauf achten | Heute (Befund) | Ziel |
|---|---|---|---|---|---|
| 1 | Ankommen | Startseite → „Fahrt beginnen“, dann nichts drücken; Stoppuhr | Wann ist das Gemälde zu sehen, wann das Dorf? Springt der Bildausschnitt? Wann und wie setzt das Klavier ein? | Titelbild bildschirmfüllend, dann Sprung auf den kleinen Rahmen; Scan nach 0,42 s weg; Gasse nach 2,7 s; Klavier stumm bis zur ersten Eingabe | Gemälde gerahmt schon beim Laden; 1 s Halt; Flug über die Dächer; Gasse nach rund 7,5 s; Klavier blendet ab dem Klick über 4 s ein |
| 2 | Antippen im Einstieg | wie 1; nach 0,5 s aufs Bild tippen (Touch) oder D drücken | Bricht der Flug ab? | bricht sofort ab | läuft weiter; nur W oder S überspringen ab rund 3 s |
| 3 | Freies Bild | erste 15 s der Fahrt | Welche Anzeigen liegen über dem Bild? Gibt es englische Wörter? | 24,7 % der Fläche verdeckt; „Highlights“, „Tab next“ | nur „Menü“ sowie gedämpft ☾ und ♪; alles deutsch; T (Touch: ☾ antippen) blendet alles ein |
| 4 | Gasse geradeaus | nach dem Einstieg nichts drücken, das Rad rollt von selbst; die rechte Häuserzeile beobachten | Treppen an Dachkanten? Flimmern entlang der Traufe? | harte Treppen, Tupfer flirren | glatte, ruhige Kanten; Tupfer erkennbar statt pixelig |
| 5 | Kurve | weiter rollen lassen, bis die Gasse auf den Kirchplatz mündet und die helle Kirche vorn rechts steht (rund 7 s, rund 60 m), dann D 0,6 s halten | Ziehen die Hausecke rechts und der Platzrand vorbei, während Kirchturm, Hügel und Himmel stehen bleiben? Kippt der Horizont? | Kirchturm wischt mit bis zu 1000 px/s; Kamera dreht nach der Kurve 38° nach, Ruhe erst nach 1,8 s; Horizont 3° schief | Fernraum steht nach höchstens 1,2 s; höchstens 22° Nachdrehen nach der Kurve; Horizont etwa 1° |
| 6 | Anhalten | S kurz halten bis 0 km/h, dann loslassen; 10 s auf den Sternring neben dem Mond schauen | Bleibt das Rad stehen? Pulsiert der Sternring? Flackert Gelb? | Rad rollt wieder an; Himmel „atmet“ im Rhythmus von rund 3 s; Gelb ±12 % | Rad steht; Striche ruhig, sanftes Leben ohne Pulsieren |
| 7 | Gemäldedetail | Menü → „Zur Startseite“ → „Gemälde erkunden“ → mit + auf die Kirchturmspitze bis zum Anschlag | Plastische Striche oder Blockraster? | Anschlag bei 2,2 CSS-px je Bildpixel (4,4 Geräte-Pixel bei DPR 2); Blockraster sichtbar | Anschlag bei 2 Geräte-Pixeln je Bildpixel; Striche plastisch |
| 8 | Klavier aus und an | in der Fahrt M, 3 s warten, wieder M | Wird hart abgeschnitten oder ausgeblendet? | Aus nach 220 ms bei −26 dB; Ein in rund 0,2 s | Aus in 0,5 s ohne Abriss; Ein über 1,2 s |
| 9 | Ansichtswechsel | Menü → „Zur Startseite“, dann wieder „Fahrt beginnen“ | Läuft die Musik weiter? | harter Schnitt, danach Stille bis zur ersten Eingabe | Musik läuft ohne Unterbrechung |
| 10 | Schleifenstelle | Klavier 4:17 min laufen lassen | Atempause oder Loch und Klick? | rund 1 s natürliche Atempause, kein Klick (gemessen) | unverändert |
| 11 | Sichtfeld bei W | in der Gasse W 3 s halten, dann loslassen | Schrumpfen Kirchturm und Hügel beim Beschleunigen und wachsen beim Loslassen? | Sichtfeld 73,5° → 83,9°, Fernraum schrumpft um 17 % | Sichtfeld ändert sich höchstens um 4°; Kirchturm bleibt gleich groß |

**Bewertung:** Die Überarbeitung hat sich gelohnt, wenn die Punkte 1, 3, 5, 8 und 9 jeweils um mindestens 2 Stufen besser sind und kein Punkt schlechter geworden ist. Zusätzlich darf die Bildrate bei Punkt 4 am Desktop nicht unter 50 fps und am Tablet nicht unter 30 fps fallen; die Anzeige liefert Chrome-DevTools → Rendering → „Frame Rendering Stats“.

---

## Anhang

### Was ein echtes Gerät oder echtes Hören erfordert

- Bildrate, Ruckeln, Eingabelatenz und die Kosten von BQ1/BQ2 auf echter GPU (Desktop, Tablet, Handy).
- Start der Einzeldatei auf iPhone und Android (OF1). Dazu das Freischalten des Audiokontexts im äußeren Dokument durch einen Tipp im `iframe` unter iOS (AU1).
- Safari und Firefox: `blob:`- und `data:`-Verhalten, Schleife des Audio-Elements.
- Klangwirkung des Stücks, das Verhältnis zu den Fahrgeräuschen und die Hörbarkeit des Abrisses beim Stummschalten.
- Die Referenz live: Sie wurde nicht interaktiv befahren.

### Prüfvermerk

**Gegenprüfung:** 24.11.2026, durch einen unabhängigen Prüfagenten mit frischem Kontext, ohne Kenntnis des Gesprächsverlaufs.

**Geprüft gegen**
- den Auftrag: Abschnitte A–D und Pflichtangaben je Befund,
- die Quellen: entpackte Bundles, Referenz-Bundle, Messprotokolle und Testskripte,
- innere Widersprüche, Rechenwege und Terminologie.

**Bestätigt**
- Alle Suchtexte mit Eindeutigkeitsanspruch kommen genau einmal vor.
- Alle Ersetzungen aus C1, C3, C4 und C6 lassen sich eindeutig anwenden; die geänderten Bundles bestehen `node --check`. C2 und C5 sind syntaktisch gültig.
- Nachgerechnet oder nachgemessen wurden:
  - die Zeittabelle des Einstiegs,
  - Halte- und Überspringzeit,
  - Texturspeicher und die Größe des Startseiten-`srcdoc`,
  - die Sichtfelder,
  - Renderauflösung und Hochskalierung,
  - die Stick-Kennlinie,
  - die Audio-Messwerte,
  - das Asset-Inventar.

**Gefunden und behoben**
- Muss-Korrekturen:
  - ein Logikfehler im Klavier-Entwurf C2: keine Fortsetzung nach einem Tab-Wechsel; behoben und nachgetestet,
  - ein Platzhalter,
  - zwei Inkonsistenzen: 37° statt 38° Nachdrehen und der Begriff „Nachlauf“ in D.
- 13 Präzisierungen, unter anderem:
  - Speicherbedarf der Kantenglättung je Auflösung,
  - Geltungsbereich der Sichtfeld-Formel,
  - ♪ im Modus „Bild pur“,
  - Überspringen mit W oder S,
  - Ausweichlösung beim Klavierstart,
  - Zoomangaben je DPR,
  - zwei nicht wörtliche Suchtexte,
  - Verweise in Abschnitt D,
  - die Marke [Berechnet].
- 9 kleinere Hinweise: Messgenauigkeit einzelner Audio-Werte, Zählweise der Shader, Menütext, Größe der alten Routen, Fachwörter.

**Nicht von der Gegenprüfung nachgeprüft** (aus den Teilprüfungen übernommen):
- Nebel-, Hügel- und Rollwerte in R0 sowie 5,1° Neigung beim Sprint,
- die Kamerageschwindigkeiten 45 und 14,6 m/s,
- die Restabdeckung in „Bild pur“,
- Laplace- und JPEG-Qualitätswerte des Betrachters,
- die Kontrastwerte,
- die Hochfrequenz-Verhältnisse,
- das Speicherwachstum des Renderers,
- der Test von C5.

### Belege beim Prüfer (nicht beigefügt; auf Wunsch nachreichbar)

- **Eigene Aufnahmen** (offline, `file://`):
  - Startseite, Werkinfo, Fahrt bei 1280 × 720,
  - Einstiegsfolge und Gasse mit virtueller Uhr,
  - Betrachter-Zoom auf Kirchturm und Mond,
  - Handy-Querformat (Emulation).
- **Testskripte:**
  - `entry_test.js` (C1),
  - `piano_test.js` (C2),
  - `dive_timing.js` (Einstiegs-Zeitpunkte),
  - Netz- und Zeitmessungen, Messung der HUD-Fläche.
- **Teilprüfungen:**
  - Bild und Rendering: Shader-Diff, Detail- und Mipmap-Simulationen, Zeichenflächen, Kachelanalyse.
  - Fahrgefühl: Kurven- und Sichtfeldsimulation, Touch-Layout, Ersatz ohne WebGL.
  - Audio und Offline: Lautheit, Schleifenstelle, Abriss- und Fade-Messungen, Speicher.
