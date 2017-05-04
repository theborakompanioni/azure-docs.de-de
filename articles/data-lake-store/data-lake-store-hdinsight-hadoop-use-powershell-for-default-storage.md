---
title: Erstellen von HDInsight-Clustern mit Data Lake Store als Standardspeicher mithilfe von PowerShell | Microsoft-Dokumentation
description: Verwenden von Azure PowerShell zum Erstellen und Verwenden von HDInsight-Clustern mit Azure Data Lake Store
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
ms.date: 03/02/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 0053f93218e9fec4d72fb229bfb2c6159d8b5bc7
ms.lasthandoff: 04/27/2017


---
# <a name="create-hdinsight-clusters-with-data-lake-store-as-default-storage-by-using-powershell"></a>Erstellen von HDInsight-Clustern mit Data Lake Store als Standardspeicher mithilfe von PowerShell
> [!div class="op_single_selector"]
> * [Verwenden des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Verwenden von PowerShell (für Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Verwenden von PowerShell (für zusätzlichen Speicher)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Verwenden von Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Erfahren Sie, wie Sie mithilfe von Azure PowerShell Azure HDInsight-Cluster mit Azure Data Lake Store als Standardspeicher konfigurieren. Anleitungen zum Erstellen eines HDInsight-Clusters mit Data Lake Store als zusätzlichen Speicher finden Sie unter [Verwenden von Azure PowerShell zum Erstellen von HDInsight-Clustern mit Data Lake Store (als Standardspeicher)](data-lake-store-hdinsight-hadoop-use-powershell.md).

Hier finden Sie einige wichtige Überlegungen zur Verwendung von HDInsight mit Data Lake Store:

* Eine Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für HDInsight-Version 3.5 verfügbar.

* Die Option zum Erstellen von HDInsight-Clustern mit Zugriff auf Data Lake Store als Standardspeicher ist für HDInsight Premium-Cluster *nicht verfügbar*.

* Für HBase-Cluster (Windows und Linux) wird Data Lake Store *nicht* als Speicheroption unterstützt, weder für Standardspeicher noch für zusätzlichen Speicher.

Befolgen Sie die Anweisungen in den nächsten fünf Abschnitten, um HDInsight zum Arbeiten mit Data Lake Store mithilfe von PowerShell zu konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, stellen Sie sicher, dass Sie die folgenden Anforderungen erfüllen:

* **Ein Azure-Abonnement**: Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/)
* **Azure PowerShell 1.0 oder höher**: Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).
* **Windows Software Development Kit (SDK)**: Wechseln Sie zu [Downloads und Tools für Windows 10](https://dev.windows.com/en-us/downloads), um Windows SDK zu installieren. Sie verwenden Windows SDK, um ein Sicherheitszertifikat zu erstellen.
* **Azure Active Directory-Dienstprinzipal**: In diesem Tutorial wird beschrieben, wie ein Dienstprinzipal in Azure Active Directory (Azure AD) erstellt wird. Sie müssen jedoch Azure AD-Administrator sein, um einen Dienstprinzipal erstellen zu können. Wenn Sie Administrator sind, können Sie diese Voraussetzung ignorieren und mit dem Tutorial fortfahren.

    >[!NOTE]
    >Sie können einen Dienstprinzipal nur dann erstellen, wenn Sie ein Azure AD-Administrator sind. Ihr Azure AD-Administrator muss zunächst einen Dienstprinzipal erstellen, bevor Sie einen HDInsight-Cluster mit Data Lake Store erstellen können. Der Dienstprinzipal muss mit einem Zertifikat erstellt werden, wie unter [Erstellen eines Dienstprinzipals mit Zertifikat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority) beschrieben.
    >

## <a name="create-a-data-lake-store-account"></a>Erstellen eines Data Lake-Speicherkontos
Führen Sie folgende Schritte aus, um ein Data Lake Store-Konto zu erstellen:

1. Öffnen Sie auf dem Desktop ein PowerShell-Fenster, und geben Sie den anschließend folgenden Codeausschnitt ein.

        # Sign in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    > [!NOTE]
    > Wenn Sie den Data Lake Store-Ressourcenanbieter registrieren und einen Fehler erhalten, der `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` ähnlich ist, ist Ihr Abonnement möglicherweise nicht in der Zulassungsliste für Data Lake Store enthalten. Um Ihr Azure-Abonnement für die öffentliche Preview von Data Lake Store zu aktivieren, befolgen Sie die Anweisungen unter [Erste Schritte mit Azure Data Lake Store mithilfe des Azure-Portals](data-lake-store-get-started-portal.md).
    >

2. Wenn Sie zum Anmelden aufgefordert werden, melden Sie sich als einer der Abonnementadministrator oder -besitzer an.
3. Ein Data Lake Store-Konto wird einer Azure-Ressourcengruppe zugeordnet. Beginnen Sie, indem Sie eine Ressourcengruppe erstellen.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Erstellen einer Azure-Ressourcengruppe](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Erstellen einer Azure-Ressourcengruppe")
3. Erstellen Sie ein Data Lake Store-Konto. Der angegebene Kontoname darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Erstellen eines Azure Data Lake-Kontos](./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Erstellen eines Azure Data Lake-Kontos")
4. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Die Ausgabe sollte **TRUE** sein.

5. Wenn Sie Data Lake Store als Standardspeicher verwenden, müssen Sie einen Stammpfad angeben, in den clusterspezifische Dateien während der Clustererstellung kopiert werden. Verwenden Sie folgende Cmdlets, um einen Stammpfad zu erstellen, der im Ausschnitt **/clusters/hdiadlcluster** enthalten ist.

        $myrootdir = "/"
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/clusters/hdiadlcluster


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Einrichten der Authentifizierung für rollenbasierten Zugriff auf den Data Lake-Speicher
Jedes Azure-Abonnement ist mit einer Azure AD-Entität verknüpft. Benutzer und Dienste, die über das Azure-Portal oder die Azure Resource Manager-API auf Abonnementressourcen zugreifen, müssen sich zunächst bei Azure AD authentifizieren. Azure-Abonnements und -Dienste erhalten Zugriff, indem ihnen die entsprechende Rolle für eine Azure-Ressource zugewiesen wird. Für Dienste wird ein Dienstprinzipal in Azure AD den Dienst identifizieren.

In diesem Abschnitt wird veranschaulicht, wie ein Anwendungsdienst erteilt wird, z.B. HDInsight, Zugriff auf eine Azure-Ressource (das zuvor von Ihnen erstellte Data Lake Store-Konto). Dazu erstellen Sie einen Dienstprinzipal für die Anwendung und weisen diesem Rollen über PowerShell zu.

Sie müssen Aufgaben in den folgenden zwei Abschnitten ausführen, um die Active Directory-Authentifizierung für Azure Data Lake einzurichten.

### <a name="create-a-self-signed-certificate"></a>Erstellen eines selbstsignierten Zertifikats
Stellen Sie sicher, dass Sie das [Windows SDK](https://dev.windows.com/en-us/downloads) installiert haben, bevor Sie mit den Schritten in diesem Abschnitt fortfahren. Sie müssen auch ein Verzeichnis erstellt haben, z.B. *C:\mycertdir*, in dem das Zertifikat erstellt werden soll.

1. Navigieren Sie im PowerShell-Fenster zu dem Speicherort, an dem Sie das Windows SDK installiert haben (normalerweise *C:\Programme (x86)\Windows Kits\10\bin\x86*), und verwenden Sie das Hilfsprogramm [MakeCert][makecert], um ein selbstsigniertes Zertifikat und einen privaten Schlüssel zu erstellen. Verwenden Sie die folgenden Befehle:

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -r -len 2048

    Sie werden aufgefordert, das Kennwort für den privaten Schlüssel einzugeben. Nach erfolgreicher Ausführung des Befehls sollten im von Ihnen angegebenen Zertifikatsverzeichnis die Dateien **CertFile.cer** und **mykey.pvk** enthalten sein.
2. Verwenden Sie das Hilfsprogramm [Pvk2Pfx][pvk2pfx], um die von MakeCert erstellten PVK- und CER-Dateien in eine PFX-Datei zu konvertieren. Führen Sie den folgenden Befehl aus:

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Geben Sie bei Aufforderung das Kennwort für den privaten Schlüssel ein, das Sie bereits angegeben haben. Der Wert, den Sie für den Parameter **-po** angeben, ist das Kennwort, das der PFX-Datei zugeordnet ist. Nachdem der Befehl erfolgreich abgeschlossen wurde, sollte im von Ihnen angegebenen Zertifikatsverzeichnis auch die Datei „CertFile.pfx“ enthalten sein.

### <a name="create-an-azure-ad-and-a-service-principal"></a>Erstellen einer Azure AD-Anwendung und eines Dienstprinzipals
In diesem Abschnitt erstellen Sie einen Dienstprinzipal für eine Azure AD-Anwendung, weisen eine Rolle für den Dienstprinzipal zu, und authentifizieren Sie den Dienstprinzipal durch Bereitstellen eines Zertifikats. Führen Sie die folgenden Befehle zum Erstellen einer Anwendung in Azure AD aus.

1. Fügen Sie die folgenden Cmdlets im PowerShell-Konsolenfenster ein. Stellen Sie sicher, dass der Wert, den Sie für die **-DisplayName**-Eigenschaft angeben, eindeutig ist. Die Werte für **-HomePage** und **-IdentiferUris** sind Platzhalterwerte und werden nicht überprüft.

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
3. Gewähren Sie dem Data Lake Store-Stamm und allen Ordner im Stammpfad, den Sie zuvor angegeben haben, den Dienstprinzipalzugriff. Verwenden Sie die folgenden Cmdlets:

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters -AceType User -Id $objectId -Permissions All
        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path /clusters/hdiadlcluster -AceType User -Id $objectId -Permissions All

## <a name="create-an-hdinsight-linux-cluster-with-data-lake-store-as-the-default-storage"></a>Erstellen eines HDInsight-Linux-Clusters mit Data Lake Store als Standardspeicher

In diesem Abschnitt erstellen Sie einen HDInsight Hadoop-Linux-Cluster mit Data Lake Store als Standardspeicher. Für diese Version müssen sich der HDInsight-Cluster und Data Lake Store an dem gleichen Standort befinden.

1. Rufen Sie die Abonnementmandanten-ID auf, und speichern Sie sie, um sie später zu verwenden.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Erstellen Sie den HDInsight-Cluster, indem Sie die folgenden Cmdlets verwenden:

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

    Nachdem Sie das Cmdlet erfolgreich abgeschlossen wurde, sollte eine Ausgabe angezeigt werden, in denen die Clusterdetails aufgeführt werden.

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-store"></a>Ausführen von Testaufträgen im HDInsight-Cluster zur Verwendung von Data Lake Store
Nachdem Sie einen HDInsight-Cluster konfiguriert haben, können Sie Testaufträge ausführen, um zu testen, ob er auf Daten in Data Lake Store zugreifen kann. Hierzu führen wir einen Hive-Beispielauftrag aus, um eine Tabelle zu erstellen, die bereits in Data Lake Store unter *<cluster root>/example/data/sample.log* verfügbar ist.

In diesem Abschnitt erstellen Sie eine Secure Shell-Verbindung (SSH) zum HDInsight Linux-Cluster her, den Sie erstellt haben, und führen anschließend eine Hive-Beispielabfrage aus.

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

1. Nachdem Sie die Verbindung hergestellt haben, starten Sie die Hive-Befehlszeilenschnittstelle (CLI) mithilfe des folgenden Befehls:

        hive
2. Verwenden Sie die Befehlszeilenschnittstelle, um folgende Anweisungen anzugeben.Damit können Sie eine neue Tabelle mit dem Namen **vehicles** (Fahrzeuge) erstellen, indem Sie die Beispieldaten in Data Lake Store verwenden:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'adl:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Die Abfrageausgabe sollte in der SSH-Konsole angezeigt werden.

    >[!NOTE]
    >Der Pfad zu den Beispieldaten im obigen Befehl CREATE TABLE ist `adl:///example/data/`, wobei `adl:///` der Clusterstamm ist. Mit dem Beispiel des Clusterstamms, der in diesem Tutorial spezifisch ist, ist der Befehl `adl://hdiadlstore.azuredatalakestore.net/clusters/hdiadlcluster`. Daher könnten Sie entweder die kürzere Alternative verwenden oder den vollständigen Pfad zum Clusterstamm angeben.
    >

## <a name="access-data-lake-store-by-using-hdfs-commands"></a>Zugreifen auf Data Lake Store mit HDFS-Befehlen
Nachdem Sie den HDInsight-Cluster für die Verwendung von Data Lake Store konfiguriert haben, können Sie die HDFS-Shellbefehle verwenden, um auf den Speicher zuzugreifen.

In diesem Abschnitt stellen Sie eine SSH-Verbindung zum HDInsight Linux-Cluster her, den Sie erstellt haben. Anschließend führen Sie die HDFS-Befehle aus.

* Wenn Sie einen Windows-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).
* Wenn Sie einen Linux-Client für die SSH-Verbindung mit dem Cluster verwenden, finden Sie entsprechende Informationen unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

Nachdem Sie die Verbindung hergestellt haben, können Sie die Dateien in Data Lake Store mithilfe des folgenden HDFS-Dateisystembefehls auflisten.

    hdfs dfs -ls adl:///

Sie können auch den Befehl `hdfs dfs -put` verwenden, um Dateien in Data Lake Store hochzuladen, und dann mit `hdfs dfs -ls` überprüfen, ob der Upload der Dateien erfolgreich war.

## <a name="see-also"></a>Weitere Informationen
* [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

