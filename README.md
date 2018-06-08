# TI-Praktikum SoSe18

## Praktikum 1

[Aufgabenstellung](p1/Aufgabenstellung.pdf)

Ziel des ersten Praktikumversuches ist die Entwicklung eines Parsers für WHILE0-Programme mittels JavaCC.

Wie in Übungsaufgabe 13 sind WHILE0-Programme WHILE-Programme (Definition siehe Skript), in denen Wertzuweisungen auf solche der Form `V1 = V2 + 1` und `V1 = 0` beschränkt sind und weitere Anweisungen nur durch Sequenz `α1; α2` und Wiederholungsanweisungen der Form `while V1 != V2 do begin α end` konstruiert sind.

### Aufgabe 1
Geben Sie zunächst ein Syntaxdiagramm zur Beschreibung der Syntax von WHILE0-Programmen an.

Lösung:
* [while0parser.ebnf](p1/diagram/while0parser.ebnf)
* [while0parser.xhtml](https://cdn.rawgit.com/albertkasdorf/TI-Praktikum/170038b9/p1/diagram/while0parser.xhtml)

Erstellen Sie eine .JJ-Datei, die den Scanner (Tokendefinition als Reguläre Ausdrücke) und Parser (Typ 2-Grammatik in javaCC-Notation) enthält.

* Eine Dokumentation zu JavaCC finden Sie z.B. hier: https://javacc.org/doc
* In der .JJ-Datei definieren Sie die Syntax der While0-Sprache mittels EBNF-ähnlichen Regeln, die Regeln können mit beliebigem Java-Code erweitert werden.
* Definieren Sie zuerst sinnvolle Token als reguläre Ausdrücke zur Beschreibung der lexikalischen Konstrukte, z.B. NUMBER, IDENT usw.
* Anschließend erstellen Sie die Regeln für die Syntax der WHILE0-Sprache. In JavaCC werden Regeln als Methoden implementiert (in diesem Versuch ist es noch nicht zwingend notwendig die Regeln mit Java-Code zu erweitern).
* Im letzten Schritt muss in der .JJ-Datei ein Hauptprogramm geschrieben werden, das ein WHILE0-Programm einliest und dazu eine Ausgabe liefert im ersten Praktikum soll die Ausgabe nur beinhalten, ob das eingegebene Programm.
* Ein gültiges While0-Programm ist (JA/NEIN). Im zweiten Praktikum liefert die Ausgabe dann den in URM übersetzten Quellcode.

Lösung:
* [while0parser.jj](p1/while0parser/src/while0parser.jj)

### Aufgabe 2
Überlegen Sie sich, wie Sie den WHILE0-Befehl `while V1 != V2 do begin α end` in gültige URM-Befehle übersetzen können.

Lösung:
* [a2_while_to_urm.graphml](p1/diagram/a2_while_to_urm.graphml)

![a2_while_to_urm.graphml](p1/diagram/a2_while_to_urm.png)


## Praktikum 2

[Aufgabenstellung](p2/Aufgabenstellung.pdf)

In diesem Versuch sollen Sie aufbauend auf Versuch 1 einen vollständigen Compiler für "WHILE0-" nach "URM-Quellcode" entwickeln.

Dieser URM-Quellcode soll anschließend im URM Simulator korrekt ausführbar sein. Der URM Simulator geht dabei davon aus, dass der (Eingabe-)URM-Quellcode keine Zeilennummern enthält (diese werden implizit vom URM Simulator gesetzt) und dass nur eine endliche, nicht-leere Folge von Befehlen folgt (Programmspeicher).

Der hier zu entwickelnde Compiler für "WHILE0-" nach "URM-Quellcode" ist entsprechend dieser Kriterien zu implementieren.

### Schritt 1
* Erweitern Sie im ersten Schritt die EBNF-Regeln mit Java-Code. Hierfür können Sie den Java-Code direkt an den richtigen Stellen in die Grammatik-Datei schreiben. Vermeiden Sie viel Code innerhalb der Grammatik-Regeln, in dem Sie zusätzlich Hilfsklassen definieren und verwenden.
* Sie müssen die Zuweisungen von Variablen zu den entsprechenden URM-Registern speichern können. Hier bietet sich zur Implementierung einer Symboltabelle eine Hash-Map an, die beim Lesen der Variablendeklarationen aufgebaut und immer als Parameter der rekursiven Methoden weitergereicht wird.
* Sie können den Teil-Code jeweils als Rückgabewert der JavaCC-Methoden übergeben (der Code wird also rekursiv erzeugt, alternativ kann auch eine Hilfsklasse verwendet werden) Hier muss es möglich sein temporäre Sprungmarken einzufügen und später durch Zeilennummern zu ersetzen (siehe Schritt 2).
* Erzeugen Sie für jede WHILE0-Anweisung gemäß der Transformationen im Skript URM-Code. Verwenden Sie dabei zur Übersetzung der While-Schleife Ihre Lösung aus Praktikum 1, Aufgabe 2.
* Überlegen Sie sich sinnvolle Exception-Klassen:

### Schritt 2
Eine Schwierigkeit bei der Entwicklung dieses Compilers ist die Übersetzung der Sprungmarken. URM unterstützt keine Textsprungmarken, sondern nur direkte Zeilennummern als Sprungziele. Ein möglicher Lösungsansatz ist der Folgende:

* Erzeugen Sie in der Ausgabe zunächst temporäre, eindeutige Textsprungmarken (Marker).
* Ersetzen Sie am Schluss der Transformation die Marker durch die richtigen Zeilennummern (überlegen Sie sich hierfür, wie Sie an die Zeilennummern kommen).

Bei der Umsetzung einer Schleife in URM-Code ist demnach folgendes Vorgehen sinnvoll:
* Existieren die verwendeten Identifier überhaupt?
* Ermitteln des zugewiesenen Registers für die Variablen im Schleifenkopf Temporäre Sprungmarken erzeugen.
* URM-Code für die Schleifenstruktur erzeugen.
* Nachträglich: die temporären Sprungmarken durch Zeilennummern ersetzen.
