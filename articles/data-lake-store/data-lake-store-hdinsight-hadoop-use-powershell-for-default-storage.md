---
title: 'PowerShell: Azure HDInsight-Cluster mit Data Lake Store als Standardspeicher | Microsoft-Dokumentation'
description: Verwenden von Azure PowerShell zum Erstellen und Verwenden von HDInsight-Clustern mit Azure Data Lake
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8917af15-8e37-46cf-87ad-4e6d5d67ecdb
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/14/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 307070c755cff059b4b82494d28e06cf490a6f7a
ms.openlocfilehash: 6c5badbbea7385cac1407e4af148d5b647af04ac
ms.lasthandoff: 02/16/2017


---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-default-storage"></a>Verwenden von Azure PowerShell zum Erstellen von HDInsight-Clustern mit Data Lake Store (als Standardspeicher)
> [!div class="op_single_selector"]
> * [Verwenden des Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Verwenden von PowerShell (für Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Verwenden von PowerShell (für zusätzlichen Speicher)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Verwenden von Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Erfahren Sie, wie Sie mithilfe von Azure PowerShell einen HDInsight-Cluster mit Azure Data Lake Store **als Standardspeicher** konfigurieren. Anleitungen zum Erstellen eines HDInsight-Clusters mit Azure Data Lake Store als zusätzlichen Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe von Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md).

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Verwenden von Data Lake Store für die Speicherung von HDInsight-Clustern

Hier finden Sie einige wichtige Überlegungen zur Verwendung von HDInsight mit Data Lake Store:

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für die HDInsight-Version 3.5 verfügbar.

* Für HBase-Cluster (Windows und Linux) wird Data Lake Store **nicht** als Speicheroption unterstützt, weder für Standardspeicher noch für zusätzlichen Speicher.


Zum Konfigurieren von HDInsight für den Data Lake-Speicher mithilfe von PowerShell sind folgende Schritte erforderlich:

* Erstellen eines Azure Data Lake-Speichers
* Einrichten der Authentifizierung für rollenbasierten Zugriff auf den Data Lake-Speicher
* Erstellen eines HDInsight-Clusters mit Authentifizierung für den Data Lake-Speicher
* Ausführen eines Testauftrags im Cluster

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Mindestens Azure PowerShell 1.0**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* **Windows SDK**. Das Installationspaket finden Sie [hier](https://dev.windows.com/en-us/downloads). Dies dient zum Erstellen eines Sicherheitszertifikats.
* **Azure Active Directory-Dienstprinzipal** Die Schritte in diesem Tutorial enthalten Anweisungen zum Erstellen eines Dienstprinzipals in Azure AD. Sie müssen jedoch Azure AD-Administrator sein, um einen Dienstprinzipal erstellen zu können. Wenn Sie Azure AD-Administrator sind, können Sie diese Voraussetzung ignorieren und mit dem Tutorial fortfahren.

    **Wenn Sie kein Azure AD-Administrator sind**, können Sie die erforderlichen Schritte zum Erstellen eines Dienstprinzipals nicht ausführen. In diesem Fall muss Ihr Azure AD-Administrator zunächst einen Dienstprinzipal erstellen, bevor Sie einen HDInsight-Cluster mit Data Lake Store erstellen können. Zudem muss der Dienstprinzipal mit einem Zertifikat erstellt werden, wie unter [Erstellen eines Dienstprinzipals mit Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) beschrieben.

## <a name="create-an-azure-data-lake-store"></a>Erstellen eines Azure Data Lake-Speichers
Führen Sie folgende Schritte aus, um einen Data Lake-Speicher zu erstellen.

1. Öffnen Sie auf dem Desktop ein neues Azure PowerShell-Fenster, und geben Sie den folgenden Codeausschnitt ein. Stellen Sie bei der Aufforderung zum Anmelden sicher, dass Sie sich als einer der Administratoren bzw. Besitzer des Abonnements anmelden:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

   > [!NOTE]
   > Wenn beim Registrieren des Ressourcenanbieters für den Data Lake-Speicher ein Fehler wie `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` gemeldet wird, ist Ihr Abonnement möglicherweise nicht in der Positivliste für den Azure Data Lake-Speicher enthalten. Stellen Sie sicher, dass Sie Ihr Azure-Abonnement für die öffentliche Vorschauversion des Data Lake Store aktivieren, indem Sie diese [Anweisungen](data-lake-store-get-started-portal.md)befolgen.
   >
   >
2. Ein Azure Data Lake-Speicherkonto wird einer Azure-Ressourcengruppe zugeordnet. Erstellen Sie zunächst eine Azure-Ressourcengruppe.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Erstellen einer Azure-Ressourcengruppe](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Erstellen einer Azure-Ressourcengruppe")
3. Erstellen Sie ein Azure Data Lake-Speicherkonto. Der angegebene Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Erstellen eines Azure Data Lake-Kontos](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Erstellen eines Azure Data Lake-Kontos")
4. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe sollte **True**lauten.

5. Wenn Sie Data Lake Store als Standardspeicher verwenden, müssen Sie einen Stammpfad angeben, in den clusterspezifische Dateien während der Clustererstellung kopiert werden. Verwenden Sie die folgenden Cmdlets, um einen Stammpfad zu erstellen. Im folgenden Codeausschnitt ist dies **/clusters/hdiadlcluster**.

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Einrichten der Authentifizierung für rollenbasierten Zugriff auf den Data Lake-Speicher
Jedes Azure-Abonnement ist mit einem Azure Active Directory verknüpft. Benutzer und Dienste, die über das klassische Azure-Portal oder die Azure Resource Manager-API auf Abonnementressourcen zugreifen, müssen sich zunächst bei diesem Azure Active Directory authentifizieren. Azure-Abonnements und -Dienste erhalten Zugriff, indem ihnen die entsprechende Rolle für eine Azure-Ressource zugewiesen wird.  Für Dienste identifiziert der Dienstprinzipal den Dienst in Azure Active Directory (AAD). In diesem Abschnitt wird veranschaulicht, wie Sie einem Anwendungsdienst, z. B. HDInsight, Zugriff auf eine Azure-Ressource gewähren (das zuvor erstellte Azure Data Lake-Speicherkonto), indem Sie einen Dienstprinzipal für die Anwendung erstellen und ihr über Azure PowerShell Rollen zuweisen.

Sie müssen die folgenden Aufgaben ausführen, um die Active Directory-Authentifizierung für Azure Data Lake einzurichten.

* Erstellen eines selbstsignierten Zertifikats
* Erstellen einer Anwendung in Azure Active Directory und eines Dienstprinzipals

### <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats
Stellen Sie sicher, dass Sie das [Windows SDK](https://dev.windows.com/en-us/downloads) installiert haben, bevor Sie mit den Schritten in diesem Abschnitt fortfahren. Sie müssen auch ein Verzeichnis erstellt haben, z.B. **C:\mycertdir**, in dem das Zertifikat erstellt werden soll.

1. Navigieren Sie im PowerShell-Fenster zu dem Speicherort, an dem Sie das Windows SDK installiert haben (normalerweise `C:\Program Files (x86)\Windows Kits\10\bin\x86`), und verwenden Sie das Hilfsprogramm [MakeCert][makecert], um ein selbstsigniertes Zertifikat und einen privaten Schlüssel zu erstellen. Verwenden Sie die folgenden Befehle:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        
        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Sie werden aufgefordert, das Kennwort für den privaten Schlüssel einzugeben. Nach erfolgreicher Ausführung des Befehls sollten im von Ihnen angegebenen Zertifikatsverzeichnis die Dateien **CertFile.cer** und **mykey.pvk** enthalten sein.
2. Verwenden Sie das Hilfsprogramm [Pvk2Pfx][pvk2pfx], um die von MakeCert erstellten PVK- und CER-Dateien in eine PFX-Datei zu konvertieren. Führen Sie den folgenden Befehl aus:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Geben Sie bei Aufforderung das Kennwort für den privaten Schlüssel ein, das Sie bereits angegeben haben. Der Wert, den Sie für den Parameter **-po** angeben, ist das Kennwort, das der PFX-Datei zugeordnet ist. Nachdem der Befehl erfolgreich abgeschlossen wurde, sollte im von Ihnen angegebenen Zertifikatsverzeichnis auch die Datei „CertFile.pfx“ enthalten sein.

### <a name="create-an-azure-active-directory-and-a-service-principal"></a>Erstellen einer Azure Active Directory-Instanz und eines Dienstprinzipals
In diesem Abschnitt führen Sie folgende Schritte aus: Erstellen eines Dienstprinzipals für eine Azure Active Directory-Anwendung, Zuweisen einer Rolle für den Dienstprinzipal und Authentifizieren als Dienstprinzipal durch Bereitstellen eines Zertifikats. Führen Sie die folgenden Befehle zum Erstellen einer Anwendung in Azure Active Directory aus.

1. Fügen Sie die folgenden Cmdlets im PowerShell-Konsolenfenster ein. Stellen Sie sicher, dass der Wert, den Sie für die **-DisplayName** -Eigenschaft angeben, eindeutig ist. Die Werte für **-HomePage** und **-IdentiferUris** sind Platzhalterwerte und werden nicht überprüft.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
            -DisplayName "HDIADL" `
            -HomePage "https://contoso.com" `
            -IdentifierUris "https://mycontoso.com" `
            -CertValue $credential  `
            -StartDate $certificatePFX.NotBefore  `
            -EndDate $certificatePFX.NotAfter

        $applicationId = $application.ApplicationId
2. Erstellen Sie einen Dienstprinzipal, indem Sie die Anwendungs-ID verwenden.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id
3. Gewähren Sie dem Data Lake Store-Stamm und allen Ordner im Stammpfad, den Sie zuvor angegeben haben, den Dienstprinzipalzugriff. Verwenden Sie die folgenden Cmdlets.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-default-storage"></a>Erstellen eines HDInsight-Linux-Clusters mit Data Lake Store als Standardspeicher

In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Linux-Cluster mit Data Lake Store als Standardspeicher. Für diese Version müssen sich der HDInsight-Cluster und der Data Lake Store an dem gleichen Standort befinden.

1. Beginnen Sie, indem Sie die Mandanten-ID für das Abonnement abrufen. Diese benötigen Sie weiter unten.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
        
2. Erstellen Sie den HDInsight-Cluster. Verwenden Sie die folgenden Cmdlets:

        # Set these variables
        
        $location = "East US 2"
        $storageAccountName = $dataLakeStoreName                          # Data Lake Store account name
        $storageRootPath = "<Storage root path you specified earlier>" # E.g. /clusters/hdiadlcluster
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster `
               -ClusterType Hadoop `
               -OSType Linux `
               -ClusterSizeInNodes $clusterNodes `
               -ResourceGroupName $resourceGroupName `
               -ClusterName $clusterName `
               -HttpCredential $httpCredentials `
               -Location $location `
               -DefaultStorageAccountType AzureDataLakeStore `
               -DefaultStorageAccountName "$storageAccountName.azuredatalakestore.net" `
               -DefaultStorageRootPath $storageRootPath `
               -Version "3.5" `
               -SshCredential $sshCredentials `
               -AadTenantId $tenantId `
               -ObjectId $objectId `
               -CertificateFilePath $certificateFilePath `
               -CertificatePassword $password

    Nachdem das Cmdlet erfolgreich abgeschlossen wurde, sollten in der Ausgabe die Clusterdetails aufgeführt werden.

        
## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Ausführen von Testaufträgen im HDInsight-Cluster zur Verwendung des Data Lake-Speichers
Nachdem Sie einen HDInsight-Cluster konfiguriert haben, können Sie Testaufträge für den Cluster ausführen, um zu testen, ob der HDInsight-Cluster auf Daten im Data Lake-Speicher zugreifen kann. Hierzu führen wir einen Hive-Beispielauftrag aus. Es wird eine Tabelle aus den Beispieldaten erstellt, die bereits in Data Lake Store unter **<cluster root>/example/data/sample.log** verfügbar sind.

In diesem Abschnitt stellen Sie eine SSH-Verbindung mit dem erstellten HDInsight-Linux-Cluster her und führen eine Hive-Beispielabfrage aus.

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Nachdem die Verbindung hergestellt wurde, starten Sie die Hive-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl:

        hive
2. Geben Sie die folgenden Anweisungen zum Erstellen einer neuen Tabelle namens **vehicles** mithilfe der Befehlszeilenschnittstelle ein, wobei Sie die Beispieldaten im Data Lake-Speicher verwenden:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Die Abfrageausgabe sollte in der SSH-Konsole angezeigt werden. 

    > [!NOTE]
       > Der Pfad zu den Beispieldaten im obigen Befehl CREATE TABLE ist `adl:///example/data/`, wobei `adl:///` der Clusterstamm ist. Mit dem in diesem Tutorial als Beispiel angegebenen Clusterstamm ist dies `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Daher könnten Sie entweder die kürzere Alternative verwenden oder den vollständigen Pfad zum Clusterstamm angeben. 
       >
       >



## <a name="access-data-lake-store-using-hdfs-commands"></a>Zugreifen auf den Data Lake-Speicher mit HDFS-Befehlen
Nachdem Sie den HDInsight-Cluster für die Verwendung des Data Lake-Speichers konfiguriert haben, können Sie die HDFS-Shellbefehle verwenden, um auf den Speicher zuzugreifen.

In diesem Abschnitt stellen Sie eine SSH-Verbindung mit dem erstellten HDInsight-Linux-Cluster her und führen die HDFS-Befehle aus. 

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Nachdem die Verbindung hergestellt wurde, listen Sie mithilfe des folgenden HDFS-Dateisystembefehls die Dateien im Data Lake-Speicher auf.

    hdfs dfs -ls adl:///

Sie können auch den Befehl `hdfs dfs -put` verwenden, um Dateien in den Data Lake-Speicher hochzuladen, und dann mit `hdfs dfs -ls` überprüfen, ob der Upload der Dateien erfolgreich war.

## <a name="see-also"></a>Weitere Informationen
* [Portal: Erstellen eines HDInsight-Clusters für die Verwendung des Data Lake-Speichers](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

