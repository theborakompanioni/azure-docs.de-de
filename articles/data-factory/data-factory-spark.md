---
title: Aufrufen von Spark-Programmen aus Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Spark-Programme mithilfe der MapReduce-Aktivität aus einer Azure Data Factory aufrufen."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 57894bbdd9208f8c32eb65e29f04e2ae723780ca
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Aufrufen von Spark-Programmen aus Azure Data Factory-Pipelines

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-Aktivität](data-factory-hive-activity.md)
> * [Pig-Aktivität](data-factory-pig-activity.md)
> * [MapReduce-Aktivität](data-factory-map-reduce.md)
> * [Hadoop-Streamingaktivität](data-factory-hadoop-streaming-activity.md)
> * [Spark-Aktivität](data-factory-spark.md)
> * [Machine Learning-Batchausführungsaktivität](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-Ressourcenaktualisierungsaktivität](data-factory-azure-ml-update-resource-activity.md)
> * [Aktivität „Gespeicherte Prozedur“](data-factory-stored-proc-activity.md)
> * [U-SQL-Aktivität für Data Lake Analytics](data-factory-usql-activity.md)
> * [Benutzerdefinierte .NET-Aktivität](data-factory-use-custom-activities.md)

## <a name="introduction"></a>Einführung
Die Spark-Aktivität ist eine der [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md), die von Azure Data Factory unterstützt werden. Diese Aktivität führt das angegebene Spark-Programm auf dem Apache Spark-Cluster in Azure HDInsight aus.    

> [!IMPORTANT]
> - Die Spark-Aktivität unterstützt keine HDInsight Spark-Cluster, die Azure Data Lake Store als primären Speicher verwenden.
> - Die Spark-Aktivität unterstützt nur vorhandene (Ihre eigenen) HDInsight Spark-Cluster. Bedarfsgesteuerte verknüpfte HDInsight-Dienste werden nicht unterstützt.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>Exemplarische Vorgehensweise: Erstellen einer Pipeline mit der Spark-Aktivität
Dies sind die typischen Schritte zum Erstellen einer Data Factory-Pipeline mit einer Spark-Aktivität.  

1. Erstellen einer Data Factory.
2. Erstellen Sie einen verknüpften Azure Storage-Dienst, um Ihren Azure-Speicher, der Ihrem HDInsight Spark-Cluster zugeordnet ist, mit der Data Factory zu verknüpfen.     
2. Erstellen Sie einen verknüpften Azure HDInsight-Dienst, um Ihren Apache Spark-Cluster in Azure HDInsight mit der Data Factory zu verknüpfen.
3. Erstellen Sie ein Dataset, das auf den mit Azure Storage verknüpften Dienst verweist. Derzeit müssen Sie ein Ausgabedataset für eine Aktivität angeben, auch wenn keine Ausgabe erzeugt wird.  
4. Erstellen Sie eine Pipeline mit Spark-Aktivität, die auf den in Schritt 2 erstellten verknüpften HDInsight-Dienst verweist. Die Aktivität wird mit dem Dataset, das Sie im vorherigen Schritt erstellt haben, als Ausgabedataset konfiguriert. Das Ausgabedataset stellt den Treiber des Zeitplans dar (stündlich, täglich usw.). Daher müssen Sie das Augabedataset auch dann eingeben, wenn die Aktivität keine Ausgabe erzeugt.

