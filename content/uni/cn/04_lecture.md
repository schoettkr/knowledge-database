+++
title = "Computer Networks - Lecture 04"
author = ["eo shiru"]
date = 2019-04-30
lastmod = 2019-05-20T10:44:38+02:00
tags = ["uni", "computer-networks"]
draft = false
+++

## Signals and Bit Transmission {#signals-and-bit-transmission}


### Introduction {#introduction}

**Data Transmission - Bit to Signal to Bit**:<br />

-   sits directly on the physical medium (eg cable)
-   Layer 1 in the ISO/OSI and Internet Reference (TCP/IP) Model
-   transmission of unstructured bit sequences via physical medium
-   data transmission entails the physical connection and the conversion between data <-> signals
-   there are norms for interfaces (especially for physical interfaces)


### Signals and their Parameters {#signals-and-their-parameters}

A **signal** is a state or a modification of material systems. Such systems can be for example of acoustic, electromagnetic or biochemical nature. In a first state, a signal is considered as a pure information carrier without any symbolic character. This distinction of information and symbol will be of later interest for the coding of information. It enables the technical analysis of information without their interpretation in a symbolic form. For the intuitive understanding, this is an elementary hurdle: auditory signals, thus acoustic waves, are instantaneously perceived by humans on a symbolic layer (i.e. language). For the encoding however, this layer can here be regarded as a secondary meaning.<br />
**Signal parameters** are the physical parameters of signal which represent the data by their values. We distinguish between signals in the _time_ and _space_ domain (ortsabhaengige Signale, zeitabhaengige Signale):

-   signals in the time domain: values of the signal parameter S are a function of time \\(S = S(t)\\)
-   signals in the space domain: values of the signal parameter are a function of the space(Ort) for example the storage medium like CD/DVD

Every signal in the space domain can be transfered into a signal in the time domain and vica versa. In this lecture we solely focus on signals in the time domain.<br />
Generally there are four classes of signals in the time domain:

-   zeitkontinuierliche, signalwertkontinuierliche Signale
-   zeitdiskrete, signalwertkontinuierliche Signale
-   zeitkontinuierliche, signalwertdiskrete Signale
-   zeitdiskrete, signalwertdiskrete Signale

{{< figure src="/knowledge-database/images/signal-classes.png" >}}


### Analog to Digital Conversion {#analog-to-digital-conversion}

This is how a digital signal can be formed via quantization:<br />
![](/knowledge-database/images/quantization.png)

**Principles of A/D Conversion:**<br />

1.  _Sampling_ (Abtastung, Scan): reduction of a continous-time signal to a discrete -time signal (Auslesen des Signalwertes zu diskreten Zeitpunkten)
2.  _Quantization_: transformation of the sampled value into a discrete value by mapping it onto so called quantization intervalls of size a (quantization error max a/2 &rarr; difference between input value and its quantized value)

**Hertz** = _Hertz_ is a unit of frequency (of change in state or cycle in a sound wave, alternating current, or other cyclical waveform) of one cycle per second. It replaces the earlier term of "cycle per second (cps)."<br />
For example, in the United States, common house electrical supply is at 60 hertz (meaning the current changes direction or polarity 120 times, or 60 cycles, a second). (In Europe, line frequency is 50 hertz, or 50 cycles per second.) Broadcast transmission is at much higher frequency rates, usually expressed in kilohertz (kHz) or megahertz (MHz).

H. Nyquist (1924): The maximum data rate (Datenrate/ Kanalkapazität) for a noise-free channel with a limited bandwith is maxDataRate [bit/s] = 2 B ld n where B = bandwith of the channel in Hz and n is the amount of discrete signal levels (Signalstufe).<br />
C.E. Shannon (1948): The maximum data rate (Datenrate/ Kanalkapazität) for a channel with random (thermal) noise in dependance of the Signal-Noise-Ratio = maxDataRate [bit/s] = B ld (1 + S/N) where S/N is the signal-noise-ratio where the Dämpfung D [dB] = 10 log<sub>10</sub> (S/N)<br />
Example:<br />
Channel with bandwith 3000 Hz, binaere Kodierung, Dämpfung 30 dB

-   **Nyquist**: data rate = \\((2 \* 3000 \* ld 2 ) = 6000\\) bit/s
-   **Shannon**: data rate = \\(3000 \* ld (1+1000) ≈ 30000\\) bit/s
    -   S/N = 1000 = 10^3, because 30 dB = 10 log<sub>10</sub> (S/N)

