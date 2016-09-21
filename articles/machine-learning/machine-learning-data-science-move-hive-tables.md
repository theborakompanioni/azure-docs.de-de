<properties 
	pageTitle="Erstellen und Laden von Daten in Hive-Tabellen aus Azure Blob Storage | Microsoft Azure" 
	description="Erstellen Sie Hive-Tabellen, und laden Sie Daten aus Blobs in Hive-Tabellen." 
	services="machine-learning,storage" 
	documentationCenter="" 
	authors="bradsev"
	manager="paulettm"
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/14/2016" 
	ms.author="hangzh;bradsev" />

 
#Erstellen und Laden von Daten in Hive-Tabellen aus Azure Blob Storage

In diesem Thema werden allgemeine Hive-Abfragen beschrieben, mit denen Hive-Tabellen erstellt und Daten aus Azure Blob Storage geladen werden. Außerdem finden Sie hier einige Hinweise zur Partitionierung von Hive-Tabellen sowie zur Verbesserung der Abfrageleistung mithilfe des ORC-Formats (Optimized Row Columnar).

Das Menü enthält Links zu Themen mit Informationen zur Erfassung von Daten in Zielumgebungen, in denen die Daten im Rahmen des Team Data Science-Prozesses (TDSP) gespeichert und verarbeitet werden.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie folgende Schritte bereits ausgeführt haben:
 
