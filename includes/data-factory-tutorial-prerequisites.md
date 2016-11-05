In diesem Tutorial erstellen Sie Ihre erste Azure Data Factory mit einer Datenpipeline zur Datenverarbeitung mittels Ausführung eines Hive-Skripts in einem Azure HDInsight (Hadoop)-Cluster.

Dieser Artikel bietet keine grundlegende Übersicht über den Azure Data Factory-Dienst. Eine Übersicht über das Konzept des Diensts finden Sie unter [Einführung in Azure Data Factory](../articles/data-factory/data-factory-introduction.md).

## Inhalt dieses Tutorials
**Azure Data Factory** ermöglicht Ihnen das Zusammensetzen von Aufgaben zur **Datenverschiebung** und **Datenverarbeitung** zu einem datengesteuerten Workflow (der auch als Datenpipeline bezeichnet wird). Erfahren Sie, wie Sie Ihre erste Data Pipeline mit einer Datenverarbeitungsaufgabe (oder Datentransformationsaufgabe) erstellen. Bei dieser Aufgabe werden mit einem Azure HDInsight Cluster Webprotokolle transformiert und analysiert.

In diesem Tutorial führen Sie die folgenden Schritte aus:

1. Erstellen Sie die **Data Factory**. Eine Data Factory kann eine oder mehrere Datenpipelines enthalten, die Daten verschieben und verarbeiten.
2. Erstellen Sie die **verknüpften Dienste**. Sie erstellen einen verknüpften Dienst, um einen Datenspeicher oder einen Computedienst mit der Data Factory zu verknüpfen. Ein Datenspeicher wie Azure Storage hält Ein-/Ausgabedaten von Aktivitäten in der Pipeline. Ein Computedienst wie Azure HDInsight verarbeitet/transformiert Daten.
3. Erstellen von **Datasets** für Eingabe und Ausgabe Ein Eingabedataset stellt die Eingabe für eine Aktivität in der Pipeline dar, und ein Ausgabedataset stellt die Ausgabe für die Aktivität dar.
4. Erstellen Sie die **Pipeline**. Eine Pipeline kann eine oder mehrere Aktivitäten aufweisen (Beispiele: Kopieraktivität, HDInsight Hive-Aktivität). Dieses Beispiel verwendet die HDInsight-Hive-Aktivität, die ein Hive-Skript ausführt. Das Skript erstellt zuerst eine externe Tabelle, die auf die Webprotokollrohdaten im Azure-Blobspeicher verweist, und partitioniert die Rohdaten dann nach Jahr und Monat.

![Diagrammansicht im Data Factory-Tutorial](./media/data-factory-tutorial-prerequisites/data-factory-tutorial-diagram-view.png)

In diesem Tutorial enthält „adfgetstarted“ (Container) = > „inputdata“ (Ordner) eine Datei mit dem Namen „input.log“. Diese Protokolldatei enthält Einträge von drei Monaten: Januar, Februar und März 2014. Hier sind die Beispielzeilen für jeden Monat in der Eingabedatei.

    2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Wenn die Datei von der Pipeline mit HDInsight-Hive-Aktivität verarbeitet wird, führt die Aktivität ein Hive-Skript auf dem HDInsight-Cluster aus, das Eingabedaten nach Jahr und Monat partitioniert. Das Skript erstellt drei Ausgabeordner, die eine Datei mit Einträgen aus jedem Monat enthalten.

    adfgetstarted/partitioneddata/year=2014/month=1/000000_0
    adfgetstarted/partitioneddata/year=2014/month=2/000000_0
    adfgetstarted/partitioneddata/year=2014/month=3/000000_0

Von den obigen Beispielzeilen wird die erste (mit „2014-01-01“) in die Datei „000000\_0“ im Ordner „month=1“ geschrieben. Auf ähnliche Weise wird die zweite Zeile in die Datei im Ordner „month=2“ geschrieben und die dritte Zeile in die Datei im Ordner „month=3“.

## Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, müssen folgende Voraussetzungen erfüllt sein:

1. **Azure-Abonnement** – Wenn Sie über kein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein kostenloses Testversionskonto einrichten. Im Artikel [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) erfahren Sie, wie Sie ein kostenloses Testkonto erhalten.
2. **Azure Storage** – Sie benötigen ein Azure-Speicherkonto zum Speichern der Daten in diesem Tutorial. Falls Sie noch kein Azure-Speicherkonto haben, lesen Sie den Artikel [Erstellen eines Speicherkontos](../articles/storage/storage-create-storage-account.md#create-a-storage-account). Nachdem Sie das Speicherkonto erstellt haben, müssen Sie den Kontoschlüssel für den Zugriff auf den Speicher abrufen. Siehe [Anzeigen, Kopieren und erneutes Generieren von Speicherzugriffsschlüsseln](../articles/storage/storage-create-storage-account.md#view-and-copy-storage-access-keys).

## Hochladen von Dateien in Azure Storage für das Tutorial
Bevor Sie mit dem Tutorial beginnen, müssen Sie den Azure-Speicher mit Dateien vorbereiten, die für das Tutorial benötigt werden.

In diesem Abschnitt führen Sie die folgenden Aufgaben aus:

1. Hochladen der Hive-Abfragedatei (HQL) in den Ordner **script** des Containers **adfgetstarted**.
2. Hochladen der Eingabedatei in den Ordner **inputdata** des Containers **adfgetstarted**.

### Erstellen der HQL-Skriptdatei
1. Starten Sie **Editor**, und fügen Sie das folgende HQL-Skript ein. Mit diesem Hive-Skript werden zwei Tabellen erstellt: **WebLogsRaw** und **WebLogsPartitioned**. Klicken Sie im Menü auf **Datei**, und wählen Sie **Speichern unter** aus. Wechseln Sie zum Ordner **C:\\adfgetstarted** auf Ihrer Festplatte. Wählen Sie **Alle Dateien (*.*)** im Feld **Dateityp** aus. Geben Sie **partitionweblogs.hql** als **Dateinamen** ein. Überprüfen Sie, ob im Feld **Codierung** unten im Dialogfeld **ANSI** festgelegt ist. Falls nicht, legen Sie es auf **ANSI** fest.
   
        set hive.exec.dynamic.partition.mode=nonstrict;
   
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
   
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
   
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
   
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Zur Laufzeit übergibt die Hive-Aktivität in der Data Factory-Pipeline Werte für die Parameter „inputtable“ und „partitionedtable“ (wie im folgenden Codeausschnitt dargestellt). „storageaccountname“ ist der Name Ihres Azure Storage-Kontos.

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

### Erstellen einer Beispieleingabedatei
Erstellen Sie mithilfe des Editors eine Datei namens **input.log** in **c:\\adfgetstarted** mit folgendem Inhalt:

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2014-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

### Hochladen von Eingabe- und HQL-Datei in Ihren Azure-Blobspeicher
Dieser Abschnitt enthält Anweisungen zur Verwendung des Tools **AzCopy** zum Kopieren von Dateien in Azure Blob Storage. Für diese Aufgabe können Sie ein beliebiges Tool verwenden (etwa [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/) oder [CloudXPlorer von ClumsyLeaf Software](http://clumsyleaf.com/products/cloudxplorer)).

1. So bereiten Sie den Azure-Speicher für das Tutorial vor:
   
   1. Laden Sie die [neueste Version von **AzCopy**](http://aka.ms/downloadazcopy) oder die [neueste Vorschauversion](http://aka.ms/downloadazcopypr) herunter. Im Artikel [Verwenden von AzCopy ](../articles/storage/storage-use-azcopy.md) finden Sie eine Anleitung zur Verwendung des Hilfsprogramms.
   2. Fügen Sie den AzCopy-Speicherort dem Systempfad hinzu, indem Sie den folgenden Befehl an der Eingabeaufforderung ausführen.
      
           set path=%path%;C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
   3. Navigieren Sie zum Ordner c:\\adfgetstarted, und führen Sie den folgenden Befehl aus. Mit diesem Befehl laden Sie die Datei **input.log** in das Speicherkonto hoch (**Adfgetstarted** Container und Ordner **Inputdata**). Ersetzen Sie **StorageAccountName** durch den Namen Ihres Speicherkontos und **Storage Key** durch den Speicherkontoschlüssel.
      
           AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storagekey>  /Pattern:input.log
      
      > [!NOTE]
      > Dieser Befehl erstellt einen Container mit dem Namen **adfgetstarted** in Ihrem Azure Blob Storage und kopiert die Datei **input.log** von Ihrem lokalen Laufwerk in den Ordner **inputdata** des Containers.
      > 
      > 
   4. Nachdem die Datei erfolgreich hochgeladen wurde, sehen Sie eine Ausgabe ähnlich der folgenden von AzCopy.
      
           Finished 1 of total 1 file(s).
           [2015/12/16 23:07:33] Transfer summary:
           -----------------
           Total files transferred: 1
           Transfer successfully:   1
           Transfer skipped:        0
           Transfer failed:         0
           Elapsed time:            00.00:00:01
   5. Führen Sie den folgenden Befehl aus, um die Datei **partitionweblogs.hql** in den Ordner **script** des Containers **adfgetstarted** hochzuladen. Hier ist der Befehl:
      
           AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storagekey>  /Pattern:partitionweblogs.hql

<!---HONumber=AcomDC_0914_2016-->