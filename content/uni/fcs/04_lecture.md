+++
title = "Einführung in die Funktion von Computersystemen - Lecture 04: Von der Befehls- zur Programmausführung"
author = ["eo shiru"]
date = 2019-10-22
lastmod = 2019-12-03T10:33:42+01:00
tags = ["uni", "funktion-computersysteme"]
draft = false
+++

## Programmierung {#programmierung}

-   wir können nun prinzipiell arithmetisch & logische Operationen ausführen und mit dem Speicher kommunizieren
-   Problem: Was genau soll wann gemacht werden?
    -   **Makroebene**: _Programm_ (durch Nutzer/Programmierer)
        -   Programm liegt in durch den Prozessor "lesbarer" Form im Speicher
-   Problem: Wie wird es "verstanden"?
    -   **Mikroebene**: _Sequencing_ (durch Hersteller)
        -   **automatische** Interpretation des Nutzerprogramms

&rarr; gemeinsame Schnittstelle: **Rechnerorganisation** (zB Register, Befehle, ...)

**Wiederholung: verschiedene Register**

-   die sichtbaren Elemente der CPU und der Speicher können mit Befehlen (Instruktionen) durch den Programmierer manipuliert werden
    -   die CPU scheint die Befehle zu "verstehen"
-   dieses "verstehen" ist wieder ein automatisierter Vorgang


## Abarbeitung von Instruktionen {#abarbeitung-von-instruktionen}

-   grundlegender Zyklus der Befehlsabarbeitung:
    -   fetch & execute (Befehlsholen und Befehlsausführung)

{{< figure src="/knowledge-database/images/fetch-execute.png" >}}

-   das während der Fetch-Phase gelesene Datenwort wird **als Befehl interpretiert** und nach IR gespeichert
-   dieses Interpretieren wird auch als _decodieren_ bezeichnet (es erfolgt "nebenbei")
-   Zyklus wird trotzdem häufig auch als Fetch-Decode-Execute Zyklus bezeichnet

{{< figure src="/knowledge-database/images/beispiel-cpu.png" >}}

-   in unserer obigen Beispiel-CPU passiert folgendes im Fetch-Execute Zyklus:
    -   lade PC in das MAR
    -   lese Befehl aus dem Speicher und speichere ihn in IR
    -   dekodiere Befehl und inkrementiere PC
    -   _lade evtl. benötigte Operandenadresse nach MAR_
    -   _lese den/die Operanden aus dem Speicher_
    -   führe Operation aus; _lade evtl. benötigte Ergebnisadresse nach MAR_
    -   schreibe Ergebnis _evtl. in den Speicher_
    -   beginne von vorn

die Schritte in kursiver Schrift werden nur ausgeführt, wenn die entsprechenden Operanden/Ergebnisse im Speicher sind


## Sequencing {#sequencing}

-   der Standardzyklus wird in eine Sequenz von Steuersignalen übersetzt
-   zur Generierung dieser Sequenzen gibt es verschiedene Möglichkeiten:
    -   **festverdrahtete** CPU
    -   **mikroprogrammierte** CPU
-   betrachten zweiten Ansatz &rarr; Funktionsidee ist ähnlich wie bei einer Spieluhr


#### Sequencing - Mikroprogrammierte CPU {#sequencing-mikroprogrammierte-cpu}

-   Analogie:
    -   IR bestimmt welche "Walze" gewählt wird
    -   Taktgeber/Zähler entsprechen Kurbel + Getriebe
    -   Steuersignale sind Töne
    -   Ergebnisabhängigkeit (Flags) gibt es bei Spieluhr (leider?) nicht

{{< figure src="/knowledge-database/images/mikro-cpu.png" >}}

**Welche Steuersignale?**

