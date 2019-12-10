+++
title = "Einführung in die Funktion von Computersystemen - Lecture 05: Systemsoftware - Prozesse und Prozesswechsel"
author = ["eo shiru"]
date = 2019-10-29
lastmod = 2019-12-10T10:18:21+01:00
tags = ["uni", "funktion-computersysteme"]
draft = false
+++

## Betriebsssytem {#betriebsssytem}

-   das _Laden_ von Programmen ist ein Dienst, der zur Laufzeit (im Gegensatz zur Übersetzungszeit) die Programmausführung unterstützt
-   es gibt mehrere solcher Dienste und zusammengefasst werden sie zu einem **Betriebssystem**
-   allgemein hat ein Betriebsssytem zwei Aufgaben:
    -   **abstrakter Computer**: es bietet den Programmen "schönere" Interfaces & Abstraktionen als ein realer Computer
    -   **Ressourcenmanager**: es koordiniert den (konkurrierenden) Zugriff auf Ressourcen


### Arten von Betriebsssytem {#arten-von-betriebsssytem}

-   Batch OS (**Stapelbetriebsysteme**)
    -   verschiedene Programme (Jobs) werden _nacheinander_ abgearbeitet
    -   meist keine Interaktion
-   Multitasking OS (**Mehrprogrammbetriebssysteme**)
    -   verschiedene Programme laufen (quasi) gleichzeitig ab (nebenläufig)
    -   Interaktion ist möglich
-   Multiuser OS (**Mehrnutzerbetriebssysteme**)
    -   mehrere Nutzer können gleichzeitig interaktiv am System arbeiten
    -   Time Sharing (Zeitscheibenbetrieb)
-   Real Time OS (**Echtzeit-** oder **Prozesssteuerbetriebssystem**)
    -   Einsatz für Steuerungsaufgaben
    -   bestimmte Zeitanforderungen müssen einhaltbar sein

Heutige Betriebsssyteme vereinen meist mehrere der obigen Aspekte


### Zweiteilung des Betriebsssytem {#zweiteilung-des-betriebsssytem}

-   Nutzer-/Programmiersicht: aktive Programme sind **Prozesse**
-   Prozesse interagieren
-   Prozesse sind **nicht** in Hardware enthalten &rarr; also muss etwas Prozesse und ihre Interaktion zur Verfügung stellen & unterstützen
-   dieses "Etwas" heißt **Kern** (**kernel**) des Betriebsssytems und er stellt die grundlegende Infrastruktur bereit

{{< figure src="/knowledge-database/images/os-zweiteilung.png" >}}

-   Grobgliederung in zwei Bereiche
    -   **Prozessbereich**, in dem die eigentlichen Funktionen erbracht werden
    -   \*Kern\*(bereich), der für die Prozesse die Infrastruktur zur Verfügung stellt


## Prozessbegriff {#prozessbegriff}

-   in dieser Vorlesung verstehen wir einen "Prozess" als:
    -   die Einheit der Ausführung (Aktivität), bei der ein Programm oder ein Teil eines Programms auf einem **virtuellen Prozessor** ausgeführt wird
    -   der "virtuelle Prozessor" wird implizit durch die Betriebssystem-Infrastruktur erzeugt und (idR) nicht extra benannt &rarr; Konzept der **Aktivitätstoken**: Code der die Adresse enthält, auf die der PC zeigt
    -   andere Prozesskonzepte (Adressräume, Ressourcenbehälter, ...) werden hier als nachrangig betrachtet
-   Prozess wird im Betriebsssytem durch eine Datenstruktur repräsentiert, den _process control block_, PCB)

**Warum überhaupt Prozesse?**<br />

-   ein Programm muss sehr häufig warten, meist auf ein Gerät
-   damit die CPU effektiver genutzt werden kann bietet es sich an in diesem Zeitraum den Prozess zu wechseln (Prozessumschaltung, Prozesswechsel, process switching)
-   Prozesswechsel bedeutet also einen Übergang von einer Befehlsfolge in eine andere (Prozesssicht)
    -   aber: Prozessor hat nur eine Befehlsfolge
-   Umschalten: Ändern des Befehlszählers (Register) &rarr; **Dispatching**


## Dispatching {#dispatching}

-   im einfachsten Fall kann man das Umschalten direkt in die Prozesse "einprogrammieren"
-   es wird dann direkt zu einem anderen Prozess _gesprungen_
-   unterbrochener Prozess soll korrekt weitergeführt werden &rarr; _Stelle merken_
-   eine Umschaltstelle besteht also mindestens aus:
    -   **Fortsetzadresse** (wo wurde die Arbeit unterbrochen)
    -   **Sprungbefehl** (wo soll weitergemacht werden)


