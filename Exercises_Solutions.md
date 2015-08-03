#Lösungen der Aufgaben
| Aufgabe # | Beschreibung              | Lösung                             |
|-----------|---------------------------|------------------------------------|
| 1         | Störungen (textuell)      | ./A1-Mögliche_Störungen.txt        |
| 2         | Fault Tree Analysis       | ./A2-FaultTreeAnalysis.pdf         |
| 3         | FMEA                      | ./A3-FMEA.pdf                      |
| 5         | Sollverhalten             | siehe VisualStudio Solution          |
| 6, 7      | Plausibilitätschecks      | ./Safety/BehaviourWithoutFaults.tt |
| 8         | Integration der Störungen | siehe VisualStudio Solution          |
| 9         | Störungschecks            | ./Safety/BehaviourWithFaults.tt |
| 10        | DCCA                      | ./Safety/DCCA.tt                   |
| 11        | Vergleich FTA, FMEA, DCCA | ./A11-Vergleich-DCCA_FTA_FMEA.pdf  |


**Achtung**: die PDFs sollten mit dem Adobe Reader geöffnet werden, der Windows Reader zeigt oft nicht den ganzen Text.


##Detailliertere Informationen zu einigen Aufgaben
####A5 Sollverhalten - Besonderheiten und Entscheidungen

**Modellierung der Antriebs-, Brems- und Reibungswirkung**

Um möglichst nah an der Realität zu sein (...und um die einelementige Fehlermenge Zugmotor_Ausfall zu ermöglichen), setzt sich unsere Zuggeschwindigkeit zusammen aus:
* Antriebswirkung des Motors [0..2]
* Bremswirkung der Bremse [-1..0]
* Reibungswirkung [-1]

Daraus ergeben sich verschiedene Betriebsmodi:
* Beschleunigen: der Motor muss eine Antriebswirkung von 2 annehmen, um die Reibungswirkung von -1 auszugleichen und insgesamt die Geschwindigkeit um 1 pro Tick zu erhöhen
* Geschwindigkeit halten: um die Reibungswirkung auszugleichen, muss der Motor in Mittelstellung auf 1 bleiben, womit die Geschwindigkeitsänderung gleich 0 ist
* Bremsen: hier wird der Motor ausgeschalten und die Bremswirkung auf -1 gesetzt, wodurch die Geschwindigkeit verringert wird (allerdings nie unter 0)

Der Zugmotor versucht dabei immer die Maximalgeschwindigkeit von 5 zu erreichen, wenn die Zugsteuerung Fahren vorgibt.

**Hohe Modularität**

Wir haben stark darauf geachtet, unsere einzelnen Komponenten so einfach wie möglich zu halten und sie so lose wie möglich miteinander zu koppeln.
Weiterhin war uns eine Kohäsion wichtig: so haben wir beispielsweise die Hauptkomponenten Bahnuebergang und Zug bereits in der Namensgebung der Teilkomponenten unterschieden. Die einzige Interaktion stellt der Funk dar.

So haben wir beispielsweise auch die ZugFunksteuerung nochmal von der Zugsteuerung getrennt, sodass jede Komponente eine einzelne Verantwortlichkeit hat.

**Kein Funkkanal**

Unser Zug- und Bahnübergang-Funk kommunizieren direkt ohne die Modellierung eines Funkkanals.

Auch der Sensor kommuniziert direkt mit dem Bahnübergang, ohne die kabelbasierte Kommunikation dazwischen.

**Zustandsverringerung**

Um die Model-Checking-Zeit ertragbarer zu machen, haben wir uns entschieden, im Zughodometer die Geschwindigkeit und Position nicht auszulesen.
Dies führt dazu, dass die ZugPunktpositionsbestimmung und der Zugmotor direkt von der realen Position bzw. Geschwindigkeit lesen. (Auch so kommen wir noch auf 3,6 Millionen BDDs beim LTL Model-Checking)


####A8 Störungen
Folgende Störungen haben wir ausgewählt:
* BahnuebergangFunksender_Stoerung: Sicherungsbestätigung wird zufällig gesendet
* Schrankenmotor_Ausfall: persistenter Verlust der Veränderbarkeit des Schrankenwinkels
* Schrankensensor_Stoerung: Schrankensensor meldet zufällig, dass Schranke geschlossen
* Sensor_Stoerung: Zug wird zufällig erkannt
* Zugbremse_Ausfall: persistenter Verlust der Bremsfähigkeit des Zuges
* ZugFunksender_Ausfall: persistenter Verlust der Sendefähigkeit des Zuges
* Zugmotor_Ausfall: Zugmotor gibt keinen Antrieb mehr, kann aber wieder anspringen (durch die Reibung verliert der Zug ohne Motor an Geschwindigkeit)
* Zugpunktpositionsbestimmung_Stoerung: Zug wird zufällig als auf freier Strecke erkannt