* Sie haben ein Azure-Speicherkonto erstellt. Eine entsprechende Anleitung finden Sie unter [Erstellen eines Azure-Speicherkontos](../hdinsight-get-started.md#storage). 
* Sie haben einen angepassten Hadoop-Cluster mit dem HDInsight-Dienst bereitgestellt. Eine entsprechende Anleitung finden Sie unter [Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess](machine-learning-data-science-customize-hadoop-cluster.md).
* Sie haben den Remotezugriff auf den Cluster aktiviert, sich angemeldet und die Hadoop-Befehlszeilenkonsole geöffnet. Eine entsprechende Anleitung finden Sie unter [Zugreifen auf den Hauptknoten des Hadoop-Clusters](machine-learning-data-science-customize-hadoop-cluster.md#headnode). 

## Hochladen von Daten in Azure Blob Storage
Wenn Sie sich beim Erstellen eines virtuellen Azure-Computers an die Anweisungen unter [Einrichten eines virtuellen Azure-Computers als IPython Notebook-Server für die erweiterte Analyse](machine-learning-data-science-setup-virtual-machine.md) gehalten haben, wurde die entsprechende Skriptdatei bereits in das Verzeichnis *C:\\Benutzer\\<Benutzername>\\Dokumente\\Data Science Scripts* auf dem virtuellen Computer heruntergeladen. Zur Übermittlung dieser Hive-Abfragen müssen Sie lediglich Ihr eigenes Datenschema und die Konfiguration von Azure Blob Storage in die entsprechenden Felder integrieren.

Es wird davon ausgegangen, dass die Daten für die Hive-Tabellen in einem **unkomprimierten** Tabellenformat vorliegen und dass die Daten in den Standardcontainer (oder in einen zusätzlichen Container) des Speicherkontos hochgeladen wurden, das vom Hadoop-Cluster verwendet wird.

Wenn Sie mit den _NYC Taxi Trip-Daten_ üben möchten, müssen Sie zuerst alle 24 Dateien mit <a href="http://www.andresmh.com/nyctaxitrips/" target="_blank">NYC Taxi Trip-Daten</a> herunterladen (12 Fahrtendateien und 12 Preisdateien), alle Dateien als CSV-Dateien **entzippen** und sie dann in den Standardcontainer (oder in einen geeigneten Container) des Azure-Speicherkontos hochladen, das im Rahmen des im Thema [Anpassen von Azure HDInsight Hadoop-Clustern für den Team Data Science-Prozess](machine-learning-data-science-customize-hadoop-cluster.md) beschriebenen Verfahrens erstellt wurde. Den Prozess zum Hochladen der CSV-Dateien in den Standardcontainer für das Speicherkonto finden Sie [hier](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Gewusst wie: Übermitteln von Hive-Abfragen

Hive-Abfragen können wie folgt übermittelt werden:

1. [Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile im Hauptknoten des Hadoop-Clusters](#headnode)
2. [Übermitteln von Hive-Abfragen mit dem Hive-Editor](#hive-editor)
3. [Übermitteln von Hive-Abfragen mit Azure PowerShell-Befehlen](#ps)
 
Hive-Abfragen sind vergleichbar mit SQL-Abfragen. Für Benutzer, die bereits mit SQL vertraut sind, sind unter Umständen die Informationen unter [Cheat Sheet „Hive for SQL Users“](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) (Spickzettel zu Hive für SQL-Benutzer) hilfreich.

Beim Übermitteln von Hive-Abfragen können Sie auch das Ausgabeziel der Hive-Abfragen steuern. Die Daten können auf dem Bildschirm, in eine lokale Datei auf dem Hauptknoten oder in ein Azure-Blob ausgegeben werden.


###<a name="headnode"></a> 1. Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile im Hauptknoten des Hadoop-Clusters

Bei komplexen Hive-Abfragen führt die direkte Übermittlung über den Hauptknoten des Hadoop-Clusters üblicherweise schneller zu Ergebnissen als die Übermittlung mittels Hive-Editor oder Azure PowerShell-Skripts.

Melden Sie sich beim Hauptknoten des Hadoop-Clusters an, öffnen Sie die Hadoop-Befehlszeile auf dem Desktop des Hauptknotens, und geben Sie den Befehl `cd %hive_home%\bin` ein.

Benutzer können Hive-Abfragen auf drei Arten über die Hadoop-Befehlszeile übermitteln:

* Direkt
* Mithilfe von HQL-Dateien
* Über die Hive-Befehlskonsole

#### Direktes Übermitteln von Hive-Abfragen über die Hadoop-Befehlszeile 

Benutzer können einen Befehl wie `hive -e "<your hive query>;` ausführen, um einfache Hive-Abfragen direkt über die Hadoop-Befehlszeile zu übermitteln. Im folgenden Beispiel ist der Befehl zur Übermittlung der Hive-Abfrage mit einem roten und die Ausgabe der Hive-Abfrage mit einem grünen Kästchen markiert:

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### Übermitteln von Hive-Abfragen in HQL-Dateien

Bei komplexeren Hive-Abfragen mit mehreren Zeilen ist die direkte Bearbeitung von Abfragen in der Befehlszeile oder in der Hive-Konsole nicht geeignet. In diesem Fall können die Hive-Abfragen alternativ mit einem Text-Editor auf dem Hauptknoten des Hadoop-Clusters in einer HQL-Datei in einem lokalen Verzeichnis auf dem Hauptknoten gespeichert werden. Die Hive-Abfrage in der HQL-Datei kann dann wie folgt mithilfe des `-f`-Arguments übermittelt werden:
	
	hive -f "<path to the .hql file>"

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Unterdrücken der Fortschrittsanzeige bei Hive-Abfragen**

Standardmäßig wird bei Hive-Abfragen, die über die Hadoop-Befehlszeile übermittelt werden, der Fortschritt des Map/Reduce-Auftrags auf dem Bildschirm angezeigt. Um die Fortschrittsanzeige des Map/Reduce-Auftrags zu unterdrücken, können Sie das Argument `-S` („S“ als Großbuchstabe) wie folgt in der Befehlszeile verwenden:

	hive -S -f "<path to the .hql file>"
	hive -S -e "<Hive queries>"

#### Übermitteln von Hive-Abfragen in der Hive-Befehlskonsole

Benutzer können auch zunächst die Hive-Befehlskonsole starten, indem Sie den Befehl `hive` in die Hadoop-Befehlszeile eingeben, und dann die Hive-Abfragen über die Hive-Befehlskonsole übermitteln. Dies wird im folgenden Beispiel veranschaulicht. In diesem Beispiel kennzeichnen die beiden roten Kästchen die Befehle, die zum Starten der Hive-Befehlskonsole verwendet werden, sowie die Hive-Abfrage, die über die Hive-Befehlskonsole übermittelt wird. Das grüne Kästchen kennzeichnet die Ausgabe der Hive-Abfrage.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

In den vorherigen Beispielen wurden die Ergebnisse der Hive-Abfrage direkt auf dem Bildschirm ausgeben. Sie können die Ausgabe aber auch in eine lokale Datei auf dem Hauptknoten oder in ein Azure-Blob schreiben. Dadurch können Sie die Ausgabe der Hive-Abfragen dann mithilfe anderer Tools analysieren.

**Ausgeben der Hive-Abfrageergebnisse in eine lokale Datei**

Für die Ausgabe der Hive-Abfrageergebnisse in ein lokales Verzeichnis auf dem Hauptknoten müssen Sie die Hive-Abfrage in der Hadoop-Befehlszeile wie folgt übermitteln:

	hive -e "<hive query>" > <local path in the head node>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage in die Datei `hivequeryoutput.txt` im Verzeichnis `C:\apps\temp` geschrieben.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Ausgeben der Hive-Abfrageergebnisse in ein Azure-Blob**

Sie können die Hive-Abfrageergebnisse auch in ein Azure-Blob im Standardcontainer des Hadoop-Clusters ausgeben. Die Hive-Abfrage muss wie folgt lauten:

	insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Im folgenden Beispiel wird die Ausgabe der Hive-Abfrage in das Blob-Verzeichnis `queryoutputdir` innerhalb des Standardcontainers des Hadoop-Clusters geschrieben. Hier müssen Sie nur den Namen des Verzeichnisses ohne den Namen des Blobs angeben. Wenn Sie sowohl den Verzeichnis- als auch den Blob-Namen angeben (beispielsweise `wasb:///queryoutputdir/queryoutput.txt`), tritt ein Fehler auf.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Wenn Sie den Standardcontainer des Hadoop-Clusters mit Tools wie dem Azure-Speicher-Explorer öffnen, sieht die Ausgabe der Hive-Abfrage wie weiter unten dargestellt aus. Sie können Filter (markiert durch den roten Kasten) anwenden, um nur das Blob mit den angegebenen Buchstaben im Namen abzurufen.

![Arbeitsbereich erstellen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a> 2. Übermitteln von Hive-Abfragen mit dem Hive-Editor

Sie können auch die Abfrage-Konsole (Hive-Editor) verwenden. Geben Sie hierzu die URL

*https://&#60;Hadoop-Clustername>.azurehdinsight.net/Home/HiveEditor*

in einen Webbrowser ein. Beachten Sie, dass Sie zur Eingabe der Anmeldeinformationen für den Hadoop-Cluster aufgefordert werden.

###<a name="ps"></a> 3. Übermitteln von Hive-Abfragen mit Azure PowerShell-Befehlen

Benutzer können Hive-Abfragen auch mithilfe von PowerShell übermitteln. Eine entsprechende Anleitung finden Sie unter [Übermitteln von Hive-Aufträgen mit PowerShell](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Erstellen von Hive-Datenbanken und -Tabellen

Die Hive-Abfragen werden im [GitHub-Repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) freigegeben und können von dort heruntergeladen werden.

Die Hive-Abfrage zum Erstellen einer Hive-Tabelle sieht wie folgt aus:

    create database if not exists <database name>;
	CREATE EXTERNAL TABLE if not exists <database name>.<table name>
	(
		field1 string, 
		field2 int, 
		field3 float, 
		field4 double, 
		...,
		fieldN string
	) 
	ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>' 
	STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Im Anschluss werden die zu integrierenden Felder sowie weitere Konfigurationen beschrieben:

- **&#60;Datenbankname>**: Der Name der zu erstellenden Datenbank. Wenn Sie die Standarddatenbank verwenden möchten, kann die Abfrage *create database...* ausgelassen werden. 
- **&#60;Tabellenname>**: Der Name der zu erstellenden Tabelle in der angegebenen Datenbank. Wenn Sie die Standarddatenbank verwenden möchten, kann mit *&#60;Tabellenname>* ohne &#60;Datenbankname> direkt auf die Tabelle verwiesen werden.
- **&#60;Feldtrennzeichen>**: Das Trennzeichen für die Felder in der Datendatei, die in die Hive-Tabelle hochgeladen werden soll. 
- **&#60;Zeilentrennzeichen>**: Das Trennzeichen für die Zeilen in der Datendatei. 
- **&#60;Speicherort>**: Der Azure-Speicherort zum Speichern der Daten von Hive-Tabellen. Ohne Angabe von *LOCATION &#60;Speicherort>* werden die Datenbank und die Tabellen im Verzeichnis *hive/warehouse/* im Standardcontainer des Hive-Clusters gespeichert. Wenn Sie den Speicherort angeben möchten, muss sich dieser im Standardcontainer für die Datenbank und die Tabellen befinden. Dieser Speicherort muss relativ zum Standardcontainer des Clusters im Format *wasb:///&#60;Verzeichnis 1>/* oder *wasb:///&#60;Verzeichnis 1>/&#60;Verzeichnis 2>/* usw. angegeben werden. Nach dem Ausführen der Abfrage werden die relativen Verzeichnisse innerhalb des Standardcontainers erstellt. 
- **TBLPROPERTIES("skip.header.line.count"="1")**: Wenn die Datendatei einen Header enthält, müssen Sie diese Eigenschaft **am Ende** der Abfrage *create table* hinzufügen. Andernfalls wird der Header als Datensatz in die Tabelle geladen. Wenn die Datendatei keinen Header enthält, kann dieser Konfigurationsschritt in der Abfrage ausgelassen werden. 

## <a name="load-data"></a>Laden von Daten in Hive-Tabellen
Die folgende Hive-Abfrage dient zum Laden von Daten in eine Hive-Tabelle:

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **&#60;Pfad zu Blobdaten>**: Wenn sich die in die Hive-Tabelle hochzuladende Blobdatei im Standardcontainer des HDInsight Hadoop-Clusters befindet, muss *&#60;Pfad zu Blobdaten>* das Format *wasb:///&#60;Verzeichnis in diesem Container>/&#60;Blobdateiname>* haben. Die Blobdatei kann sich auch in einem zusätzlichen Container des HDInsight Hadoop-Clusters befinden. In diesem Fall muss *& #60;Pfad zu Blobdaten>* das Format *wasb://&#60;Containername>@&#60;Speicherkontoname>.blob.core.windows.net/&#60;Blobdateiname>* haben.

	>[AZURE.NOTE] Die Blobdaten, die in die Hive-Tabelle hochgeladen werden sollen, müssen sich im Standardcontainer oder in einem zusätzlichen Container des Speicherkontos für den Hadoop-Cluster befinden. Andernfalls ist die Abfrage *LOAD DATA* nicht erfolgreich, da sie keinen Zugriff auf die Daten hat.


## <a name="partition-orc"></a>Weiterführende Themen: Partitionieren von Tabellen und Speichern von Hive-Daten im ORC-Format

Wenn die Daten sehr umfangreich sind, ist eine Partitionierung der Tabelle bei Abfragen, bei denen nur wenige Partitionen der Tabelle durchsucht werden müssen, vorteilhaft. So ist es beispielsweise sinnvoll, die Protokolldaten einer Website nach Datum zu partitionieren.

Neben der Partitionierung der Hive-Tabellen ist es auch von Vorteil, die Hive-Daten im ORC-Format (Optimized Row Columnar) zu speichern. Weitere Informationen zum ORC-Format finden Sie unter <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">ORC File Format</a> (ORC-Dateiformat).

### Partitionierte Tabelle
Die folgende Hive-Abfrage erstellt eine Hive-Tabelle und lädt Daten in diese Tabelle:

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
	(field1 string,
	...
	fieldN string
	)
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
		 lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
	LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name> 
		PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Beim Abfragen partitionierter Tabellen empfiehlt es sich, die Partitionierungsbedingung **am Anfang** der Klausel `where` hinzuzufügen, um die Effizienz der Suche zu verbessern.

    select 
		field1, field2, ..., fieldN
	from <database name>.<partitioned table name> 
	where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Speichern von Hive-Daten im ORC-Format

Im ORC-Format gespeicherte Daten können nicht direkt aus Blob Storage in Hive-Tabellen geladen werden. Mit den folgenden Schritten können Sie Daten im ORC-Format aus Azure-Blobs in Hive-Tabellen laden.

1. Erstellen Sie die externe Tabelle **STORED AS TEXTFILE**, und laden Sie Daten aus dem Blobspeicher in die Tabelle:

		CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
		(
			field1 string,
			field2 int,
			...
			fieldN date
		)
		ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' 
			lines terminated by '<line separator>' STORED AS TEXTFILE 
			LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

		LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

2. Erstellen Sie eine interne Tabelle mit dem gleichen Schema und dem gleichen Feldtrennzeichen wie bei der in Schritt 1 erstellten externen Tabelle, und speichern Sie die Hive-Daten im ORC-Format:

		CREATE TABLE IF NOT EXISTS <database name>.<ORC table name> 
		(
			field1 string,
			field2 int,
			...
			fieldN date
		) 
		ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

3. Wählen Sie Daten aus der in Schritt 1 erstellten externen Tabelle aus, und fügen Sie sie in die ORC-Tabelle ein:

		INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

	>[AZURE.NOTE] Wenn die TEXTFILE-Tabelle *&#60;Datenbankname>.&#60;Name der externen TEXTFILE-Tabelle>* Partitionen besitzt, fügt der Befehl `SELECT * FROM <database name>.<external textfile table name>` in Schritt 3 die Partitionsvariable als Feld in das zurückgegebene Dataset ein. Das Einfügen in *&#60;Datenbankname>.&#60;ORC-Tabellenname>* ist nicht erfolgreich, da *&#60;Datenbankname>.&#60;ORC-Tabellenname>* die Partitionsvariable nicht als Feld im Tabellenschema enthält. In diesem Fall müssen Sie die in *&#60;Datenbankname>.&#60;ORC-Tabellenname>* einzufügenden Felder explizit wie folgt auswählen:

		INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
		   SELECT field1, field2, ..., fieldN
		   FROM <database name>.<external textfile table name> 
		   WHERE <partition variable>=<partition value>;

4. Sie können *&#60;Name der externen TEXTFILE-Tabelle>* problemlos mithilfe der folgenden Abfrage löschen, nachdem alle Daten in *&#60;Datenbankname>.&#60;ORC-Tabellenname>* eingefügt wurden:

		DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Sie besitzen nun eine einsatzbereite Tabelle mit Daten im ORC-Format.

<!---HONumber=AcomDC_0622_2016-->