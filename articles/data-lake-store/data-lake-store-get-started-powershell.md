---
title: Erste Schritte mit Data Lake Store | Microsoft Docs
description: "Verwenden von Azure PowerShell zum Erstellen eines Data Lake-Speicherkontos und Ausführen grundlegender Vorgänge"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/04/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0c70eff85416b6b41eb455edc430a66b588dec41


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Erste Schritte mit Azure Data Lake-Speicher mithilfe von Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> 
> 

Erfahren Sie, wie Sie mit Azure PowerShell ein Azure Data Lake-Speicherkonto erstellen und grundlegende Vorgänge ausführen, z. B. Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen usw. Weitere Informationen zu Data Lake Store finden Sie unter [Übersicht über Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Mindestens Azure PowerShell 1.0**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Authentifizierung
In diesem Artikel wird ein einfacheres Authentifizierungskonzept mit Data Lake Store verwendet, bei dem Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben. Die Zugriffsebene für das Data Lake Store-Konto und das Dateisystem hängt dann von der Zugriffsebene des angemeldeten Benutzers ab. Für die Authentifizierung mit Data Lake Store stehen mit **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung** aber auch noch andere Konzepte zur Verfügung. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)(Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory).

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen eines Azure Data Lake-Speicherkontos
1. Öffnen Sie vom Desktop aus ein neues Windows PowerShell-Fenster, und geben Sie den folgenden Ausschnitt ein, um sich bei Ihrem Azure-Konto anzumelden, das Abonnement einzurichten und den Data Lake Store-Anbieter zu registrieren. Stellen Sie bei der Aufforderung zum Anmelden sicher, dass Sie sich als einer der Administratoren bzw. Besitzer des Abonnements anmelden:
   
        # Log in to your Azure account
        Login-AzureRmAccount
   
        # List all the subscriptions associated to your account
        Get-AzureRmSubscription
   
        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>
   
        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Ein Azure Data Lake-Speicherkonto wird einer Azure-Ressourcengruppe zugeordnet. Erstellen Sie zunächst eine Azure-Ressourcengruppe.
   
        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"
   
    ![Erstellen einer Azure-Ressourcengruppe](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Erstellen Sie ein Azure Data Lake-Speicherkonto. Der angegebene Name darf nur Kleinbuchstaben und Zahlen enthalten.
   
        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"
   
    ![Erstellen eines Azure Data Lake-Speicherkontos](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")
4. Stellen Sie sicher, dass das Konto erfolgreich erstellt wurde.
   
        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName
   
    Die Ausgabe sollte **True**lauten.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Erstellen von Verzeichnisstrukturen im Azure Data Lake-Speicher
Sie können in Ihrem Azure Data Lake-Speicherkonto Verzeichnisse zum Verwalten und Speichern von Daten erstellen.

1. Legen Sie ein Stammverzeichnis fest.
   
        $myrootdir = "/"
2. Erstellen Sie ein neues Verzeichnis namens **mynewdirectory** unter dem festgelegten Stammverzeichnis.
   
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Stellen Sie sicher, dass das neue Verzeichnis erfolgreich erstellt wurde.
   
        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir
   
    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:
   
    ![Überprüfen des Verzeichnisses](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Hochladen von Daten in den Azure Data Lake-Speicher
Sie können Ihre Daten direkt auf die Stammebene eines Data Lake-Speichers oder in ein im Konto erstelltes Verzeichnis hochladen. Die folgenden Codeausschnitte veranschaulichen das Hochladen von Beispieldaten in das im vorigen Abschnitt erstellte Verzeichnis (**mynewdirectory**).

Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)herunterladen. Laden Sie die Datei herunter, und speichern Sie sie in ein lokales Verzeichnis auf dem Computer, z. B. „C:\sampledata\.“.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, Herunterladen und Löschen von Daten im Data Lake-Speicher
Verwenden Sie zum Umbenennen einer Datei den folgenden Befehl:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Verwenden Sie zum Downloaden einer Datei den folgenden Befehl:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Verwenden Sie zum Löschen einer Datei den folgenden Befehl:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Geben Sie nach entsprechender Aufforderung **Y** ein, um das Element zu löschen. Wenn mehrere Dateien gelöscht werden sollen, können Sie die betreffenden Pfade durch Kommas getrennt bereitstellen.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Löschen eines Azure Data Lake-Speicherkontos
Verwenden Sie den folgenden Befehl zum Löschen Ihres Data Lake-Speicherkontos.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Geben Sie nach entsprechender Aufforderung **Y** ein, um das Konto zu löschen.

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Nov16_HO2-->


