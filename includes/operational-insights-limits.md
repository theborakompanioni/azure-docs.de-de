
Die folgenden Grenzwerte gelten für jeden Operational Insights-/Log Analytics-Arbeitsbereich.

|  | Kostenlos | Standard | Premium | Eigenständig | OMS |
| --- | --- | --- | --- | --- | --- |
| Tägliche Begrenzung für die Datenübertragung |500 MB<sup>1</sup> |Keine |Keine | Keine | Keine
| Aufbewahrungszeitraum |7 Tage |1 Monat |12 Monate | 1 Monat<sup>2</sup> | 1 Monat<sup>2</sup>|
| Speicherbegrenzung für Daten |500 MB * 7 Tage = 3,5 GB |Unbegrenzt |Unbegrenzt | Unbegrenzt |Unbegrenzt | 

<sup>1</sup>Wenn Kunden bei der Datenübertragung das Tageslimit von 500 MB erreichen, wird die Datenanalyse beendet und am nächsten Morgen fortgesetzt. Ein Tag wird in UTC-Zeit ausgegeben.

<sup>2</sup>Der Aufbewahrungszeitraum für die Tarife „Eigenständig“ und „OMS“ kann auf 730 Tage erhöht werden.

| Kategorie | Grenzen | Kommentare
| --- | --- | --- |
| Data Collector API (Datensammler-API) | Die maximale Größe für einen einzelnen Beitrag beträgt 30 MB.<br>Die maximale Größe für Feldwerte beträgt 32 KB. | Teilen Sie größere Volumen auf mehrere Beiträge auf.<br>Felder mit einer Länge von mehr als 32 KB werden abgeschnitten. |
| Search-API | 5000 zurückgegebene Datensätze (nicht aggregierte Daten)<br>500.000 Datensätze (aggregierte Daten) | Bei aggregierten Daten handelt es sich um eine Suche, die den Befehl `measure` enthält.
 


<!--HONumber=Jan17_HO4-->


