+++
title = "Computer Networks - Lecture 05"
author = ["eo shiru"]
date = 2019-05-07
lastmod = 2019-05-21T14:49:26+02:00
tags = ["uni", "computer-networks"]
draft = false
+++

## Modems {#modems}

Modem = MOdulator/DEmodulator

-   basieren urspruenglich uaf der klassischen Teilnehmeranschlussleitung des Fernsprechnetzes

{{< figure src="/knowledge-database/images/modem-verbindung.png" >}}

-   **Leistungsanschaltung** = signaltechnische (Sende- und Empfangs-) Verstaerkung der zu uebertragenen Signale
-   **Modulationsteil** = Modulation/Demodulation (zB Amplitude, Frequenz, ...)
    -   Modulation beschreibt einen Vorgang, bei dem ein zu uebertragendes Nutzsignal einen sog. Traeger veraendert (moduliert), dadurch wird eine hochfrequente Uebertragung des niederfrequenten Nutzsignals ermoeglicht
    -   die Nachricht wird empfangsseitig durch einen Demodulator zurueck gewonnen, beispielsweise um ein Audiosignal in UKW-Empfaengern aus dem hochfrequenten, frequenzmodulierten Signal (87,5-108 MHz) zurueckzugewinnen (Radio)
-   **Steuer-/Meldeteil** =  Analyse der vom Netz kommenden Dienstsignale, An-/Abschaltung des Modems an Steuerfunktionen (zB Betriebs-, Sendebereitschaft)


### Mehrfachnutzung von Medien {#mehrfachnutzung-von-medien}

**Multiplexverfahren** (lat. multiplex „vielfach, vielfältig“) sind Methoden zur Signal- und Nachrichtenübertragung, bei denen mehrere Signale zusammengefasst (gebündelt) und simultan über ein Medium (Leitung, Kabel oder Funkstrecke) übertragen werden. Oftmals werden Multiplexverfahren auch kombiniert, um eine noch höhere Nutzung zu erreichen. Die Bündelung erfolgt, nachdem die Nutzdaten auf ein Trägersignal moduliert wurden. Entsprechend werden sie beim Empfänger nach der Entbündelung (dem Demultiplexen) demoduliert.

**Raummultiplex-Verfahren**

-   Uebertragunskanaele (Leitungen, Richtfunkstrecken) werden zu parallelen, aber exklusiven Nutzung durch mehrere Sender und Empfaenger gebuendelt
-   Buendelung von Adern(paaren)
-   Analogie: Personen sprechen an verschiedenen Orten miteiander, bei genuegend grossem Abstand stoeren sich die Gespraeche nicht gegenseitig

**Zeitmultiplex-Verfahren**

-   mehrere Signale werden zeitversetzt uebertragen
-   die Signale sind zeitlich ineinander verschachtelt
-   Zeitfenster koennen synchronisiert und gleich lang oder asynchron und bedarfsabhaengig sein
-   Aufteilung in Zeitschlitze
-   Analogie: in einer Schulkklasse oder beim Funken hat in der Regel nur je ein Sprecher gleichzeitig das Wort (asynchrones Zeitmultiplexverfahren), in Parlamenten hat jeder Redner eine Redezeit definierter Laenge (synchrones Zeitmultiplexverfahren)

**Frequenzmultiplex-Verfahren**

-   mehrere Signale weden in unterschiedlichen Frequenzbereichen uebertragen
-   _Wellenlaengenmultiplex_ (Sonderform Frequenzmultiplex)
    -   bei Funk- und Lichtwellenleiteruebertragung werden unterschiedlichen Signalen unterschiedliche Wellenlaengen zugewiesen
    -   Uebertragung unterschiedlicher Wellenlaengen ("Farben") ueber eine Glasfaser
    -   momentan bis zu 80 verschiedene Wellenlaengen moeglich
    -   innerhalb einer Wellenlaenge ist zudem Zeitmultiplexverfahren moeglich (derzeit Datenraten von 80 GBit/s pro Wellenlaenge erreichbar, was theoretisch 6,4 TBit/ pro Faser entspricht)
-   Analogie: Eine Hundepfeife oder Fledermaeuse erzeugen fuer den Menschen unhoerbare Signale, eine parallele Kommunikatoin ist moeglich

**Codemultiplex-Verfahren**

-   wird in Funktechnik (drahtlose Netze) und Datenbussen eingesetzt
-   verschiedene Signalfolgen werden ueber eine Leitung oder eine Funkfrequenz uebertragen & koennen anhand ihrer unterschiedlichen Codierung zugeordnet werden
-   im Empfaenger wird nur das passend kodierte Signal erkannt und ausgewertet
-   Analogie: bei vielen gleichzeitig gesprochenen Sprachen hoert man seine Muttersprache heraus; bekannte Personen erkennt am Klang ihrer Stimme


### Uebertragungsverfahren und Umformung (Modulationsteil) {#uebertragungsverfahren-und-umformung--modulationsteil}

Zur Uebertragung erforderlich:

-   Umformung des primaeren Quellsignals in das Eingabesignal des Mediums
-   Rueckformung des Ausgabesignals in das primaere Senkensignal

Bei **digitalen** Kanaelen wird im Wesentlichen unterschieden zwischen:

