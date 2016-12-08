---
title: "Erste Schritte mit Data Lake-Speicher mithilfe der plattformübergreifenden Befehlszeilenschnittstelle | Microsoft Docs"
description: "Verwenden Sie die plattformübergreifende Befehlszeile zum Erstellen eines Data Lake-Speicherkontos und Durchführen grundlegender Vorgänge."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 380788f3-041d-4ae5-b6be-37ca74ca333d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/21/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c157da7bf53e2d0762624e8e71e56e956db04a24
ms.openlocfilehash: 236563a8814640391130ba53886c5cebfea67a8f


---
# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Erste Schritte mit Azure Data Lake-Speicher unter Verwendung der Azure-Befehlszeile
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Hier erfahren Sie, wie Sie mit der Azure-Befehlszeilenschnittstelle (Azure-CLI) ein Azure Data Lake-Speicherkonto erstellen und grundlegende Vorgänge ausführen (also etwa Ordner erstellen, Datendateien hoch- und herunterladen, Ihr Konto löschen und Ähnliches). Weitere Informationen zu Data Lake Store finden Sie unter [Übersicht über Data Lake Store](data-lake-store-overview.md).

Die Azure-CLI ist in Node.js implementiert. Sie kann auf allen Plattformen verwendet werden, die Node.js unterstützen, inklusive Windows, Mac und Linux. Die Azure-Befehlszeilenschnittstelle ist Open Source. Der Quellcode wird auf GitHub unter <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a> verwaltet. In diesem Artikel wird lediglich die Verwendung der Azure-CLI mit dem Data Lake-Speicher behandelt. Eine allgemeine Anleitung zur Verwendung der Azure-CLI finden Sie unter [Verwenden der Azure-CLI][azure-command-line-tools].

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* **Azure-CLI** – Informationen zur Installation und Konfiguration finden Sie unter [Installieren und Konfigurieren der Azure-CLI](../xplat-cli-install.md) . Achten Sie darauf, Ihren Computer nach der Installation der Befehlszeilenschnittstelle neu zu starten.

## <a name="authentication"></a>Authentifizierung
In diesem Artikel wird ein einfacheres Authentifizierungskonzept mit Data Lake Store verwendet, bei dem Sie sich als Endbenutzer anmelden. Die Zugriffsebene für das Data Lake Store-Konto und das Dateisystem hängt dann von der Zugriffsebene des angemeldeten Benutzers ab. Für die Authentifizierung mit Data Lake Store stehen mit **Endbenutzerauthentifizierung** und **Dienst-zu-Dienst-Authentifizierung** aber auch noch andere Konzepte zur Verfügung. Anweisungen und weitere Informationen zur Authentifizierung finden Sie unter [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)(Authentifizieren bei Data Lake Store mithilfe von Azure Active Directory).

## <a name="login-to-your-azure-subscription"></a>Anmelden bei Ihrem Azure-Abonnement
1. Führen Sie die Schritte aus, die unter [Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure-Befehlszeilenschnittstelle (Azure-CLI)](../xplat-cli-connect.md) dokumentiert sind, und stellen Sie über die `azure login`-Methode eine Verbindung mit Ihrem Abonnement her.
2. Listen Sie mithilfe des `azure account list`-Befehls die mit Ihrem Konto verknüpften Abonnements auf.
   
        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false
   
    In der obigen Ausgabe ist **Azure-sub-1** aktiviert. Das andere Abonnement ist **Azure-sub-2**. 
3. Wählen Sie das Abonnement aus, mit dem Sie arbeiten möchten. Wenn Sie mit dem Abonnement „Azure-sub-2“ arbeiten möchten, verwenden Sie den Befehl `azure account set`.
   
        azure account set Azure-sub-2

## <a name="create-an-azure-data-lake-store-account"></a>Erstellen eines Azure Data Lake-Speicherkontos
Öffnen Sie eine Eingabeaufforderung, Shell oder Terminalsitzung, und führen Sie die folgenden Befehle aus:

1. Wechseln Sie mit dem folgenden Befehl in den Azure-Ressourcen-Manager-Modus:
   
        azure config mode arm
2. Erstellen Sie eine neue Ressourcengruppe. Geben Sie für den folgenden Befehl die Parameterwerte ein, die Sie verwenden möchten.
   
        azure group create <resourceGroup> <location>
   
    Wenn der Name des Standorts Leerzeichen enthält, setzen Sie ihn in doppelte Anführungszeichen. Beispiel: „USA (Ost) 2“.
3. Erstellen Sie das Data Lake-Speicherkonto.
   
        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Erstellen von Ordnern im Data Lake-Speicher
Sie können in Ihrem Azure Data Lake-Speicherkonto Ordner zum Verwalten und Speichern von Daten erstellen. Verwenden Sie den folgenden Befehl, um im Stammverzeichnis des Data Lake-Speichers den Ordner „mynewfolder“ zu erstellen.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Beispiel:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Hochladen von Daten in den Data Lake-Speicher
Sie können Ihre Daten direkt auf die Stammebene eines Data Lake-Speichers oder in einen im Konto erstellten Ordner hochladen. Die folgenden Codeausschnitte veranschaulichen das Hochladen von Beispieldaten in den im vorigen Abschnitt erstellten Ordner (**mynewfolder**).

Wenn Sie Beispieldaten zum Hochladen verwenden möchten, können Sie den Ordner **Ambulance Data** aus dem [Azure Data Lake-Git-Repository](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData)herunterladen. Laden Sie die Datei herunter, und speichern Sie sie in ein lokales Verzeichnis auf dem Computer, z. B. „C:\sampledata\.“.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Beispiel:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Auflisten von Dateien im Data Lake-Speicher
Verwenden Sie den folgenden Befehl, um die Dateien in einem Data Lake-Speicherkonto aufzulisten.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Beispiel:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Die Ausgabe sollte in etwa wie folgt aussehen:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Umbenennen, Herunterladen und Löschen von Daten im Data Lake-Speicher
* Verwenden Sie zum **Umbenennen einer Datei** den folgenden Befehl:
  
        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>
  
    Beispiel:
  
        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv
* **Verwenden Sie zum Herunterladen einer Datei** den folgenden Befehl: Stellen Sie sicher, dass der angegebene Zielpfad bereits vorhanden ist.
  
        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>
  
    Beispiel:
  
        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"
* Verwenden Sie zum **Löschen einer Datei** den folgenden Befehl:
  
        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>
  
    Beispiel:
  
        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv
  
    Geben Sie nach entsprechender Aufforderung **Y** ein, um das Element zu löschen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Anzeigen der Zugriffssteuerungsliste für einen Ordner im Data Lake-Speicher
Verwenden Sie den folgenden Befehl, um die Zugriffssteuerungslisten in einem Ordner des Data Lake-Speichers anzuzeigen. In der aktuellen Version können Zugriffssteuerungslisten nur im Stammverzeichnis des Data Lake-Speichers festgelegt werden. Für den path-Parameter im folgenden Codeabschnitt wird also immer das Stammverzeichnis (/) angegeben.

    azure datalake store permissions show <dataLakeStoreName> <path>

Beispiel:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Löschen Ihres Data Lake-Speicherkontos
Verwenden Sie zum Löschen eines Data Lake-Speicherkontos den folgenden Befehl.

    azure datalake store account delete <dataLakeStoreAccountName>

Beispiel:

    azure datalake store account delete mynewdatalakestore

Geben Sie nach entsprechender Aufforderung **Y** ein, um das Konto zu löschen.

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../xplat-cli-install.md



<!--HONumber=Nov16_HO4-->


