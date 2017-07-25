Mit dem Auftrag wird eine JSON-Ausgabedatei erzeugt, die Metadaten zu erkannten und nachverfolgten Gesichtern enthält. Die Metadaten enthalten Koordinaten, die die Position der Gesichter sowie eine Gesichts-ID angeben, um die Nachverfolgung einer einzelnen Person kenntlich zu machen. Gesichts-IDs können zurückgesetzt werden, wenn die Frontalansicht des Gesichts verloren geht oder im Frame überlappt wird, sodass einigen Personen möglicherweise mehrere IDs zugewiesen werden.

Die JSON-Ausgabe enthält die folgenden Attribute:

| Element | Beschreibung |
| --- | --- |
| Version |Dies bezieht sich auf die Version der Video-API. |
| index | Definiert den Frameindex des aktuellen Ereignisses (gilt nur für Azure Media Redactor). |
| timescale |„Ticks“ pro Sekunde des Videos. |
| offset |Dies ist der Zeitoffset für Zeitstempel. In Version 1.0 von Video-APIs wird dies immer 0 sein. In zukünftigen Szenarien, die wir unterstützen, kann sich dieser Wert ändern. |
| framerate |Frames des Videos pro Sekunde. |
| fragments |Die Metadaten werden in verschiedene, als Fragmente bezeichnete Segmente aufgeteilt. Jedes Fragment enthält Startzeitpunkt, Dauer, Intervallnummer und Ereignis(se). |
| Start |Die Startzeit des ersten Ereignisses in „Ticks“. |
| duration |Die Länge des Fragments in „Ticks“. |
| interval |Das Intervall jedes Ereigniseintrags innerhalb des Fragments in „Ticks“. |
| events |Jedes Ereignis enthält die innerhalb dieser Zeitspanne erkannten und nachverfolgten Gesichter. Es ist ein Array eines Arrays von Ereignissen. Das äußere Array stellt ein Zeitintervall dar. Das innere Array besteht aus 0 oder mehr Ereignissen, die zu diesem Zeitpunkt aufgetreten sind. Eine leere Klammer ( [] ) bedeutet, dass keine Gesichter erkannt wurden. |
| id |Die ID des Gesichts, das nachverfolgt wird. Diese Nummer kann sich unbeabsichtigt ändern, wenn ein Gesicht nicht mehr erkannt wird. Eine bestimmte Person sollte während des gesamten Videos die gleiche ID aufweisen, doch dies kann aufgrund von Einschränkungen im Erkennungsalgorithmus (Verdecken usw.) nicht garantiert werden. |
| x, y |Die oberen linken X- und Y-Koordinaten des Rahmens, der das Gesicht umgibt, in einem normalisierten Maßstab von 0,0 bis 1,0. <br/>X- und Y-Koordinaten sind immer relativ zum Querformat, d.h. bei einem Video im Hochformat (oder, im Fall von iOS, einem kopfstehenden) müssen Sie die Koordinaten entsprechend vertauschen. |
| width, height |Die Breite und Höhe des Rahmens, der das Gesicht umgibt, in einem normalisierten Maßstab von 0,0 bis 1,0. |
| facesDetected |Dieses Element befindet sich am Ende der JSON-Ergebnisse und fasst die Anzahl der Gesichter zusammen, die der Algorithmus während des Videos erkannt hat. Da die IDs unabsichtlich zurückgesetzt werden können, wenn ein Gesicht nicht mehr erkannt wird (z. B. wenn das Gesicht sich aus dem Bild bewegt, die Person sich abwendet), entspricht diese Zahl möglicherweise nicht immer der tatsächlichen Anzahl der Gesichter im Video. |

