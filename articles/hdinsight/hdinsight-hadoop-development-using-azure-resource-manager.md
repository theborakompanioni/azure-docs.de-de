---
title: "Migrieren zu Azure Resource Manager-Entwicklungstools für HDInsight-Cluster | Microsoft-Dokumentation"
description: "Gewusst wie: Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 938abf03191dec10da8d2fabf27c5db2415d6bc5
ms.openlocfilehash: 660ea89373fe77dac9b77e529adf37025a0a80cc


---
# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Migrieren zu Azure Resource Manager-basierten Entwicklungstools für HDInsight-Cluster
Die Azure Service Manager (ASM)-basierten Tools für HDInsight sind veraltet und werden ersetzt. Wenn Sie bisher Azure PowerShell, die Azure-CLI oder das HDInsight .NET SDK für die Arbeit mit HDInsight-Clustern verwendet haben, empfehlen wir Ihnen von nun an die Verwendung der Azure Resource Manager (ARM)-basierten Versionen von PowerShell, der CLI und des .NET SDK. Dieser Artikel bietet Ihnen hilfreiche Informationen für die Migration zum neuen ARM-basierten Ansatz. Außerdem weist dieser Artikel auch auf eventuelle Unterschiede zwischen den ASM- und ARM-basierten Ansätzen für HDInsight hin.

> [!IMPORTANT]
> Die ASM-basierten Versionen von PowerShell, der CLI und des .NET SDK werden noch bis zum **1. Januar 2017**unterstützt.
> 
> 

## <a name="migrating-azure-cli-to-azure-resource-manager"></a>Migrieren der Azure-CLI zu Azure Resource Manager
Die Azure-CLI wird nun standardmäßig im Azure Resource Manager (ARM)-Modus verwendet. Ausgenommen davon sind Upgrades von vorherigen Installationen; in dem Fall müssen Sie ggf. den Befehl `azure config mode arm` für einen Wechsel zum ARM-Modus ausführen.

Die grundlegenden Befehle, die die Azure-CLI für die Arbeit mit HDInsight mithilfe von Azure Service Management (ASM) bereitgestellt hat, gelten auch für die Verwendung von ARM. Einige Parameter und Switches haben jedoch unter Umständen andere Namen. Außerdem sind bei der Verwendung von ARM viele neue Parameter verfügbar. So können Sie jetzt beispielsweise `azure hdinsight cluster create` verwenden, um zu bestimmen, in welchem Azure Virtual Network ein Cluster erstellt werden soll, oder für Hive- und Oozie-Metastore-Informationen.

Grundlegende Befehle für die Arbeit mit HDInsight über Azure Resource Manager sind:

* `azure hdinsight cluster create` – erstellt einen neuen HDInsight-Cluster
* `azure hdinsight cluster delete` – löscht einen vorhandenen HDInsight-Cluster
* `azure hdinsight cluster show` – zeigt Informationen zu einem vorhandenen Cluster an
* `azure hdinsight cluster list` – listet HDInsight-Cluster für Ihr Azure-Abonnement auf

Verwenden Sie den Switch `-h` , um die Parameter und Switches für jeden Befehl zu überprüfen.

### <a name="new-commands"></a>Neue Befehle
Neue mit Azure Resource Manager verfügbare Befehle sind:

* `azure hdinsight cluster resize` – ändert dynamisch die Anzahl von Workerknoten im Cluster
* `azure hdinsight cluster enable-http-access` – aktiviert HTTPs-Zugriff auf den Cluster (standardmäßig aktiviert)
* `azure hdinsight cluster disable-http-access` – deaktiviert den HTTPs-Zugriff auf den Cluster
* `azure hdinsight-enable-rdp-access` – aktiviert das Remotedesktopprotokoll auf einem Windows-basierten HDInsight-Cluster
* `azure hdinsight-disable-rdp-access` – deaktiviert das Remotedesktopprotokoll auf einem Windows-basierten HDInsight-Cluster
* `azure hdinsight script-action` – enthält Befehle zum Erstellen/Verwalten von Skriptaktionen auf einem Cluster
* `azure hdinsight config` – enthält Befehle zum Erstellen einer Konfigurationsdatei, die mit dem Befehl `hdinsight cluster create` zum Bereitstellen von Konfigurationsinformationen genutzt werden kann

### <a name="deprecated-commands"></a>Veraltete Befehle
Die `azure hdinsight job`-Befehle, die Sie ggf. zum Übermitteln von Aufträgen an Ihren HDInsight-Cluster verwenden, stehen in den ARM-Befehlen nicht zur Verfügung. Wenn Sie Aufträge programmgesteuert von Skripts an HDInsight übermitteln möchten, verwenden Sie die von HDInsight bereitgestellten REST-APIs. Weitere Informationen zum Übermitteln von Aufträgen mithilfe von REST-APIs finden Sie in den folgenden Dokumenten.

