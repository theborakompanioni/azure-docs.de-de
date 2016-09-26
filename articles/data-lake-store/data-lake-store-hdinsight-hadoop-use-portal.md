<properties
   pageTitle="Erstellen von HDInsight-Clustern mit Azure Data Lake-Speicher mithilfe des Portals | Azure"
   description="Verwenden des Azure-Portals zum Erstellen und Verwenden von HDInsight-Clustern mit Azure Data Lake-Speicher"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/29/2016"
   ms.author="nitinme"/>

# Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals

> [AZURE.SELECTOR]
- [Verwenden des Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Mithilfe von PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)


Erfahren Sie, wie Sie das Azure-Portal zum Erstellen eines HDInsight-Clusters (Hadoop, HBase, Spark oder Storm) mit Zugriff auf den Azure Data Lake-Speicher verwenden. Einige wichtige Hinweise zu dieser Version:

* **Bei Spark-Clustern (Linux) und Hadoop-Clustern (Windows und Linux)** kann der Data Lake-Speicher nur als zusätzliches Speicherkonto verwendet werden. Standardspeicherkonten für solche Cluster sind weiterhin Azure-Speicherblobs (WASB).

* **Bei Storm-Clustern (Windows und Linux)** kann der Data Lake-Speicher zum Schreiben von Daten aus einer Storm-Topologie verwendet werden. Der Data Lake-Speicher kann auch zum Speichern von Verweisdaten verwendet werden, die anschließend von einer Storm-Topologie gelesen werden. Weitere Informationen finden Sie unter [Verwenden des Data Lake-Speichers in einer Storm-Topologie](#use-data-lake-store-in-a-storm-topology).

* **Für HBase-Cluster (Windows und Linux)** können Sie Data Lake Store als standardmäßigen oder zusätzlichen Speicher verwenden. Weitere Informationen finden Sie unter [Verwenden des Data Lake-Speichers mit HBase-Clustern](#use-data-lake-store-with-hbase-clusters).

> [AZURE.NOTE] Einige wichtige Punkte sind zu beachten.
> 
> * Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake-Speicher ist nur für die HDInsight-Versionen 3.2 und 3.4 (für Hadoop-, HBase- und Storm-Cluster unter Windows und Linux) verfügbar. Für Spark-Cluster unter Linux ist diese Option nur auf HDInsight 3.4-Clustern verfügbar.
>
> * Wie bereits erwähnt, steht Data Lake-Speicher als Standardspeicher für einige Clustertypen (HBase) und zusätzlicher Speicher für andere Cluster (Hadoop, Spark, Storm) zur Verfügung. Das Verwenden von Data Lake-Speicher als zusätzliches Speicherkonto wirkt sich nicht auf Leistung oder die Fähigkeit aus, Daten aus dem Cluster in den Speicher zu lesen bzw. zu schreiben. In einem Szenario, in dem Data Lake-Speicher als zusätzlicher Speicher verwendet wird, werden clusterbezogene Dateien (z.B. Protokolle usw.) in den Standardspeicher (Azure-Blobs) geschrieben, während die Daten, die Sie verarbeiten möchten, in einem Data Lake-Speicherkonto gespeichert werden können.


## Voraussetzungen

Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Store. Weitere Informationen finden Sie in den [Anweisungen](data-lake-store-get-started-portal.md#signup).
- **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md) aus. Führen Sie nach dem Erstellen des Kontos die folgenden Schritte aus, um einige Beispieldaten hochzuladen. Sie benötigen diese Daten später im Tutorial zum Ausführen von Aufträgen über einen HDInsight-Cluster, bei denen auf Daten im Data Lake-Speicher zugegriffen wird.

	* [Erstellen Sie einen Ordner im Data Lake-Speicher](data-lake-store-get-started-portal.md#createfolder).
	* [Laden Sie eine Datei in Ihren Data Lake-Speicher hoch](data-lake-store-get-started-portal.md#uploaddata). Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData) herunterladen.

## Lernen Sie schneller mit Videos?

Sehen Sie sich die folgenden Videos an, um zu verstehen, wie HDInsight-Cluster mit Zugriff auf den Data Lake-Speicher bereitgestellt werden.

* [Erstellen eines HDInsight-Clusters mit Zugriff auf den Data Lake-Speicher](https://mix.office.com/watch/l93xri2yhtp2)
* [Zugreifen auf Daten im Data Lake-Speicher mit Hive- und Pig-Skripts](https://mix.office.com/watch/1n9g5w0fiqv1q), nachdem der Cluster eingerichtet wurde

## Erstellen eines HDInsight-Clusters mit Zugriff auf den Azure Data Lake-Speicher

In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster, für den der Data Lake-Speicher als zusätzlicher Speicher verwendet wird. In dieser Version kann der Data Lake-Speicher für einen Hadoop-Cluster nur als zusätzlicher Speicher für den Cluster verwendet werden. Als Standardspeicher werden weiterhin Azure Storage-BLOBS (WASB) verwendet. Wir erstellen daher zuerst das Speicherkonto und die Speichercontainer, die für den Cluster erforderlich sind.

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an.

2. Führen Sie die Schritte unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-provision-clusters.md#create-using-the-preview-portal) aus, um mit der Bereitstellung eines HDInsight-Clusters zu beginnen.

3. Klicken Sie auf dem Blatt **Optionale Konfiguration** auf **Datenquelle**. Geben Sie auf dem Blatt **Datenquelle** die Details für das Speicherkonto und den Speichercontainer an, geben Sie den **Standort** als **USA (Osten) 2** an, und klicken Sie dann auf **Cluster-AAD-Identität**.

	![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

4. Auf dem Blatt **Cluster-AAD-Identität** können Sie einen vorhandenen Dienstprinzipal auswählen oder einen neuen erstellen.

	* **Erstellen eines neuen Dienstprinzipals**

		* Klicken Sie auf dem Blatt **Azure Active Directory-Identität für den Cluster** auf **Neu erstellen** und dann auf **Dienstprinzipal**, und geben Sie auf dem Blatt **Dienstprinzipal erstellen** Werte zum Erstellen eines neuen Dienstprinzipals an. Hierbei werden auch ein Zertifikat und eine Azure Active Directory-Anwendung erstellt. Klicken Sie auf **Erstellen**.

			![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

		* Klicken Sie auf dem Blatt **Azure Active Directory-Identität für den Cluster** auf **ADLS-Zugriff verwalten**. In diesem Bereich werden die Data Lake-Speicherkonten angezeigt, die dem Abonnement zugeordnet sind. Sie können die Berechtigungen aber nur für das von Ihnen erstellte Konto festlegen. Wählen Sie Berechtigungen vom Typ LESEN/SCHREIBEN/AUSFÜHREN für das Konto aus, das Sie dem HDInsight-Cluster zuordnen möchten, und klicken Sie dann auf **Berechtigungen speichern**.

			![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

		* Klicken Sie auf dem Blatt **Azure Active Directory-Identität für den Cluster** auf **Zertifikat herunterladen**, um das Zertifikat herunterzuladen, das dem von Ihnen erstellten Dienstprinzipal zugeordnet ist. Dies ist hilfreich, wenn Sie denselben Dienstprinzipal später beim Erstellen weiterer HDInsight-Cluster verwenden möchten. Klicken Sie auf **Auswählen**.

			![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.4.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")


	* **Auswählen eines vorhandenen Dienstprinzipals**

		* Klicken Sie auf dem Blatt **Azure Active Directory-Identität für den Cluster** auf **Vorhandene verwenden** und dann auf **Dienstprinzipal**. Suchen Sie anschließend auf dem Blatt **AD-Dienstprinzipal auswählen** nach einem vorhandenen Dienstprinzipal. Klicken Sie auf den Namen eines Dienstprinzipals und dann auf **Auswählen**.

			![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

		* Laden Sie auf dem Blatt **Azure Active Directory-Identität für den Cluster** das Zertifikat (PFX) hoch, das dem ausgewählten Dienstprinzipal zugeordnet ist, und geben Sie dann das Zertifikatkennwort an.

5. Klicken Sie auf **ADLS-Zugriff verwalten** und dann auf **Dateiberechtigungen auswählen**.

	![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.existing.save.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

6. Wählen Sie auf dem Blatt **Dateiberechtigungen auswählen** in der Dropdownliste **Konto** das Data Lake Store-Konto aus, das Sie mit dem HDInsight-Cluster verknüpfen möchten. Auf dem Blatt werden alle im ausgewählten Data Lake Store-Konto verfügbaren Dateien und Ordner aufgeführt.
 
	![Zugriff auf Data Lake Store bereitstellen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi-adl-permission-1.png "Zugriff auf Data Lake Store bereitstellen")

	Danach bestimmen Sie die Berechtigungen, die für die ausgewählten Dateien und Ordner bereitgestellt werden sollen. Geben Sie bei Ordnern auch an, ob die Berechtigungen nur für den Ordner oder für den Ordner und alle darin enthaltenen untergeordneten Elemente gelten. Wählen Sie dazu den geeigneten Wert aus der Dropdownliste **Anwenden auf** aus. Um eine Berechtigung zu entfernen, klicken Sie auf das Symbol **Löschen**.

	![Zugriff auf Data Lake Store bereitstellen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi-adl-permission-2.png "Zugriff auf Data Lake Store bereitstellen")

	Wiederholen Sie diese Schritte auch für die zugehörigen Dateien und Ordner aus anderen Data Lake Store-Konten. Wenn Sie mit der Zuweisung der Berechtigungen fertig sind, klicken Sie unten auf dem Blatt auf **Auswählen**.

7. Überprüfen Sie auf dem Blatt **Ausgewählte Berechtigungen zuweisen** die bereitgestellten Berechtigungen, und klicken Sie dann auf **Ausführen**, um diese Berechtigungen zu gewähren.

	![Zugriff auf Data Lake Store bereitstellen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi-adl-permission-3.png "Zugriff auf Data Lake Store bereitstellen")

	Die Statusspalte zeigt den Fortschritt an. Sobald alle Berechtigungen erfolgreich zugewiesen sind, klicken Sie auf **Fertig**.

6. Klicken Sie auf den Blättern **Azure Active Directory-Identität für den Cluster** und **Datenquelle** auf **Auswählen**, und fahren Sie mit der Clustererstellung wie unter [Erstellen von Hadoop-Clustern in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) beschrieben fort.

7. Nach der Bereitstellung des Clusters können Sie überprüfen, ob der Dienstprinzipal dem HDInsight-Cluster zugeordnet wurde. Klicken Sie auf dem Clusterblatt auf **Azure Active Directory-Identität für den Cluster**, um den zugeordneten Dienstprinzipal anzuzeigen.

	![Dienstprinzipal für HDInsight-Cluster hinzufügen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "Dienstprinzipal für HDInsight-Cluster hinzufügen")

## Ausführen von Testaufträgen im HDInsight-Cluster zur Verwendung des Azure Data Lake-Speichers

Nachdem Sie einen HDInsight-Cluster konfiguriert haben, können Sie Testaufträge für den Cluster ausführen, um zu testen, ob der HDInsight-Cluster auf Daten im Azure Data Lake-Speicher zugreifen kann. Hierzu führen wir einige Hive-Abfragen für den Data Lake-Speicher aus.

### Für einen Linux-Cluster

1. Öffnen Sie das Clusterblatt für den Cluster, den Sie bereitgestellt haben, und klicken Sie dann auf **Dashboard**. Dadurch wird Ambari für den Linux-Cluster geöffnet. Beim Zugriff auf Ambari werden Sie aufgefordert, sich bei der Website zu authentifizieren. Geben Sie den Administratorkontonamen (Standardeinstellung: admin) und das Kennwort ein, den bzw. das Sie beim Erstellen des Clusters verwendet haben.

	![Clusterdashboard starten](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster1.png "Clusterdashboard starten")

	Sie können auch direkt zu Ambari navigieren, indem Sie in einem Webbrowser https://CLUSTERNAME.azurehdinsight.net aufrufen (wobei **CLUSTERNAME** der Name Ihres HDInsight-Clusters ist).

2. Öffnen Sie die Hive-Ansicht. Klicken Sie im Seitenmenü auf die Gruppe von Quadraten (neben dem Link und der Schaltfläche für **Admin** rechts auf der Seite), um verfügbare Ansichten aufzulisten. Wählen Sie die Ansicht **Hive** aus.

	![Ambari-Ansichten auswählen](./media/data-lake-store-hdinsight-hadoop-use-portal/selecthiveview.png)

3. Eine Seite ähnlich der folgenden wird angezeigt:

	![Bild der Hive-Ansichtenseite, die einen Abfrage-Editor-Abschnitt enthält](./media/data-lake-store-hdinsight-hadoop-use-portal/hiveview.png)

4. Fügen Sie im Abschnitt **Abfrage-Editor** der Seite die folgende HiveQL-Anweisung in das Arbeitsblatt ein:

		CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder'

5. Klicken Sie unten im **Abfrage-Editor** auf die Schaltfläche **Ausführen**, um die Abfrage zu starten. Der Abschnitt **Abfrageprozessergebnisse** sollte unterhalb des **Abfrage-Editors** angezeigt werden und Informationen über den Auftrag enthalten.

6. Nach Abschluss der Abfrage werden im Abschnitt **Abfrageprozessergebnisse** die Ergebnisse des Vorgangs angezeigt. Die Registerkarte **Ergebnisse** sollte folgende Informationen enthalten:

7. Führen Sie die folgende Abfrage aus, um sicherzustellen, dass die Tabelle erstellt wurde.

		SHOW TABLES;

	Auf der Registerkarte **Ergebnisse** sollte Folgendes angezeigt werden:

		hivesampletable
		vehicles

	**vehicles** ist die Tabelle, die Sie bereits erstellt haben. **hivesampletable** ist eine Beispieltabelle, die in allen HDInsight-Clustern standardmäßig verfügbar ist.

8. Sie können auch eine Abfrage ausführen, um Daten aus der Tabelle **vehicles** abzurufen.

		SELECT * FROM vehicles LIMIT 5;

### Für einen Windows-Cluster

1. Öffnen Sie das Clusterblatt für den Cluster, den Sie bereitgestellt haben, und klicken Sie dann auf **Dashboard**.

	![Clusterdashboard starten](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster1.png "Clusterdashboard starten")

	Geben Sie die Administrator-Anmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

2. Die Microsoft Azure HDInsight-Abfragekonsole wird geöffnet. Klicken Sie auf **Hive-Editor**.

	![Hive-Editor öffnen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster2.png "Hive-Editor öffnen")

3. Geben Sie im Hive-Editor die folgende Abfrage ein, und klicken Sie dann auf **Senden**.

		CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder'

	In dieser Hive-Abfrage erstellen wir eine Tabelle aus den Daten, die im Data Lake-Speicher unter `adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder` gespeichert sind. An diesem Speicherort befindet sich eine Beispieldatendatei, die Sie zuvor hochgeladen haben sollten.

	Unten in der Tabelle **Auftragssitzung** wird angezeigt, wie sich der Status des Auftrags von **Wird initialisiert** in **Wird ausgeführt** und **Abgeschlossen** ändert. Sie können auch auf **Details anzeigen** klicken, um weitere Informationen zum abgeschlossenen Auftrag anzuzeigen.

	![Tabelle erstellen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdiadlcluster3.png "Tabelle erstellen")

4. Führen Sie die folgende Abfrage aus, um sicherzustellen, dass die Tabelle erstellt wurde.

		SHOW TABLES;

	Klicken Sie für diese Abfrage auf **Details anzeigen**. Die Ausgabe sollte wie folgt angezeigt werden:

		hivesampletable
		vehicles

	**vehicles** ist die Tabelle, die Sie bereits erstellt haben. **hivesampletable** ist eine Beispieltabelle, die in allen HDInsight-Clustern standardmäßig verfügbar ist.

5. Sie können auch eine Abfrage ausführen, um Daten aus der Tabelle **vehicles** abzurufen.

		SELECT * FROM vehicles LIMIT 5;


## Zugreifen auf den Data Lake-Speicher mit HDFS-Befehlen

Nachdem Sie den HDInsight-Cluster für die Verwendung des Data Lake-Speichers konfiguriert haben, können Sie die HDFS-Shellbefehle verwenden, um auf den Speicher zuzugreifen.

### Für einen Linux-Cluster

In diesem Abschnitt stellen Sie eine SSH-Verbindung mit dem Cluster her und führen die HDFS-Befehle aus. Windows bietet keinen integrierten SSH-Client. Wir empfehlen die Verwendung von **PuTTY**. Sie können das Programm unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) herunterladen.

Weitere Informationen zum Verwenden von PuTTY finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Nachdem die Verbindung hergestellt wurde, listen Sie mithilfe des folgenden HDFS-Dateisystembefehls die Dateien im Data Lake-Speicher auf.

	hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Hierbei sollte auch die Datei aufgeführt werden, die Sie bereits in den Data Lake-Speicher hochgeladen haben.

	15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
	Found 1 items
	-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Sie können auch den Befehl `hdfs dfs -put` verwenden, um Dateien in den Data Lake-Speicher hochzuladen, und dann mit `hdfs dfs -ls` überprüfen, ob der Upload der Dateien erfolgreich war.


### Für einen Windows-Cluster

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Durchsuchen**, auf **HDInsight-Cluster** und dann auf den HDInsight-Cluster, den Sie erstellt haben.

3. Klicken Sie auf dem Clusterblatt auf **Remotedesktop** und dann auf dem Blatt **Remotedesktop** auf **Verbinden**.

	![Remoteverbindung mit HDI-Cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/ADL.HDI.PS.Remote.Desktop.png "Erstellen einer Azure-Ressourcengruppe")

	Geben Sie bei Aufforderung die Anmeldeinformationen ein, die Sie für den Remotedesktopbenutzer bereitgestellt haben.

4. Starten Sie in der Remotesitzung die Windows PowerShell, und verwenden Sie die HDFS-Dateisystembefehle, um die Dateien im Azure Data Lake-Speicher aufzulisten.

	 	hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

	Hierbei sollte auch die Datei aufgeführt werden, die Sie bereits in den Data Lake-Speicher hochgeladen haben.

		15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
		Found 1 items
		-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

	Sie können auch den Befehl `hdfs dfs -put` verwenden, um Dateien in den Data Lake-Speicher hochzuladen, und dann mit `hdfs dfs -ls` überprüfen, ob der Upload der Dateien erfolgreich war.

## Verwenden eines Data Lake-Speichers mit Spark-Clustern

In diesem Abschnitt führen Sie mithilfe des Jupyter-Notebooks, das in HDInsight Spark-Clustern zur Verfügung steht, einen Auftrag aus, bei dem Daten nicht aus dem standardmäßigen Azure Storage-Blobkonto, sondern aus einem Data Lake-Speicherkonto gelesen werden, das Sie einem HDInsight Spark-Cluster zugeordnet haben.

1. Kopieren Sie einige Beispieldaten aus dem Standardspeicherkonto (WASB), das dem Spark-Cluster zugeordnet ist, in das Azure Data Lake-Speicherkonto, das ebenfalls dem Cluster zugeordnet ist. Sie können hierfür das [AdlCopy-Tool](http://aka.ms/downloadadlcopy) verwenden. Laden Sie das Tool über den Link herunter, und installieren Sie es.

2. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Verzeichnis, in dem „AdlCopy“ installiert ist, in der Regel `%HOMEPATH%\Documents\adlcopy`.

3. Führen Sie den folgenden Befehl zum Kopieren eines bestimmten Blobs aus dem Quellcontainer in einen Data Lake-Speicher aus:

		AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

	Kopieren Sie für dieses Tutorial die Beispieldatendatei **HVAC.csv** aus dem Pfad **/HdiSamples/HdiSamples/SensorSampleData/hvac/** in das Azure Data Lake Store-Konto. Der Codeausschnitt sollte folgendermaßen aussehen:

		AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

	>[AZURE.WARNING] Stellen Sie sicher, dass die Groß-/Kleinschreibung für die Datei- und Pfadnamen beachtet wurde.

4. Sie werden aufgefordert, die Anmeldeinformationen für das Azure-Abonnement einzugeben, dem Ihr Data Lake-Speicherkonto zugeordnet ist. Eine Ausgabe ähnlich der folgenden wird angezeigt.

		Initializing Copy.
		Copy Started.
		100% data copied.
		Copy Completed. 1 file copied.

	Die Datendatei (**HVAC.csv**) wird in einen Ordner **/hvac** im Data Lake Store-Konto kopiert.

4. Klicken Sie im [Azure-Portal](https://portal.azure.com/) im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.

2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Quicklinks** und anschließend auf dem Blatt **Cluster Dashboard** auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

	> [AZURE.NOTE] Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie __CLUSTERNAME__ durch den Namen Ihres Clusters:
	>
	> `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Erstellen Sie ein neues Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

	![Erstellen eines neuen Jupyter Notebooks](./media/data-lake-store-hdinsight-hadoop-use-portal/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notebooks")

3. Ein neues Notebook mit dem Namen **Untitled.pynb** wird erstellt und geöffnet.

4. Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark-, und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie können zunächst die Typen importieren, die für dieses Szenario erforderlich sind. Fügen Sie dazu den folgenden Codeausschnitt in eine Zelle ein, und drücken Sie **UMSCHALT+EINGABE**.

		from pyspark.sql.types import *
		
	Bei jedem Ausführen eines Auftrags in Jupyter wird in der Titelleiste Ihres Webbrowserfensters neben dem Notebooktitel der Status **(Beschäftigt)** angezeigt. Außerdem sehen Sie in der rechten oberen Ecke einen ausgefüllten Kreis neben dem Text **PySpark**. Wenn der Auftrag abgeschlossen ist, wird ein Kreis ohne Füllung angezeigt.

	 ![Status eines Jupyter Notebook-Auftrags](./media/data-lake-store-hdinsight-hadoop-use-portal/hdispark.jupyter.job.status.png "Status eines Jupyter Notebook-Auftrags")

4. Verwenden Sie die Datei **HVAC.csv**, die Sie in das Data Lake Store-Konto kopiert haben, um Beispieldaten in eine temporäre Tabelle zu laden. Sie können mithilfe des folgenden URL-Musters auf die Daten im Data Lake-Speicherkonto zugreifen.

		adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

	Fügen Sie das folgende Codebeispiel in eine leere Zelle ein, ersetzen Sie **MYDATALAKESTORE** durch den Namen Ihres Data Lake Store-Kontos, und drücken Sie **UMSCHALT+EINGABETASTE**. Mit diesem Beispielcode werden die Daten in einer temporären Tabelle mit dem Namen **hvac** registriert.

		# Load the data
		hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/hvac/HVAC.csv")
		
		# Create the schema
		hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])
		
		# Parse the data in hvacText
		hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))
		
		# Create a data frame
		hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)
		
		# Register the data fram as a table to run queries against
		hvacdf.registerTempTable("hvac")

5. Da Sie einen PySpark-Kernel verwenden, können Sie jetzt direkt eine SQL-Abfrage für die temporäre Tabelle **hvac** ausführen, die Sie gerade mit der `%%sql`-Magic erstellt haben. Weitere Informationen zur `%%sql`-Magic sowie anderen mit dem PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit HDInsight Spark-Linux-Clustern in HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).
		
		%%sql
		SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = "6/1/13"

5. Nachdem der Auftrag erfolgreich abgeschlossen wurde, wird die folgende tabellarische Ausgabe standardmäßig angezeigt.

 	![Tabellenausgabe des Abfrageergebnisses](./media/data-lake-store-hdinsight-hadoop-use-portal/tabular.output.png "Tabellenausgabe des Abfrageergebnisses")

	Sie können die Ergebnisse auch in anderen Visualisierungen anzeigen. Beispielsweise würde ein Bereichsdiagramm für dieselbe Ausgabe wie folgt aussehen.

	![Bereichsdiagramm des Abfrageergebnisses](./media/data-lake-store-hdinsight-hadoop-use-portal/area.output.png "Bereichsdiagramm des Abfrageergebnisses")


6. Nach Ausführung der Anwendung empfiehlt es sich, das Notebook herunterzufahren, um die Ressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum Schließen und Anhalten. Dadurch wird das Notebook heruntergefahren und geschlossen.

## Verwenden des Data Lake-Speichers in einer Storm-Topologie

Sie können den Data Lake-Speicher verwenden, um dort Daten aus einer Storm-Topologie zu schreiben. Informationen zum Umsetzen dieses Szenarios finden Sie unter [Verwenden von Azure Data Lake Store mit Apache Storm und HDInsight](../hdinsight/hdinsight-storm-write-data-lake-store.md).

## Verwenden des Data Lake-Speichers mit HBase-Clustern

Mit HBase-Clustern können Sie den Data Lake-Speicher sowohl als Standardspeicher als auch zusätzlichen Speicher verwenden. Gehen Sie dazu wie folgt vor:

1.  Wählen Sie auf dem Blatt **Datenquelle** für **HBase-Datenspeicherort** die Option **Data Lake Store** aus.
2.  Wählen Sie den Namen des Data Lake-Speichers aus, den Sie verwenden möchten, oder erstellen Sie einen neuen.
3.  Geben Sie zum Abschluss den **HBase-Stammordner** im Data Lake Store an. Wenn das Data Lake-Speicherkonto keinen Stammordner besitzt, erstellen Sie einen neuen.

	![HBase mit Data Lake-Speicher](./media/data-lake-store-hdinsight-hadoop-use-portal/hbase-data-lake-store.png "Erstellen einer Azure-Ressourcengruppe")

### Überlegungen zur Verwendung des Data Lake-Speichers als Standardspeicher für HBase-Cluster

* Sie können das gleiche Data Lake-Speicherkonto für mehrere HBase-Cluster verwenden. Allerdings muss der **HBase-Stammordner**, den Sie für den Cluster bereitstellen (Schritt 4 im Screenshot oben), eindeutig sein. Sie **dürfen nicht** für zwei verschiedene HBase-Cluster den gleichen Stammordner verwenden.
* Obwohl Sie das Data Lake-Speicherkonto als Standardspeicher verwenden, werden die Protokolldateien des HBase-Clusters weiterhin in den mit dem Cluster verbundenen Azure Storage-Blobs (WASB) gespeichert. Dies wird im Screenshot oben im blauen Feld hervorgehoben.



## Siehe auch

* [PowerShell: Erstellen eines HDInsight-Clusters für die Verwendung des Data Lake-Speichers](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

<!---HONumber=AcomDC_0914_2016-->