**1. Basisbanduebertragung**:<br />
Unter einer Basisbandübertragung wird die Übertragung eines Zeitsignals in jenem Frequenzbereich verstanden, welches das Zeitsignal als Frequenzspektrum aufweist. Dieses Frequenzspektrum wird dann auch als Basisband bezeichnet. Bei digitalen Übertragungen kann durch die Leitungscodierung Einfluss auf die Eigenschaften des übertragenen Frequenzspektrums im Basisband genommen werden. Die Leitungscodierung ist die Abbildungsvorschrift, dafuer wie das digitale Primaersignal auf das digitale Uebertragungssignal abgebildet wird.
Im einfachen Verfahren unterscheidet man zwischen Einfachstrom (zB 0 = 0V, 1 = 5V) und Doppelstrom-Verahren (zB 0 = -5V, 1 = 5V). Die angestrebten Eigenschaften einer   Leitungscodierung:

-   Eliminierung des Gleichstromanteils
-   Wiedergewinnung des Takts aus dem Signal (selbsttaktender Signalcode)
-   Erkennung von Signalfehlern auf Signalebene

Slides: digitale elektrische Uebertragung, bei der das zu uebertragende Signal direkt auf das Medium gelangt; das Signal beansprucht bei diesem Verfahren die gesamte Bandbreite des Mediums

Beispiel Manchester Codierung:<br />

-   Doppelstromverfahren
-   jedes zu uebertragene Bit (0 oder 1) wird in _zwei Uebertragungsschritte_ gleicher Dauer aufgeteilt
    -   1 = High-Low
    -   0 = Low-High

{{< figure src="/knowledge-database/images/manchester-codierung.png" >}}

**2. Breitbanduebertragung**:<br />
Die Bandbreite eines Mediums wird in verschiedene Kanaele aufgeteilt (vgl Frequenz-/Zeitmultiplex)

-   Nutzung der unterschiedlichen Kanaele durch Aufpraegung des Quellsignals auf harmonische Traegerschwingung, Umformung digital <-> analog
    -   Amplitudentastung (Amplitudenmodulation)
    -   Frequenztastung (Frequenzmodulation)
    -   Phasentastung (Phasenmodulation)

-    Amplitudenmodulation

    -   Uebertragung mittels Basisbanduebertragung nicht moeglich
        -   primaeres Signal wird durch Amplitudenveraenderung auf Traegersignal moduliert
    -   Amplitudenmodulation ist sehr stoeranfaellig
    -   Erkennung langer Folgen von 1 erfordert sehr stabile Taktgeneratoren

    {{< figure src="/knowledge-database/images/amplitudenmodulation.png" >}}

-    Frequenzmodulation

    -   primaeres Signal wird durch gezielte Aenderung der Traegerfrequenz moduliert
    -   Frequenzmodulation ist das unter anderem auch bei UKW-Rundfunk eingesetzte Modulationsverfahren

    {{< figure src="/knowledge-database/images/frequenzmodulation.png" >}}

-    Phasenmodulation

    -   primaeres Signal wird mittels gezielter Phasenspruenge des Traegersignals moduliert
    -   CCITT-Empfehlung:
        -   0 = Phasendrehung um 180 Grad
        -   1 = keine Phasendrehung
    -   Phasenmodulation ist das beste, aber auch aufwendigste Verfahren

    {{< figure src="/knowledge-database/images/phasenmodulation.png" >}}

-    Quadratur-Amplituden-Modulation (QAM)

    -   Kombination  aus Amplituden- und Phasenmodulation
        -   gleichzeitige Uebertragung von mehreren Bits pro Uebertragungsschritt
        -   Verwendung von mehreren Amplituden und Phasenzustaenden
        -   bis zu 10 Bit pro Schritt uebertragbar (1024)
        -   Stoeranfaelligkeit steigt mit Anzahl der zu uebertragenen Bits pro Schritt
    -   Beispiel: 2 Amplituden, 8 Phasen &rarr; 16 Zustaende = 4 Bit

    {{< figure src="/knowledge-database/images/qam.png" >}}


### Technologien zur Datenuebertragung ueber Telefonleitungen {#technologien-zur-datenuebertragung-ueber-telefonleitungen}


#### DSL (Digital Subscriber Line) {#dsl--digital-subscriber-line}

-   hochratige, digitale Datenuebertragung zwischen Teilnehmern und Vermittlungsstellen ueber installierte Kupferadern des Telefonnetzes (2-adrig, Twisted Pair)
-   Uebertragungsraten im Bereich von Mbit/s
-   Koexistenz von DSL und bestehenden Telefonsystem (analoges Telefonnetz und ISDN)
-   Grundidee: Ausnutzung der gesamten Bandbreite des Uebertragungsmediums
-   es gibt unterschiedliche Realisierungen der DSL-Technik {x}DSL:

-    SDSL (Symmetric DSL)

    -   HDSL ueber Zweidraht-Leitung, 760 kbit/s

-    TDSL (Stand ca 2002)

    -   768 kbit/s downstream
    -   128 kbit/s upstream

-    HDSL (High Bitrate DSL)

    -   1.5 Mbit/s ueber Viedraht-Leitung

-    RADSL (Rate Adaptive DSL)

    -   2.2 Mbit/s Downstream
    -   1.1 Mbit/s Upstream

-    ADSL (Asymmetric Digital Subscriber Line)

    -   Grundprinzip: "hohe Bitrate zum Teilnehmer, niedrige Bitrate vom Telnehmer"
    -   Datenraten:
        -   max 8 Mbit/s Downstream (je nach Entfernung & Leitungsqualitaet)
        -   max 640 kbit/s Upstream (je nach Entfernung & Leitungsqualitaet)
    -   Reichweite in der Praxis 5000-6000m
        -   in DE ist die durchschn. Laenger einer Teilnehmeranschlussleitung 2km

-    VDSL (Very High Bitrate DSL)

    -   13-52 Mbit/s Downstream, bis 6 Mbit/s Upstream
