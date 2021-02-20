# Nützliche Links

- [YAML nach JSON online konvertieren](https://onlineyamltools.com/convert-yaml-to-json)
- [UUIDs generieren](https://www.uuidgenerator.net/)

# Ziele

- Erstellen von Learnjourneys nach dem [lernOS](https://lernos.org) Prinzip (oder abstrakter: Leanjourneys zu definieren, die gerichtet, iterativ und (in Gruppen) selbst gesteuert ablaufen)
- Separation of concerns: Flipped Classroom Übungen (Katas) sind separiert und als YAML-Files erstellt
- Dadurch können Katas prinzipiell von jedem submitted werden
- Es können sehr einfach und sehr schnell eigene Lernpfade (zB GTD for Beginner, GTD for Advanced) generiert werden, mit einer Collection von Katas
- Katas als YAML weil maschinenlesbar und damit auch für andere Systeme konvertierbar
- Automatisierter build process via Github Actions sowie pandoc docker image

# Katas

- Katas sind YAML Dateien
- Katas sind lokalisiert
- Katas enthalten pandoc markdown templates, um YAML nach Markdown konvertieren zu lassen
- Katas enthalten eine eindeutige UUID (erzeugt zB mit [einem UUID Generator](https://www.uuidgenerator.net/))
- Katas bekommen ein YAML Schema, um validiert werden zu können

# Learnjourneys

- Learnjourneys sind Markdown-Dateien mit erklärendem Text
- Zusätzlich eine Ansammlung von Katas, konfiguriert per YAML file, das enthält
  - Language ISO Code
  - Liste der UUID der Katas, die eingebunden werden sollen

# Kata Repostruktur

REPONAME wird per git submodule in die eigentliche Build-Infra des Haupt-Repos eingebunden.

```
REPONAME
`-de_DE/
 `- katas/
  `- mykata1.md
  `- mykata2.md
  `- mykata3.md
 `- assets/
  `- ab1b3f13-006b-4507-ab7e-e7dc09662c03/
   `- file.png
 `- templates/
  `- tpl1.md
  `- tpl2.md
 `-output/ # generated during build process
  `- ab1b3f13-006b-4507-ab7e-e7dc09662c03/
    `- kata.md
    `- assets/
      `- file.png
`-en_US
 `-katas
  `- mykata1.md
  `- mykata2.md
 `-templates
  `- tpl1.md
  `- tpl2.md
```

## Von YAML Template nach Markdown konvertieren

Beispiel:

```sh
echo '' | pandoc --metadata-file katas/po_foo_1.yaml -t markdown --template katalib/templates/mytemplate.md -o output/output.md
```

## Kata Automations

Die Idee ist, über eine build pipeline anhand der Kata Templates fertige Markdown-Dateien (plus Assets) zu generieren, die dann weiter verwendet werden können.

Da Katas einzelne atomare Einheiten sind, können diese zu bestimmten Themen beliebig hinzugefügt werden. Anhand ihrer UUID sind sie eineindeutig identifizierbar.

# Einen Lernpfad definieren

## Ein Lernpfad besteht aus:

- Einer Konfiguration von
 - Lern-Methodik (zB lernOS)
 - Benötigte Markdown-Dateien in Reihenfolge
 - Iterations-Definition
  - Katas je Iteration (Kata IDs aus dem Kata Repo)
 - Weiteren Angaben

Die Konfiguration des Lernpfads definiert also, welche Markdown-Dateien es für erläuternden Text braucht, und wie die einzelnen Iterationen (zB in Wochen organisiert) aussehen. Je Iteration sind dann ein oder mehrere Katas zugeordnet, die durchlaufen werden sollen.

Damit ist es möglich, Lernpfade zu bauen, die zB nur sehr kurz sind (4 Wochen), oder länger dauern (12 Wochen). Darüber hinaus sind alle relevanten Meta-Daten für den Lernpfad (Subject Matter Expert, Zielstellung des Lernpfads etc.) von eigentlichen Text-Inhalten separiert.

Nach einem Konvertierungsprozess entstehen für diesen Lernpfad Dateien in entsprechenden Formaten wie PDF oder eBooks (epub/mobi) oder direkt nutzbare HTML Webseiten, um einen Lernpfad zu beschreiten.

## Was ist bei einem Lernpfad wichtig zu wissen?

- Das Thema: Also zum Beispiel "GTD for Beginners" oder "Mastering the Pitfalls of GTD"
- Das Ziel / Outcome: Was wird der Lernende nach Durcharbeiten des Lernpfads erreicht haben, wenn er konsequent dabei bleibt?
- Die Dauer: bei lernOS sind es 13 Wochen, aber vielleicht gibt es kürzere Lernpfade
- Lern-Setup: Ist das ein Lernpfad für Alleine und/oder fürs Tandem und/oder für einen Lern-Circle? Als Empfehlung gedacht
- Katas: Auf jeden Fall ein Gesamtüberblick aller Katas (Kata-Title + ID)
- See also: Vielleicht ein Verweis auf weitere, andere Lernpfade ("GTD for Beginners" verweist auf "Mastering the pitfalls of GTD" und "GTD with Omnifocus")
- SME (Subject Matter Expert): Von wem stammt dieser Lernpfad, und welche Lizenz hat dieser Lernpfad

## Beispiel-Konfiguration eines Lernpfads:

```
meta:
  author: "Björn Schotte"
  email: "bjoern.schotte@mayflower.de"
  version: "0.1"
  license: "CC-BY-SA 4.0"
learnjourney:
  language: "de_DE"
  title: "Gettting Things Done for Beginners"
  goal: |
    With this learning journey, you will be able to follow the GTD methodology and live a more organized life.
  iterations: 10
  teamSetup:
    - single
    - double
    - circle
  introTexts:
    - intro1.md
    - intro2.md
  outroTexts:
    - outro1.md
    - outro2.md
  katas:
    -
      iteration: 1
      id: "<id>"
    -
      iteration: 2
      id:
      - "<id2>"
      - "<id23>"
    -
      iteration: 3
      id: "<id3>"
  seeAlso:
    - <journeyId>
    - <journey2Id>
```

## Verzeichnis-Struktur eines Lernpfads

Die Verzeichnisstruktur könnte wie folgt aussehen, am Beispiel des "GTD für Anfänger"-Lernpfads:

```
gtdbeginners/
`- config.yaml
`- de_DE
 `- src/
  `- introduction-short.md
  `- introduction-long.md
  `- thankyou-short.md
  `- thankyou-long.md
 `- assets/
  `- gtd-overview-de.png
`-en_US
 `- src/
  `- introduction-short.md
  `- introduction-long.md
  `- thankyou-short.md
  `- thankyou-long.md
 `- assets/
  `- gtd-overview-en.png
```

```config.yaml``` enthält die maschinenlesbare Konfiguration des Lernpfads (siehe oben). Daneben befinden sich die einzelnen Dateien sortiert nach Sprachen (ISO-Code) in entsprechenden Unterverzeichnissen.

## Wie wird ein Lernpfad gebaut?

Katas sind Übungen, die für sich atomar stehen und Bestandteil eines eigenen Repositories sind, das in das allgemeine Build-Repo reinverlinkt ist. Sie könnten auch aus einer Datenbank kommen.

Anhand der UUID ist eine Kata eineindeutig identifizierbar.

Abstrakt gesehen besteht ein Lernpfad aus

- Einer Einleitung, bestehend aus einer oder mehreren Markdown-Dateien
- Einer Abfolge von Katas für die jeweilige Iteration (ebenfalls Markdown-Dateien)
- Einem Outtro, bestehend aus einer oder mehreren Markdown-Dateien

Diese Dateien werden nacheinander entsprechend der Konfigurations-Reihenfolge dazu genutzt, ein Gesamtkonstrukt, zB ein PDF oder ePub, zu bauen. In ihrer Gesamtheit ergeben sie einen fertigen Lernpfad.

# Tool Notizen

## mkdocs

Tags plugin: https://github.com/jldiaz/mkdocs-plugin-tags
