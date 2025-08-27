# LaTeX Vorlage DHSN

**27.08.1985**
Auf Grundlage des aktuellen BAS-DD-Leitfaden zum wissenschaftlichen Arbeiten der DHSN Dresden.
Grundlagen für LaTeX siehe in Repository LaTeX.
Danke an Marius und Louis für die Vorlage!

## Benutzung:
### Umgebung einrichten.
#### Editor:
Overleaf ist zwar die einfachste Methode, hat aber zum 25.08.2025 die Kompilierzeit verkürzt, sodass es möglich ist, dass das Projekt zu groß ist.
Es gibt auch weitere TeX-Editoren, ich habe aktuell nur Visual Studio Code ausprobiert.

##### Visual Studio Code - Setup
- Download VSC
- Download MiKTeX `https://miktex.org/` (hier wäre übrigens ein weitere Editor, "TeXworks", dabei)
- In Visual Studio Code folgende Extensions herunterladen: "LaTeX Workshop", optional: "LTeX+" (Rehtschreibprüfung)
- irgendwie die Fehler lösen

Ich habe folgenden Code in `settings.json` kopiert. Damit wurden mir alle kompilierten Files in den ".output"-Ordner gelegt (wo dann auch die PDF ist) und der Arbeitsplatz bleibt gut sortiert. Außerdem wird gleich richtig mit biber kompiliert und so, das war auch ziemlich lustig.
Aber Achtung: Möglicherweise funktioniert es auf anderen Systemen nicht oder kann Fehler verbergen.

`"latex-workshop.latex.tools": [
  {
    "name": "pdflatex",
    "command": "pdflatex",
    "args": [
      "-synctex=1",
      "-interaction=nonstopmode",
      "-file-line-error",
      "-output-directory=.output",
      "%DOC%"
    ]
  },
  {
  "name": "biber",
  "command": "biber",
  "args": [
      "--input-directory=.output",
      "--output-directory=.output",
      "%DOCFILE%"
  ]
  }
],
"latex-workshop.latex.recipes": [
  {
    "name": "pdflatex - biber - pdflatex*2",
    "tools": ["pdflatex", "biber", "pdflatex", "pdflatex"]
  },
  {
    "name": "pdflatex only",
    "tools": ["pdflatex"]
  }
],
"latex-workshop.latex.autoBuild.run": "onSave",
"ltex.additionalRules.motherTongue": "de-DE",
"ltex.diagnosticSeverity": "information",
"workbench.editorAssociations": {
    "*.pdf": "latex-workshop-pdf-hook"
},
"latex-workshop.view.pdf.viewer":"tab",
"latex-workshop.view.pdf.sidebar.view": null,
"editor.wordWrap": "on",
"latex-workshop.latex.outDir": ".output",
"latex-workshop.latex.clean.enabled": true,
"latex-workshop.latex.clean.fileTypes": [
    "*.aux",
    "*.bbl",
    "*.bcf",
    "*.blg",
    "*.log",
    "*.lof",
    "*.lot",
    "*.fls",
    "*.fdb_latexmk",
    "*.toc",
    "*.synctex.gz"
],
"ltex.language": "de-DE"
}`

#### Zotero:
- Download: https://www.zotero.org/
- Programm und Browser-Connector
- Über den Zauberstab dann alle Quellen (ISBN z.B.) eintragen oder über den Connector dann in die richtige Bibliothek kopieren
- Tipp: Mit den Bibliotheken Quellen einfacher verwalten.
- Fun Fact: In Word kann Zotero auch eingebunden werden.

- Exportieren s. Literaturverzeichnis :)

### Unter einstellungen.tex alle wichtigen Daten eintragen.
- Auch ob Sperrvermerk und sowas benötigt wird.

### Kapitel jeweils schreiben und einzeln in Ordner Kapitel einfügen.
- `\\` macht einen Zeilenumbruch
- `\glqq{}Beispiel\grqq{}` setzt das Wort in die deutsche Variante der Anführungszeichen
- eine Leerzeile erstellt einen neuen Absatz
- `\section{Titel}` für Überschrift nicht vergessen
- `\subsection{Titel}` für Untersüberschrift (`\subsubsection{Titel}` usw.)
- `\paragraph{Titel}` für Teilüberschriften (auch hier sub, subsub, etc.)
- mit Stern werden sie nicht im Inhaltsverzeichnis aufgeführt `\section*{}`

