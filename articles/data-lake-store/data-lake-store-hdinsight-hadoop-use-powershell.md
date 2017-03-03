---
title: 'PowerShell: Azure HDInsight-Cluster mit Data Lake Store als Add-On-Speicher | Microsoft Docs'
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 164ada5a-222e-4be2-bd32-e51dbe993bc0
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/14/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: d8100903d78a9ca8d88d2649ad5245ce3f456518
ms.openlocfilehash: c21f244408ed6f6ca3168ee193bcba4d3b26cd40
ms.lasthandoff: 02/16/2017


---
# <a name="use-azure-powershell-to-create-an-hdinsight-cluster-with-data-lake-store-as-additional-storage"></a>Erstellen eines HDInsight-Clusters mit Data Lake Store (als zusätzlichem Speicher) mithilfe von Azure PowerShell
> [!div class="op_single_selector"]
> * [Verwenden des Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Verwenden von PowerShell (für Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Verwenden von PowerShell (für zusätzlichen Speicher)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Verwenden von Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Erfahren Sie, wie Sie mithilfe von Azure PowerShell einen HDInsight-Cluster mit Azure Data Lake Store **als zusätzlichem Speicher** erstellen. Anleitungen zum Erstellen eines HDInsight-Clusters mit Data Lake Store als Standardspeicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store als Standardspeicher](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md).

Für unterstützte Clustertypen kann Data Lake Store als Standardspeicherkonto oder als zusätzliches Speicherkonto verwendet werden. Wenn Data Lake Store als zusätzlicher Speicher verwendet wird, ist das Standardspeicherkonto für den Cluster weiterhin Azure Storage Blob (WASB), und clusterbezogene Dateien (z.B. Protokolle usw.) werden weiterhin in den Standardspeicher geschrieben, während die Daten, die Sie verarbeiten möchten, in einem Data Lake Store-Konto gespeichert werden können. Das Verwenden von Data Lake-Speicher als zusätzliches Speicherkonto wirkt sich nicht auf Leistung oder die Fähigkeit aus, Daten aus dem Cluster in den Speicher zu lesen bzw. zu schreiben.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Verwenden von Data Lake Store für die Speicherung von HDInsight-Clustern

Hier finden Sie einige wichtige Überlegungen zur Verwendung von HDInsight mit Data Lake Store:

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als zusätzlichen Speicher ist für die HDInsight-Versionen 3.2, 3.4 und 3.5 verfügbar.

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
5. Laden Sie einige Beispieldaten in Azure Data Lake hoch. Sie werden später in diesem Artikel verwendet, um zu überprüfen, ob auf die Daten aus einem HDInsight-Cluster zugegriffen werden kann. Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)herunterladen.

        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


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
3. Gewähren Sie dem Dienstprinzipal Zugriff auf den Data Lake Store-Ordner und auf die Datei, auf die Sie über den HDInsight-Cluster zugreifen werden. Der unten aufgeführte Codeausschnitt bietet Zugriff auf den Stamm des Data Lake Store-Kontos (in das Sie die Beispieldatendatei kopiert haben) und auf die Datei selbst.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /vehicle1_09142014.csv -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-additional-storage"></a>Erstellen eines HDInsight-Clusters mit Data Lake Store als zusätzlichem Speicher

In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Cluster mit Data Lake Store als zusätzlichem Speicher. Für diese Version müssen sich der HDInsight-Cluster und der Data Lake Store an demselben Standort befinden.

1. Beginnen Sie, indem Sie die Mandanten-ID für das Abonnement abrufen. Diese benötigen Sie weiter unten.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId
2. In dieser Version kann der Data Lake-Speicher für einen Hadoop-Cluster nur als zusätzlicher Speicher für den Cluster verwendet werden. Als Standardspeicher werden weiterhin Azure Storage-BLOBS (WASB) verwendet. Wir erstellen daher zuerst das Speicherkonto und die Speichercontainer, die für den Cluster erforderlich sind.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = (Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext
3. Erstellen Sie den HDInsight-Cluster. Verwenden Sie die folgenden Cmdlets:

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $sshCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.4" -OSType Linux -SshCredential $sshCredentials -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Nachdem das Cmdlet erfolgreich abgeschlossen wurde, sollten in der Ausgabe die Clusterdetails aufgeführt werden.

        
## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Ausführen von Testaufträgen im HDInsight-Cluster zur Verwendung des Data Lake-Speichers
Nachdem Sie einen HDInsight-Cluster konfiguriert haben, können Sie Testaufträge für den Cluster ausführen, um zu testen, ob der HDInsight-Cluster auf Daten im Data Lake-Speicher zugreifen kann. Hierzu führen wir einen Hive-Beispielauftrag aus. Es wird eine Tabelle aus den Beispieldaten erstellt, die Sie zuvor in Ihren Data Lake-Speicher hochgeladen haben.

In diesem Abschnitt stellen Sie eine SSH-Verbindung mit dem erstellten HDInsight-Linux-Cluster her und führen eine Hive-Beispielabfrage aus.

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Nachdem die Verbindung hergestellt wurde, starten Sie die Hive-Befehlszeilenschnittstelle (CLI) mit dem folgenden Befehl:

        hive
2. Geben Sie die folgenden Anweisungen zum Erstellen einer neuen Tabelle namens **vehicles** mithilfe der Befehlszeilenschnittstelle ein, wobei Sie die Beispieldaten im Data Lake-Speicher verwenden:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1

## <a name="access-data-lake-store-using-hdfs-commands"></a>Zugreifen auf den Data Lake-Speicher mit HDFS-Befehlen
Nachdem Sie den HDInsight-Cluster für die Verwendung des Data Lake-Speichers konfiguriert haben, können Sie die HDFS-Shellbefehle verwenden, um auf den Speicher zuzugreifen.

In diesem Abschnitt stellen Sie eine SSH-Verbindung mit dem erstellten HDInsight-Linux-Cluster her und führen die HDFS-Befehle aus. 

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Nachdem die Verbindung hergestellt wurde, listen Sie mithilfe des folgenden HDFS-Dateisystembefehls die Dateien im Data Lake-Speicher auf.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

Hierbei sollte auch die Datei aufgeführt werden, die Sie bereits in den Data Lake-Speicher hochgeladen haben.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Sie können auch den Befehl `hdfs dfs -put` verwenden, um Dateien in den Data Lake-Speicher hochzuladen, und dann mit `hdfs dfs -ls` überprüfen, ob der Upload der Dateien erfolgreich war.

## <a name="see-also"></a>Weitere Informationen
* [Portal: Erstellen eines HDInsight-Clusters für die Verwendung des Data Lake-Speichers](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

