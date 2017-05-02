---
title: Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure CLI 2.0 | Microsoft-Dokumentation
description: 'Erfahren Sie, wie Sie die Azure-Befehlszeilenschnittstelle 2.0 (Azure CLI 2.0) zum Erstellen eines Data Lake Analytics-Kontos, zum Erstellen eines Data Lake Analytics-Auftrags mit U-SQL und zum Senden des Auftrags verwenden. '
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/06/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 109460cecc4e11c729203af97c9bf1c22b90e61a
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-cli-20-preview"></a>Tutorial: Erste Schritte mit Azure Data Lake Analytics mithilfe von Azure CLI 2.0 (Vorschau)
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Enthält Informationen zur Verwendung von Azure CLI 2.0 zum Erstellen von Azure Data Lake Analytics-Konten, zum Definieren von Data Lake Analytics-Aufträgen in [U-SQL](data-lake-analytics-u-sql-get-started.md) und zum Übermitteln von Aufträgen an Data Lake Analytics-Konten. Weitere Informationen zu Data Lake Analytics finden Sie unter [Übersicht über Azure Data Lake Analytics](data-lake-analytics-overview.md).

In diesem Tutorial entwickeln Sie einen Auftrag, bei dem eine Datei mit tabulatorgetrennten Werten (TSV) gelesen und in eine Datei mit kommagetrennten Werten (CSV) konvertiert wird. Um das gleiche Tutorial unter Verwendung anderer unterstützter Tools durchzuarbeiten, können Sie die Dropdownliste oben in diesem Abschnitt nutzen.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Azure CLI 2.0**. Weitere Informationen finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli).
* **Aktivierung von Data Lake Store/Analytics CLI 2.0 (Vorschau)**. Data Lake Store und Data Lake Analytics CLI 2.0 befinden sich noch in der Vorschauphase. Führen Sie die folgenden Befehle aus, um beides zu aktivieren:

    ```azurecli
    az component update --add dls
    az component update --add dla 
    ```

## <a name="log-in-to-azure"></a>Anmelden an Azure

Gehen Sie wie folgt vor, um sich an Ihrem Azure-Abonnement anzumelden:

```azurecli
az login
```

Sie werden aufgefordert, zu einer URL zu navigieren und einen Authentifizierungscode einzugeben.  Befolgen Sie anschließend die Anleitung zum Eingeben Ihrer Anmeldeinformationen.

Nachdem Sie sich angemeldet haben, werden mit dem Anmeldebefehl Ihre Abonnements aufgeführt.

Gehen Sie wie folgt vor, um ein bestimmtes Abonnement zu verwenden:

```azurecli
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a>Erstellen eines Data Lake Analytics-Kontos
Zum Ausführen von Aufträgen ist ein Data Lake Analytics-Konto erforderlich. Zum Erstellen eines Data Lake Analytics-Kontos müssen Sie die folgenden Elemente angeben:

* **Azure-Ressourcengruppe**: Es muss ein Data Lake Analytics-Konto in einer Azure-Ressourcengruppe erstellt werden. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) ermöglicht es Ihnen, mit den Ressourcen in Ihrer Anwendung als Gruppe zu arbeiten. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren oder löschen.  
  
    Gehen Sie wie folgt vor, um die vorhandenen Ressourcengruppen unter Ihrem Abonnement aufzulisten:
  
    ```azurecli
    az group list 
    ```

    So erstellen Sie eine neue Ressourcengruppe:
  
    ```azurecli
    az group create --name "<Resource Group Name>" --location "<Azure Location>"
    ```

* **Name des Data Lake Analytics-Kontos**: Jedes Data Lake Analytics-Konto hat einen Namen.
* **Standort**: Verwenden Sie eines der Azure-Rechenzentren, die Data Lake Analytics unterstützen.
* **Data Lake Store-Standardkonto**: Jedes Data Lake Analytics-Konto verfügt über ein Data Lake Store-Standardkonto.
  
    Gehen Sie wie folgt vor, um das vorhandene Data Lake Store-Konto anzuzeigen:

    ```azurecli
    az dls account list
    ```
  
    Gehen Sie wie folgt vor, um ein neues Data Lake Store-Konto zu erstellen:
  
    ```azurecli
    az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
    ```

Verwenden Sie die folgende Syntax, um ein Data Lake Analytics-Konto zu erstellen:

```azurecli
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