* [Ausführen von MapReduce-Aufträgen mit Hadoop in HDInsight mithilfe von Curl](hdinsight-hadoop-use-mapreduce-curl.md)
* [Ausführen von Hive-Abfragen mit Hadoop in HDInsight mit Curl](hdinsight-hadoop-use-hive-curl.md)
* [Ausführen von Pig-Aufträgen mit Hadoop in HDInsight mithilfe von Curl](hdinsight-hadoop-use-pig-curl.md)

Informationen zu anderen Verfahren zur interaktiven Ausführung von MapReduce, Hive und Pig finden Sie unter [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md), [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md) und [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md).

### <a name="examples"></a>Beispiele
**Erstellen eines Clusters**

* Alter Befehl (ASM) – `azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Neuer Befehl (ARM) – `azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**Löschen eines Clusters**

* Alter Befehl (ASM) – `azure hdinsight cluster delete myhdicluster`
* Neuer Befehl (ARM) – `azure hdinsight cluster delete mycluster -g myresourcegroup`

**Auflisten von Clustern**

* Alter Befehl (ASM) – `azure hdinsight cluster list`
* Neuer Befehl (ARM) – `azure hdinsight cluster list`

> [!NOTE]
> Wenn die Ressourcengruppe für den Listenbefehl mithilfe von `-g` festgelegt wird, werden nur die Cluster in der angegebenen Ressourcengruppe zurückgegeben.
> 
> 

**Anzeigen von Clusterinformationen**

