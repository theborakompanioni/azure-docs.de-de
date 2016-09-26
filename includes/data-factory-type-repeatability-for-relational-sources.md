## Wiederholbarkeit beim Kopieren

Beim Kopieren von Daten aus und in relationale Speicher müssen Sie die Wiederholbarkeit berücksichtigen, um unbeabsichtigte Ergebnisse zu vermeiden.

Ein Slice kann in Azure Data Factory automatisch gemäß der angegebenen Wiederholungsrichtlinie wiederholt werden. Sie sollten eine Wiederholungsrichtlinie zum Schutz vor vorübergehenden Fehlern festzulegen. Wiederholbarkeit ist ein wichtiger Aspekt, den Sie beim Verschieben von Daten berücksichtigen sollten.

**Als Quelle:**

> [AZURE.NOTE] Die folgenden Beispiele beziehen sich auf Azure SQL, sie gelten aber auch für beliebige Datenspeicher, die rechteckige Datasets unterstützen. Sie müssen möglicherweise den **Typ** der Quelle und die **query**-Eigenschaft (z.B. „query“ anstelle von sqlReaderQuery) für den Datenspeicher anpassen.

Generell möchten Sie beim Lesen aus relationalen Speichern nur die Daten lesen, die dem Slice entsprechen. Sie können dies beispielsweise erreichen, indem Sie die Variablen "WindowStart" und "WindowEnd" verwenden, die in Azure Data Factory verfügbar sind. Im Artikel [Planung und Ausführung](../articles/data-factory/data-factory-scheduling-and-execution.md) erfahren Sie mehr über die Variablen und Funktionen in Azure Data Factory. Beispiel:
	
	  "source": {
	    "type": "SqlSource",
	    "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm\\'', WindowStart, WindowEnd)"
	  },

Mit dieser Abfrage werden Daten aus „MyTable“ gelesen, die zum Dauerbereich für den Slice gehören. Bei einer erneuten Ausführung dieses Slices wäre dieses Verhalten auch immer sichergestellt.

In anderen Fällen möchten Sie möglicherweise die gesamte Tabelle lesen (beispielsweise bei einer einmaligen Verschiebung) und könnten "sqlReaderQuery" wie folgt definieren:

	
	"source": {
	            "type": "SqlSource",
	            "sqlReaderQuery": "select * from MyTable"
	          },
	

<!---HONumber=AcomDC_0914_2016-->