Nach dem Erstellen eines Kontos können Sie die folgenden Befehle verwenden, um die Konten aufzulisten und die Kontodetails anzuzeigen:

```azurecli
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a>Hochladen von Daten in den Data Lake-Speicher
In diesem Tutorial verarbeiten Sie einige Suchprotokolle.  Das Suchprotokoll kann entweder in einem Data Lake-Speicher oder einem Azure-BLOB-Speicher gespeichert werden. 

Das Azure-Portal enthält eine Benutzeroberfläche zum Kopieren einiger Beispieldatendateien in das Data Lake Store-Standardkonto. Hierzu gehört auch eine Suchprotokolldatei. Weitere Informationen zum Hochladen von Daten in das Data Lake-Standardspeicherkonto finden Sie unter [Vorbereiten von Quelldaten](data-lake-analytics-get-started-portal.md#prepare-source-data).

Zum Hochladen von Dateien per CLI 2.0 verwenden Sie die folgenden Befehle:

```azurecli
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

Data Lake Analytics hat auch Zugriff auf den Azure-Blob-Speicher.  Informationen zum Hochladen von Daten nach Azure Blob Storage finden Sie unter [Verwenden der Azure-CLI mit Azure Storage](../storage/storage-azure-cli.md).

## <a name="submit-data-lake-analytics-jobs"></a>Übermitteln von Data Lake Analytics-Aufträgen
Die Data Lake Analytics-Aufträge werden in der Sprache U-SQL geschrieben. Weitere Informationen zu U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL](data-lake-analytics-u-sql-get-started.md) und in der [U-SQL language reference](http://go.microsoft.com/fwlink/?LinkId=691348) (in englischer Sprache).

**So erstellen Sie ein Skript für Data Lake Analytics-Aufträge**

Erstellen Sie mit dem folgenden U-SQL-Skript eine Textdatei, und speichern Sie die Textdatei auf der Arbeitsstation:
  
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/Output/SearchLog-from-Data-Lake.csv"
    USING Outputters.Csv();
  
Mit diesem U-SQL-Skript wird die Quelldatei mithilfe von **Extractors.Tsv()** gelesen, und anschließend wird eine CSV-Datei mithilfe von **Outputters.Csv()** erstellt. 

Ändern Sie die beiden Pfade nur, wenn Sie die Quelldatei an einen anderen Speicherort kopieren.  Data Lake Analytics erstellt den Ausgabeordner, falls er nicht vorhanden ist.

Es ist einfacher, für Dateien, die unter Data Lake Store-Standardkonten gespeichert sind, relative Pfade zu verwenden. Sie können aber auch absolute Pfade verwenden.  Beispiel:

    adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

Sie müssen absolute Pfade verwenden, um auf Dateien in verknüpften Speicherkonten zuzugreifen.  Die Syntax für Dateien, die unter dem verknüpften Azure-Speicherkonto gespeichert werden, lautet wie folgt:

    wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
  
  > [!NOTE]
  > Azure-BLOB-Container mit öffentlichen Blobs oder Zugriffsberechtigungen für öffentliche Container werden derzeit nicht unterstützt.      
  > 
  > 

**So übermitteln Sie Aufträge**

Verwenden Sie die folgende Syntax, um einen Auftrag zu übermitteln.

```azurecli
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

Beispiel:

```azurecli
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

**So listen Sie Aufträge auf und zeigen Auftragsdetails an**

```azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

**So brechen Sie Aufträge ab**

```azurecli
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

##<a name="retrieve-job-results"></a>Abrufen von Auftragsergebnissen

Nach Abschluss eines Auftrags können Sie die folgenden Befehle verwenden, um die Ausgabedateien aufzulisten und die Dateien herunterzuladen:

```azurecli
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" 
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

Beispiel:

```azurecli
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="next-steps"></a>Nächste Schritte

* Wenn Sie dasselbe Tutorial mit anderen Tools verwenden möchten, klicken Sie oben auf der Seite auf die Registerkartenauswahl.
* Ein Referenzdokument zu Data Lake Analytics CLI 2.0 finden Sie unter [Data Lake Analytics – az dla](https://docs.microsoft.com/cli/azure/dla).
* Ein Referenzdokument zu Data Lake Store CLI 2.0 finden Sie unter [Data Lake Store – az dls](https://docs.microsoft.com/cli/azure/dls).
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
* Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).


