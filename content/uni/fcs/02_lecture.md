+++
title = "Einführung in die Funktion von Computersystemen - Lecture 02: Von der Schaltungslogik zur Informationsverarbeitung"
author = ["eo shiru"]
date = 2019-10-08
lastmod = 2019-11-26T13:09:24+01:00
tags = ["uni", "funktion-computersysteme"]
draft = false
+++

## Schaltungslogik {#schaltungslogik}

{{< figure src="/knowledge-database/images/schaltungslogik.png" >}}


## Schaltnetze {#schaltnetze}

Sogenannte Gatter implementieren die booleschen Funktionen. Signale werden dabei durch elektrische Spannungen dargestellt. Durch die Zusammensetzung von Gattern entstehen Logikschaltungen. Ein Schaltnetz ist eine Zusammensetzung von Verknüpfungsschaltungen ohne Speicherverhalten.<br />
![](/knowledge-database/images/gatter.png)<br />
Durch logisch vollständige Funktionen können beliebige andere Funktionen "gebaut" werden. Zum Beispiel lässt sich die XOR-Funktion (\\(x \oplus y\\)) durch NAND-Gatter wie folgt realisieren:<br />
![](/knowledge-database/images/xor-nand.png)<br />


### Optimierung {#optimierung}

Eine Schaltung ist nicht immer optimal (zB zuviel Gatter), daher gibt es Methoden zur Optimierung, die da wären Verfahren nach McCluskey und das Verfahren nach Karnaugh und Veitch, welches wir im folgenden betrachten.


#### Karnaugh & Veitch - Diagramm {#karnaugh-and-veitch-diagramm}