### Umschalten durch Sprung (cooperative) {#umschalten-durch-sprung--cooperative}

-   für bestimmte Einsatzgebiete (Realzeitsysteme) ist die zum Umschalten benötigte Zeit ein wichtiges Qualitätsmaß
-   das Umschalten muss daher effizient realisiert werden
-   der Sprung ist die Minimallösung

{{< figure src="/knowledge-database/images/prozess-sprung.png" >}}

-   Umschalten durch direkten Sprung ist **wenig flexibel** und daher nur in speziellen einfachen Fällen anwendbar
-   im Allgemeinen wird das Umschalten wesentlich aufwendiger sein, da:
    -   man oft nicht weiß, von wo aus man wieder zu dem unterbrochenen Prozess zurückkehrt &rarr; **Merken der Fortsetzstelle**
    -   der Prozessor wesentliche Teile der Prozessbeschreibung enthält, die nicht verloren gehen dürfen &rarr; **Zustand (Register) umladen**
    -   man nicht weiß, ob der Prozess die Sprungstelle überhaupt erreicht &rarr; **nichtkooperativer Prozesswechsel**
    -   der nächste Prozess, auf den man umschaltet, nicht immer der gleiche ist &rarr; **Scheduling** (nächstes Kapitel)


### Prozesskontext {#prozesskontext}

-   außer dem Befehlszähler enthält der Prozessor in seinen Registern eine Menge weiterer prozessspezifischer Daten:
    -   Inhalte von Rechenregister, Indexregistern etc. die den Zustand der Programmbearbeitung, also des Prozesses repräsentieren
    -   Inhalte von Adressregistern, Segmenttabellen, Unterbrechungsmasken, Zugriffskontrollinformationen etc die die Ablaufumgebung des Prozesses darstellen
-   alles zusammen, also die gesamte im Prozessor abgelegte prozessspezifische Information wird als **Kontext** des Prozesses (**process context**) bezeichnet
-   dieser Prozesskontext muss im Rahmen des Umschaltens gerettet und beim Fortsetzen des Prozesses wieder restauriert werden
-   jener **Kontextwechsel** ist der **aufwendigste** Teil des Umschaltens
-   um ihn zu beschleunigen, kann von der Prozessor-Hardware Unterstützung angeboten werden:
    -   durch spezielle Befehle, mit denen man einen kompletten Registersatz aus dem Prozessor in den Speicher schreibt und umgekehrt
    -   durch Bereitstellung mehrerer Registersätze (zB 8) auf dem Prozessor, so dass beim Umschalten uU nur das Register geändert werden muss, das die Nummer des gültigen Registersatzes angibt
-   Prozesswechsel ist dann relativ schnell, wenn nur die Rechenregister umgeladen werden müssen, also die Adressierungsumgebung dieselbe bleibt (Prozesswechsel innerhalb eines Adressraums, Leichtgewichtsprozesse, Threads)


### Automatisches Umschalten {#automatisches-umschalten}

-   in vielen Fällen ist es nicht möglich oder nicht sinnvoll Umschaltstellen explizit (Umschalten durch Sprung) in die Prozesse einzubauen
-   wünschenswerter wäre ein _automatisches Umschalten_ &rarr; **präemptiv** (preemptive)
-   notwendig hierfür ist eine Intervalluhr oder Wecker (**timer**), d.h. eine Hardware-Einrichtung (E/A-Gerät) mit den folgenden Funktionen:
    -   Vorgabe einer Frist ("Stellen des Weckers")
    -   Unterbrechung bei Fristablauf ("Wecken")
-   beim automatischen Umschalten können die Programme unverändert bleiben
-   das Umschalten wird "von außen ausgelöst" und kann zu jedem beliebigen Zeitpunkt stattfinden (Unterbrechungen dürfen nicht abgeschaltet sein)

{{< figure src="/knowledge-database/images/kooperative-preemptives-umschalten.png" >}}


### Bedingtes Umschalten {#bedingtes-umschalten}

-   im Prozessablauf sind Situationen möglich, in denen vorübergehend nicht weitergearbeitet werden kann zB:
    -   beim Warten auf einzulesende Daten
    -   bei Eintritt in "kritische Abschnitte"
-   "auf der Stelle zu treten" (**busy wait**) ungünstig, da Prozessor solange andere Arbeit erledigen könnte
-   Begriff: **bedingtes Umschalten**
    -   ob umgeschaltet wird oder nicht, hängt davon ab, ob eine Bedingung erfüllt ist oder nicht
-   Bedinung kann durch einfache binäre Variable dargestellt werden

{{< figure src="/knowledge-database/images/umschalt-bedingung.png" >}}

