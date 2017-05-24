---
title: Verwenden des Apache Spark-Streamings mit Event Hubs in Azure HDInsight | Microsoft-Dokumentation
description: Erstellen eines Apache Spark-Streamingbeispiels zum Senden eines Datenstroms an Azure Event Hub und zum Empfangen dieser Ereignisse in einem HDInsight Spark-Cluster mithilfe einer Scala-Anwendung
keywords: Apache Spark-Streaming, Spark-Streaming, Spark-Beispiel, Apache Spark-Streamingbeispiel, Event Hub-Azure-Beispiel, Spark-Beispiel
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 86ec34dccb4518c31d9762e68f272382ee544713
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark-Streaming: Verarbeiten von Daten aus Azure Event Hubs mit einem Spark-Cluster in HDInsight

In diesem Artikel erstellen Sie ein Apache Spark-Streamingbeispiel, das folgende Schritte umfasst:

1. Sie verwenden eine eigenständige Anwendung, um Nachrichten in einem Azure Event Hub zu erfassen.

2. Sie lesen die Nachrichten aus Event Hub in Echtzeit mithilfe einer Anwendung, die in einem Spark-Cluster in HDInsight ausgeführt wird.

3. Sie leiten die Daten an verschiedene Ausgabeziele, z.B. Azure Storage Blob, Hive-Tabelle oder SQL-Tabelle. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="what-is-apache-spark-streaming"></a>Was ist das Apache Spark-Streaming?

Eine ausführliche Erläuterung des Spark-Streamings finden Sie unter [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview) (Übersicht über das Apache Spark-Streaming). HDInsight stellt einem Spark-Cluster in Azure dieselben Streamingfeatures bereit.  

## <a name="what-does-this-solution-do"></a>Was ist die Aufgabe dieser Lösung?

In diesem Artikel führen Sie zum Erstellen eines Spark-Streamingbeispiels die folgenden Schritte aus:

1. Erstellen eines Azure Event Hubs, der einen Ereignisdatenstrom empfängt.

2. Ausführen einer lokalen eigenständigen Anwendung, die Ereignisse generiert und mittels Push an den Azure Event Hub überträgt. Die Beispielanwendung dafür wird unter [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples)veröffentlicht.

3. Ausführen eines Event Hub-Azure-Beispiels in einem Spark-Cluster, der Streamingereignisse aus Azure Event Hub liest und sie an verschiedenen Speicherorten schreibt (z.B. Azure-Blob, Hive-Tabelle und SQL-Datenbanktabelle)

## <a name="create-an-azure-event-hub"></a>Erstellen eines Azure Event Hubs

1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com) an, und klicken Sie links oben auf dem Bildschirm auf **Neu**.

