# Notizen

## Ziele

- Erstellen von Learnjourneys nach dem lernOS Prinzip
- Separation of concerns: Flipped Classroom Übungen (Katas) sind separiert und als YAML-Files erstellt
- Dadurch können Katas prinzipiell von jedem submitted werden
- Es können sehr einfach und sehr schnell eigene Lernpfade (zB GTD for Beginner, GTD for Advanced) generiert werden, mit einer Collection von Katas
- Katas als YAML weil maschinenlesbar und damit auch für andere Systeme konvertierbar
- Automatisierter build process via Github Actions sowie pandoc docker image

### Katas

- Katas sind YAML Dateien
- Katas sind lokalisiert
- Katas enthalten pandoc markdown templates, um YAML nach Markdown konvertieren zu lassen
- Katas enthalten eine eindeutige UUID

### Learnjourneys

- Learnjourneys sind Markdown-Dateien mit erklärendem Text
- Zusätzlich eine Ansammlung von Katas, konfiguriert per YAML file, das enthält
  - Language ISO Code
  - Liste der UUID der Katas, die eingebunden werden sollen

## Kata Repostruktur

REPONAME wird per git submodule in die eigentliche Build-Infra des Haupt-Repos eingebunden.

```
REPONAME
`-de_DE
 `-katas
  `- mykata1.md
  `- mykata2.md
  `- mykata3.md
 `-assets
  `- file.png
 `-templates
  `- tpl1.md
  `- tpl2.md
 `-output
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
