
<!--
includes/sql-data-warehouse-include-pause-description.md

Latest Freshness check:  2016-04-22 , barbkess.

As of circa 2016-04-22, the following topics might include this include:
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-powershell.md
articles/sql-data-warehouse/sql-data-warehouse-manage-scale-out-tasks-rest-api.md

-->
Um Kosten zu sparen, können Sie Computeressourcen bei Bedarf anhalten und fortsetzen. Wenn Sie die Datenbank z.B. nachts oder am Wochenende nicht verwenden, können Sie sie während dieser Zeiträume anhalten und während des Tages wieder fortsetzen. DWUs werden Ihnen nicht in Rechnung gestellt, während die Datenbank angehalten wird.

Wenn Sie eine Datenbank anhalten, geschieht Folgendes:

* Compute- und Speicherressourcen werden an den Pool der verfügbaren Ressourcen im Rechenzentrum zurückgegeben.
* Die DWU-Kosten sind für die Dauer der Pause gleich null.
* Die Speicherung von Daten ist nicht betroffen, und Ihre Daten bleiben intakt. 
* SQL Data Warehouse bricht alle Vorgänge ab, die gerade ausgeführt werden oder in der Warteschlange stehen.

