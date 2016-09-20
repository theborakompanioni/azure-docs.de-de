<properties
    pageTitle="Erstellen eines Pools für elastische Datenbanken mit C# | Microsoft Azure"
    description="Verwenden Sie C#-Datenbankentwicklungstechniken, um einen skalierbaren Pool für elastische Datenbanken in Azure SQL-Datenbank zu erstellen, damit Sie Ressourcen für zahlreiche Datenbanken freigeben können."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="09/14/2016"
    ms.author="sstein"/>

# Erstellen eines Pools für elastische Datenbanken mit C&#x23;

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


In diesem Artikel wird gezeigt, wie Sie einen Pool für elastische Datenbanken in Azure SQL-Datenbank mithilfe von C# und der [Azure SQL-Datenbankbibliothek für .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) erstellen. Informationen zum Erstellen einer eigenständigen SQL-Datenbank finden Sie unter [Ausprobieren von SQL-Datenbank: Verwenden von C# zum Erstellen einer SQL-Datenbank mithilfe der SQL-Datenbankbibliothek für .NET](sql-database-get-started-csharp.md).

Die Azure SQL-Datenbankbibliothek für .NET bietet eine [Azure-Ressourcen-Manager](../resource-group-overview.md)-basierte API, die die [Ressourcen-Manager-basierte REST-API für die SQL-Datenbank](https://msdn.microsoft.com/library/azure/mt163571.aspx) umfasst.


> [AZURE.NOTE] Die SQL-Datenbankbibliothek für .NET befindet sich derzeit in der Vorschauphase.


Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie lediglich oben auf dieser Seite auf den Link **KOSTENLOSES KONTO**. Lesen Sie anschließend diesen Artikel zu Ende.
- Visual Studio. Eine kostenlose Version von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads](https://www.visualstudio.com/downloads/download-visual-studio-vs).


## Erstellen einer Konsolen-App und Installieren der erforderlichen Bibliotheken

1. Starten Sie Visual Studio.
2. Klicken Sie auf **Datei** > **Neu** > **Projekt**.
3. Erstellen Sie eine **Konsolenanwendung** mit C#, und nennen Sie sie *SqlElasticPoolConsoleApp*.


Laden Sie zum Erstellen einer SQL-Datenbank mit C# die erforderlichen Verwaltungsbibliotheken (mithilfe der [Paket-Manager-Konsole](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Klicken Sie auf **Tools** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole**.
2. Geben Sie `Install-Package Microsoft.Azure.Management.Sql –Pre` ein, um die [Microsoft Azure SQL-Verwaltungsbibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql) zu installieren.
3. Geben Sie `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` ein, um die [Microsoft Azure Resource Manager-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager) zu installieren.
4. Geben Sie `Install-Package Microsoft.Azure.Common.Authentication –Pre` ein, um die [Microsoft Azure-Bibliothek für die allgemeine Authentifizierung](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication) zu installieren.



> [AZURE.NOTE] Bei den Beispielen in diesem Artikel wird eine synchrone Form der einzelnen API-Anforderungen und -Blöcke bis zur Beendigung des REST-Aufrufs für den zugrunde liegenden Dienst verwendet. Es stehen asynchrone Methoden zur Verfügung.


## Beispiel: Erstellen eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit C#

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





## Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen

Mit dem folgenden PowerShell-Skript werden die Active Directory (AD)-Anwendung und der Dienstprinzipal erstellt, den wir zum Authentifizieren der C#-App benötigen. Das Skript gibt Werte aus, die für das vorhergehende C#-Beispiel erforderlich sind. Ausführliche Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../resource-group-authenticate-service-principal.md).

   
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


  

## Nächste Schritte

- [Verwalten Ihres Pools](sql-database-elastic-pool-manage-csharp.md)
- [Erstellen elastischer Aufträge](sql-database-elastic-jobs-overview.md): Elastische Aufträge ermöglichen die Ausführung von T-SQL-Skripts für eine beliebige Anzahl von Datenbanken in einem Pool.
- [Skalieren mit der Azure SQL-Datenbank](sql-database-elastic-scale-introduction.md): Verwenden Sie elastische Datenbanktools für das horizontale Hochskalieren.

## Weitere Ressourcen

- [SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)
- [APIs für Azure-Ressourcenverwaltung](https://msdn.microsoft.com/library/azure/dn948464.aspx)

<!---HONumber=AcomDC_0914_2016-->