<properties
	pageTitle="Verwenden von Hadoop Sqoop in HDInsight | Microsoft Azure"
	description="Erfahren Sie, wie Sie mit dem HDInsight .NET SDK einen Sqoop-Import und -Export zwischen einem Hadoop-Cluster und einer Azure-SQL-Datenbank durchführen können."
	editor="cgronlun"
	manager="paulettm"
	services="hdinsight"
	documentationCenter=""
	tags="azure-portal"
	authors="mumian"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/06/2016"
	ms.author="jgao"/>

#Ausführen von Sqoop-Aufträgen mithilfe des .NET SDK für Hadoop in HDInsight

[AZURE.INCLUDE [Sqoop-Auswahl](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie das HDInsight .NET SDK zum Ausführen von Sqoop-Aufträgen in HDInsight zum Importieren und Exportieren zwischen HDInsight-Cluster und Azure SQL-Datenbank oder SQL Server-Datenbank verwendet werden kann.

> [AZURE.NOTE] Die Schritte in diesem Artikel können in Windows- oder Linux-basierten HDInsight-Clustern angewendet werden, sie funktionieren jedoch nur von einem Windows-Client aus. Wählen Sie über die Registerkartenauswahl am Anfang dieses Artikels andere Methoden aus.

###Voraussetzungen

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:

- **Hadoop-Cluster in HDInsight**. Informationen finden Sie unter [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## Ausführen von Sqoop mit dem .NET SDK

Das HDInsight .NET SDK enthält .NET-Clientbibliotheken, die das Arbeiten mit HDInsight-Clustern in .NET vereinfachen. In diesem Abschnitt erstellen Sie eine C#-Konsolenanwendung zum Exportieren der hivesampletable in die SQL-Datenbanktabelle, die Sie zuvor in diesem Lernprogramm erstellt haben.

Das folgende Beispiel verwendet die interaktive Benutzerauthentifizierung. Weitere Informationen zur nicht interaktiven Authentifizierung finden Sie unter [Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung](hdinsight-create-non-interactive-authentication-dotnet-applications.md).


**So übermitteln Sie einen Sqoop-Job**

1. Erstellen Sie eine C#-Konsolenanwendung in Visual Studio.
2. Führen Sie in der Paket-Manager-Konsole von Visual Studio den folgenden NuGet-Befehl aus, um das Paket zu importieren.

		Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.HDInsight.Job -Pre
        
3. Verwenden Sie den folgenden Code in der Datei „Program.cs“:

        using System;
        using System.Collections.Generic;
        using System.Security;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
                private static HDInsightJobManagementClient _hdiJobManagementClient;

                private static Guid SubscriptionId = new Guid("<Your Subscription ID>");
                private const string ResourceGroupName = "<Your Resource Group Name>";

                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var tokenCreds = GetTokenCloudCredentials();
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);

                    var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                    var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");

                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitSqoopJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                public static TokenCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
                {
                    var authFactory = new AuthenticationFactory();

                    var account = new AzureAccount { Type = AzureAccount.AccountType.User };

                    if (username != null && password != null)
                        account.Id = username;

                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

                    var accessToken =
                        authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto)
                            .AccessToken;

                    return new TokenCloudCredentials(accessToken);
                }

                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(TokenCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), creds.Token);
                }


                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";

                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";

                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;

                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string>{"/user/oozie/share/lib/sqoop/sqljdbc41.jar"}, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };

                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Drücken Sie die Taste **F5**, um das Programm auszuführen.

##Nächste Schritte

Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

- [Verwenden von Oozie mit HDInsight](hdinsight-use-oozie.md): Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
- [Analysieren von Daten zu Flugverspätungen mit HDInsight](hdinsight-analyze-flight-delay-data.md): Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
- [Hochladen von Daten in HDInsight](hdinsight-upload-data.md): Andere Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.

<!---HONumber=AcomDC_0413_2016-->