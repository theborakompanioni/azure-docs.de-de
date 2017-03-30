---
title: Verwenden von Hadoop Pig mit PowerShell in HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie mit Azure PowerShell Pig-Aufträge an einen Hadoop-Cluster in HDInsight übermitteln."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/21/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: b4f2f1195887d8b64599e82334ab36b21eef1d1d
ms.lasthandoff: 03/22/2017

---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a>Ausführen von Pig-Aufträgen in HDInsight mithilfe von Azure PowerShell

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel zur Verwendung von Azure PowerShell zum Übermitteln von Pig-Aufträgen an einen Hadoop-Cluster in HDInsight. Mithilfe von Pig können Sie MapReduce-Aufträge in einer Sprache (Pig Latin) verfassen, die Datentransformationen anstelle von Map- und Reduce-Funktionen modelliert.

> [!NOTE]
> Dieses Dokument bietet keine detaillierten Beschreibungen zu den in diesem Beispiel verwendeten Pig Latin-Anweisungen. Informationen zu den in diesem Beispiel verwendeten Pig Latin-Anweisungen finden Sie unter [Verwenden von Pig Latin mit Hadoop in HDInsight](hdinsight-use-pig.md).

## <a id="prereq"></a>Voraussetzungen

* **Einen Azure HDInsight-Cluster**

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* **Eine Arbeitsstation mit Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a id="powershell"></a>Ausführen von Pig-Aufträgen mit PowerShell

Azure PowerShell stellt *Cmdlets* bereit, mit denen Sie Pig-Aufträge in HDInsight remote ausführen können. Intern verwendet PowerShell REST-Aufrufe von [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat), das im HDInsight-Cluster ausgeführt wird.

Die folgenden Cmdlets werden zum Ausführen der Pig-Aufträge in einem HDInsight-Remotecluster verwendet:

* **Login-AzureRmAccount**: Authentifiziert Azure PowerShell für Ihr Azure-Abonnement
* **New-AzureRmHDInsightPigJobDefinition:** erstellt mithilfe der angegebenen Pig Latin-Anweisungen eine *Auftragsdefinition*
* **Start-AzureRmHDInsightJob**: sendet die Auftragsdefinition an HDInsight, startet den Auftrag und gibt ein *Auftrags* -Objekt zurück, mit dem der Status des Auftrags geprüft werden kann
* **Wait-AzureRmHDInsightJob**: verwendet das Auftragsobjekt, um den Status des Auftrags zu prüfen. Es wird gewartet, bis der Auftrag abgeschlossen oder die Wartezeit abgelaufen ist.
* **Get-AzureRmHDInsightJobOutput**: wird zum Abrufen der Ausgabe des Auftrags verwendet

Die folgenden Schritte veranschaulichen, wie diese Cmdlets zum Ausführen eines Auftrags auf dem HDInsight-Cluster verwendet werden.

1. Speichern Sie den folgenden Code mithilfe eines Editors als **pigjob.ps1**.

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. Öffnen Sie eine neue Windows PowerShell-Eingabeaufforderung. Navigieren Sie zum Speicherort der Datei **pigjob.ps1** , und verwenden Sie folgenden Befehl zum Ausführen des Skripts:
   
        .\pigjob.ps1
   
    Sie werden aufgefordert, sich bei Ihrem Azure-Abonnement anzumelden. Außerdem werden Sie aufgefordert, den Namen und das Kennwort für das HTTPS/Administratorkonto für den HDInsight-Cluster einzugeben.

2. Nachdem der Auftrag abgeschlossen ist, muss er Informationen zurückgeben, die folgendem Text ähneln:
   
        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="troubleshooting"></a>Problembehandlung

Wenn nach Abschluss des Auftrags keine Informationen zurückgegeben werden, ist während der Verarbeitung möglicherweise ein Fehler aufgetreten. Um Fehlerinformationen für diesen Auftrag anzuzeigen, fügen Sie folgenden Befehl am Ende der Datei **pigjob.ps1** hinzu. Anschließend speichern Sie die Datei und führen sie erneut aus.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Dadurch werden die beim Ausführen des Auftrags an STDERR auf dem Server geschriebenen Informationen zurückgegeben, die möglicherweise hilfreich sind, um festzustellen, warum der Auftrag fehlgeschlagen ist.

## <a id="summary"></a>Zusammenfassung
Wie Sie sehen können, bietet Azure PowerShell eine einfache Möglichkeit, um Pig-Aufträge auf einem HDInsight-Cluster auszuführen, den Auftragsstatus zu überwachen und die Ausgabe abzurufen.

## <a id="nextsteps"></a>Nächste Schritte
Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)


