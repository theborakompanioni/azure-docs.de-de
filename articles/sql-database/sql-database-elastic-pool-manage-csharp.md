---
title: "C#: Erstellen und Verwalten eines Pools für elastische Datenbanken in Azure SQL-Datenbank | Microsoft-Dokumentation"
description: Verwenden von Methoden zur Entwicklung von C#-Datenbanken, um einen elastischen Pool in Azure SQL-Datenbank zu verwalten.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: f6e6ff3b-6b60-43c1-afe9-575991e38237
ms.service: sql-database
ms.custom: multiple databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: csharp
ms.workload: data-management
ms.date: 10/04/2016
ms.author: sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: b34cfe18ac3d03802173605f5483879217d1fe1f
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="create-and-manage-an-elastic-pool-with-cx23"></a>Erstellen und Verwalten eines Pools für elastische Datenbanken mit C&#x23;

In diesem Thema wird das Erstellen und Verwalten skalierbarer [Pools für elastische Datenbanken](sql-database-elastic-pool.md) mit C# erläutert. Sie können einen Azure-Pool für elastische Datenbanken auch mit dem [Azure-Portal](https://portal.azure.com/), mit [PowerShell](sql-database-elastic-pool-manage-powershell.md) oder der REST-API erstellen und verwalten. Mithilfe von [Transact-SQL](sql-database-elastic-pool-manage-tsql.md) können Sie Datenbanken auch erstellen und in und aus Pools für elastische Datenbanken verschieben.