* Alter Befehl (ASM) – `azure hdinsight cluster show myhdicluster`
* Neuer Befehl (ARM) – `azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-to-azure-resource-manager"></a>Migrieren von Azure PowerShell zu Azure Resource Manager
Allgemeine Informationen zu Azure PowerShell im Azure Resource Manager (ARM)-Modus finden Sie unter [Verwenden von Windows PowerShell mit dem Azure-Ressourcen-Manager](../powershell-azure-resource-manager.md).

Azure PowerShell-ARM-Cmdlets können neben ASM-Cmdlets installiert werden. Die Cmdlets der beiden Modi können nach ihren Namen unterschieden werden.  Die Namen der Cmdlets enthalten im ARM-Modus *AzureRmHDInsight*, im ASM-Modus hingegen *AzureHDInsight*.  Beispiel: *New-AzureRmHDInsightCluster* und *New-AzureHDInsightCluster* Parameter und Switches haben unter Umständen neue Namen, und für die Verwendung von ARM stehen viele neue Parameter zur Verfügung.  So erfordern beispielsweise verschiedene Cmdlets einen neuen Switch namens *-ResourceGroupName*. 

Bevor Sie die HDInsight-Cmdlets verwenden können, müssen sich mit Ihrem Azure-Konto verbinden und eine neue Ressourcengruppe erstellen:

* Login-AzureRmAccount oder [Select-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Weitere Informationen finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Umbenannte Cmdlets
So listen Sie die HDInsight-ASM-Cmdlets in der Windows-PowerShell-Konsole auf:

    help *azurermhdinsight*

Die folgende Tabelle enthält die ASM-Cmdlets und deren Namen im ARM-Modus:

| ASM-Cmdlets | ARM-Cmdlets |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Add-AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Add-AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Add-AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Add-AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Grant-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Grant-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[New-AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[New-AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[New-AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[New-AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[New-AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[New-AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[Revoke-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[Revoke-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[Stop-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Use-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Wait-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Neue Cmdlets
Die folgenden Cmdlets sind neu und nur im ARM-Modus verfügbar. 

**Cmdlets zu Skriptaktionen:**

* **Get-AzureRmHDInsightPersistedScriptAction**: Ruft persistente Skriptaktionen für einen Cluster ab und listet diese in chronologischer Reihenfolge auf oder ruft Details zu einer bestimmten persistenten Skriptaktion ab. 
* **Get-AzureRmHDInsightScriptActionHistory**: Ruft den Skriptaktionsverlauf für einen Cluster ab und listet ihn in umgekehrter chronologischer Reihenfolge auf oder ruft Details zu zuvor ausgeführten Skritpaktionen ab. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: Entfernt eine persistente Skriptaktion von einem HDInsight-Cluster.
* **Set-AzureRmHDInsightPersistedScriptAction**: Legt eine zuvor ausgeführte Skriptaktion als persistente Skriptaktion fest.
* **Submit-AzureRmHDInsightScriptAction**: Übermittelt eine neue Skriptaktion an einen Azure HDInsight-Cluster. 

Weitere Informationen zur Verwendung finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

**Cmdlets zur Clusteridentität:**

* **Add-AzureRmHDInsightClusterIdentity**: Fügt einem Clusterkonfigurationsobjekt eine Clusteridentität hinzu, sodass der HDInsight-Clusgter auf Azure Data Lake-Speicher zugreifen kann. Siehe hierzu auch [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe von Azure PowerShell](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Beispiele
**Cluster erstellen**

Alter Befehl (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Neuer Befehl (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Löschen eines Clusters**

Alter Befehl (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Neuer Befehl (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Auflisten von Clusterdetails**

Alter Befehl (ASM):

    Get-AzureHDInsightCluster

Neuer Befehl (ARM):

    Get-AzureRmHDInsightCluster 

**Anzeigen von Clustern**

Alter Befehl (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Neuer Befehl (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Weitere Beispiele
* [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Übermitteln von Hive-Jobs](hdinsight-hadoop-use-hive-powershell.md)
* [Übermitteln von Pig-Aufträgen](hdinsight-hadoop-use-pig-powershell.md)
* [Übermitteln von Squoop-Aufträgen](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>Migrieren zum ARM-basierten HDInsight .NET SDK
Das [Azure Service Management (ASM)-basierte HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) ist veraltet. Wir empfehlen Ihnen, das [Azure Resource Management (ASM)-basierte HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx)zu verwenden. Die folgenden ASM-basierten HDInsight-Pakete sind veraltet.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Dieser Abschnitt enthält weitere Informationen dazu, wie Sie bestimmte Aufgaben mithilfe des ARM-basierten SDK ausführen können.

| Gewusst wie: Verwenden des ARM-basierten HDInsight SDK | Links |
| --- | --- |
| Erstellen von HDInsight-Clustern mit dem .NET SDK |Siehe [Erstellen von HDInsight-Clustern mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| Anpassen eines Clusters mithilfe von Skriptaktionen mit dem .NET SDK |Siehe [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Authentifizieren von Clientanwendungen, die Azure Active Directory mit dem .NET SDK interaktiv nutzen |Weitere Informationen finden Sie unter [Ausführen von Hive-Abfragen per HDInsight .NET-SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md). Der Codeausschnitt in diesem Artikel verwendet den interaktiven Authentifizierungsansatz. |
| Authentifizieren von Clientanwendungen, die Azure Active Directory mit dem .NET SDK nicht interaktiv nutzen |Siehe [Erstellen von HDInsight-Anwendungen für die nicht interaktive Authentifizierung](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Übermitteln eines Hive-Auftrags mit dem .NET SDK |Siehe [Übermitteln von Hive-Aufträgen](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Übermitteln eines Pig-Auftrags mit dem .NET SDK |Siehe [Übermitteln von Pig-Aufträgen](hdinsight-hadoop-use-pig-dotnet-sdk.md) |
| Übermitteln eines Sqoop-Auftrags mit dem .NET SDK |Siehe [Übermitteln von Sqoop-Aufträgen](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Auflisten von HDInsight-Clustern mit dem .NET SDK |Weitere Informationen finden Sie unter [List HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Skalieren von HDInsight-Clustern mit dem .NET SDK |Weitere Informationen finden Sie unter [Scale HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Gewähren/Widerrufen von Zugriff auf HDInsight-Cluster mit dem .NET SDK |Weitere Informationen finden Sie unter [Grant/revoke access to HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| Aktualisieren von HTTP-Anmeldeinformationen für HDInsight-Cluster mit dem .NET SDK |Weitere Informationen finden Sie unter [Update HTTP user credentials for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Suchen des standardmäßigen Speicherkonto für HDInsight-Cluster mit dem .NET SDK |Weitere Informationen finden Sie unter [Find the default storage account for HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Löschen von HDInsight-Clustern mithilfe des .NET SDK |Weitere Informationen finden Sie unter [Delete HDInsight clusters](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Beispiele
Im Folgenden finden Sie einige Beispiele dazu, wie ein Vorgang mit dem ASM-basierten SDK durchgeführt wird und welcher der entsprechende Codeausschnitt für das ARM-basierte SDK ist.

**Erstellen eines Cluster-CRUD-Clients**

* Alter Befehl (ASM)
  
        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Neuer Befehl (ARM) (Dienstprinzipalautorisierung)
  
        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Neuer Befehl (ARM) (Benutzerautorisierung)
  
        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Erstellen eines Clusters**

* Alter Befehl (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Neuer Befehl (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Windows,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**Aktivieren von HTTP-Zugriff**

* Alter Befehl (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Neuer Befehl (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Löschen eines Clusters**

* Alter Befehl (ASM)
  
        client.DeleteCluster(dnsName);
* Neuer Befehl (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);




<!--HONumber=Dec16_HO4-->


