
Das ist ein kleiner Überblick über die Paper und deren Inhalte, um effizient
für die Prüfung lernen zu können.

**unvollständig**

# 01 - Design Phil von DARPA

[Link zum Paper](http://ccr.sigcomm.org/archive/1995/jan95/ccr-9501-clark.pdf)

## Autoren, Jahr, Motivation

- David D. Clark
- Aug 1988
- Massachusetts Inst of Tech (Cambridge)
- Überblick, Konsequenzen und Erfolg der Designentscheidungen von vor 15 Jahren
- SIGCOMM

## Inhaltlicher Aufbau / Argumentationen

  - Paper fasst alte Quellen und Designüberlegungen zusammen
  - Ziele
      - ARPANET und ARPA packet radio Network sollten zusammengeführt werden
      - nicht effizient alles neu, sondern Basisnetz auch für zukünftige Netze/Funktionen
      - dezentrale Administration mit Gateways sollte bestehen bleiben
      - Basis war packetbasiert, da gute Erfahrungen damit gemacht wurden
  - TCP/IP soll bestehende Netze über Gateway einbinden
  - Store-and-Forward war die erste Design-Idee
  - Prio
      - Ausfall von Teilnetzen soll nicht zu komplettausfall führen
      - Anbieten versch. Kommunikationsservies
      - Verbinden untersch. Netze
      - verteiltes Management der nötigen Resourcen
      - kostengünstig
      - je Host einfach anzuschließen
      - Erfassung des Resourcenverbrauchs (Wer, Wo, Wie viel)
- Militär: unkaputtbar wichtiger als Resourcen kosten oder Verbrauchserfassung (heute anders)

### Grundidee Ausfallsicherheit

- Schichtenmodell mit Maskierung+Behandlung von Fehlern je Schicht
- Statusinfos dürfen nur auf (den sicheren) Endpunkten gehalten werden

### Types of Service

- TCP macht IP Verbindungsorientiert
- UDP ist nur dafür da, um eigene Protokolle entwickeln zu können auf Basis von IP

## Kernaussagen

- damalige Ziele führten zu guten Design
- Integration bestehender Netze war klarer Vorteil statt Neuentwicklung
- Abrechenbarkeit könnte mehr in Vordergrund gestellt werden

## Begriffe

DARPA
:   Defense Adv Research Proj Agency

Packet switching
:   im Gegensatz zum *circuit switching* (Standleitung), wird die Nachricht
in Packete zerteilt und kann untersch. Wege im Netz nehmen.

Store-Forward
:   es kommt zu delays, dafür können aber die Daten an den Netzknoten bereits
auf Richtigkeit (z.B. in der IP und TCP Schicht) geprüft werden. 

XNET
:   Debugger, der mit Datagram arbeitet und dies auch braucht. Die Natur von
Bugs ist derart, dass ein komplexer Handshake wie bei TCP im Ernstfall nicht
mehr möglich ist.





# 02 - Selbstähnlichkeit von Ethernet Traffik

[Link zum Paper](http://ccr.sigcomm.org/archive/1995/jan95/ccr-9501-leland.pdf)

## Autoren, Jahr, Motivation

- Herren Leland, Willinger, Taqqu, Wilson
- Bellcore NY, Uni Boston
- ca 1993
- Traffic ist statistisch nicht glatt, wie bislang angenommen. Nachweis und Konsequnezen.
- SIGCOMM

## Kernaussagen

 -  "Entropie" (oder H) bzw. "Burstiness" als Maß der Auslastung !!
 -  Kapitel 5.1: ein User hat kein Poison Verteiltes Netzwerk-Verhalten. Aggregierung dieser führt zu Selbstähnlichkeit.
 -  Kapitel 5.3: TCP skaliert auch bei Überlast gut (Burst-Phasen sind möglich)
 -  Überlegungen Buffer immer weiter zu vergrößern machen keinen Sinn: Burst-Phasen machen sowas kaputt
 -  Buffer vergrößern bringt nichts: Bursts machen das immer "kaputt"
 -  Theoretische Überlegungen zur Verbesserung der Performance führen zwangsläufig zu "nicht optimalen" Lösungen, da Traffic eben KEIN weißes Rauschen ist.
 -  Es wird versucht mit untersch. Methoden die selbstähnlichkeit zu erfassen:
     -  Hurst-Parameter am einfachsten.
     -  log-Auftragen von Varianz in Blöcken von untersch. großen Messreihen und entsprechend großen Zeitfenstern
     -  leider gibt es keine Grenzen bzgl. H und somit ist es schwer einen statistische Vergleich mit Konfidenzintervall und Parametern bzgl. H und den Messwerten an zu geben.
     -  Einen Schätzer für H via maximum likelihood ist aber möglich
     -  in Kapitel 4.2 wird auch ein "Periodogram" benutzt (Grafiken mit Aggregation Level auf der rechtswert-Achse)

## Begriffe

Rescaled Adjusted Range
:   Eine Art Verhältnis zwischen den größtmöglichen tatsächlichen Auftreten einer Abweichung der Messwerte vom Mittelwert gegenüber der Streuung. Im stochastischen Modell würde bei der Zunahme an Messwerten eine Annäherung an H = 0,5 bedeuten. Werte darüber deuten auf Selbstähnlichkeit hin

$$ \frac{R(n)}{S(n)} \sim a * n^{H} $$

Hurst Effekt
:   Hurst Parameter ist in Natur 0,73 und deutet auf Selbstähnlichkeit hin. 1968 wird mit 1/2 < H < 1 der Begriff "fractional Gaussian noise" geschaffen (Mandelbrot, van Ness).

## Kritik am Paper

- sehr kleine, überschaubare beispielhafte Messung






# 03 - Synchronisation periodisch Routing Nachrichten/Traffik

[Link zum Paper](https://www2.cs.arizona.edu/classes/cs525/papers/fj93_sync.pdf)

## Autoren, Jahr, Motivation

- Frau Floyd, Herr Jacobson
- Lawrence Berkeley Lab
- April 1994
- keine Konferenz (IEEE/ACM)
- praktische Analyse und theor. Erklärung für zykl. Netzwerkprobleme

## Inhaltlicher Aufbau / Argumentationen

- fällt Strom aus sind alle Router gleichzeitig betroffen
- periodische Engpässe, Latenzen und Ausfälle treten auf
- die "leichte Kopplung": direktes Update bei wegfall einer Route
- wegfall einer Route kann zu Engpass und Ausfall anderer bedeuten
- Route durch leichte Kopplung trotzdem synchron

## Kernaussagen

- Routerausfälle, während Update ist Mist
- Umgehendes Update nach Routerausfall ist auch Mist
- mehr zufälliger Puffer muss sein, obwohl Latenz dadurch beeinträchtigt wird

## Kritik am Paper

Das ist heute nicht mehr so ein Problem:

- heute Software und Protokollimpl. stabieler
- Router kann Routen und Update laufen lassen






# 04 - Verstopfungsvermeidung und Kontrolle

[Link zum Paper](https://people.eecs.berkeley.edu/~sylvia/papers/congavoid.pdf)

## Autoren, Jahr, Motivation

- wieder Van Jacobson, Herr M. J. Karels
- Lawrence Berkeley Lab, Uni of California
- Untersuchung und Begründung zu Verstopfungen im Netzwerk und Verfahren zur Erkennung und Behebung
- Nov 1988
- ACM SIGCOMM

## Inhaltlicher Aufbau / Argumentationen

 -  Autoren beobachteten, dass 10% der Pakets am Gateway wegen überfüllter Buffer gedroppt wird
 -  es gab 1986 einen Crash: 32000bps auf 40bps
 -  Problem liegt in TCP Protokollimplementierung
 -  Verbesserung 4BSD TCP in diesem Paper beschrieben:
     -  bessere Roundtrip Time Abschätzung
     -  exponentieller Timer bei erneutem Senden
     -  slow-start
     -  dynamisch Fensteranpassung bei Verstopfung
 -  Gründe dafür, dass die Sender-Empfänger-Sender Schleife und das "Konservieren der Pakete in der Leitung" gestört ist:
     -   die Verbindung ist von Beginn an bereits gestört
     -   der Sender fügt bereits Pakete hinzu, obwohl alte Pakete noch nicht angekommen sind
     -   durch Resourcenlimit entlang des Pfads kann nie eine gleichmäßige Verbindung erreicht werden

Das Paper arbeitet sich an diesen 3 letzten Punkten ab

### Slow-Start

- um ein "equilibrium" zu Anfang erreichen zu können, darf man nicht bereits zu Anfang mit Kanonen auf Spatzen schießen
- der erste Schwall an Paketen einer 10Mbps Leitung kann eine 56kbps Verbindung bereits dauerhaft einen dauerhaften Schaden anrichten
- Fenstergröße öffnet sich nach jedem ACK linear
- bei Verlust oder nach langer Pause startet es wieder mit kleiner Fenstergröße
- Es wird das minimum aus der Berechneten und der "veröffentlichten" Fenstergröße genommen

### Roundtrip Time - Wann kann ich ein ACK erwarten?

 -  bei Last $\rho$ skaliert die RRT mit $(1 - \rho)$
 -  man hatte im April 88 und einer Auslastung von 75% im Arpanet eine RRT die um das 16fache variierte
 -  bisheriger Tiefpassfilter für die RRT anhand gemessender ACKs schlecht:
     -  bei einem Load von 30% varriert RRT zu stark
     -  die falsche vorhersage der RRT ist wie Benzin ins Feuer kippen
     -  obwohl Leitung belastet, werden unnötig Re-Transmits hinzugefügt
     -  eigentlich kommen ACKs nur verspätet an
     -  Leitung verstopft noch mehr

Argumentation bzgl. exponetielles Warten bei (vermutetem) Packetverlust:

- Netzwerk ist in etwa ein lineares System
- Ein lin. System sei stabil, wenn die stabilität exponentiell ist
- Wenn ein zufälliger load Shock die Stabilität zerstört, bekommt man es nur mit expon. Eingriff wieder hin

### Verstopfungsvermeidung

- Packetverlust eigentlich immer weil ein Buffer zu voll ist
- unter 1 % der Packete kommen nicht an, weil kaputt
- Puffer ist an Bandbreite angepasst
- Paketverlust erkennen heißt Verstopfung erkennen
- wenn alle, die über die verstopfte Stelle senden, ihre Fenstergröße bei Verstopfung halbieren, dann ist schnell allen geholfen
- Fenstergröße dann immer um einen konstanten Wert nach ACK anwachsen lassen
- konstant: geht schnell genug und ist "feiner" als multiplikativ
- multiplikativ: würde ein oszilierendes Verhalten verursachen
- Vorsicht: Algorithmus muss mit "slow-start" kombiniert werden!


## Kernaussagen

- wollen ein Bit bzgl Verstopfung im Header haben
- sofort erneutes Senden bei Paketverlust unsinnig; Fenstergröße halbieren besser.
- Begründung zur Fensterhalbierung bei Verstopfung
- Konzept des langsamen Anwachsens der Fenstergröße beim Verbindungsaufbau bzw nach langer Pause
- sofort erneutes Senden bei Paketverlust verstopf nur noch mehr
- Paketverlust eigentlich immer nur, weil Verstopfung da ist

## Kritik am Paper

- (zu) viel in Fußnoten, Anhang andere Paper ausgelagert




# 06 - Zeit, Ordnung und die synchr. in Verteilten Systemen

[Link zum Paper](http://amturing.acm.org/p558-lamport.pdf)

## Autoren, Jahr, Motivation

- Leslie Lamport
- Juli 1978
- Beweis und Algorithmus zur Möglichkeit der Zustandsermittlung eines Verteilten Systems
- Eingereicht: ACM ?

## Inhaltlicher Aufbau / Argumentationen

- Ein System besteht aus Prozessen
- Ein Prozess besteht aus Events
- Nachrichtenaustausch zwischen Prozessen sind ebenfalls Events
- System ist verteilt, wenn man die Zeit zum Nachrichtenaustausch nicht vernachlässigen darf

## Kernaussagen

- aus Hierarchie der Prozesse und deren Zeit kann man beim Mitführen des "lokalen Zeit" Zeitstemples das Ereignis "total" einordnen
- die schnellste Uhr dominiert
- mit echten Uhren kann man auch Bezüge zu Events außerhalb des Systems herstellen
- besser, wenn man keine echte Uhrzeit nimmt
- durch eine totale Ordnung kann das System als "State Maschine" betrachtet werden

## Begriffe

anormales Verhalten
:   A happend before B aber die Uhr von A ist nicht kleiner als die von B. Es wird eine Uhr (z.B. Vektoruhr) gebraucht, die die Strong Clock Condition erfüllt

Happend Before Relation
:   Die Relation (fetter Pfeil) $a \rightarrow b$ heißt: a passiert vor b, a sendet, b empfängt, transitiv (Dreiecksungleichung)

Gleichzeitig
:   a und b ist gleichzeitig, wenn kein Nachrichtenaustausch zwischen a und b passiert und daher keine Aussage getroffen werden kann, ob a vor b, oder b vor a passiert. Bei Vektoruhren liefert before(a,b) und before(b,a) jeweils NICHT.

schwaches Konsistenzkriterium (oder nur "Clock Condition")
:    gilt $a \rightarrow b$ dann folgt daraus, dass die Uhr von a einen kleineren Wert hatte, als die von b. VORSICHT ! Das bedeutet nicht, dass a "Happend Before" b gilt!

Strong Clock Condition (starkes K.k.)
:   auch die Umkehrung (genau dann, wenn) gilt. Nebenläufige/Unabh. Ereignisse sind dann erkennbar. Implementierung als Vektoruhren und Erkennung durch partielle Ordnung.

Vektoruhr
:   a passierte nicht vor b, wenn min. eine Prozess-Zeit-Komponente des Vektors von a größer ist als die jeweilige von b


### PC1

Es existiert ein oberes Maß, wie falsch eine Uhr tickt. $\kappa$

### PC2

Es existiert ein oberes Maß, wie stark Uhren zur selben Zeit abweichen. $\epsilon$

### anormales Verhalten

Auf der Basis von PC1 und PC2 und dem sync Algorithmus beim Datenaustausch,
der die Uhren nie zurück stellt, folgt:

- anormales Verhalten ist nicht möglich, wenn ungefähr die Zeit $\mu$ zum Transver der Nachricht größer ist, als die obigen Werte "zusammen"

$$ \frac{\epsilon}{1- \kappa} \leq \mu $$

## Kritik am Paper

- Anzahl der beteiligten Prozesse muss bekannt sein




# 08 - Accountweise Transfermessung durch wkeitbassiertes Zählen

[Link zum Paper](https://wwwcn.cs.uni-duesseldorf.de/publications/publications/library/Lieven2010a.pdf)

## Autoren, Jahr, Motivation

- Herr Lieven, Herr Scheuermann
- 2010 (IEEE)
- Uni Düsseldorf
- Weiterentwicklung von FM Sketches zur CPU- und speichereff. Zählung auf Routern

## Inhaltlicher Aufbau / Argumentationen

- Auflistung, was es schon gibt
- ist in den 50/50 (geoverteilt) Feldern >30% noch Null, dann Hitcounting
- Hitcounting nutzt Füllrate des Bitarrays zur Ermittling der "false positive" Bits
- 1. Trick: Zufall statt Hash
- 2. Trick: Hash für Matrixpos und flowid
- 3. Trick: Hitcounting (nur geom. Verteilung betrachten) bei geringer Füllrate


## Kernaussagen

- zählen muss in wenigen Nanosekunden erfolgen
- Mischung geometrisch (Größenordnung) und normalverteilter (reduzierung falsch positiver Bitsetzung) Zufallsgenerator: wird mit flow id gehasht und in Bitmaske ein Bit gesetzt
- unwichtig, wie aufwändig man hinterher an die Zahlen kommt
- kleine Zahlen werden anders gezählt
- genauer (kleine Zahlen) und weniger aufwändig, als bisherige Verfahren (MRSCBF)

## Stichworte

virtuelle Matrizen
:   eine Art fake FM-Sketch

Bloom-Filter
:   erkennen, ob etwas in einer Teilmenge enthalten ist

FM-Sketch
:   wie viel bzw. Größenordnung einer Teilmenge




# 09 - Tor: die 2. Generation des Onion Routings

[Link zum Paper](https://www.onion-router.net/Publications/tor-design.pdf)

## Autoren, Jahr, Motivation

- Herren Dingledine, Mathewson, Syverson
- Free Haven Project, Naval Research Lab (US Marine) 
- August 2004
- Eingereicht: USENIX Security Symposium
- Abgrenzung zur 1. Generation, Designziele, Angriffsszenarien und Gegenstrategien

## Inhaltlicher Aufbau / Argumentationen

- Kapitel bzgl. Überlastkontrolle: Was da steht ist falsch. Es gibt Szenarien wo Tor2 zusammenbricht. Tor3 hat da nachgebessert!

## Kernaussagen

- Wer/ob man es nutzt, ist erkennbar!
- Wer mit wem kommuniziert ist nur erschwert erkennbar (gewisser Hardwareaufwand und tricks nötig)
- Es hilft nicht bei Protokollen, die von sich aus unverschlüsselt Userdaten übertragen
- Es hilft nicht gegen Angriffe auf mehrere Knoten einer Verbindung
- Es hilft gegen Angriffe auf einen speziellen Konten durch Umleitungen und Verschlüsselung der Verbindungsinfos

## Begriffe

Ablauf DNS
:   DNS ist normalerweise UDP und passiert vor Verbindungsaufbau. Privoxy als HTTP Proxy übernimmt das, so dass DNS nicht vorher passiert.

Ablauf Hidden Service

- Bob hinterlegt signiert infos senes service auf lookup service ab
- Bob baut zu ORs Verbindungen auf, die als Introduction Points (IP) dienen
- Alice bekommt von Bob info über, dass er einen Service hat
- Alice holt infos bei lookup service ab
- Alice baut zu ORs Verbindungen auf, die als Rendevous Points (RP) dienen
- Alice teilt einem IP ihre RPs mit + rendevous Cookie
- Bob teilt einem RP von Alice ihren rendevous Cookie mit und einen Session Key
- Ab jetzt läuft alles nur zwischen Bob-service-OP-OR-...IP-RP-...OR-OP-client-Alice
- Service und Client müssen nur mit Domains der Art "authCookie.ServicepupKeyHash.onion" umgehen können und müssen nicht angepasst werden

Onion Routing
:   Die Hops sind in einer *onion* verschlüsselt (also mehrfach verschlüsselt).

teleskope path building
:   im Gegensatz zur Onion wird hierbei zu jedem Hop eine eigene verschlüsselte Session aufgebaut.

Zur Verbindung zwischen Alice und Bob sind mind. 2 Onion Router (OR) nötig, um ein Ziel zu erreichen: Der erste "Hop" kennt Alice aber Bob nicht. Der 2. Hop (OR) kennt nur Bob (das Ziel) und den OR vor Alice.  










# TODO 05 - Bitcoin und darüber hinaus: tech. Überblick dezentraler digitaler Währungen

[Link zum Paper](https://eprint.iacr.org/2015/464.pdf)

## Autoren, Jahr, Motivation

- Herr Tschorsch/Scheuermann
- ca 2015
- Humboldt Uni Berlin
- Überblick: Technik, Wildwuchs, Probleme, Sicherheit, Aussicht
- Eingereicht: IEEE ?

## Inhaltlicher Aufbau / Argumentationen

- Überlegungen bzgl. alternative zu CPU-lastigen Verfahren (z.B. Würfeln nach Zeitspanne)
- Problem: Coin 2x ausgeben
- Problem: Coin ausgegeben, aber verteiltes System behauptet, es sei nicht passiert (ungültig?)

## Kernaussagen

- trotz spezieller Dienste: Bitcoin ist nicht anonym (wer mit wem) !
- problematisch sind chains, die versteckt ablaufen und dann plötzlich ins "Spiel" kommen
- es funktioniert und problematische Angriffe bzgl. Zusammenbruch von Keinem gewollt

## Stichworte

Inflation
:   das Fee, was man bekommt, erhöht im System die Anzahl der Coins. Gegenmaßnahmen: initial 50, ca alle 4 Jahre (alle 210000 Blocks) halbierung. 2140 ende und 10exp-8 BTC = 1 satoshi

Bitcoin
:   ein Bitcoin gibt es nicht - die Coins haben keine ID

## Begriffe

## Kritik am Paper





# TODO 07 - Datenreduktion durch XOR Kodierung in kabellosen Netzwerken

[Link zum Paper](http://nms.csail.mit.edu/~sachin/papers/copesc.pdf)

## Autoren, Jahr, Motivation

- div. Autoren & Autorinnen (inkl. Jon Crowcroft der Uni Cambridge)
- Sep 2006
- SIGCOMM
- Nutzung der Routingprotokolle und des Broadcast Charakters von Wifi, um optional Pakete zusammen zu mischen.

## Inhaltlicher Aufbau / Argumentationen

- Wir stellen COPE vor
- so und so viel schneller ...
- Zeitspanne zum Verfall mitgehorchter Pakete ?

## Kernaussagen

- Erweiterung und daher nie schlechter
- TCP muss wegen Packetreihenfolge gepuffert werden
- mitgehörte Packete werden in einem Buffer für große und kleine Packete hinzugemischt
- Problem der Hidden Terminals ?!?!

## Stichworte

## Begriffe

## Kritik am Paper

- bei Implementierung ein wenig die Schichten vermischt