- nicht vergessen, in `main.tex` einzufügen, `\include` für neue Seite, `\input` wäre ohne neuer Seite 
    (Durchnummerieren der Dateien verschafft Übersicht ^^")

### Einfügen von PDF: `\includepdf[pages=-]{anhang.pdf}`
- `pages=-` bedeutet alle Seiten einfügen
- Bestimmte Seiten: `pages=1-3` --> Seiten 1 bis 3
- `pages={1,3,5}` --> Nur diese Seiten
- Optional: `pagecommand={}` entfernt die Seitenzahlen
- Pfad beachten!

### Literaturverzeichnis:
- über Zotero sammeln
- Bibliothek exportieren als BibTeX unter >>`Datei --> Bibliothek`<< exportieren
- als `literatur.bib` in >>`frontbackmatter/bib/`<< einfügen

Hinweis: Es muss unbedingt überprüft werden, ob alle Quellen korrekt formatiert werden. Angepasst wurden nur echte Bücher (so solche mit Papier und Einband und so) und URLs. Zeitschriften, Gesetzestexte usw. werden möglicherweise falsch dargestellt, das muss evtl. noch angepasst werden.
Ich bin nur mittlerweile ein bisschen durch mit LaTeX und solche Quellen sind eher seltener. ^^" In der Theorie und mit ein bisschen Glück werden sie auch so richtig angezeigt.

### Zitieren:
- `\footnote{text}` gibt Text in die Fußnote

- `\fn[Vorzitatstext]{Schlüssel}[erster Buchstabe Autor][Seitenzahl]`
    - für alle Literaturangaben, die in bib-Datei ein year-Feld haben
  
    - Vorzitatstext wie "Vgl.", "siehe auch", "in Anlehnung an"; schreibt das vor die Fußnote
    - Schlüssel... unter dem Schlüssel die Quelle zu finden ist
    - erster Buchstabe Autor (geht leider nicht automatisch (nur Buchstabe angeben!))
    - Seitenzahl
    - alles in eckigen Klammern kann auch weggelassen werden, sieht dann trotzdem schön aus (Ausnahme: wenn Seitenanzahl angegeben, aber kein Buchstabe, dann darf nicht weggelassen werden)
 
    - mit `\quelle` und derselben Syntax lässt sich die Quelle wie in der Fußnote nur im Fließtext angeben

- `\fnlink[Vorzitatstext]{label}[erster Buchstabe Autor][Seitenzahl]`
    - wie \fussnote, aber für Literaturangaben, die kein year-Feld haben, hauptsächlich gedacht für Links/URLs
    - Unterschied: setzt als Jahr dann das gesetzte Jahr unter einstellungen.tex ein (Gefordert ist das Abrufjahr, da wäre es schlau, wenn es das Jahr der Abgabe ist)

    - mit `\quellelink` und derselben Syntax lässt sich die Quelle wie in der Fußnote nur im Fließtext angeben

### Quellenangaben unter Bildern
Dafür einfach den vorgesehenen `\quelle`-Befehl nutzen und als Bildunterschrift `\caption[OSI-Schichtmodell]{OSI-Schichtmodell\\\quelle[Quelle:]{loebeOSI}[D]}` schreiben. Nur das in `[Das hier]` taucht im Abbildungsverzeichnis auf.

### Anhänge
Jeden Anhang wieder unter >>`/Kapitel/`<< als Datei anfügen, am besten Dateinamen mit A anfangen für Übersicht ^^"

Unter >>`/frontbackmatter/anhaenge.tex`<< dann hinzufügen wie die Kapitel (`\include`).

### Abkürzungsverzeichnis
Unter >>`frontbackmatter/abkuerzungsverzeichnis.tex`<<:

    \begin{acronym}[längste Abkürzung]
        \acro{Schlüssel}[Abkürzung]{Volle Schreibweise der Abkürzung}
        \acro{Schlüssel}[Abkürzung]{Volle Schreibweise der Abkürzung}
    \end{acronym}

Im Text:`\ac{Schlüssel}`    --> Die erste Abkürzung: Lange Schreibweise (Abkürzung), bei jeder weiteren Erwähnung: nur Abkürzung
        `\acl{Schlüssel}`   --> Nur die lange Form
        `\acs{Schlüssel}`   --> Nur die kurze Form
        `\acf{Schlüssel}`   --> Als wäre es die erste Erwähnung

### Glossar

    \newglossaryentry{Schlüssel}
    {
        name=Begriff,
        description={Definition oder Begriffserklärung}
    }

Im Text einfach "`\gls{Schlüssel}`".

!Achtung: keine Erwähnung im Leitfaden

### Abkürzungsbefehle
Weil man ja grundsätzlich wenig schreiben will, kann man auch eigene Befehle bauen, um lange Wörter nicht schreiben zu müssen.
Dafür einfach unter einstellungen.tex unter "eigene Kurzbefehle" den Befehl definieren und nutzen.
Beispiel:
`\newcommand{\dhsn}{\ac{dhsn}}`
Dann kann \dhsn als Kurzbefehl im Text genutzt werden.

Tipp: Ich habe den Befehl \az{Beispiel} eingefügt, um die Anführungszeichen \glqq{}Beispiel\grqq{} einfacher schreiben zu können.

# la-dhsn-tex - ursprüngliche Version:

> **Version: v0.1**   
> **Datum: 09.07.2025**   
> 
> - Louis - Original Template
> - Marius - Minor Changes and Documentation
> 
> Kontakt: s********@edu.dhsn.de