> [!NOTE]
> Viele neue Funktionen von SQL-Datenbank werden nur bei Verwendung des [Azure Resource Manager-Bereitstellungsmodells](../azure-resource-manager/resource-group-overview.md) unterstützt. Daher wird empfohlen, stets die aktuelle **Azure SQL-Datenbank-Verwaltungsbibliothek für .NET zu verwenden ([Dokumentationen](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die älteren [auf dem klassischen Bereitstellungsmodell basierenden Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden nur zum Zweck der Abwärtskompatibilität unterstützt. Daher wird die Verwendung der neueren Resource Manager-basierten Bibliotheken empfohlen.
> 

In diesem Thema erfahren Sie, wie Sie in Azure SQL-Datenbank mithilfe von C# und der [Azure SQL-Datenbankbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) einen elastischen Pool erstellen und verwalten. Informationen zum Erstellen einer einzelnen SQL-Datenbank finden Sie unter [Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md).

Die Azure SQL-Datenbank-Bibliothek für .NET bietet eine [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)-basierte API, die die [Resource Manager-basierte REST-API für SQL-Datenbank](https://msdn.microsoft.com/library/azure/mt163571.aspx) umfasst.

> [!NOTE]
> Viele neue Funktionen von SQL-Datenbank werden nur bei Verwendung des [Azure Resource Manager-Bereitstellungsmodells](../azure-resource-manager/resource-group-overview.md) unterstützt. Daher wird empfohlen, stets die aktuelle **Azure SQL-Datenbank-Verwaltungsbibliothek für .NET zu verwenden ([Dokumentationen](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet-Paket](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Die älteren [auf dem klassischen Bereitstellungsmodell basierenden Bibliotheken](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) werden nur zum Zweck der Abwärtskompatibilität unterstützt. Daher wird die Verwendung der neueren Resource Manager-basierten Bibliotheken empfohlen.
> 

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie lediglich oben auf dieser Seite auf **KOSTENLOSES KONTO**. Lesen Sie anschließend diesen Artikel zu Ende.
* Visual Studio. Eine kostenlose Version von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

## <a name="create-a-console-app-and-install-the-required-libraries"></a>Erstellen einer Konsolen-App und Installieren der erforderlichen Bibliotheken
1. Starten Sie Visual Studio.
2. Klicken Sie auf **Datei** > **Neu** > **Projekt**.
3. Erstellen Sie eine **Konsolenanwendung** mit C#, und nennen Sie sie *SqlElasticPoolConsoleApp*

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank

Laden Sie zum Erstellen einer SQL-Datenbank mit C# die erforderlichen Verwaltungsbibliotheken (mithilfe der [Paket-Manager-Konsole](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicken Sie auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Geben Sie `Install-Package Microsoft.Azure.Management.Sql -Pre` ein, um die [Microsoft Azure SQL-Verwaltungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)zu installieren.
3. Geben Sie `Install-Package Microsoft.Azure.Management.ResourceManager -Pre` ein, um die [Microsoft Azure Resource Manager-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager)zu installieren.
4. Geben Sie `Install-Package Microsoft.Azure.Common.Authentication -Pre` ein, um die [Microsoft Azure-Bibliothek für die allgemeine Authentifizierung](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication)zu installieren. 

> [!NOTE]
> Bei den Beispielen in diesem Artikel wird eine synchrone Form der einzelnen API-Anforderungen und -Blöcke bis zur Beendigung des REST-Aufrufs für den zugrunde liegenden Dienst verwendet. Es stehen asynchrone Methoden zur Verfügung.
>  

## <a name="create-a-sql-elastic-pool---c-example"></a>Erstellen eines elastischen SQL-Pools – C#-Beispiel
Im folgenden Beispiel werden die folgenden Komponenten erstellt: Ressourcengruppe, Server, Firewallregel, elastischer Pool und eine SQL-Datenbank im Pool. Informationen zum Abrufen der `_subscriptionId, _tenantId, _applicationId, and _applicationSecret`-Variablen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen](#create-a-service-principal-to-access-resources).

Ersetzen Sie den Inhalt von **Program.cs** durch folgende Angaben, und aktualisieren Sie `{variables}` mit Ihren App-Werten (ohne `{}`).

```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
{
    class Program
        {

        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    ElasticPoolName = poolName
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
    }
}
```

## <a name="create-a-service-principal-to-access-resources"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen
Mit dem folgenden PowerShell-Skript werden die Active Directory (AD)-Anwendung und der Dienstprinzipal erstellt, den wir zum Authentifizieren der C#-App benötigen. Das Skript gibt Werte aus, die für das vorhergehende C#-Beispiel erforderlich sind. Ausführliche Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Ein Pool für elastische Datenbanken. Informationen zum Erstellen eines Pools für elastische Datenbanken finden Sie unter [Erstellen und Verwalten eines Pools für elastische Datenbanken mit C#](sql-database-elastic-pool-manage-csharp.md).
* Visual Studio. Eine kostenlose Version von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs) .

## <a name="move-a-database-into-an-elastic-pool"></a>Verschieben einer Datenbank in einen elastischen Pool
Sie können eine eigenständige Datenbank in einen und aus einem Pool für elastische Datenbanken verschieben.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Auflisten von Datenbanken in einem elastischen Pool
Um alle Datenbanken in einem Pool für elastische Datenbanken abzurufen, rufen Sie die [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases)-Methode auf.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-an-elastic-pool"></a>Ändern der Leistungseinstellungen eines Pools für elastische Datenbanken
Rufen Sie die vorhandenen Pooleigenschaften ab. Ändern Sie die Werte, und führen Sie die CreateOrUpdate-Methode aus.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

## <a name="latency-of-elastic-pool-operations"></a>Latenzzeit der elastischen Poolvorgänge
* Änderungen der minimalen oder maximalen Anzahl von eDTUs pro Datenbank werden in der Regel in höchstens fünf Minuten durchgeführt.
* Wie lange es dauert, die Poolgröße (eDTUs) zu ändern, hängt von der kombinierten Größe aller Datenbanken im Pool ab. Änderungen dauern durchschnittlich 90 Minuten oder weniger pro 100 GB. Wenn beispielsweise der gesamte Speicherplatz für alle Datenbanken im Pool 200 GB beträgt, ist für die Änderung der eDTUs pro Pool eine Latenzzeit von drei Stunden oder weniger zu erwarten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
*  SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen sowie Informationen zu Datenbankverbindungsfehlern und anderen Probleme finden Sie unter [SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md).
* [APIs für Azure-Ressourcenverwaltung](https://msdn.microsoft.com/library/azure/dn948464.aspx)
* Informationen zu Pools für elastische Datenbanken finden Sie unter [Wann sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool.md).