-   die Steuersequenz muss aus dem Befehl gewonnen werden &rarr; Befehl steht in IR
-   Steuersignale realisiert bewirken zB Registerein- und -ausgaben  (zB `Y_{out}`), Parameterwahl (Operation) der ALU etc
-   im Zusammenwirken kann über einen Bus ein Transfer erfolgen
    -   Beispiel: `Y_{out}` gleichzeitig mit `X_{in}`, wobei beide am gleichen Bus liegen
        -   damit wird der Inhalt von Register Y nach X gebracht
-   Bus kann pro Zyklus nur einmal verwendet werden
-   komplexere Rechenwerke haben deshalb mehrere interne Busse

**Beispiel:** Unbedingter relativer Sprung<br />

-   PC<sub>out</sub>; MAR<sub>in</sub>; read; clear Y; Set carry-in to ALU; Add to Z
-   Z<sub>out</sub>; PC<sub>in</sub>; Wait for MFC
-   MDR<sub>out</sub>; IR<sub>in</sub>;
-   PC<sub>out</sub>; Y<sub>in</sub>
-   Adr(IR)<sub>out</sub>; Add to Z
-   Z<sub>out</sub>; PC<sub>in</sub>; End

&rarr; 6 nicht überlappende Zeiteinheiten sind für die Abarbeitung der Sprungoperationen nötig

-   siehe nochmals CPU Bild


## Befehlssatz {#befehlssatz}

-   jeder Prozessor hat einen vorgegebenen Befehlssatz (Menge von Befehlen die er "versteht")
-   Befehlssätze sind meist nach einem einheitlichen Schema gestaltet
-   prinzipieller Aufbau:
    -   Op-Code-Feld &rarr; Was soll gemacht werden?
    -   Feld für Addressierungsart &rarr; Welche Art von Adressen sollen genutzt werden?
    -   Feld zur Darstellung Adressen &rarr; Welche Operanden sind betroffen?
-   beim Entwurf von Befehlssätzen gab es zwei (gegensätzliche) Entwurfsprinzipien:
    -   **RISC** (Reduced Instruction Set Computer)
        -   Verzicht auf komplexe, "bequeme" Befehle
        -   Konzentration auf wenige, dafür schneller ausführbare Befehle
        -   Speicher wird meist nur bei Transferbefehlen benutzt, sonst Register
        -   meist festverdrahtet implementiert
    -   **CISC** (Complex Instruction Set Computer)
        -   komplexe, mächtige und "bequeme" Befehle für Spezialzwecke
        -   möglichst alle Adressierungsarten für Befehle
        -   meist mittels Mikroprogramm implementiert
-   moderne Rechner versuchen einen guten Kompromiss zwischen RISC- und CISC-Entwurf zu realisieren


#### Befehlssatz - Aufbau {#befehlssatz-aufbau}

-   allgemein kann eine Operation analog zu einer Funktion aufgefasst werden:

{{< figure src="/knowledge-database/images/operand-funktion.png" >}}

-   Unterscheidung von Befehlssätzen nach Anzahl der Adressen
    -   **Drei-Adress-Maschine** &rarr; beide Operanden und das Ziel sind Bestandteil der Instruktion
    -   **Zwei-Adress-Maschine** &rarr; beide Operanden sind Bestandteil der Instruktion, ein Operand ist gleichzeitig Ziel oder Ziel ist implizit der Akkumulator
    -   **Ein-Adress-Maschine** &rarr; ein Operand in Instruktion, zweiter Operand und Ziel in Akkumulator (ausgezeichnetes Register)


### Addressierungsarten {#addressierungsarten}

-   **absolut (direct)** = Adresse (Speicherstelle) des Operanden ist explizit als Teil der Instruktion gegeben
-   **unmittelbar (immediate)** = Operand ist Teil der Instruktion
-   **indirekt** = effektive Adresse des Operanden ist in einem Register oder einer Hauptspeicherstelle, deren Adresse in der Instruktion erscheint
-   **indiziert (indexed)** = effektive Adresse des Operanden wird generiert durch Addieren einer Konstanten zu einem Registerinhalt
-   manche Befehle nutzen auch **implizit** bestimmte Operanden, wie den **Akkumulator** oder den **Stack**


## Stack {#stack}