2. Klicken Sie auf **Internet der Dinge**, und klicken Sie dann auf **Event Hubs**.
   
    ![Erstellen einer Event Hub-Instanz für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Erstellen einer Event Hub-Instanz für das Spark-Streamingbeispiel")

3. Geben Sie auf dem Blatt **Namespace erstellen** einen Namen für den Namespace ein. Wählen Sie den Tarif (Basic oder Standard). Wählen Sie außerdem ein Azure-Abonnement, eine Ressourcengruppe und einen Standort, an dem die Ressource erstellt werden soll. Klicken Sie auf **Erstellen** , um den Namespace zu erstellen.
   
    ![Angeben eines Event Hub-Namens für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Angeben eines Event Hub-Namens für das Spark-Streamingbeispiel")

    > [!NOTE]
       > Wählen Sie den gleichen **Speicherort** wie für Ihren Apache Spark-Cluster unter HDInsightStorm aus, um die Latenz und die Kosten zu reduzieren.
       > 
       > 

4. Klicken Sie in der Liste der Event Hubs-Namespaces auf den neu erstellten Namespace.      
   
    
5. Klicken Sie auf dem Blatt „Namespace“ auf **Event Hubs** und dann auf **+ Event Hub**, um einen neuen Event Hub zu erstellen.
   
    ![Erstellen einer Event Hub-Instanz für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Erstellen einer Event Hub-Instanz für das Spark-Streamingbeispiel")

6. Geben Sie einen Namen für Ihren Event Hub ein. Legen Sie die Anzahl der Partitionen auf 10 und Nachrichtenaufbewahrung auf 1 fest. Wir archivieren die Nachrichten in dieser Lösung nicht, sodass Sie für den Rest die Standardeinstellungen übernehmen können. Klicken Sie dann auf **Erstellen**.
   
    ![Angeben von Event Hub-Informationen für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Angeben von Event Hub-Informationen für das Spark-Streamingbeispiel")

7. Der neu erstellte Event Hub wird auf dem Blatt „Event Hub“ aufgeführt.
    
     ![Anzeigen der Event Hub-Instanz für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "Anzeigen der Event Hub-Instanz für das Spark-Streamingbeispiel")

8. Klicken Sie auf dem Blatt mit Namespaces (nicht auf dem Blatt für den bestimmten Event Hub) auf **Shared access policies** (Richtlinien für gemeinsamen Zugriff), und klicken Sie dann auf **RootManageSharedAccessKey**.
    
     ![Festlegen von Event Hub-Richtlinien für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Festlegen von Event Hub-Richtlinien für das Spark-Streamingbeispiel")

9. Klicken Sie auf die Schaltfläche „Kopieren“, um den Primärschlüssel **RootManageSharedAccessKey** und die Verbindungszeichenfolge in die Zwischenablage zu kopieren. Speichern Sie diese, um sie später in diesem Tutorial zu verwenden.
    
     ![Anzeigen der Event Hub-Richtlinienschlüssel für das Spark-Streamingbeispiel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "Anzeigen der Event Hub-Richtlinienschlüssel für das Spark-Streamingbeispiel")

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a>Senden von Nachrichten an Azure Event Hub mithilfe einer Scala-Beispielanwendung

In diesem Abschnitt verwenden Sie eine eigenständige lokale Scala-Anwendung, die einen Ereignisdatenstrom generiert und an die Azure Event Hub-Instanz sendet, die Sie im vorherigen Schritt erstellt haben. Diese Anwendung steht unter [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer) auf GitHub zur Verfügung. Bei den folgenden Schritten wird davon ausgegangen, dass Sie dieses GitHub-Repository bereits verzweigt haben.

1. Stellen Sie sicher, dass die folgende Software auf dem Computer installiert ist, auf dem Sie diese Anwendung ausführen.

    * Oracle Java Development Kit. Das Installationspaket finden Sie [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Eine Java-IDE. In diesem Artikel wird IntelliJ IDEA 15.0.1 verwendet. Das Installationspaket finden Sie [hier](https://www.jetbrains.com/idea/download/).

2. Öffnen Sie die Anwendung **EventhubsSampleEventProducer**in IntelliJ IDEA.

3. Erstellen Sie das Projekt. Klicken Sie im Menü **Build** auf **Make Project**. Die ausgegebene JAR-Datei wird je nach IntelliJ IDEA-Konfiguration erstellt. Meist erfolgt die Speicherung unter **\classes\artifacts**.

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a>Erstellen einer Anwendung zum Empfangen von Nachrichten von Event Hub in einem Spark-Cluster 

Eine Beispielanwendung für das Scala-Streaming, die Ereignisse empfängt und an verschiedene Ziele weiterleitet, finden Sie unter [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Befolgen Sie die unten stehenden Schritte, um die Anwendung für Ihre Event Hub-Konfiguration zu aktualisieren und die JAR-Ausgabedatei zu erstellen.

1. Starten Sie IntelliJ IDEA, und wählen Sie auf dem Startbildschirm **Check out from Version Control**. Klicken Sie dann auf **Git**.
   
    ![Apache Spark-Streamingbeispiel: Abrufen der Quellen von Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark-Streamingbeispiel: Abrufen der Quellen von Git")

2. Geben Sie im Dialogfeld **Clone Repository** die URL des zu klonenden Git-Repositorys an, geben Sie das Verzeichnis an, in dem der Klon gespeichert werden soll, und klicken Sie dann auf **Clone**.
   
    ![Apache Spark-Streamingbeispiel: Klonen von Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark-Streamingbeispiel: Klonen von Git")
3. Befolgen Sie die Anweisungen, bis das Projekt vollständig geklont ist. Drücken Sie **ALT + 1**, um die **Projektansicht** zu öffnen. Die Ansicht sollte wie folgt aussehen:
   
    ![Apache Spark-Streamingbeispiel: Projektansicht](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark-Streamingbeispiel: Projektansicht")
4. Stellen Sie sicher, dass der Anwendungscode mit Java8 kompiliert wird. Klicken Sie hierzu auf **File** und **Project Structure**, und vergewissern Sie sich, dass auf der Registerkarte **Project** die Projektsprachebene auf **8 - Lambdas, type annotations, etc.** festgelegt ist.
   
    ![Apache Spark-Streamingbeispiel: Festlegen des Compilers](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark-Streamingbeispiel: Festlegen des Compilers")
5. Öffnen Sie die Datei **pom.xml** , und stellen Sie sicher, dass die Spark-Version korrekt ist. Suchen Sie unter dem `<properties>`-Knoten nach dem folgenden Codeausschnitt, und überprüfen Sie die Spark-Version.
   
        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. Die Anwendung erfordert eine Abhängigkeit in Form einer JAR-Datei mit der Bezeichnung **JDBC-JAR-Treiberdatei**. Diese ist erforderlich, um die Nachrichten von Event Hub in eine Azure SQL-Datenbank zu schreiben. Sie können diese JAR-Datei (Version 4.1 oder höher) [hier](https://msdn.microsoft.com/sqlserver/aa937724.aspx) herunterladen. Fügen Sie der Projektbibliothek einen Verweis auf diese JAR-Datei hinzu. Führen Sie die folgenden Schritte aus:
     
     1. Klicken Sie im IntelliJ IDEA-Fenster, in dem die Anwendung geöffnet ist, auf **File**, klicken Sie auf **Project Structure** und dann auf **Libraries**. 
     2. Klicken Sie auf das Symbol zum Hinzufügen (![Hinzufügen-Symbol](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)) und auf **Java**, und navigieren Sie dann zu dem Speicherort, von dem Sie die JDBC-JAR-Treiberdatei heruntergeladen haben. Befolgen Sie die Anweisungen zum Hinzufügen der JAR-Datei in die Projektbibliothek.
        
         ![Hinzufügen fehlender Abhängigkeiten](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Hinzufügen fehlender JAR-Abhängigkeitsdateien")
     3. Klicken Sie auf **Übernehmen**.

7. Erstellen Sie die JAR-Ausgabedatei. Führen Sie die folgenden Schritte aus:
   
   1. Klicken Sie im Dialogfeld **Project Structure** auf **Artifacts** und anschließend auf das Pluszeichen. Klicken Sie im Popupdialogfeld auf **JAR** und anschließend auf **From modules with dependencies** (Aus Modulen mit Abhängigkeiten).
      
       ![Apache Spark-Streamingbeispiel: Erstellen der JAR-Datei](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark-Streamingbeispiel: Erstellen der JAR-Datei")
   2. Klicken Sie im Dialogfeld **Create JAR from Modules** auf die Auslassungspunkte (![Ellipse](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) für die **Hauptklasse**.
   3. Wählen Sie im Dialogfeld **Select Main Class** eine der verfügbaren Klassen aus, und klicken Sie dann auf **OK**.
      
       ![Apache Spark-Streamingbeispiel: Auswählen der Klasse für JAR-Datei](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark-Streamingbeispiel: Auswählen der Klasse für JAR-Datei")
   4. Vergewissern Sie sich im Dialogfeld **Create JAR from Modules**, dass die Option zum **Extrahieren in die JAR-Zieldatei** aktiviert ist, und klicken Sie anschließend auf **OK**. Dadurch wird eine einzelne JAR-Datei mit allen Abhängigkeiten erstellt.
      
       ![Apache Spark-Streamingbeispiel: Erstellen der JAR-Datei aus Modulen](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark-Streamingbeispiel: Erstellen der JAR-Datei aus Modulen")
   5. Die Registerkarte **Output Layout** führt alle JAR-Dateien des Maven-Projekts auf. Sie können die Dateien auswählen und löschen, zu denen die Scala-Anwendung keine direkte Abhängigkeit hat. Bei der hier erstellten Anwendung können Sie bis auf die letzte (**microsoft-spark-streaming-examples compile output**) alle entfernen. Wählen Sie die zu löschenden JAR-Dateien aus, und klicken Sie anschließend auf das **Löschen**-Symbol (![Löschen-Symbol](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark-Streamingbeispiel: Löschen der extrahierten JAR-Dateien](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark-Streamingbeispiel: Löschen der extrahierten JAR-Dateien")
      
       Vergewissern Sie sich, dass das Kontrollkästchen **Build on make** aktiviert ist, damit die JAR-Datei bei jeder Projekterstellung oder -aktualisierung erstellt wird. Klicken Sie auf **Übernehmen**.
   6. Auf der Registerkarte **Output Layout** am Ende der Spalte **Available Elements** werden die beiden SQL JDBC-JAR-Abhängigkeitsdateien aufgeführt, die Sie zuvor der Projektbibliothek hinzugefügt haben. Sie müssen diese der Registerkarte **Output Layout** hinzufügen. Klicken Sie mit der rechten Maustaste auf die JAR-Datei, und klicken Sie dann auf **Extract Into Output Root**.
      
       ![Apache Spark-Streamingbeispiel: Extrahieren der abhängigen JAR-Datei](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark-Streamingbeispiel: Extrahieren der abhängigen JAR-Datei")  
      
       Die Registerkarte **Output Layout** sollte jetzt wie folgt aussehen.
      
       ![Apache Spark-Streamingbeispiel: abschließende Ausgaberegisterkarte](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark-Streamingbeispiel: abschließende Ausgaberegisterkarte")        
      
       Klicken Sie im Dialogfeld **Project Structure** auf **Apply** und anschließend auf **OK**.    
   7. Klicken Sie auf der Menüleiste auf **Build** und anschließend auf **Make Project**. Die JAR-Datei kann auch durch Klicken auf **Build Artifacts** erstellt werden. Die JAR-Ausgabedatei wird unter **\out\artifacts** erstellt.
      
       ![Apache Spark-Streamingbeispiel: JAR-Ausgabedatei](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark-Streamingbeispiel: JAR-Ausgabedatei")

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a>Remoteausführen der Anwendung in einem Spark-Cluster mithilfe von Livy

In diesem Artikel verwenden Sie Livy, um die Apache Spark-Streaminganwendung remote in einem Spark-Cluster auszuführen. Ausführliche Informationen zur Verwendung von Livy mit HDInsight Spark-Clustern finden Sie unter [Übermitteln von Aufträgen an einen Apache Spark-Cluster in Azure HDInsight per Remoteverbindung](hdinsight-apache-spark-livy-rest-interface.md). Bevor Sie die Spark-Streaminganwendung ausführen können, müssen Sie einige Aktionen durchführen:

1. Starten Sie die lokale eigenständige Anwendung zum Generieren von Ereignissen und zum Senden an Event Hub. Verwenden Sie dazu den folgenden Befehl:
   
        java -cp com-microsoft-azure-eventhubs-client-example.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Kopieren Sie die JAR-Streamingdatei (**microsoft-spark-streaming-examples.jar**) in den mit dem Cluster verbundenen Azure Blob Storage. Dadurch kann Livy auf die JAR-Datei zugreifen. Hierzu können Sie das Befehlszeilenprogramm [**AzCopy**](../storage/storage-use-azcopy.md) verwenden. Die Daten können aber auch mit zahlreichen anderen Clients hochgeladen werden. Weitere Informationen finden Sie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md).
3. Installieren Sie CURL auf dem Computer, auf dem Sie diese Anwendungen ausführen möchten. Wir verwenden CURL, um die Livy-Endpunkte für die Remoteausführung der Aufträge aufzurufen.

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a>Ausführen der Spark-Streaminganwendung für den Empfang der Ereignisse in einem Azure Storage Blob als Text

Öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Verzeichnis, in dem Sie CURL installiert haben, und führen Sie den folgenden Befehl aus (ersetzen Sie Benutzername, Kennwort und Clustername):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Die Parameter in der Datei **inputBlob.txt** sind wie folgt definiert:

    { "file":"wasbs:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Nachfolgend finden Sie eine Erläuterung der Parameter in der Eingabedatei:

* **File** ist der Pfad zur JAR-Anwendungsdatei im Azure-Speicherkonto, das dem Cluster zugeordnet ist.
* **ClassName** ist der Name der Klasse in der JAR-Datei.
* **Args** ist die Liste der Argumente, die von der Klasse benötigt werden.
* **NumExecutors** ist die Anzahl der Kerne, die von Spark zum Ausführen der Streaminganwendung verwendet werden. Dies sollte immer mindestens die doppelte Anzahl an Event Hub-Partitionen sein.
* **executorMemory**, **executorCores** und **driverMemory** sind Parameter, mit denen die erforderlichen Ressourcen der Streaminganwendung zugewiesen werden.

> [!NOTE]
> Sie müssen nicht die Ausgabeordner (EventCheckpoint, EventCount/EventCount10) erstellen, die als Parameter verwendet werden. Die Streaminganwendung erstellt sie automatisch.
> 
> 

Beim Ausführen des Befehls sollte Folgendes angezeigt werden:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

Notieren Sie sich die Batch-ID in der letzten Zeile der Ausgabe (in diesem Beispiel die „1“). Um sicherzustellen, dass die Anwendung erfolgreich ausgeführt wird, überprüfen Sie das mit dem Cluster verbundene Azure-Speicherkonto, in dem der **/EventCount/EventCount10** -Ordner erstellt worden sein soll. Dieser Ordner sollte Blobs enthalten, die die Anzahl der Ereignisse erfassen, die innerhalb des für den Parameter **batch-interval-in-seconds**angegebenen Zeitraums verarbeitet werden.

Die Spark-Streaminganwendung wird so lange ausgeführt, bis Sie sie beenden. Verwenden Sie dazu den folgenden Befehl:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a>Ausführen von Anwendungen für den Empfang der Ereignisse in einem Azure Storage Blob als JSON
Öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Verzeichnis, in dem Sie CURL installiert haben, und führen Sie den folgenden Befehl aus (ersetzen Sie Benutzername, Kennwort und Clustername):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Die Parameter in der Datei **inputJSON.txt** sind wie folgt definiert:

    { "file":"wasbs:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Die Parameter sind ähnlich jenen, die Sie für die Textausgabe im vorherigen Schritt angegeben haben. Auch hier müssen Sie nicht die Ausgabeordner (EventCheckpoint, EventCount/EventCount10) erstellen, die als Parameter verwendet werden. Die Streaminganwendung erstellt sie automatisch.

 Nach dem Ausführen des Befehls können Sie das mit dem Cluster verbundene Azure Storage-Konto überprüfen. Es sollte der erstellte Ordner **/EventStore10** angezeigt werden. Öffnen Sie eine Datei mit dem Präfix **part-**. Die Ereignisse sollten in einem JSON-Format verarbeitet worden sein.

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a>Ausführen von Anwendungen für den Empfang der Ereignisse in einer Hive-Tabelle
Zum Ausführen der Spark-Streaminganwendung, die Ereignisse in eine Hive-Tabelle streamt, benötigen Sie einige zusätzliche Komponenten. Diese sind wie folgt:

* datanucleus-api-jdo-3.2.6.jar
* datanucleus-rdbms-3.2.9.jar
* datanucleus-core-3.2.10.jar
* hive-site.xml

Die **JAR**-Dateien stehen in Ihrem HDInsight Spark-Cluster unter `/usr/hdp/current/spark-client/lib` zur Verfügung. Die Datei **hive-site.xml** ist unter `/usr/hdp/current/spark-client/conf` verfügbar.

Sie können [WinScp](http://winscp.net/eng/download.php) verwenden, um diese Dateien aus dem Cluster auf den lokalen Computer zu kopieren. Sie können dann Tools verwenden, um diese Dateien in das Ihrem Cluster zugeordnete Speicherkonto zu kopieren. Weitere Informationen zum Hochladen von Dateien in das Speicherkonto finden Sie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md).

Nachdem Sie die Dateien in Ihr Azure-Speicherkonto kopiert haben, öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Verzeichnis, in dem Sie CURL installiert haben, und führen Sie den folgenden Befehl aus (ersetzen Sie Benutzername, Kennwort und Clustername):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Die Parameter in der Datei **inputHive.txt** sind wie folgt definiert:

    { "file":"wasbs:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasbs:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasbs:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasbs:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasbs:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Die Parameter sind ähnlich jenen, die Sie für die Textausgabe in vorherigen Schritten angegeben haben. Auch hier müssen Sie nicht die Ausgabeordner (EventCheckpoint, EventCount/EventCount10) oder die Hive-Ausgabetabelle (EventHiveTable10) erstellen, die als Parameter verwendet werden. Die Streaminganwendung erstellt sie automatisch. Beachten Sie, dass die Optionen **jars** und **files** Pfade zu den JAR-Dateien und zur Datei „hive-site.xml“ enthält, die Sie in das Speicherkonto kopiert haben.

Um sicherzustellen, dass die Hive-Tabelle erfolgreich erstellt wurde, können Sie mit SSH eine Verbindung zum Cluster herstellen und Hive-Abfragen ausführen. Anweisungen finden Sie unter [Verwenden von Hive mit Hadoop in HDInsight über SSH](hdinsight-hadoop-use-hive-ssh.md). Sobald Sie eine Verbindung mit SSH hergestellt haben, können Sie den folgenden Befehl ausführen, um zu überprüfen, ob die Hive-Tabelle **EventHiveTable10**erstellt wurde.

    show tables;

Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

    OK
    eventhivetable10
    hivesampletable

Sie können auch eine SELECT-Abfrage zum Anzeigen des Tabelleninhalts ausführen.

    SELECT * FROM eventhivetable10 LIMIT 10;

Folgendes sollte angezeigt werden:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a>Ausführen von Anwendungen für den Empfang der Ereignisse in einer Azure SQL-Datenbanktabelle
Stellen Sie vor diesem Schritt sicher, dass Sie eine Azure SQL-Datenbank erstellt haben. Anleitungen finden Sie unter [Erstellen einer SQL-Datenbank in wenigen Minuten](../sql-database/sql-database-get-started.md). Zum Abschließen dieses Abschnitts benötigen Sie die Werte für Datenbankname, Datenbankservername und die Anmeldeinformationen des Datenbankadministrators als Parameter. Sie müssen jedoch nicht die Datenbanktabelle erstellen. Die Spark-Streaminganwendung erstellt sie automatisch.

Öffnen Sie ein Eingabeaufforderungsfenster, navigieren Sie zu dem Verzeichnis, in dem Sie CURL installiert haben, und führen Sie den folgenden Befehl aus:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

Die Parameter in der Datei **inputSQL.txt** sind wie folgt definiert:

    { "file":"wasbs:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Um sicherzustellen, dass die Anwendung erfolgreich ausgeführt wird, können Sie mit SQL Server Management Studio eine Verbindung zur Azure SQL-Datenbank herstellen. Informationen hierzu finden Sie unter [Herstellen einer Verbindung zur SQL-Datenbank mit SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md). Nachdem Sie die Verbindung zur Datenbank hergestellt haben, navigieren Sie zur **EventContent** -Tabelle, die von der Streaminganwendung erstellt wurde. Sie können eine schnelle Abfrage ausführen, um Daten aus der Tabelle abzurufen. Führen Sie die folgende Abfrage aus:

    SELECT * FROM EventCount

Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: ../storage-create-storage-account/ 

