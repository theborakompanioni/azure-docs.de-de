---
title: Erste Schritte mit Azure Data Lake Store mithilfe von PowerShell | Microsoft-Dokumentation
description: "Verwenden von Azure PowerShell zum Erstellen eines Data Lake-Speicherkontos und Ausführen grundlegender Vorgänge"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/06/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 09f24fa2b55d298cfbbf3de71334de579fbf2ecd
ms.openlocfilehash: 1fe2b4b6c84e3c4d96677e3da5a94b9462e2a7bf
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Erste Schritte mit Azure Data Lake-Speicher mithilfe von Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Erfahren Sie, wie Sie mit Azure PowerShell ein Azure Data Lake-Speicherkonto erstellen und grundlegende Vorgänge ausführen, z. B. Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen usw. Weitere Informationen zu Data Lake Store finden Sie unter [Übersicht über Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Mindestens Azure PowerShell 1.0**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview).

## <a name="authentication"></a>Authentifizierung
In diesem Artikel wird ein einfacheres Authentifizierungskonzept mit Data Lake Store verwendet, bei dem Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben. Die Zugriffsebene für das Data Lake Store-Konto und das Dateisystem hängt dann von der Zugriffsebene des angemeldeten Benutzers ab. Für die Authentifizierung mit Data Lake Store stehen mit **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung** aber auch noch andere Konzepte zur Verfügung. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Endbenutzerauthentifizierung](data-lake-store-end-user-authenticate-using-active-directory.md) oder [Dienst-zu-Dienst-Authentifizierung](data-lake-store-authenticate-using-active-directory.md).

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

    ![Erstellen einer Azure-Ressourcengruppe](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Erstellen einer Azure-Ressourcengruppe")
3. Erstellen Sie ein Azure Data Lake-Speicherkonto. Der angegebene Name darf nur Kleinbuchstaben und Zahlen enthalten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Erstellen eines Azure Data Lake Store-Kontos](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Erstellen eines Azure Data Lake Store-Kontos")
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

    ![Überprüfen des Verzeichnisses](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Überprüfen des Verzeichnisses")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Hochladen von Daten in den Azure Data Lake-Speicher
Sie können Ihre Daten direkt auf die Stammebene eines Data Lake-Speichers oder in ein im Konto erstelltes Verzeichnis hochladen. Die folgenden Codeausschnitte veranschaulichen das Hochladen von Beispieldaten in das im vorigen Abschnitt erstellte Verzeichnis (**mynewdirectory**).

Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)herunterladen. Laden Sie die Datei herunter, und speichern Sie sie in einem lokalen Verzeichnis auf dem Computer, z.B. „C:\sampledata\“.

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

## <a name="performance-guidance-while-using-powershell"></a>Hinweise zur Leistung bei Verwendung von PowerShell

Unten sind die wichtigsten Einstellungen angegeben, die optimiert werden können, um die bestmögliche Leistung zu erzielen, wenn Sie PowerShell für die Arbeit mit Data Lake Store verwenden:

| Eigenschaft            | Standard | Beschreibung |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Mit diesem Parameter können Sie die Anzahl von parallelen Threads zum Hochladen oder Herunterladen der einzelnen Dateien auswählen. Diese Zahl steht für die maximalen Threads, die pro Datei zugeordnet werden können, aber Sie erhalten je nach Szenario ggf. weniger Threads. (Wenn Sie beispielsweise eine Datei mit 1 KB hochladen, erhalten Sie auch dann nur einen Thread, wenn Sie 20 Threads anfordern.)  |
| ConcurrentFileCount | 10      | Dieser Parameter gilt speziell für das Hochladen oder Herunterladen von Ordnern. Mit diesem Parameter wird die Anzahl von gleichzeitigen Dateien bestimmt, die hoch- oder heruntergeladen werden können. Diese Zahl steht für die maximale Anzahl von Dateien, die gleichzeitig hoch- oder heruntergeladen werden können, aber je nach Szenario erhalten Sie ggf. eine geringere Parallelität. (Wenn Sie beispielsweise zwei Dateien hochladen, erhalten Sie auch dann nur zwei gleichzeitige Dateiuploads, wenn Sie 15 anfordern.) |

**Beispiel**

Mit diesem Befehl werden Dateien aus Azure Data Lake Store auf den lokalen Datenträger des Benutzers heruntergeladen, indem 20 Threads pro Datei und 100 gleichzeitige Dateien verwendet werden.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a>Wie kann ich den Wert ermitteln, der für diese Parameter festgelegt werden soll?

Hier sind einige hilfreiche Informationen zur Vorgehensweise angegeben.

* **Schritt 1: Ermitteln der Gesamtzahl von Threads** – Beginnen Sie, indem Sie die Gesamtzahl der Threads berechnen, die verwendet werden sollen. Als Faustregel gilt, dass Sie sechs Threads für jeden physischen Kern verwenden sollten.

        Total thread count = total physical cores * 6

    **Beispiel**

    Annahme: Sie führen die PowerShell-Befehle über eine D14-VM mit 16 Kernen aus.

        Total thread count = 16 cores * 6 = 96 threads


* **Schritt 2: Berechnen von PerFileThreadCount** – Wir berechnen den Wert für PerFileThreadCount basierend auf der Größe der Dateien. Für Dateien, die kleiner als 2,5 GB sind, muss dieser Parameter nicht geändert werden, da die Standardeinstellung 10 ausreicht. Für Dateien, die größer als 2,5 GB sind, sollten Sie zehn Threads als Basis für die ersten 2,5 GB nutzen und für jede weitere Steigerung der Dateigröße um 256 MB einen Thread hinzufügen. Wenn Sie einen Ordner mit Dateien mit stark variierender Größe haben, können Sie erwägen, die Dateien nach Größe zu gruppieren. Wenn die Dateigrößen sehr unterschiedlich sind, kann dies die Leistung beeinträchtigen. Falls das Gruppieren nach ähnlichen Dateigrößen nicht möglich sein sollte, ist es ratsam, den Wert für PerFileThreadCount basierend auf der größten Datei festzulegen.

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Beispiel**

    Angenommen, Sie haben 100 Dateien im Größenbereich von 1 GB bis 10 GB. Wir verwenden für die Gleichung dann 10 GB als größte Dateigröße und erhalten Folgendes:

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Schritt 3: Berechnen von ConcurrentFilecount** – Verwenden Sie die Gesamtzahl der Threads und den Wert von PerFileThreadCount, um ConcurrentFileCount basierend auf der folgenden Gleichung zu berechnen.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Beispiel**

    Basierend auf den verwendeten Beispielwerten:

        96 = 40 * ConcurrentFileCount

    **ConcurrentFileCount** ist also **2,4**, und dieser Wert kann auf **2** abgerundet werden.

### <a name="further-tuning"></a>Weitere Optimierung

Unter Umständen ist eine weitere Optimierung erforderlich, da viele verschiedene Dateigrößen verwendet werden. Die obige Berechnung funktioniert gut, wenn die meisten oder alle Dateien größer sind und eher im Bereich von 10 GB liegen. Falls dagegen viele unterschiedliche Dateigrößen und viele kleinere Dateien vorhanden sind, können Sie den Wert für PerFileThreadCount verringern. Wenn wir PerFileThreadCount reduzieren, kann der Wert für ConcurrentFileCount erhöht werden. Nehmen wir also an, dass die meisten Dateien kleiner sind und sich im Bereich von 5 GB bewegen. Wir können die Berechnung dann entsprechend ändern:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

**ConcurrentFileCount** beträgt jetzt also 96/20 = 4,8, abgerundet auf **4**.

Sie können diese Einstellungen weiter optimieren, indem Sie den Wert für **PerFileThreadCount** je nach Verteilung der Dateigrößen nach oben oder unten anpassen.

### <a name="limitation"></a>Einschränkung

* **Anzahl von Dateien ist geringer als ConcurrentFileCount**: Wenn die Anzahl von hochgeladenen Dateien kleiner als der von Ihnen berechnete Wert von **ConcurrentFileCount** ist, sollten Sie **ConcurrentFileCount** reduzieren und gemäß der Anzahl von Dateien festlegen. Sie können alle verbleibenden Threads verwenden, um **PerFileThreadCount** zu erhöhen.

* **Zu viele Threads**: Wenn Sie die Anzahl von Threads zu stark erhöhen, ohne die Clustergröße zu erhöhen, können sich Leistungseinbußen ergeben. Es kann zu Problemen aufgrund von Konflikten kommen, die beim Kontextwechsel in der CPU entstehen.

* **Unzureichende Parallelität**: Falls die Parallelität nicht ausreicht, ist Ihr Cluster ggf. zu klein. Sie können die Anzahl von Knoten in Ihrem Cluster erhöhen, um mehr Parallelität zu erzielen.

* **Drosselungsfehler**: Es können Drosselungsfehler auftreten, wenn die Parallelität zu hoch ist. Falls es zu Drosselungsfehlern kommt, sollten Sie entweder die Parallelität reduzieren oder mit uns Kontakt aufnehmen.

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)