-   viele Architekturen kennen einen Stack, wobei eine weitere Art der Adressierung genutzt wird:
    -   Speicher wird nicht als frei adressierbar angesehen, sondern als ein Stapel (_stack_) auf deren oberstes Element zugegriffen werden kann
-   typische Befehle:
    -   `push x` lege Element `x` auf den Stack
    -   `pop x` hole Element `x` vom Stack
    -   `call adr` lege PC auf den Stack und springe zu Adresse `adr`
    -   `ret` hole PC vom Stack
-   beim Aufruf eines Unterprogramms können benutzte Register auf den Stack zwischengelagert und anschließend wieder zurückgeholt werden

{{< figure src="/knowledge-database/images/unterprogramme.png" >}}

{{< figure src="/knowledge-database/images/unterprogramme-stack.png" >}}


## Typen von Instruktionen {#typen-von-instruktionen}

-   typischerweise enthält ein Befehlssatz folgende Arten von Instruktionen:
    -   **Transfer-Operation** &rarr; zB lade Registerinhalt nach Speicher, lege Wert auf Stack
    -   **arithmetische Operationen** &rarr; zB inkrementiere Registerinhalt, addiere die obersten beiden Werte auf dem Stack
    -   **logische Operationen** &rarr; zB bilde das Komplement des Akkumulatorinhalts, bilde die UND-Verknuepfung der obersten beide Werte auf dem Stack
    -   **Operationen zur Programmsteuerung** &rarr; zB Springe bei Z = 1 nach Adresse _adr_, halte Programmausführung an, rufe Unterprogramm
    -   **Eingabe/Ausgabe-Operationen** &rarr; zB gebe den Akkumulatorinhalt an Port _ptr_ aus

die Zahl der Instruktionen variiert zwischen den verschiedenen Architekturen


### Instruktionen - Maschinenbefehle {#instruktionen-maschinenbefehle}

-   Programme bestehen aus Instruktionen (Bitmustern) die der Prozessor "versteht" &rarr; **Maschinenbefehle**
-   es entsteht die Illusion, dass der Prozessor diese Befehle unmittelbar ausführt &rarr; **Programmierermodell**
-   damit der Mensch sich nicht die Bitmuster der Befehle merken muss, gibt es symbolische Abkürzungen &rarr; **Mnemonics**
-   spezielle Programme übersetzen Mnemonics in Maschinencode &rarr; **Assembler**
-   Programme zum übersetzen von Hochsprachen (C, Pascal, Prolog, etc) generieren heute oft direkt Maschinencode &rarr; **Compiler**
    -   bei ihnen liegt im Gegensatz zum Assemblern **keine** 1-zu-1 Abbildung vom (Hochsprach-)Befehl auf Maschinencode vor
-   Adressierungsart wird durch das Mnemonic oder durch die Operanden angegeben
-   typische Mnemonics sind:
    -   `mov x,y` oder `ld x,y` (move bzw load)
    -   `add, sub, and, or, mul, div,` etc (add, subtract ...)
    -   `jmp, call jr, jrcc` oder `bra, bcc` (jump, call, jump relative, jump relative if condition `cc` is true, branch, branch if condition `cc` is true)
-   die Reihenfolge von Quelle (source) und Ziel (destination) ist vom Assembler abhängig
    -   `mov ax, 30H` AX &larr; 3016 im i80286-Assembler
    -   `moveq #17, D0` 17 &rarr; im MC68020-Assembler
-   Klammern (), [] oder @ zeigen indirekte Addressierung an zum Beispiel:
    -   `mov ax, [bx]` i80x86
    -   `ld a, (h1)` Z80
    -   `moveb @R1, 0x17` PDP-11


### Instruktionen - Interrupts {#instruktionen-interrupts}

-   auf bestimmte Bedingungen soll sehr schnell reagiert werden
-   Möglichkeiten:
    -   im Code wird die Bedingung ständing abgefragt &rarr; **Polling**
        -   höherer Aufwand für Software, keine Hardware-Unterstützung nötig
    -   die HW erkennt die Bedingung &rarr; **Interrupt**
        -   weniger Aufwand für Software, höherer Aufwand für Hardware
