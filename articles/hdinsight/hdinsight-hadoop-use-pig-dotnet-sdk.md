<properties
   pageTitle="Verwenden von Hadoop Pig mit .NET in HDInsight | Microsoft Azure"
   description="Erfahren Sie, wie Sie mithilfe des .NET SDK für Hadoop Pig-Aufträge an Hadoop in HDInsight übermitteln."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/06/2016"
   ms.author="larryfr"/>

#Ausführen von Pig-Aufträgen mithilfe des .NET SDK für Hadoop in HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Dieses Dokument enthält ein Beispiel zur Verwendung von .NET SDK für Hadoop zur Übermittlung von Pig-Aufträgen an einen Hadoop-Cluster in HDInsight.

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. Mithilfe von Pig können Sie MapReduce-Vorgänge erstellen, indem Sie eine Reihe von Datentransformationen modellieren. Sie werden erfahren, wie Sie mit einer einfachen C#-Anwendung einen Pig-Auftrag an einen HDInsight-Cluster übermitteln.

[AZURE.INCLUDE [azure-portal](../../includes/hdinsight-azure-portal.md)]

* [Ausführen von Pig-Aufträgen mithilfe des .NET SDK für Hadoop in HDInsight](hdinsight-hadoop-use-pig-dotnet-sdk-v1.md)

## Voraussetzungen

Damit Sie die in dieser Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Einen Azure HDInsight-Cluster (Hadoop in HDInsight), der auf Windows oder Linux basiert
* Visual Studio 2012, 2013 oder 2015

## Erstellen der Anwendung

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen.


1. Öffnen Sie Visual Studio 2012 oder 2013.
2. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
3. Geben Sie für das neue Projekt die folgenden Werte ein, oder wählen Sie sie aus.

	<table>
	<tr>
	<th>Eigenschaft</th>
	<th>Wert</th>
	</tr>
	<tr>
	<th>Kategorie</th>
	<th>Vorlagen/Visual C#/Windows</th>
	</tr>
	<tr>
	<th>Vorlage</th>
	<th>Konsolenanwendung</th>
	</tr>
	<tr>
	<th>Name</th>
	<th>SubmitPigJob</th>
	</tr>
	</table>
4. Klicken Sie auf **OK**, um das Projekt zu erstellen.
5. Wählen Sie im Menü **Extras** die Option **Bibliothekspaket-Manager** oder **Nuget-Paket-Manager** und dann **Paket-Manager-Konsole** aus.
6. Führen Sie den folgenden Befehl in der Konsole aus, um die .NET SDK-Pakete zu installieren.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Doppelklicken Sie im Projektmappen-Explorer auf **Program.cs**, um die Datei zu öffnen. Ersetzen Sie den vorhandenen Code durch den folgenden Code:

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
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
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


7. Drücken Sie **F5**, um die Anwendung zu starten.
8. Drücken Sie die **EINGABETASTE**, um die Anwendung zu beenden.

## Zusammenfassung

Wie Sie sehen, können Sie mit dem .NET SDK für Hadoop .NET-Anwendungen erstellen, die Pig-Aufträge an einen HDInsight-Cluster übermitteln, und den Auftragsstatus überwachen.

## Nächste Schritte

Allgemeine Informationen zu Pig in HDInsight:

* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)

Informationen zu anderen Möglichkeiten, wie Sie mit Hadoop in HDInsight arbeiten können:

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)

* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)
[preview-portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0914_2016-->