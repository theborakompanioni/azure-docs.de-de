---
title: "Hochladen von Daten für Hadoop-Aufträge in HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Daten für Hadoop-Aufträge in HDInsight mithilfe der Azure-Befehlszeilenschnittstelle, Azure-Speicher-Explorer, Azure PowerShell, der Hadoop-Befehlszeile oder Sqoop hochladen und abrufen können."
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 56b913ee-0f9a-4e9f-9eaf-c571f8603dd6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 0caac0c7ad76df7e7fa657227339d1df8b4a3c77
ms.lasthandoff: 04/27/2017


---
# <a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Hochladen von Daten für Hadoop-Aufträge in HDInsight
Azure HDInsight stellt über Azure-BLOB-Speicher ein Hadoop Distributed File System (HDFS) mit vollem Funktionsumfang zur Verfügung. Es ist als eine HDFS-Erweiterung konzipiert, die eine nahtlose Benutzererfahrung bietet. Der vollständige Satz von Komponenten im Hadoop-Ökosystem kann direkt mit den verwalteten Daten verwendet werden. Azure-BLOB-Speicher und HDFS sind unterschiedliche Dateisysteme, die jeweils für die Datenspeicherung und Berechnungen dieser Daten optimiert sind. Die Vorteile der Verwendung von Azure Blob Storage werden unter [Verwenden von Azure Blob Storage mit HDInsight][hdinsight-storage] beschrieben.

**Voraussetzungen**

Beachten Sie die folgenden Voraussetzungen, bevor Sie beginnen:

* Ein Azure HDInsight-Cluster. Anweisungen finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started] or [Bereitstellen eines HDInsight-Clusters][hdinsight-provision].

## <a name="why-blob-storage"></a>Warum Blob-Speicher?
Azure HDInsight-Cluster werden in der Regel bereitgestellt, um MapReduce-Aufträge auszuführen. Die Cluster werden gelöscht, sobald diese Aufträge abgeschlossen sind. Nachdem die Berechnungen erfolgt sind, wäre es zu teuer, diese Daten weiterhin in den HDFS-Clustern zu speichern. Der Azure-BLOB-Speicher ist eine hoch skalierbare und verfügbare, kostengünstige und freigabefähige Speicheroption mit hoher Kapazität für Daten, die mit HDInsight verarbeitet werden sollen. Das Speichern von Daten in einem Blob sorgt dafür, dass die für Berechnungen verwendeten HDInsight-Cluster sicher freigegeben werden können, ohne Daten zu verlieren.

### <a name="directories"></a>Verzeichnisse
In Azure-BLOB-Speichercontainern werden Daten als Schlüssel-Wert-Paare gespeichert, und es gibt keine Verzeichnishierarchie. Allerdings kann im Schlüsselnamen das Zeichen '/' verwendet werden, damit es so aussieht, als wäre eine Datei in einer Verzeichnisstruktur gespeichert. HDInsight betrachtet diese Schlüsselnamen, als wären es tatsächliche Verzeichnisse.

Der Schlüssel eines Blobs kann z. B. *input/log1.txt* heißen. Das Verzeichnis 'input' existiert zwar nicht, wegen des Zeichens '/' im Schlüsselnamen sieht es jedoch so aus, als gäbe es einen Dateipfad.

Daher werden in den Azure Explorer-Tools möglicherweise einige Dateien mit 0 Byte angezeigt. Diese Dateien dienen zwei Zwecken:

* Bei leeren Ordnern markieren sie das Vorhandensein der Ordner. Azure-BLOB-Speicher erkennt, dass es bei Vorhandensein eines Blobs mit dem Namen "foo/bar" einen Ordner namens **foo**gibt. Soll es jedoch einen leeren Ordner namens **foo** geben, kann dieser nur mit der speziellen 0-Byte-Datei bezeichnet werden.
* Diese Dateien enthalten einige spezielle vom Hadoop-Dateisystem benötigte Metadaten, insbesondere die Berechtigungen und Eigentümer der Ordner.

## <a name="command-line-utilities"></a>Befehlszeilenprogramme
Microsoft bietet die folgenden Hilfsprogramme für die Verwendung mit dem Azure-Blob-Speicher:

| Tool | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure-Befehlszeilenschnittstelle][azurecli] |✔ |✔ |✔ |
| [Azure PowerShell][azure-powershell] | | |✔ |
| [AzCopy][azure-azcopy] | | |✔ |
| [Hadoop-Befehl](#commandline) |✔ |✔ |✔ |

> [!NOTE]
> Während die Azure-Befehlszeilenschnittstelle, Azure PowerShell und AzCopy von außerhalb von Azure verwendet werden können, ist der Hadoop-Befehl nur im HDInsight-Cluster verfügbar, außerdem lässt er nur das Laden von Daten aus dem lokalen Dateisystem in den Azure-Blob-Speicher zu.
>
>

### <a id="xplatcli"></a>Azure-Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle ist ein plattformübergreifendes Tool zur Verwaltung von Azure-Diensten. Verwenden Sie die folgenden Schritte zum Hochladen von Daten in Azure-BLOB-Speicher:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Installieren und konfigurieren Sie die Azure-Befehlszeilenschnittstelle für Mac, Linux und Windows](../cli-install-nodejs.md).
2. Öffnen Sie eine Eingabeaufforderung, Bash oder eine andere Shell, und authentifizieren Sie Ihr Azure-Abonnement wie folgt.

        azure login

    Geben Sie nach Aufforderung den Benutzernamen und das Kennwort für Ihr Abonnement ein.
3. Geben Sie den folgenden Befehl ein, um die Speicherkonten für Ihr Abonnement aufzulisten:

        azure storage account list
4. Wählen Sie das Speicherkonto mit dem zu verwendenden Blob aus, und rufen Sie mit dem folgenden Befehl den Schlüssel für dieses Konto ab:

        azure storage account keys list <storage-account-name>

    Damit sollten der **primäre** und der **sekundäre** Schlüssel zurückgegeben werden. Kopieren Sie den Wert des **primären** Schlüssels für die Verwendung in den nächsten Schritten.
5. Rufen Sie mit dem folgenden Befehl eine Liste der Blobcontainer im Speicherkonto ab:

        azure storage container list -a <storage-account-name> -k <primary-key>
6. Verwenden Sie die folgenden Befehle zum Hoch- und Herunterladen von Dateien im Blob:

   * So laden Sie eine Datei hoch:

           azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>
   * So laden Sie eine Datei herunter:

           azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [!NOTE]
> Wenn Sie immer mit demselben Speicherkonto arbeiten, können die folgenden Umgebungsvariablen festlegen, anstatt das Konto und den Schlüssel für jeden Befehl anzugeben:
>
> * **AZURE\_STORAGE\_ACCOUNT**: der Name des Speicherkontos
> * **AZURE\_STORAGE\_ACCESS\_KEY**: der Speicherkontoschlüssel
>
>

### <a id="powershell"></a>Azure PowerShell
Azure PowerShell ist eine Skriptumgebung, mit der Sie die Bereitstellung und Verwaltung Ihrer Workloads in Azure steuern und automatisieren können. Informationen zum Konfigurieren der Arbeitsstation für die Ausführung von Azure PowerShell finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**So laden Sie eine lokale Datei in den Azure-Blobspeicher hoch**

1. Öffnen Sie die Azure PowerShell-Konsole entsprechend den Anweisungen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
2. Legen Sie die Werte der ersten fünf Variablen im folgenden Skript fest:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext
3. Fügen Sie das Skript in die Azure PowerShell-Konsole ein, um es auszuführen und die Datei zu kopieren.

PowerShell-Skripts, die für die Verwendung mit HDInsight erstellt wurden, finden Sie z.B. unter [HDInsight-Tools](https://github.com/blackmist/hdinsight-tools).

### <a id="azcopy"></a>AzCopy
AzCopy ist ein Befehlszeilenprogramm, das den Austausch von Daten mit einem Azure Storage-Konto vereinfachen soll. Sie können dieses Dienstprogramm als eigenständiges Tool verwenden oder in eine vorhandene Anwendung integrieren. [Laden Sie AzCopy herunter][azure-azcopy-download].

Die AzCopy-Syntax lautet folgendermaßen:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

Weitere Informationen finden Sie unter [AzCopy – Hochladen/Herunterladen von Dateien für Azure-Blobs][azure-azcopy].

### <a id="commandline"></a>Hadoop-Befehlszeile
Die Hadoop-Befehlszeile eignet sich nur zum Speichern von Daten im Blob-Speicher, wenn die Daten bereits auf dem Hauptknoten des Clusters vorhanden sind.

Um den Hadoop-Befehl verwenden zu können, müssen Sie zunächst mithilfe einer der folgenden Methoden eine Verbindung zum Hauptknoten herstellen:

* **HDInsight (Windows-basiert)**: [Herstellen einer Verbindung mithilfe von Remotedesktop](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)
* **HDInsight (Linux-basiert)**: Herstellen einer Verbindung mithilfe von SSH ([SSH-Befehl](hdinsight-hadoop-linux-use-ssh-unix.md) oder [PuTTY](hdinsight-hadoop-linux-use-ssh-windows.md))

Nachdem die Verbindung hergestellt wurde, verwenden Sie die folgende Syntax, um eine Datei in den Speicher hochzuladen.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Beispiel: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Da sich das Standarddateisystem für HDInsight im Azure-Blob-Speicher befindet, befindet sich die Datei "/example/data.txt" auch tatsächlich im Azure-Blob-Speicher. Sie können auch folgendermaßen auf die Datei verweisen:

    wasbs:///example/data/data.txt

oder

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Eine Liste weiterer Hadoop-Befehle für die Arbeit mit Dateien finden Sie unter [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]
> In HBase-Clustern ist die standardmäßige Blockgröße beim Schreiben von Daten 256 KB. Bei Verwendung von HBase-APIs oder REST-APIs funktioniert dies einwandfrei. Die Nutzung der Befehle `hadoop` und `hdfs dfs` zum Schreiben von Daten mit einem Umfang von mehr als ca. 12 GB führt allerdings zu einem Fehler. Weitere Informationen finden Sie im Abschnitt [Speicherausnahme beim Schreiben in ein Blob](#storageexception) weiter unten.
>
>

## <a name="graphical-clients"></a>Clients mit grafischer Benutzeroberfläche
Es gibt auch einige Anwendungen, die eine grafische Benutzeroberfläche für die Arbeit mit Azure Storage bereitstellen. Im Folgenden sind einige dieser Anwendungen aufgelistet:

| Client- | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio-Tools für HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) |✔ |✔ |✔ |
| [Azure-Speicher-Explorer](http://storageexplorer.com/) |✔ |✔ |✔ |
| [Cloud Storage Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | |✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

### <a name="visual-studio-tools-for-hdinsight"></a>Visual Studio-Tools für HDInsight
Weitere Informationen finden Sie unter [Navigieren durch die verknüpften Ressourcen](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

### <a id="storageexplorer"></a>Azure-Speicher-Explorer
*Azure-Speicher-Explorer* ist ein hilfreiches Tool zum Prüfen und Ändern der Daten in BLOBs. Dieses kostenlose Open Source-Tool kann von [http://storageexplorer.com/](http://storageexplorer.com/)heruntergeladen werden. Der Quellcode ist über diesen Link ebenfalls verfügbar.

Bevor Sie das Tool verwenden können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen. Anleitungen zum Abrufen dieser Informationen finden Sie unter [Erstellen, Verwalten oder Löschen von Speicherkonten][azure-create-storage-account] im Abschnitt „Anzeigen, Kopieren und Neuerstellen von Speicherzugriffsschlüsseln“.

1. Führen Sie den Azure-Speicher-Explorer aus. Wenn Sie den Speicher-Explorer erstmals ausführen, werden Sie zur Eingabe des **Speicherkontonamens** und **Speicherkontoschlüssels** aufgefordert. Wenn Sie den Speicher-Explorer zuvor bereits ausgeführt haben, wählen Sie die Schaltfläche **Hinzufügen** aus, um einen neuen Speicherkontonamen und -schlüssel hinzuzufügen.

    Geben Sie den Namen und Schlüssel für das vom HDinsight-Cluster verwendete Speicherkonto ein, und wählen Sie dann **SPEICHERN UND ÖFFNEN** aus.

    ![HDI.AzureStorageExplorer][image-azure-storage-explorer]
2. Klicken Sie in der Liste der Container links auf der Benutzeroberfläche auf den Namen des Containers, der Ihrem HDInsight-Cluster zugeordnet ist. Standardmäßig handelt es sich dabei um den Namen des HDInsight-Clusters, der jedoch abweichen kann, wenn Sie bei der Clustererstellung einen bestimmten Namen eingegeben haben.
3. Wählen Sie auf der Symbolleiste das Uploadsymbol aus.

    ![Symbolleiste mit hervorgehobenem Uploadsymbol](./media/hdinsight-upload-data/toolbar.png)
4. Geben Sie eine hochzuladende Datei an, und klicken Sie auf **Öffnen**. Sobald Sie dazu aufgefordert werden, wählen Sie **Hochladen** aus, um die Dateien in das Stammverzeichnis des Speichercontainers hochzuladen. Wenn Sie die Datei in einen bestimmten Pfad hochladen möchten, geben Sie den Pfad im Feld **Ziel** ein, und wählen Sie dann **Hochladen** aus.

    ![Dateiupload (Dialogfeld)](./media/hdinsight-upload-data/fileupload.png)

    Nachdem der Upload der Datei abgeschlossen wurde, können Sie sie in den Aufträgen des HDInsight-Clusters verwenden.

## <a name="mount-azure-blob-storage-as-local-drive"></a>Bereitstellen von Azure-BLOB-Speicher als lokales Laufwerk
Informationen finden Sie unter [Bereitstellen von Azure-BLOB-Speicher als lokales Laufwerk](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="services"></a>Dienste
### <a name="azure-data-factory"></a>Azure Data Factory
Der Azure Data Factory-Dienst ist ein vollständig verwalteter Dienst für das Kombinieren von Verarbeitungs-, Speicher- und Verschiebungsdienste für Daten in optimierten, skalierbaren und zuverlässigen Datenproduktions-Pipelines.

Mit Azure Data Factory können Daten in den Azure-Blob-Speicher verschoben oder Datenpipelines erstellt werden, die HDInsight-Funktionen, z. B. Hive und Pig, direkt verwenden.

Weitere Informationen finden Sie in der [Dokumentation zu Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/).

### <a id="sqoop"></a>Apache Sqoop
Sqoop ist ein Tool zum Übertragen von Daten zwischen Hadoop und relationalen Datenbanken. Sie können damit Daten aus einem Managementsystem für relationale Datenbanken (RDBMS) wie SQL Server, MySQL oder Oracle in das verteilte Dateisystem von Hadoop (HDFS) importieren, die Daten in Hadoop mit MapReduce oder Hive transformieren und sie anschließend wieder in ein RDBMS exportieren.

Weitere Informationen finden Sie unter [Verwenden von Sqoop mit HDInsight][hdinsight-use-sqoop].

## <a name="development-sdks"></a>Entwicklungs-SDKs
Auf den Azure-Blob-Speicher kann auch mithilfe eines Azure-SDK über die folgenden Programmiersprachen zugegriffen werden:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Weitere Informationen zum Installieren der Azure-SDKs finden Sie unter [Azure-Downloads](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Problembehandlung
### <a id="storageexception"></a>Speicherausnahme beim Schreiben in ein Blob
**Symptome**: Bei Verwendung der Befehle `hadoop` und `hdfs dfs`, um Dateien mit etwa 12 GB oder größer in einem HBase-Cluster zu schreiben, wird möglicherweise die folgende Fehlermeldung angezeigt:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Ursache**: Für HBase in HDInsight-Clustern wird standardmäßig eine Blockgröße von 256 KB beim Schreiben in einen Azure-Speicher verwendet. Für HBase-APIs oder REST-APIs funktioniert dies, bei Verwendung der Befehlszeilenprogramme `hadoop` und `hdfs dfs` führt es allerdings zu einem Fehler.

**Lösung**: Verwenden Sie `fs.azure.write.request.size`, um eine größere Blockgröße anzugeben. Sie können dies auf Nutzungsbasis mithilfe des `-D`-Parameters erreichen. Im folgenden Beispiel wird dieser Parameter mit dem Befehl `hadoop` verwendet:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Sie können auch den Wert von `fs.azure.write.request.size` mithilfe von Ambari global erhöhen. Die folgenden Schritte können verwendet werden, um den Wert in der Ambari-Webbenutzeroberfläche zu ändern:

1. Öffnen Sie in Ihrem Browser die Ambari-Webbenutzeroberfläche für den Cluster. Die Adresse lautet „https://CLUSTERNAME.azurehdinsight.net“, wobei **CLUSTERNAME** den Namen des Clusters angibt.

    Geben Sie bei entsprechender Aufforderung den Namen und das Kennwort des Administrators für den Cluster ein.
2. Wählen Sie auf der linken Bildschirmseite **HDFS** und dann die Registerkarte **Configs** aus.
3. Geben Sie in das Feld **Filter** den Text `fs.azure.write.request.size` ein. Dadurch werden das Feld und der aktuelle Wert in der Mitte der Seite angezeigt.
4. Ändern Sie den Wert von 262144 (256 KB) in den neuen Wert. Beispiel: 4194304 (4 MB).

![Abbildung der Änderung des Werts über die Ambari-Webbenutzeroberfläche](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mithilfe der Ambari-Webbenutzeroberfläche](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Nächste Schritte
Jetzt wissen Sie, wie Sie Daten in HDInsight importieren. Lesen Sie in den folgenden Artikeln, wie Sie die Daten analysieren:

* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]
* [Programmgesteuerte Übermittlung von Hadoop-Aufträgen][hdinsight-submit-jobs]
* [Verwenden von Hive mit HDInsight][hdinsight-use-hive]
* [Verwenden von Pig mit HDInsight][hdinsight-use-pig]

[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[azurecli]: ../cli-install-nodejs.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png

