**Der grundlegende Data Lake Analytics-Prozess:**
	
![Azure Data Lake Analytics-Prozessflussdiagramm](./media/data-lake-analytics-basic-process-include/data-lake-analytics-process.png)
	
1. Erstellen Sie ein Data Lake Analytics-Konto.
2. Vorbereiten der Quelldaten. Data Lake Analytics-Aufträge können Daten entweder von Azure Data Lake-Speicherkonten oder von Azure Blob-Speicherkonten lesen.
3. Entwickeln Sie ein U-SQL-Skript.
4. Übermitteln Sie einen Auftrag (U-SQL-Skript) an das Data Lake Analytics-Konto. Für den Auftrag werden die Quelldaten gelesen, die Daten werden gemäß Anweisung im U-SQL-Skript verarbeitet, und anschließend wird die Ausgabe entweder in einem Data Lake-Speicherkonto oder einem BLOB-Speicherkonto gespeichert.

<!---HONumber=AcomDC_0921_2016-->