+++
title = "Computer Networks - Chapter 04 Netz-Zugang"
author = ["eo shiru"]
date = 2019-05-07
lastmod = 2019-05-28T13:10:50+02:00
tags = ["uni", "computer-networks"]
draft = false
+++

**DLC-Protokolle: Uebersicht**<br />
Data Link Control

-   asynchrone Uebertragung
    -   Uebertragung _nicht_ innerhalb eines groesseren Uebertragungsrahmen, sondern _zeichenweiser Start-Stop-Betrieb_ (Telexdienst)
-   synchrone Uebertragung
    -   _zeichenorientiert_ = kleinste Uebertragungseinheit ist ein Zeichen, zB 8 Bit
        -   zB PPP
    -   _bitorientiert_ = kleinste Uebertragungseinheit ist 1 Bit
        -   zB HDLDC, LLC (LAN), LAPD (ISDN)


## 4.1 Synchronisation {#4-dot-1-synchronisation}

Asynchrone Uebertragung (zeichenweiser Start/Stop Betrieb)<br />
![](/knowledge-database/images/asynchrone-uebertragung.png)

-   setzt Ruhepegel und feste Zahl von Nutzschritten voraus
-   3 aus 11 Overhead (8 Nutzbitz bei 11 zu uebertragenen Bits)

Synchrone Uebertragung (Blocksynchronisation)<br />
![](/knowledge-database/images/synchrone-uebertragung.png)

-   setzt Benutzung eindeutiger Blockstart- und/oder Blockendmuster voraus
-   Modifikation / rueckgaengig machen entsprechender Muster im Block (Zeichen-/Bitstopfen)

**Codetransparenz** bei synchroner Uebertragung = Vereinbarung von Regeln zur codetransparenten Uebertragung von Nutzdaten (d.h. Uebertragung beliebiger Bit- bzw Zeichenkombinationen im Nutzdatenfeld)<br />
&rarr; einfacher Loesungsansatz: Laengenangabe der Nutzdaten

-   {{< figure src="/knowledge-database/images/code-transparenz.png" >}}


#### Synchrone zeichenorientierte Uebertragung {#synchrone-zeichenorientierte-uebertragung}

**Character Stuffing**

-   Anfang und Ende der Nutzdaten (eines Rahmens) werden durch STX bzw ETX symbolisiert
    -   {{< figure src="/knowledge-database/images/c-stuffing-1.png" >}}
    -   Problem: wenn ein ETX in den Nutzdaten enthalten ist signalisiert dies das vorzeitige Ende des Rahmens
    -   Loesung: das Sonderzeichen DLE (Data Link Escape) macht nutzdaten transparent, ein ETX wird daher nur dann als solches erkannt, wenn ein DLE davor steht
    -   {{< figure src="/knowledge-database/images/c-stuffing-2.png" >}}
    -   Problem: ein DLE in den Nutzdaten koennte jederzeit zu einer Fehlinterpretation fuehren
    -   Loesung: Sender verdoppelt DLEs innerhalb der Nutzdaten. Der Empfaenger interpretiert einfache DLEs als Steuerzeichen und bei doppeltne DLEs wird das kuenstlich eingefuegte wieder geloescht:
    -   {{< figure src="/knowledge-database/images/c-stuffing-3.png" >}}

{{< figure src="/knowledge-database/images/character-stuffing.png" >}}


#### Synchrone bitorientierte Uebertragung {#synchrone-bitorientierte-uebertragung}

**Bit Stuffing**

-   Blockbegrenzung (Flag) ist eine ausgewaehlte Bitfolge (zB 01111110)
    -   {{< figure src="/knowledge-database/images/bit-stuffing.png" >}}
    -   Problem: zufaelliges Auftretet dieser Bitfolge 01111110 in Nutzdaten
    -   Loesung: Bit Stuffing
        -   Sender fuegt _innerhalb_ der Nutzdaten nach 5 auf einander folgenden "1" eine "0" ein
        -   Empfaenger entfernt nach 5 aufeinanderfolgenden "1" eine "0"
    -   {{< figure src="/knowledge-database/images/bit-stuffing-2.png" >}}