-    Nyquist-Shannon Sampling Theorem (Slides: Abtasttheorem von Shannon & Raabe)

    The Nyquist-Shannon Theorem, also known as the sampling theorem, is a principle that engineers follow in the digitization of analog signals. For analog-to-digital conversion (ADC) to result in a faithful reproduction of the signal, slices, called samples, of the analog waveform must be taken frequently. The number of samples per second is called the sampling rate or sampling frequency.<br />
    Suppose the highest frequency component, in hertz, for a given analog signal is f<sub>max</sub>. According to the Nyquist-Shannon Theorem, the sampling rate must be at least 2f<sub>max</sub>, or twice the highest analog frequency component. The sampling in an analog-to-digital converter is actuated by a pulse generator (clock). If the sampling rate is less than 2f<sub>max</sub>, some of the highest frequency components in the analog input signal will not be correctly represented in the digitized output.

    Note: It is kind of hard to research what really is the Nyquist, the Shannon and the Raabe theorem. This will be taken from the slides but may not be the truth.

    **H. Nyquist** (1924): Max. Datenrate (Kanalkapazität) fuer einen rauschfreien Kanal mit lediglich eingeschraenkter Bandbreite.<br />
    Maximale Datanrate [bit/s] \\(= 2 B \log\_{2} n\\) <br />
    B = Bandbreite des Kanals (Hz)<br />
    n = Anzahl diskreter Signalstufen

    **Abtasttheorem von Shannon und Raabe**:<br />
    Die Abtastfrequenz \\(f\_A\\) muss mind zweimal so hoch wie die Grenzfrequenz \\(f\_{Grenz}\\) sein: \\(f\_A > 2 \* f\_{Grenz}\\).

    **Shannon Theorem** (1948):<br />
    Max. Datenrate fuer Kanal mit zufaelligem (thermischem) Rauschen. Das Shannon Theorem gibt eine obere Grenze fuer die auf einer Datenleitung erzielbare Datenrate in Abhaengigkeit des Signal-Rausch-Abstandes (Signal-Noise-Ratio, SNR):<br />
    Maximale Datenrate [bit/s] = \\(B log\_2 (1 + \frac{S}{N})\\) <br />
    B = Bandbreite des Kanals (Hz)<br />
    \\(\frac{S}{N}\\) = Signal-Rauschabstand, wobei Daempfung D[db] = \\(10 \log\_{10} (\frac{S}{N})\\) (Signalenergie / Rauschenergie)

    Beispiel:<br />
    Kanal mit Bandbreite 3000 Hz, binaere Kodierung, Daempfung 30dB

    -   Nyquist: Datenrate = \\((2\*3000\*\log\_{2} 2 )\\) bit/s = 6000 bit/s
    -   Shannon: Datenrate = \\(3000 \* \log\_2 (1+1000) \approx 30000\\) bit/s [S/N = 1000 = 10^3 , da 30 dB = 10 log<sub>10</sub> (S/N)]


#### Periodische und digitale Signale {#periodische-und-digitale-signale}

Die wesentlichen Kenngroessen periodischer Signale sind die:

-   **Periode T**
    -   die Periode/Periodendauer ist das kleinste oertliche oder zeitliche Intervall, nach dem sich der Vorgang wiederholt
-   **Frequenz \\(f = \frac{1}{T}\\)**
    -   ein Mass dafuer, wie schnell bei einem periodischen Vorgang die Wiederholungen aufeinander folgen
    -   ist der Kehrwert der Periodendauer
    -   als Einheit fuer die Frequenz wird idR **Hertz** Hz verwendet
        -   Hz = Anzahl sich wiederholender Vorgaenge pro Sekunde in einem periodischen Signal
-   **Amplitude S(t)**
-   **Phase &phi;** (auch _Phasenwinkel_)
    -   gibt die aktuelle Position im Ablauf eines periodischen Vorgangs an
    -   die Phasendifferenz (auch Phasenverschiebung) &Delta; &phi; ist ein Begriff im Zusammenhang von periodischen Vorgaengen
        -   bezeichnet bei Wellenvorgaengen gleicher Frequenz die Differenz zwischen ihren Phasen zur Zeit Null; zwei Wellen gleicher Geschwindigkeit & Frequenz haben eine konstante Phasenverschiebung, welche nur von den Anfangsphasen abhaengt
        -   zwei Wellen unterschiedlicher Geschwindigkeit oder Frequenz haben eine variable Phasenverschiebung
    -   siehe [hier](http://www.abi-physik.de/buch/wellen/phasenverschiebung-gangunterschied/) und [hier](https://www.youtube.com/watch?v=T8kNcfv8LvY)
-   die **Zeitfunktion** (Zeitdarstellung) ist eine Zuordnung von Signalwert und Zeit
-   die **Frequenzfunktion** (Frequenzgang, Spektrum) ist eine Zuordnung von Werten sinusfoermiger Signale und der Frequenz

{{< figure src="/knowledge-database/images/zeit-frequenz-fkt.png" >}}

Jede periodische Funktion kann durch eine Summe von Sinus- und Kosinusfunktionen dargestellt werden (Fourier-Reihe).


#### Uebertragungssystem {#uebertragungssystem}

Letztendlich werden Nachrichten mittels eines physikalischen Mediums uebertragen:<br />
![](/knowledge-database/images/physikal-medium.png)

Jene Signaltransportmedien bzw Uebertragungssysteme sind _bandbegrenzt_, d.h. sie uebertragen stets nur ein endliches Frequenzspektrum (Rest wird abgeschnitten/begrenzt). Die _Bandbreite_ in Hz gibt den Frequenzbereich an, der ueber ein Medium uebertragen werden kann. Konkret ergibt sich die Bandbreite aus der Differenz zwischen der hoechsten und der niedrigsten uebertragbaren Frequenz. Aufgrund nicht-idealer Bandbegrenzungen werden Abschneidefrequenzen festgelegt. Demnach muessen Signale an die Uebertragungscharakteristik des jeweiligen Mediums angepasst werden.


#### Stoereinfluesse {#stoereinfluesse}

Bisher wurden lediglich medienbedingte Abweichungen betrachtet (zB Bandbegrenzung). Zusaetzlich koennen Stoerungen auftreten, die das Signal beeinflussen, beispielsweise:

-   weisses Rauschen (Grundstoeranteil)
-   Echobildung (durch zeitverschobenes Eingabesignal)
-   Nebensprechen (ggseitige Medienbeeinflussung)
-   Brummsignale (niederfrequente Stoersignale)
-   Stoerimpulse (kurzzeitig mit hoher Amplitude)