### <a name="prerequisites"></a>Voraussetzungen
1. Erstellen Sie ein **allgemeines Azure-Speicherkonto** anhand der Anweisungen in der exemplarischen Vorgehensweise: [Erstellen eines Speicherkontos](../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Erstellen Sie einen **Apache Spark-Cluster in Azure HDInsight** anhand der Anweisungen im Tutorial: [Erstellen eines Apache Spark-Clusters in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Ordnen Sie das in Schritt 1 erstellte Azure-Speicherkonto diesem Cluster zu.  
3. Laden Sie die Python-Skriptdatei **test.py** von [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py) herunter, und sehen Sie sie durch.  
3.  Laden Sie **test.py** in den Ordner **pyFiles** im Container **adfspark** in Azure Blob Storage hoch. Erstellen Sie den Container und den Ordner, wenn sie nicht vorhanden sind.

### <a name="create-data-factory"></a>Erstellen einer Data Factory
In diesem Schritt erstellen wir zunächst die Data Factory.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie im Menü auf der linken Seite auf **NEU**, und klicken Sie auf **Data + Analytics** und dann auf **Data Factory**.
3. Geben Sie auf dem Blatt **Neue Data Factory** als Namen **SparkDF** ein.

   > [!IMPORTANT]
   > Der Name der Azure Data Factory muss **global eindeutig**sein. Wenn Sie die Fehlermeldung **Die Data Factory mit dem Namen „SparkDF“ ist nicht verfügbar** erhalten: Ändern Sie den Namen der Data Factory (z.B. in „IhrNameSparkDFDatum“), und versuchen Sie, die Erstellung erneut durchzuführen. Im Thema [Data Factory – Benennungsregeln](data-factory-naming-rules.md) finden Sie Benennungsregeln für Data Factory-Artefakte.   
4. Wählen Sie das **Azure-Abonnement** , in dem die Data Factory erstellt werden soll.
5. Wählen Sie eine vorhandene **Ressourcengruppe** aus, oder erstellen Sie eine Azure-Ressourcengruppe.
6. Wählen Sie die Option **An Dashboard anheften** aus.  
6. Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.

   > [!IMPORTANT]
   > Zum Erstellen von Data Factory-Instanzen müssen Sie Mitglied der Rolle [Data Factory-Mitwirkender](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) auf Abonnement- bzw. Ressourcengruppenebene sein.
7. Sie sehen, dass die Data Factory im **Dashboard** des Azure-Portals wie folgt erstellt wird:   
8. Nachdem die Data Factory erfolgreich erstellt wurde, sehen Sie die Data Factory-Seite mit dem Inhalt der Data Factory. Wenn die Data Factory-Seite nicht angezeigt wird, klicken Sie auf dem Dashboard auf die Kachel für Ihre Data Factory.

    ![Blatt "Data Factory"](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Erstellen von verknüpften Diensten
In diesem Schritt erstellen Sie zwei verknüpfte Dienste: einen zum Verknüpfen Ihres Spark-Clusters mit Ihrer Data Factory und den anderen zum Verknüpfen Ihres Azure-Speichers mit Ihrer Data Factory.  

#### <a name="create-azure-storage-linked-service"></a>Erstellen des mit Azure Storage verknüpften Diensts
In diesem Schritt verknüpfen Sie Ihr Azure Storage-Konto mit Ihrer Data Factory. Ein Dataset, das Sie in einem späteren Schritt in dieser exemplarischen Vorgehensweise erstellen, verweist auf diesen verknüpften Dienst. Der verknüpfte HDInsight-Dienst, den Sie im nächsten Schritt definieren, verweist ebenfalls auf diesen verknüpften Dienst.  

1. Klicken Sie auf dem Blatt **Data Factory** für Ihre Data Factory auf **Erstellen und bereitstellen**. Der Data Factory-Editor sollte angezeigt werden.
2. Klicken Sie auf **Neuer Datenspeicher**, und wählen Sie **Azure Storage**.

   ![Neuer Datenspeicher – Azure Storage – Menü](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Das **JSON-Skript** zum Erstellen eines mit einem Azure-Speicher verknüpften Diensts sollte im Editor angezeigt werden.

   ![Mit Azure Storage verknüpfter Dienst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Ersetzen Sie **Kontoname** und **Kontoschlüssel** durch den Namen und den Zugriffsschlüssel Ihres Azure-Speicherkontos. Informationen zum Abrufen Ihres Speicherzugriffsschlüssels finden Sie unter [Verwalten von Speicherkonten](../storage/common/storage-create-storage-account.md#manage-your-storage-account) in den Informationen zum Anzeigen, Kopieren und erneuten Generieren von Speicherzugriffsschlüsseln.
5. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um den verknüpften Dienst bereitzustellen. Nach erfolgreicher Bereitstellung des verknüpften Diensts sollte das Fenster **Draft-1** nicht mehr angezeigt werden, und Sie sehen **AzureStorageLinkedService** in der Strukturansicht links.

#### <a name="create-hdinsight-linked-service"></a>Erstellen eines verknüpften HDInsight-Diensts
In diesem Schritt erstellen Sie einen verknüpften Azure HDInsight-Dienst, um Ihren HDInsight Spark-Cluster mit der Data Factory zu verknüpfen. Der HDInsight-Cluster wird verwendet, um das Spark-Programm auszuführen, das in der Spark-Aktivität der Pipeline in diesem Beispiel angegeben ist.  

1. Klicken Sie in der Symbolleiste auf **... Mehr**, klicken Sie auf **Neu berechnen**, und klicken Sie dann auf **HDInsight-Cluster**.

    ![Erstellen eines verknüpften HDInsight-Diensts](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster **Draft-1** ein. Gehen Sie im JSON-Editor folgendermaßen vor:
    1. Geben Sie den **URI** für den HDInsight Spark-Cluster an. Beispiel: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Geben Sie den Namen des **Benutzers** an, der auf den Spark-Cluster zugreifen darf.
    3. Geben Sie das **Kennwort** für den Benutzer an.
    4. Geben Sie den mit **Azure Storage verknüpften Dienst** an, der dem HDInsight Spark-Cluster zugeordnet ist. In diesem Beispiel ist dies: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Die Spark-Aktivität unterstützt keine HDInsight Spark-Cluster, die Azure Data Lake Store als primären Speicher verwenden.
    > - Die Spark-Aktivität unterstützt nur vorhandene (Ihre eigenen) HDInsight Spark-Cluster. Bedarfsgesteuerte verknüpfte HDInsight-Dienste werden nicht unterstützt.

    Unter [Verknüpfter HDInsight-Dienst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) finden Sie weitere Informationen zum verknüpften HDInsight-Dienst.
3.  Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um den verknüpften Dienst bereitzustellen.  

### <a name="create-output-dataset"></a>Erstellen des Ausgabedatasets
Das Ausgabedataset stellt den Treiber des Zeitplans dar (stündlich, täglich usw.). Daher müssen Sie das Augabedataset für die Spark-Aktivität in der Pipeline auch dann angeben, wenn die Aktivität eigentlich keine Ausgabe erzeugt. Das Angeben eines Eingabedatasets für die Aktivität ist optional.

1. Klicken Sie im **Data Factory-Editor** in der Befehlszeile auf **... Weitere** und dann auf **Neues Dataset**, und wählen Sie **Azure-Blobspeicher**.  
2. Kopieren Sie den folgenden Codeausschnitt, und fügen Sie ihn in das Fenster „Draft-1“ ein. Der JSON-Codeausschnitt definiert ein Dataset mit dem Namen **OutputDataset**. Darüber hinaus geben Sie an, dass die Ergebnisse im Blobcontainer **adfspark** und im Ordner **pyFiles/output** gespeichert werden. Wie bereits erwähnt, ist dieses Dataset ein Dummydataset. Das Spark-Programm in diesem Beispiel erzeugt keine Ausgabe. Der Abschnitt **availability** gibt an, dass das Ausgabedataset täglich erzeugt wird.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um das Dataset bereitzustellen.


### <a name="create-pipeline"></a>Erstellen der Pipeline
In diesem Schritt erstellen Sie eine Pipeline mit einer **HDInsightSpark**-Aktivität. Derzeit steuert das Ausgabedataset den Zeitplan, sodass Sie auch dann ein Ausgabedataset erstellen müssen, wenn die Aktivität keine Ausgabe erzeugt. Wenn die Aktivität keine Eingabe akzeptiert, können Sie das Erstellen des Eingabedatasets überspringen. Aus diesem Grund wird in diesem Beispiel kein Eingabedataset angegeben.

1. Klicken Sie im **Data Factory-Editor** auf der Befehlsleiste auf **... Mehr** und dann auf **Neue Pipeline**.
2. Ersetzen Sie das Skript im Fenster „Entwurf-1“ durch das folgende Skript:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Beachten Sie folgende Punkte:
    - Die **type**-Eigenschaft ist auf **HDInsightSpark** festgelegt.
    - **rootPath** ist auf **adfspark\\pyFiles** festgelegt, wobei „adfspark“ den Azure-Blobcontainer und „pyFiles“ einen Dateiordner in diesem Container darstellt. In diesem Beispiel ist Azure Blob Storage der dem Spark-Cluster zugeordnete Speicher. Sie können die Datei auf einen anderen Azure-Speicher hochladen. Erstellen Sie in diesem Fall einen verknüpften Azure Storage-Dienst, der das Speicherkonto mit der Data Factory verknüpft. Geben Sie dann den Namen des verknüpften Diensts als Wert für die Eigenschaft **sparkJobLinkedService** an. Details zu dieser und anderen von der Spark-Aktivität unterstützten Eigenschaften finden Sie unter [Eigenschaften von Spark-Aktivitäten](#spark-activity-properties).  
    - **entryFilePath** ist auf **test.py** festgelegt. Dies ist die Python-Datei.
    - Die **getDebugInfo**-Eigenschaft ist auf **Always** festgelegt, das heißt, Protokolldateien werden in jedem Fall (Erfolg oder Fehler) erstellt.

        > [!IMPORTANT]
        > Es wird empfohlen, dass Sie diese Eigenschaft in einer Produktionsumgebung nicht auf `Always` festlegen, es sei denn, Sie möchten ein Problem behandeln.
    - Der Abschnitt **outputs** weist ein Ausgabedataset auf. Sie müssen in jedem Fall ein Ausgabedataset angeben, selbst wenn das Spark-Programm keine Ausgabe erzeugt. Das Ausgabedataset bestimmt den Zeitplan für die Pipeline (stündlich, täglich usw.).  

        Details zu den von der Spark-Aktivität unterstützten Eigenschaften finden Sie im Abschnitt [Eigenschaften von Spark-Aktivitäten](#spark-activity-properties).
3. Klicken Sie in der Befehlsleiste auf **Bereitstellen**, um die Pipeline bereitzustellen.

### <a name="monitor-pipeline"></a>Überwachen der Pipeline
1. Klicken Sie auf **X**, um die Blätter des Data Factory-Editors zu schließen und zur Data Factory-Startseite zurückzukehren. Klicken Sie auf **Überwachen und Verwalten**, um die Überwachungsanwendung auf einer anderen Registerkarte zu starten.

    ![Kachel „Überwachen und Verwalten“](media/data-factory-spark/monitor-and-manage-tile.png)
2. Ändern Sie den Filter **Startzeit** oben in **01.02.2017**, und klicken Sie auf **Übernehmen**.
3. Nur ein Aktivitätsfenster sollte angezeigt werden, da nur ein Tag zwischen der Startzeit (01.02.2017) und der Endzeit (02.02.2017) der Pipeline liegt. Vergewissern Sie sich, dass der Datenslice den Zustand **Bereit** hat.

    ![Überwachen der Pipeline](media/data-factory-spark/monitor-and-manage-app.png)    
4. Wählen Sie das **Aktivitätsfenster** aus, um Details zur Aktivitätsausführung anzuzeigen. Wenn ein Fehler aufgetreten ist, sehen Sie die Details dazu im rechten Bereich.

### <a name="verify-the-results"></a>Überprüfen der Ergebnisse

1. Starten Sie **Jupyter Notebook** für Ihren HDInsight Spark-Cluster, indem Sie zu „https://CLUSTERNAME.azurehdinsight.net/jupyter“ navigieren. Sie können auch das Clusterdashboard für Ihren HDInsight Spark-Cluster und dann **Jupyter Notebook** starten.
2. Klicken Sie auf **Neu** -> **PySpark**, um ein neues Notebook zu starten.

    ![Neues Jupyter Notebook](media/data-factory-spark/jupyter-new-book.png)
3. Führen Sie den folgenden Befehl aus, indem Sie den Text kopieren und am Ende der zweiten Anweisung einfügen und **UMSCHALT+EINGABE** drücken.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Überprüfen Sie, ob die Daten aus der hvac-Tabelle angezeigt werden:  

    ![Jupyter-Abfrageergebnisse](media/data-factory-spark/jupyter-notebook-results.png)

Ausführliche Anweisungen finden Sie im Abschnitt [Ausführen einer Spark SQL-Abfrage](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql). 

### <a name="troubleshooting"></a>Problembehandlung
Da Sie **getDebugInfo** auf **Always** festgelegt haben, finden Sie einen Unterordner **log** im Ordner **pyFiles** im Azure-Blobcontainer. Die Protokolldatei im Ordner „log“ enthält weitere Details. Diese Protokolldatei ist besonders nützlich, wenn ein Fehler aufgetreten ist. In einer Produktionsumgebung sollten Sie sie auf **Failure** festlegen.

Führen Sie zur zusätzlichen Problembehandlung die folgenden Schritte aus:


1. Navigieren Sie zu `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![YARN-UI-Anwendung](media/data-factory-spark/yarnui-application.png)  
2. Klicken Sie für einen der Ausführungsversuche auf **Protokolle**.

    ![Anwendungsseite](media/data-factory-spark/yarn-applications.png)
3. Es sollten zusätzliche Fehlerinformationen auf der Protokollseite angezeigt werden.

    ![Protokollfehler](media/data-factory-spark/yarnui-application-error.png)

Die folgenden Abschnitte enthalten Informationen zu Data Factory-Entitäten zur Verwendung von Apache Spark-Clustern und Spark-Aktivitäten in Ihrer Data Factory.

## <a name="spark-activity-properties"></a>Eigenschaften von Spark-Aktivitäten
Hier ist eine einfache JSON-Definition einer Pipeline mit Spark-Aktivität:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Die folgende Tabelle beschreibt die JSON-Eigenschaften, die in der JSON-Definition verwendet werden:

| Eigenschaft | Beschreibung | Erforderlich |
| -------- | ----------- | -------- |
| Name | Der Name der Aktivität in der Pipeline. | Ja |
| Beschreibung | Ein Text, der beschreibt, was mit der Aktivität ausgeführt wird. | Nein |
| Typ | Diese Eigenschaft muss auf „HDInsightSpark“ festgelegt werden. | Ja |
| linkedServiceName | Name des mit HDInsight verknüpften Diensts, in dem das Spark-Programm ausgeführt wird. | Ja |
| rootPath | Der Azure-Blobcontainer und -ordner mit der Spark-Datei. Beim Dateinamen muss die Groß-/Kleinschreibung beachtet werden. | Ja |
| entryFilePath | Der relative Pfad zum Stammordner des Spark-Codes bzw. -Pakets. | Ja |
| className | Die Java-/Spark-Hauptklasse der Anwendung. | Nein |
| arguments | Eine Liste der Befehlszeilenargumente für das Spark-Programm. | Nein |
| proxyUser | Das Benutzerkonto, dessen Identität angenommen werden soll, um das Spark-Programm auszuführen. | Nein |
| sparkConfig | Geben Sie Werte für Spark-Konfigurationseigenschaften an, die im Thema [Spark-Konfiguration – Anwendungseigenschaften](https://spark.apache.org/docs/latest/configuration.html#available-properties) aufgeführt werden. | Nein |
| getDebugInfo | Gibt an, ob die Spark-Protokolldateien in den Azure-Speicher kopiert werden, der vom HDInsight-Cluster verwendet (oder) von sparkJobLinkedService angegeben wird. Zulässige Werte: Keine, Immer oder Fehler. Standardwert: Keine | Nein |
| sparkJobLinkedService | Der verknüpfte Azure Storage-Dienst, der die Datei sowie die Abhängigkeiten und Protokolle für den Spark-Auftrag enthält.  Wenn Sie für diese Eigenschaft keinen Wert angeben, wird der Speicher verwendet, der dem HDInsight-Cluster zugeordnet ist. | Nein |

## <a name="folder-structure"></a>Ordnerstruktur
Die Spark-Aktivität unterstützt im Gegensatz zu Pig- und Hive-Aktivitäten keine Inlineskripts. Spark-Aufträge lassen sich zudem besser erweitern als Pig- oder Hive-Aufträge. Bei Spark-Aufträgen können Sie mehrere Abhängigkeiten wie z.B. jar-Pakete (im Java-CLASSPATH platziert), Python-Dateien (im PYTHONPATH platziert) sowie beliebige andere Dateien bereitstellen.

Erstellen Sie folgende Ordnerstruktur in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist. Laden Sie dann abhängige Dateien in die entsprechenden Unterordner in dem Stammordner hoch, der von **entryFilePath** repräsentiert wird. Python-Dateien werden beispielsweise in den Unterordner „pyFiles“ und jar-Dateien in den Unterordner „jars“ des Stammordners hochgeladen. Zur Laufzeit erwartet der Data Factory-Dienst die folgende Ordnerstruktur im Azure-Blobspeicher:     

| path | Beschreibung | Erforderlich | Typ |
| ---- | ----------- | -------- | ---- |
| verfügbar. | Der Stammpfad des Spark-Auftrags im verknüpften Speicherdienst.  | Ja | Ordner |
| &lt;benutzerdefiniert&gt; | Der Pfad, der auf die Eingabedatei des Spark-Auftrags zeigt. | Ja | File |
| ./jars | Alle Dateien in diesem Ordner werden hochgeladen und im Java-CLASSPATH des Clusters platziert. | Nein | Ordner |
| ./pyFiles | Alle Dateien in diesem Ordner werden hochgeladen und im PYTHONPATH des Clusters platziert. | Nein | Ordner |
| ./files | Alle Dateien in diesem Ordner werden hochgeladen und im Executor-Arbeitsverzeichnis platziert. | Nein | Ordner |
| ./archives | Alle Dateien in diesem Ordner sind nicht komprimiert. | Nein | Ordner |
| ./logs | Der Ordner, in dem Protokolle aus dem Spark-Cluster gespeichert werden.| Nein | Ordner |

Hier finden Sie ein Beispiel für einen Speicher mit zwei Spark-Auftragsdateien in dem Azure-Blobspeicher, auf den der verknüpfte HDInsight-Dienst verweist.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```