#### Synchrone Uebertragung: Coderegelverletzung {#synchrone-uebertragung-coderegelverletzung}

Synchrone Uebertragung ohne Laengenfeld oder Character/Bit Stuffing:

-   Verwendung von Block-Start/Endmustern (Flags)
-   Flags enthalten (gemaess Leitungscodierung) ungueltige Bits
-   Bsp: Manchester Codierung
    -   {{< figure src="/knowledge-database/images/coderegel-verletzung.png" >}}


## 4.2 Sicherung der Datenuebertragung - Uebertragungsfehler {#4-dot-2-sicherung-der-datenuebertragung-uebertragungsfehler}

Wesentlich fuer eine fehlerfreie Uebertragung sind korrekt synchronisierte Sender und Empfaenger

-   ungenaue Abtastzeitpunkte koennen u.U zu sporadischen Bitfehlern fuehren, obwohl die Signalfolge korrekt uebertragen wurde
-   falsch synchronisierte Sender und Empfaenger fuehren haeufig zu einer vollstaendig falsch interpretierten Bitfolge

Uebertragungsfehler sind hardwareinduzierte Fehler, die vorzugsweise auf dem Uebertragungsmedium entstehen, aber auch in den Anschlusselektroniken der kommunizierenden Stationen.

-   Art & Haeufigkeit stark vom Uebertragungsmedium abhaengig
-   in der Funktechnik existieren andere Fehlerursachen, Fehlerhaeufigkeiten und Fehlerauswirkungen als in der leitungsgebundenen Uebertragungstechnik
-   **Einzelbitfehler:** zB Rauschspitzen, die die Detektionsschwelle bei digitaler Signalerfassung ueberschreiten
-   **Buendelfehler:** laenger anhaltende Stoerung zb durch Ueberspannung
-   **Synchronisierfehler:** alle Bits bzw Zeichen werden falsch erkannt


### Fehlerhaeufigkeiten {#fehlerhaeufigkeiten}

Maß fuer die Fehlerhaeufigkeit:
Bitfehlerrate = \\(\frac{\text{Summe gestoerte Bits}}{\text{Summe uebertragene Bits}}\\)

-   stark vom Uebertragungsmedium bzw Netz abhaengig
-   Bitfehlerraten zu uebertragender digitaler Daten im analogen Netz sind sehr viel hoeher als in digitalen Uebertragungssystemen
    -   moderne ISDN/PCM Systeme haben eine bessere Uebertragungsqualitaet als klassische digitale Netze
-   die Uebertragungsfehlerhaeufigkeit ist auch stark von der Gesamtlaenge des Uebertragungsweges abhaengig
-   typische Wahrscheinlichkeiten fuer Bitfehler
    -   analoges Fernsprechnetz: 2\*10<sup>-4</sup>
    -   Funkstrecke: 10<sup>-3</sup> - 10<sup>-4</sup>
    -   Ethnernet (10Base2): 10<sup>-9</sup> - 10<sup>-10</sup>
    -   Glasfaser 10<sup>-10</sup> - 10<sup>-12</sup>


### Fehlerwirkungen {#fehlerwirkungen}

-   abhaengig davon welche Bits betroffen sind
    -   (Nutz-)Datenfehler: Bits innerhalb der Nutzdaten (gesehen zB aus Sicht der Sicherungsschicht) werden gestoert
    -   Protokollfehler: Stoerungen koennen Protokollkontrolldaten, Steuerzeichen, Adressen oder sonstige protokollrelevante Daten verfaelschen oder vernichten
-   Fehlererkennungs- und Behandlungsmassnahmen (error detection and recovery) erforderlich
-   Fehlererkennung durch (kuenstliches) Hinzufuegen von Redundanz beim Sender
    -   error detecting codes
    -   Spezialfall: error correcting nodes


### Fehlererkennung {#fehlererkennung}


#### Paritaetsueberpruefung {#paritaetsueberpruefung}

-   gerade/ungerade Paritaet
    -   Gesamtzahl der "1" einschliesslich des Paritaetsbits ist gerade/ungerade
