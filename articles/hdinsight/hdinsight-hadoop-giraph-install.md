---
title: "Installieren und Verwenden von Giraph in Hadoop-Clustern in HDInsight – Azure | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie HDInsight-Cluster mit Giraph anpassen und wie Sie Giraph verwenden.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 58db68a1b5000adad4d816c6221881254b9675c7
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Installieren und Verwenden von Giraph in Windows-basierten HDInsight-Clustern

Erfahren Sie, wie Sie mithilfe von Skriptaktionen Windows-basierte HDInsight-Cluster mit Giraph anpassen und mit Giraph große Graphen verarbeiten. Informationen zur Verwendung von Giraph mit einem Linux-basierten Cluster finden Sie unter [Installieren von Giraph in HDinsight Hadoop-Clustern (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> Die Schritte in diesem Dokument funktionieren nur mit einem Windows-basierten HDInsight-Cluster. HDInsight ist unter Windows nur für HDInsight-Versionen vor 3.4 verfügbar. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement). Informationen zur Installation von Giraph in einem Linux-basierten HDInsight-Cluster finden Sie unter [Installieren von Giraph in HDinsight Hadoop-Clustern (Linux)](hdinsight-hadoop-giraph-install-linux.md).


Mithilfe von *Skriptaktionen*können Sie Giraph in einem beliebigen Clustertyp (Hadoop, Storm, HBase, Spark) in Azure HDInsight installieren. Ein Beispielskript zum Installieren von Giraph in einem HDInsight-Cluster steht in einem schreibgeschützten Azure-Speicherblob unter [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)zur Verfügung. Das Beispielskript funktioniert nur mit HDInsight-Clustern der Version 3.1. Weitere Informationen zu HDInsight-Clusterversionen finden Sie unter [HDInsight-Clusterversionen](hdinsight-component-versioning.md).

**Verwandte Artikel**

* [Installieren von Giraph in HDInsight Hadoop-Clustern (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight-Clustern.
* [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen
* [Entwickeln von Skriptaktionsskripts für HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-giraph"></a>Was ist Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> ermöglicht die Graphverarbeitung mit Hadoop und lässt sich mit Azure HDInsight nutzen. Graphen bilden Beziehungen zwischen Objekten ab, wie z. B. Verbindungen zwischen Routern in einem großen Netzwerk wie dem Internet oder Beziehungen zwischen Menschen in sozialen Netzwerken (mitunter als "Social Graph" bezeichnet). Mit der Graphverarbeitung können Sie sich Gedanken über die Beziehungen zwischen den Objekten im Diagramm machen wie etwa:

* Ermitteln potenzieller Freunde aufgrund Ihrer aktuellen Beziehungen.
* Ermitteln der kürzesten Route zwischen zwei Computern in einem Netzwerk.
* Berechnen des Seitenrangs von Webseiten.

## <a name="install-giraph-using-portal"></a>Installieren von Giraph mithilfe des Portals
1. Beginnen Sie mit dem Erstellen eines Clusters, indem Sie die Option **BENUTZERDEFINIERT ERSTELLEN** verwenden, die unter [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-provision-clusters.md) beschrieben ist.
2. Klicken Sie auf der Seite **Skriptaktionen** des Assistenten auf **Skriptaktion hinzufügen**, um wie unten gezeigt Details zur Skriptaktion bereitzustellen:

    ![Anpassen eines Clusters mithilfe einer Skriptaktion](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "Anpassen eines Clusters mithilfe einer Skriptaktion")

    <table border='1'>
        <tr><th>Eigenschaft</th><th>Wert</th></tr>
        <tr><td>Name</td>
            <td>Geben Sie einen Namen für die Skriptaktion an. Beispiel: <b>Giraph installieren</b>.</td></tr>
        <tr><td>Skript-URI</td>
            <td>Geben Sie den Uniform Resource Identifier (URI) für das Skript an, das aufgerufen wird, um den Cluster anzupassen. Beispiel: <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Knotentyp</td>
            <td>Gibt die Knoten an, auf denen das Anpassungsskript ausgeführt wird. Sie können <b>Alle Knoten</b>, <b>Nur Hauptknoten</b> oder <b>Nur Workerknoten</b> auswählen.
        <tr><td>Parameter</td>
            <td>Geben Sie die Parameter an, sofern dies für das Skript erforderlich ist. Für das Skript zum Installieren von Giraph sind keine Parameter erforderlich, sodass Sie diese leer lassen können.</td></tr>
    </table>

    Sie können dem Cluster mehr als eine Skriptaktion zum Installieren von mehreren Komponenten hinzufügen. Nachdem Sie die Skripts hinzugefügt haben, klicken Sie auf das Häkchen, um mit dem Erstellen des Clusters zu beginnen.

## <a name="use-giraph"></a>Verwenden von Giraph
Das Beispiel "SimpleShortestPathsComputation" demonstriert die grundlegende <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a>-Implementierung für das Auffinden des kürzesten Pfads zwischen Objekten in einem Graph. Mit den folgenden Schritten können Sie die Beispieldaten und das Beispiel-Jar hochladen, einen Auftrag mithilfe des Beispiels "SimpleShortestPathsComputation" ausführen und dann die Ergebnisse anzeigen.

1. Laden Sie eine Beispieldatendatei in den Azure-Blobspeicher hoch. Erstellen Sie auf der lokalen Arbeitsstation eine neue Datei namens **tiny_graph.txt**. Sie sollte folgende Zeilen enthalten:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Laden Sie die Datei "tiny_graph.txt" in den Primärspeicher Ihres HDInsight-Clusters hoch. Anweisungen zum Hochladen von Daten finden Sie unter [Hochladen von Daten für Hadoop-Aufträge in HDInsight](hdinsight-upload-data.md).

    Diese Daten beschreiben eine Beziehung zwischen Objekten in einem gerichteten Graph mithilfe des Formats [source\_id, source\_value,[[dest\_id], [edge\_value],...]]. Jede Zeile repräsentiert eine Beziehung zwischen einem **source\_id**-Objekt und einem oder mehreren **dest\_id**-Objekten. Der **edge\_value** (bzw. die Gewichtung) ist vorstellbar als die Stärke oder Distanz der Verbindung zwischen **source_id** und **dest\_id**.

    Wenn die obigen Daten auseinandergezogen und der Wert (die Gewichtung) als Abstand zwischen den Objekten verwendet werden, dann könnte das so aussehen:

    ![tiny_graph.txt als Kreise dargestellt mit Linien unterschiedlicher Länge dazwischen](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Führen Sie das Beispiel "SimpleShortstPathsComputation" aus. Verwenden Sie die folgenden Azure PowerShell-Cmdlets, um das Beispiel unter Verwendung der Datei „tiny_graph.txt“ als Eingabe auszuführen.

    > [!IMPORTANT]
    > Die Azure PowerShell-Unterstützung zum Verwalten von HDInsight-Ressourcen mithilfe von Azure Service Manager ist **veraltet** und wurde zum 1. Januar 2017 eingestellt. Die Schritte in diesem Dokument zeigen die neuen HDInsight-Cmdlets, die mit Azure Resource Manager genutzt werden können.
    >
    > Führen Sie zur Installation der neuesten Version von Azure PowerShell die unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) beschriebenen Schritte aus. Wenn Sie über Skripts verfügen, die für die Verwendung der neuen Cmdlets für Azure Resource Manager geändert werden müssen, finden Sie unter [Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster](hdinsight-hadoop-development-using-azure-resource-manager.md) weitere Informationen.

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasbs:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasbs:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasbs:///example/output/shortestpaths",
                    "-w", "2"
    # Create the definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run the job, write output to the Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    Ersetzen Sie im Beispiel oben **clustername** durch den Namen Ihres HDInsight-Clusters, in dem Giraph installiert ist.
3. Zeigen Sie die Ergebnisse an. Nach Abschluss des Auftrags werden die Ergebnisse in zwei Ausgabedateien im Ordner **wasbs:///example/out/shotestpaths** gespeichert. Die Dateien heißen **part-m-00001** und **part-m-00002**. Führen Sie die folgenden Schritte aus, um die Ausgabe herunterzuladen und anzuzeigen:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select the current subscription
    Select-AzureSubscription $subscriptionName

    # Create the Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download the job output to the workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Damit wird die Verzeichnisstruktur **example/output/shortestpaths** im aktuellen Verzeichnis auf der Arbeitsstation erstellt, und die beiden Ausgabedateien werden in diesen Speicherort heruntergeladen.

    Verwenden Sie das Cmdlet **Cat**, um den Inhalt der Dateien anzuzeigen:

        Cat example/output/shortestpaths/part*

    Die Ausgabe sollte in etwa folgendermaßen aussehen:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Das Beispiel "SimpleShortestPathComputation" wurde hartcodiert und beginnt mit der Objekt-ID 1. Es findet den kürzesten Pfad zu anderen Objekten. Die Ausgabe sollte also `destination_id distance` lauten, wobei der Abstand der Wert (oder das Gewicht) der Kanten ist, der zwischen Objekt-ID 1 und der Ziel-ID zurückgelegt wird.

    Wenn Sie dies visualisieren, können Sie die Ergebnisse überprüfen, indem Sie den kürzesten Weg zwischen ID 1 und allen anderen Objekten zurücklegen. Beachten Sie, dass 5 der kürzeste Pfad zwischen ID 1 und ID 4 ist. Dies ist die gesamte Entfernung zwischen <span style="color:orange">ID 1 und 3</span> und dann zwischen <span style="color:red">ID 3 und 4</span>.

    ![Zeichnen von Objekten als Kreise mit dem kürzesten Pfad dazwischen](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Installieren von Giraph mithilfe von Azure PowerShell
Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Das Beispiel veranschaulicht das Installieren von Spark mithilfe von Azure PowerShell. Sie müssen das Skript für die Verwendung von [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)anpassen.

## <a name="install-giraph-using-net-sdk"></a>Installieren von Giraph mithilfe des .NET SDK
Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Das Beispiel veranschaulicht das Installieren von Spark mithilfe des .NET SDK. Sie müssen das Skript für die Verwendung von [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)anpassen.

## <a name="see-also"></a>Siehe auch
* [Installieren von Giraph in HDInsight Hadoop-Clustern (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-provision-clusters.md): Allgemeine Informationen zum Erstellen von HDInsight-Clustern.
* [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen][hdinsight-cluster-customize]: Allgemeine Informationen zum Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen
* [Entwickeln von Skriptaktionsskripts für HDInsight](hdinsight-hadoop-script-actions.md)
* [Installieren und Verwenden von Spark in HDInsight-Clustern][hdinsight-install-spark]: Skriptaktionsbeispiel zum Installieren von Spark
* [Installieren von R in HDInsight-Clustern][hdinsight-install-r]: Skriptaktionsbeispiel zum Installieren von R
* [Installieren von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install.md): Skriptaktionsbeispiel zum Installieren von Solr

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md