-   Funktionswerte werden in einer Matrix so angeordnet, dass sich benachbarte Felder um einen Funktionsparameter unterscheiden
-   Matrix ist als Oberfläche eines Torus aufzufassen (Ränder oben/unten sowie rechts/links sind verbunden)
-   Werte werden in Matrix eingetragen, unbekannte (don't care) werden mit &phi; bezeichnet
-   Blöcke gleichen Werts zu Rechtecken mit 2^n Kantenlänge zusammenfassen
-   Block entspricht Ausdruck, der von weniger Variablen abhängig ist

![](/knowledge-database/images/kv-beispiel.png)<br />


## Schaltwerk {#schaltwerk}

-   ein Schaltwerk zeigt (im Gegensatz zum Schaltnetz) **Speicherverhalten** (&rarr; es besitzt einen Zustand)
-   durch Eingangssignale ändert sich der Zustand der Schaltung
-   einfachstes Speicherelement speichert die Zustände 0/1 &rarr; Flip-Flop

![](/knowledge-database/images/schaltwerk.png)<br />


#### Taktsteuerung / Binäre Signale {#taktsteuerung-binäre-signale}

-   Abstraktion von Signalübergängen: Takt
    -   Signale können dann als Rechtecksignale betrachtet werden
-   synchrone Schaltungen: hier D-Flip-Flop (auch "Latch")
-   auf die fallende Flanke an T wird der an D anliegende Datenwert (0 oder 1) "eingefroren"

![](/knowledge-database/images/d-flip-flop.png)<br />

Beispiel: Master-Slave-Flip-Flop<br />

-   mit der steigenden Flanke des Taktes wird der gesetzte Wert in die erste Stufe übernommen
-   derweil liegt am Ausgang noch der alte Wert an
-   mit der fallenden Flanke des wird die Eingangsstufe "versiegelt" und der gesetzte Wert in die zweite Stufe (und den Ausgang) geschrieben

![](/knowledge-database/images/master-slave-flip-flop.png)<br />


#### Zustandsbehaftete vs zustandslose Schaltungen {#zustandsbehaftete-vs-zustandslose-schaltungen}

-   in zustandslosen Schaltungen hängt das Ausgangssignal auschließlich vom Eingangssignal ab
-   dagegen geht in das Ausgangssignal von zustandsbehafteten Schaltungen die "Vorgeschichte" ein, die sich im Zustand manifestiert (hat) (&rarr; Automatenmodell)

![](/knowledge-database/images/zustandslos-zustandsbehaftet.png)<br />

-    Register

    -   Speicherzellen für logisch zusammengehörende Einzelbits werden zu **Registern** zusammengefasst
    -   Register bestehen aus unverbundenen Flip-Flops, die gemeinsam getaktet werden

    ![](/knowledge-database/images/register.png)<br />

-    Arbeitsspeicher

    -   abstraktes Modell: lineare Liste von Speicherzellen, auf die unter Angabe einer Adresse zugegriffen werden kann
    -   Halbleiterspeicher: Millionen von Flip-Flops auf einem Chip
    -   das Zugangssignal für jede Speicherzelle wird aus dem Wert, der auf den Adressleitungen liegt, decodiert
    -   typisches Beispiel:
        -   m = 32 (Datenwortbreite ist 32 Bit)
        -   n = 24 (es können 2^24 = 16 M Worte angesprochen werden)

    ![](/knowledge-database/images/arbeitsspeicher.png)<br />

    Neben dem hier beschriebenen Prinzip des Speicherns durch Flip-Flops gibt es auch andere Speichertechnologien. Die Struktur (Speicherzellen, Zusammenfassung zu einem Speicherwort, Addressierung über Adressdecodierer) ist aber immer gleich.

-    Schieberegister

    -   mit jedem Takt wird das Datenwort um eine Stelle verschoben und das in in das "freigewordene" Bit der Wert des Eingangsbits \\(d\_{in}\\) geschrieben:
        -   \\(t=t\_0\\), \\(d\_{in} = 1\\): \\(D=0000\\)
        -   \\(t=t\_0+1\\), \\(d\_{in} = 1\\): \\(D=0001\\)
        -   \\(t=t\_0+2\\), \\(d\_{in} = 0\\): \\(D=0011\\)
        -   \\(t=t\_0+3\\), \\(d\_{in} = 1\\): \\(D=0110\\)
        -   \\(t=t\_0+4\\), \\(d\_{in} = 1\\): \\(D=1101\\)
    -   ist \\(d\_{in} = 0\\)  so entspricht das Schieben in Richtung MSB (most significant Bit) einer **Multiplikation** mit 2, das Schieben in Richtung LSB (least significant Bit) einer Division mit 2

    ![](/knowledge-database/images/schieberegister.png)<br />


#### Addition {#addition}

-   Addition zweier Binärzahlen kann durch logische Verknüpfungen realisiert werden (&rarr; **Halbaddierer/Half-Adder**)

![](/knowledge-database/images/halfadder.png)<br />

-   Berechnung von \\(x+y=z+\\) Übertrag (carry)
-   Übertrag zur naechsthoeheren Stelle

Zur vollständigen Addition einer Stelle zweier mehrstelliger Dualzahlen muss noch der Übertrag \\(c\_{in}\\) von vorheriger Stelle berücksichtigt werden (**Volladdierer**, **Full Adder**)<br />
![](/knowledge-database/images/fulladder.png)<br />

Durch Zusammenschaltung mehrerer Volladdierer können zwei Datenworte **beliebiger Breite** addiert werden (**n-Bit-Addierer**).

-   Daten werden dabei als Zahlen im Zweierkomplement aufgefasst
-   das Carry-Bit muss immer zur nächsthöheren Stufe weitergereicht werden
    -   es muss das "Einschwingen/pendeln" aller n Stufen abgewartet werden, ehe das Ergebnis gültig ist
-   es gibt spezielle Schaltungen, um die Überträge "vorherzusagen" und damit die Addition zu beschleunigen (schnelle Addierer)

![](/knowledge-database/images/n-bit-addierer1.png)
![](/knowledge-database/images/n-bit-addierer2.png)<br />


#### Akkumulation {#akkumulation}

-   Addition mehrerer Zahlen zusammengeschalteter Addierkette und Register (**Akkumulator**)
-   fortgesetzte Addition beliebig vieler Zahlen möglich
-   Akkumulator sammelt Ergebnisse vorheriger Additionen

{{< figure src="/knowledge-database/images/akkumulator.png" >}}

Vorgehen:

1.  AC löschen (AC := 0)
2.  Addiere x zum Inhalt von AC (AC := AC + x)
3.  wenn nicht fertig, gehe zu 2)


#### Multiplikation {#multiplikation}

-   "Papier & Bleistift" Methode:

{{< figure src="/knowledge-database/images/papier-bleistift.png" >}}

**Multiplikationsarray**

-   das Papier & Bleistift Verfahren wird nachempfunden
    -   Produkt ist die Summe von Partialprodukten
-   Nachteil: sehr hoher Aufwand, für n Bit Multiplikation werden n^2 Zellen gebraucht

{{< figure src="/knowledge-database/images/multiplikationsarray.png" >}}

**Multiplikation mit Schieberegister**

-   Faktoren in \\(f\\) und \\(g\\)
-   \\(a\\) und \\(c = 0\\)
-   wenn \\(g\_0\\) gleich \\(1\\) ist wird \\(f\\) nach \\(a\\) geladen
-   das Doppelregister \\(ag\\) wird um eine Stelle nach rechts geschoben, das LSB des Ergebnis steht damit in \\(g\_{n-1}\\)
-   wenn (das neue) \\(g\_0\\) gleich \\(1\\) ist wird \\(f\\) zu dem Wert in \\(a\\) addiert und das Ergebnis nach \\(a\\) geladen
-   das Doppelregister \\(ag\\) wird um eine Stelle nach rechts geschoben etc
-   das Gesamtergebnis steht am Ende in \\(ag\\)

{{< figure src="/knowledge-database/images/mult-schieberegister.png" >}}
