---
title: "Ausführen von Apache Pig-Aufträgen mit dem .NET SDK für Hadoop – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe des .NET SDK für Hadoop Pig-Aufträge an Hadoop in HDInsight übermitteln."
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: c0c120a457e9eaf0e7ea44f3fc6502fa71d673e7
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017

---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Ausführen von Pig-Aufträgen mithilfe des .NET SDK für Hadoop in HDInsight

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Erfahren Sie, wie Sie mithilfe des .NET SDK für Hadoop Apache Pig-Aufträge an Hadoop in Azure HDInsight übermitteln.

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. Mithilfe von Pig können Sie MapReduce-Vorgänge erstellen, indem Sie eine Reihe von Datentransformationen modellieren. In diesem Dokument erfahren Sie, wie Sie mit einer einfachen C#-Anwendung einen Pig-Auftrag an einen HDInsight-Cluster übermitteln.

## <a name="prerequisites"></a>Voraussetzungen

Zur Ausführung der in diesem Artikel aufgeführten Schritte benötigen Sie Folgendes.

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Windows oder Linux basiert

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013, 2015 oder 2017.

## <a name="create-the-application"></a>Erstellen der Anwendung

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen.

1. Wählen Sie in Visual Studio im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.

2. Geben Sie für das neue Projekt die folgenden Werte ein, oder wählen Sie sie aus:

   | Eigenschaft | Wert |
   | ------ | ------ |
   | Kategorie | Vorlagen/Visual C#/Windows |
   | Vorlage | Konsolenanwendung |
   | Name | SubmitPigJob |

3. Klicken Sie auf **OK** , um das Projekt zu erstellen.

4. Wählen Sie im Menü **Extras** die Option **Bibliothekspaket-Manager** oder **Nuget-Paket-Manager** und dann **Paket-Manager-Konsole** aus.

5. Verwenden Sie den folgenden Befehl, um die .NET SDK-Pakete zu installieren:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Doppelklicken Sie im Projektmappen-Explorer auf **Program.cs** , um die Datei zu öffnen. Ersetzen Sie den vorhandenen Code durch den folgenden Code:

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. Drücken Sie **F5**, um die Anwendung zu starten.

8. Drücken Sie die **EINGABETASTE** , um die Anwendung zu beenden.

## <a name="summary"></a>Zusammenfassung

Wie Sie sehen, können Sie mit dem .NET SDK für Hadoop .NET-Anwendungen erstellen, die Pig-Aufträge an einen HDInsight-Cluster übermitteln, und den Auftragsstatus überwachen.

## <a name="next-steps"></a>Nächste Schritte

Informationen über Pig in HDInsight finden Sie unter [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md).

Weitere Informationen zur Verwendung von Hadoop in HDInsight finden Sie in den folgenden Dokumenten:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