-   Interrupts werden auf Hardwareebene **angemeldet** &rarr; Signal
    -   durch Peripheriegeräte
    -   durch Koprozessoren
    -   durch die CPU selbst wegen Ausnahmebedingung (zB Fehler)
    -   durch die CPU selbst wegen entsprechenden Befehls (Softwareinterrupt, Trap)
-   manche Interrupts können abgewiesen (maskiert) werden
-   unterstützt eine CPU Interrupts ändert sich der Grundzyklus zu: **Fetch - Execute - Interrupt handling**
-   der Aufruf einer **Interruptbehandlung** (ISR) entspricht etwa einem Unterprogrammaufruf
    -   der Befehlszähler wird gerettet (idR auf dem Stack)
    -   **Interruptserviceroutine** (ISR) wird bearbeitet
    -   Befehlszähler wird nach Bearbeitung des Interrupts zurückgeholt

**Merke:**

-   ein Interrupt kann zu jedem beliebigen Zeitpunkt auftreten (Asynchronität)
-   die ISR kann daher vom gerade ausgeführten Code nichts "wissen"
-   ein normaler Code kann von dem Auftreten eines Interrupts nichts "wissen"
-   während der Behandlung eines Interrupts können weitere Interrupts auftreten
    -   mehrere Möglichkeiten:
        -   Interrupt abweisen (maskieren)
        -   geschachtelte Interrupts
        -   Interrupt verzögern


## "Lebenslauf" eines Programms {#lebenslauf-eines-programms}

-   Quellcode schreiben
-   Quellcode übersetzen
-   binden/linken von Symbolen
-   Laden des Programms
-   Ausführen &rarr; Programm wird zum **Prozess** (Kapitel 5)
    -   Unterbrechen / Auslagern / Einlagern / Fortführen (Kapitel 5)
-   Beenden des Prozesses (Kapitel 5)


### Generieren einer ausführbaren Binär-Datei {#generieren-einer-ausführbaren-binär-datei}

-   **Compiler**
    -   zB `/usr/local/bin/gcc`
    -   übersetzen des Quellcodes
    -   Quellcode Datei &rarr; Assembler Datei
-   **Assembler**
    -   zB `/usr/local/bin/gas`
    -   übersetzen in Maschinencode, unbekannte **Symbole** bleiben erhalten
    -   Erzeugung verschiedener **Segmente**, typisch:
        -   Text-Segment: Binär Code
        -   Daten-Segment: Variablen mit initialen Werten
        -   BSS-Segment: Platzhalter für nicht initialisierte Variablen
    -   Assembler-Datei &rarr; Objekt-Datei
-   Compiler & Assembler (und z.T. Linker) sind meist in ein Programm integriert
-   **Linken** zur Ergänzung unaufgelöster Symbole
    -   zB `/bin/ld`
    -   _statisches Binden (static linking)_
        -   alle Symbole müssen zur Linkzeit bekannt sein
        -   Import der Symbole über Header-Dateien
    -   _dynamisches Binden (dynamic linking)_
        -   Symbole werden erst zur Laufzeit aufgelöst
        -   Import der Symbol-Daten über Header-Dateien
        -   Verweise auf Bibliotheken werden eingefügt
    -   Objekt-Datei &rarr; zB a.out
-   **Laden**: Platzierung von Programmen im Adressraum

Beispiel `hello.c`:

```C
#include <stdio.h>

int main(void) {
  int i = 42;
  printf("Hallo die Antwort ist i: %d", i);
  return 0;
}
```

beim GNU Compiler können mit der Option `-v` die einzelnen Schritte sichtbar gemacht werden\\
![](/knowledge-database/images/compile-assemble-link.png)

Gutes Video das diesen Ablauf erklärt: <https://www.youtube.com/watch?v=N2y6csonII4>