-   wenn ein Prozess auf eine Bedingung wartet erfolgt ein Umschalten auf einen anderen Prozess
    -   evtl ist anderer Prozess ebenfalls nicht lauffähig (zB wartet auf E/A-Operation) &rarr; erneutes Umschalten etc
-   dieses Vorgehen kann zu einem längeren "Ausprobieren" führen
-   um Suche nach einem fortsetzbaren Prozess zu beschleunigen, werden Prozesse nach ihrem Zustand (fortsetzbar, nicht fortsetzbar) zu Teilmengen zusammengefasst
-   inklusive dem gerade rechnenden Prozess, erhält man so drei Zustände:
    -   Zustand **"rechnend"** (running) = Prozesse die gerade auf einem Prozessor bearbeitet werden
    -   Zustand **"bereit"** (ready) = Prozesse die zwar fortsetzbar sind aber gerade nicht bearbeitet werden
    -   Zustand **"wartend"** (waiting) = Prozesse die nicht fortsetzbar sind weil sie auf das Eintreten einer Bedingung warten

{{< figure src="/knowledge-database/images/prozess-zustaende.png" >}}

-   für alle Zustandsübergänge (state changes) sind entsprechende Operationen im Kern vorgesehen:
-   **Aufgeben** (**relinquish**):
    -   freiwilliges Umschalten auf einen anderen Prozess; der bisher rechnende Prozess bleibt jedoch fortsetzbar, dh geht in den Zustand bereit/ready über
-   **Zuordnen** (**assign**):
    -   Aufgreifen des nächsten Prozesses aus der "Bereit-Menge" zur Menge Fortsetzung auf dem Prozessor
-   **Blockieren** (**block**):
    -   verlassen des Prozessors wegen Nichterfüllung einer Bedingung (bedingtes Umschalten); da erst die Bedingung erfüllt sein muss, geht der Prozess in den Zustand "warted" über
-   **Deblockieren** (**deblock**):
    -   ist das Ereignis eingetreten, auf das der blockierte Prozess gewartet hat, so wird durch diesen Übergang wieder in die Menge der bereiten Prozesse eingefügt


### Dynamische Systeme {#dynamische-systeme}

-   in dynamischen Systemen ist die Menge der am Geschehen teilnehmenden Prozesse variabel
-   dynamische Systeme können in zwei Schritten entstehen:
    -   **Aktivieren / Deaktivieren**
        -   ein Prozess kann zwar definiert sein (es existiert ein PCB), Programm- und Datenbereich sind vielleicht auch schon vorhanden aber der Prozess ruht, d.h. er ist nicht aktiv
        -   Unterscheidung zwischen **aktiven** und **nichtaktiven** Prozessen
        -   Übergänge zwischen diesen Zuständen durch die Operationen _aktivieren_ und _deaktivieren_ möglich
    -   **Erzeugen / Löschen**
        -   weiterer Schritt: Annahme, dass Prozesse bei Systemstart noch nicht existieren; sie werden explizit erzeugt und ggf. gelöscht
        -   dafür sind die Operationen **erzeugen** und **löschen** vorgesehen

{{< figure src="/knowledge-database/images/zustandsdiagramm.png" >}}

Unix Zustände<br />
![](/knowledge-database/images/unix-states.png)

Windows NT (thread states)<br />
![](/knowledge-database/images/thread-states.png)


#### Leerlaufproblem {#leerlaufproblem}

-   möglich: alle Prozesse warten &rarr; Prozessor hat keine Arbeit
-   elegante Lösung: Leerlauf-Prozess (**idle process**)
-   Eigenschaften:
    -   nie blockierend
    -   nie beendet (zyklischer Prozess)
    -   muss jederzeit verlassen werden können

Implementation:

-   leere Schleife: while (true) do;
-   dynamischer Stopp: falls verfügbar Spezialbefehl, der keinen Speicherzugriff durchführt und auf externe Signale reagiert
-   nützliche Aufgaben: Prüfungen, Reorganisationen (_garbage collection_)


## Leichtsgewichts- vs Schwergewichtsprozesse {#leichtsgewichts-vs-schwergewichtsprozesse}

-   Prozess: Programm in Ausführung
    -   Schwergewichtsprozess: klassischer Prozess &rarr; zum Prozess gehöhrt eine Gruppe von Ressourcen (Adressraum, geöffnete Dateien, etc)
    -   Leichtgewichtsprozesse: Thread &rarr; Ressourcen werden von den Prozessen geteilt

-   beim Multithreading gibt es keinen gegenseitigen Schutz
-   als Prozess wird dann meist der (nichtaktive) Ressourcencontainer bezeichnet
-   jeder Thread besitzt eigenen Stack

{{< figure src="/knowledge-database/images/leichtgewicht-vs-schwergewicht.png" >}}
