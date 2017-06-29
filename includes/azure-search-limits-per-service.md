Der Speicher ist durch den Festplattenspeicher oder einen festen Grenzwert für die *maximale Anzahl* von Indizes oder Dokumenten beschränkt (je nachdem, was zuerst erreicht wird).

| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Vereinbarung zum Servicelevel (SLA) |Nein <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Speicherkapazität pro Partition |50 MB |2 GB |25 GB |100 GB |200 GB |200 GB |
| Partitionen pro Dienst |– |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsgröße |– |2 GB |25 GB |100 GB |200 GB |200 GB |
| Replikate |– |3 |12 |12 |12 |12 |
| Maximale Anzahl von Indizes |3 |5 |50 |200 |200 |1000 pro Partition oder 3000 pro Dienst |
| Maximale Anzahl von Indexern |3 |5 |50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Datenquellen |3 |5 |50 |200 |200 |Keine Indexer-Unterstützung |
| Maximale Anzahl von Dokumenten |10.000 |1 Mio. |15 Millionen pro Partition oder 180 Millionen pro Dienst |60 Millionen pro Partition oder 720 Millionen pro Dienst |120 Millionen pro Partition oder 1,4 Milliarden pro Dienst |1 Millionen pro Index oder 200 Millionen pro Partition |
| Geschätzte Anzahl von Abfragen pro Sekunde (QPS) |– |~3 pro Replikat |~15 pro Replikat |~60 pro Replikat |~60 pro Replikat |>60 pro Replikat |

<sup>1</sup> Für Funktionen der Free- und Vorschauversion gilt keine Vereinbarung zum Servicelevel (Service Level Agreement, SLA). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt. 

<sup>2</sup> Für S3 HD gilt ein festes Limit von drei Partitionen. (Das Limit ist also geringer als bei S3.) Die Untergrenze für Partitionen wurde festgelegt, da die Indexanzahl für S3 HD wesentlich höher ist. Da sowohl für Computeressourcen (Speicher und Verarbeitung) als auch für Inhalte (Indizes und Dokumente) Dienstlimits gelten, wird das Inhaltslimit zuerst erreicht.
