+++
title = "Einführung in die Funktion von Computersystemen - Lecture 01: Informationen und ihre Darstellung"
author = ["eo shiru"]
date = 2019-10-01
lastmod = 2019-12-03T10:14:40+01:00
tags = ["uni", "funktion-computersysteme"]
draft = false
+++

Äquivalente Informationen können in vielen verschiedenen Darstellungen übermittelt werden. Eine symbolische Darstellung entspricht dabei der Syntax, welche via Interpretation Semantik (Bedeutung) erlangt. Bei unbekannten Interpretationsregeln kann Bedeutung jedoch nicht erkannt werden.<br />
Eine symbolische Darstellung erfolgt mittels Symbolen. Jene Symbole werden in einem Alphabet festgehalten. Ein **Alphabet** ist eine **endliche Menge** von **Symbolen**. Das kleinste Alphabet ist das der Binärzahlen \\({0,1}\\). Mit sogenannten Binärcodes also Zusammensetzungen der Symbole \\(0\\) und \\(1\\) lassen sich die Symbole aller denkbaren Alphabete ausdrücken (Binärcodierung).


## Codes für Zahlen {#codes-für-zahlen}


### Vorzeichenlose Dualzahlen {#vorzeichenlose-dualzahlen}

Was ist der Wertebereich einer n-stelliger Dualzahl?

-   Wertebereich: $0..(2^n)-1$<br />

In C beispielsweise gebräuchlich: `char` (\\(n=8\\), 0..255), `int`, `short` (\\(n=16\\), 0..65535) etc


### Vorzeichenbehaftete Dualzahlen {#vorzeichenbehaftete-dualzahlen}

Doch wie lassen sich vorzeichenbehaftete Zahlen in binärschreibweise darstellen?

-   Idee: 1 Bit dient der Darstellung des Vorzeichens (sign + value)
    -   \\(+5=00..0101\\)
    -   \\(-5=10..0101\\)
-   Wertebereich: \\(-(2^(n-1)-1)..2^(n-1)-1\\)
-   Problem:
    -   keine eindeutige Darstellung für 0 (\\(1000\\) oder \\(0000\\)?)
    -   Rechenwerk für Addition schwierig


#### Lösungsansätze zur Darstellung von vorzeichenbehafteten Zahlen {#lösungsansätze-zur-darstellung-von-vorzeichenbehafteten-zahlen}

-    Einerkomplement

    -   positiver Wert wird wie bisher dargestellt
    -   der negative Wert entsteht durch Umkehren (negieren) aller Bits
    -   \\(+5=00..0101\\) &rarr; \\(-5=11..1010\\)
    -   Eigenschaften:
        -   nicht eindeutig
        -   Addition/Subtraktion problematisch bei Vorzeichenwechsel

-    Zweierkomplement

    -   ist die häufigsten Darstellung und vermeidet Nachteile von 1er Komplement/Vorzeichenzahl
    -   positive Zahlen: werden wie bisher dargestellt
    -   negative Zahlen: zuerst 1er Komplement bilden und dann 1 addieren
    -   \\(+5=00..0101\\) &rarr; \\(-5=11..1011\\)
    -   Eigenschaften:
        -   nur eine Darstellung der 0 (eindeutig)
        -   unsymmetrischer Wertebereich \\(-2^(n-1)..2^(n-1)-1\\)


### Gebrochene Zahlen {#gebrochene-zahlen}

-   bisher nur ganze Zahlen (Komma steht ganz rechts)
-   Idee: Darstellung einer gebrochenen Zahl durch zwei ganze Zahlen
-   Problem:
    -   Wertebereich versus Genauigkeit
    -   evtl werden Bits verschenkt
-   Idee: Kommaposition verschiebbar &rarr; Speichern von Informationen über die Position des Kommas &rarr; **Gleitkommazahl**:

\\[z = (-1)^s \* m \* b^e\\]

-   \\(s\\) = Vorzeichen (sign)
-   \\(m\\) = Mantisse (Ziffernstellen einer Gleitkommazahl vor der Potenz, zB bei \\(2,9979 · 10^8\\) ist \\(2,9979\\) die Mantisse)
-   \\(b\\) = Basis (zB 2, 10, 16)
-   \\(e\\) = Exponent

bei bekannter Basis genügt es \\(s\\), \\(m\\) und \\(e\\) zu speichern


#### Normalisierung von Gleitkommazahlen {#normalisierung-von-gleitkommazahlen}

-   Problem: Gleitkommazahlen sind nicht eindeutig
-   Beispiel wie eine Dezimalzahl (\\(123\\)) umgeformt werden kann:
    -   \\(123 = 123 \* 10^0 = 12.3 \* 10^1 = 1.23 \* 10 ^ 2 = 0.123 \* 10^3 = 0.0123 \* 10^4\\)

-   daher werden Gleitkommazahlen **normalisiert**, das heißt dass sie so umgeformt wird, dass in der Mantisse die erste Ziffer die keine Null ist direkt links vor dem Komma steht
    -   im obigen Fall wäre dies \\(1.23 \* 10^2\\)
-   außerdem ist bei Binärzahlen die erste Stelle immer eine 1, kann im obigen Fall also weggelassen werden
    -   zB wird Mantisse \\(1.0110\\) als $.0110$ dargestellt

Es gibt außerdem verschiedene Präzisionsformen von Gleitkommazahlen (IEEE 754)
![](/knowledge-database/images/precision-gleitkommazahlen.png)

Probleme können jedoch beim Umgang mit ganzen Zahlen in Gleitkommarepräsentation auftreten (Ungenauigkeiten/Rundungsfehler)


## Aussagelogik {#aussagelogik}

-   Teil der Logik, in dem Eigenschaften von Aussagen, die mittels Aussagenverknüpfungen aus anderen Aussagen entstehen, untersucht werden
-   jede Aussage hat einen Wahrheitswert
-   **Prinzip der Zweiwertigkeit**
    -   jede Aussage hat entweder den Wert **wahr** oder **falsch**
-   **Satz vom ausgeschlossenen Dritten** (tertium non datur)
    -   jede Aussage ist immer entweder wahr oder falsch
-   **Satz vom ausgeschlossenen Widerspruch**
    -   keine Aussage ist zugleich wahr und falsch
-   **Prinzip der Extensionalität**
    -   der Wahrheitswert einer Aussageverknüpfung hängt ausschließlich von den Wahrheitswerten ihrer Bestandteile ab

Operatoren der Booleschen Algebra: ∧, ∨, ¬

![](/knowledge-database/images/boolsch-1.png)<br />
![](/knowledge-database/images/boolsch-2.png)<br />
![](/knowledge-database/images/boolsch-3.png)<br />
![](/knowledge-database/images/boolsch-4.png)

---

Additional Resources:

-   One's and Two's Complement: <https://www.youtube.com/watch?v=Z3mswCN2FJs>
-   Fractions in Binary: <https://www.youtube.com/watch?v=Y4Q9PnjKhac>
-   Floating Point Binary Representation: <https://www.youtube.com/watch?v=L8OYx1I8qNg> (try the exercises)
