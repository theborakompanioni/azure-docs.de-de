---
title: Kopieren von Daten aus Azure Storage-Blobs in Data Lake Store | Microsoft Docs
description: Verwenden des AdlCopy-Tools zum Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/02/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 4c0b60afdc95a44dc5fdb0e43605e8bb079278e5
ms.openlocfilehash: 9f8635cd028d7d0d6a69faf6c2dc1de05dc5bb36


---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher
> [!div class="op_single_selector"]
> * [Verwenden von DistCp](data-lake-store-copy-data-wasb-distcp.md)
> * [Verwenden von AdlCopy](data-lake-store-copy-data-azure-storage-blob.md)
> 
> 

Azure Data Lake Store bietet das Befehlszeilentool [AdlCopy](http://aka.ms/downloadadlcopy), mit dem Sie Daten aus den folgenden Quellen kopieren können:

* Aus Azure Storage-Blobs in Data Lake Store. Sie können AdlCopy nicht verwenden, um Daten aus dem Data Lake-Speicher in Azure Storage-Blobs zu kopieren.
* Von einem Azure Data Lake Store-Konto in ein anderes Azure Data Lake Store-Konto. 

Sie können das AdlCopy-Tool in zwei verschiedenen Modi:

* **Eigenständig**, wobei das Tool Ressourcen des Data Lake-Speichers verwendet, um die Aufgabe auszuführen.
* **Mit einem Data Lake Analytics-Konto**, wobei die Einheiten, die Ihrem Data Lake Analytics-Konto zugewiesen sind, zum Ausführen des Kopiervorgangs verwendet werden. Möglicherweise möchten diese Option verwenden, wenn Sie die Kopie in einer vorhersagbaren Weise ausführen möchten.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Ein Azure-Abonnement**. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/pricing/free-trial/)(in englischer Sprache).
* **Azure Storage-Blobscontainer** mit einigen Daten.
* **Ein Azure Data Lake-Speicherkonto**. Eine Anleitung zur Erstellung finden Sie unter [Erste Schritte mit dem Azure Data Lake-Speicher](data-lake-store-get-started-portal.md)
* **Azure Data Lake Analytics-Konto (optional)** – Anweisungen zum Erstellen eines Data Lake-Speicherkontos finden Sie unter [Erste Schritte mit Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) .
* **AdlCopy-Tool**. Installieren Sie das AdlCopy-Tool von [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Syntax des AdlCopy-Tools
Verwenden Sie die folgende Syntax, um mit dem AdlCopy-Tool zu arbeiten.

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

Die Parameter in der Syntax werden nachfolgend beschrieben:

| Option | Beschreibung |
| --- | --- |
| Quelle |Gibt den Speicherort der Quelldaten im Azure-Speicherblob an. Bei der Quelle kann es sich um einen Blobcontainer, ein Blob oder ein anderes Data Lake Store-Konto handeln. |
| Dest |Gibt das Data Lake-Speicherziel für das Kopieren an. |
| SourceKey |Gibt den Speicherzugriffsschlüssel für die Azure-Speicherblobquelle an. Dies ist nur erforderlich, wenn die Quelle ein Blobcontainer oder ein Blob ist. |
| Konto |**Optional**. Verwenden Sie diese Option, wenn Sie das Azure Data Lake Analytics-Konto verwenden möchten, um den Kopierauftrag auszuführen. Wenn Sie die Option „/Account“ in der Syntax verwenden, aber kein Data Lake Analytics-Konto angeben, verwendet „AdlCopy“ ein Standardkonto zum Ausführen des Auftrags. Wenn Sie diese Option verwenden, müssen Sie außerdem die Quelle (Azure Storage-Blob) und das Ziel (Azure Data Lake-Speicher) als Datenquellen für das Data Lake Analytics-Konto hinzufügen. |
| Units |Gibt die Anzahl der Data Lake Analytics-Einheiten an, die für den Kopierauftrag verwendet werden. Diese Option ist erforderlich, wenn Sie mithilfe der Option **/Account** das Data Lake Analytics-Konto angeben. |
| Muster |Gibt ein RegEx-Muster an, das festlegt, welche Blobs oder Dateien kopiert werden sollen. Beim Abgleich für AdlCopy muss die Groß-/Kleinschreibung übereinstimmen. Dies wird als Standardmuster verwendet, wenn zum Kopieren aller Elemente kein Muster angegeben wurde. Das Angeben mehrerer Dateimuster wird nicht unterstützt. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Verwenden von AdlCopy (als eigenständiges Tool) zum Kopieren von Daten aus einem Azure Storage-Blob
1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Verzeichnis, in dem „AdlCopy“ installiert ist, in der Regel `%HOMEPATH%\Documents\adlcopy`.
2. Führen Sie den folgenden Befehl zum Kopieren eines bestimmten Blobs aus dem Quellcontainer in einen Data Lake-Speicher aus:
   
        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>
   
    Beispiel:
   
        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] Die oben stehende Syntax gibt die Datei an, die in einen Ordner im Data Lake Store-Konto kopiert werden soll. Das AdlCopy-Tool erstellt einen Ordner, falls der angegebene Ordnername nicht vorhanden ist.

    Sie werden aufgefordert, die Anmeldeinformationen für das Azure-Abonnement einzugeben, dem Ihr Data Lake-Speicherkonto zugeordnet ist. Eine Ausgabe ähnlich der folgenden wird angezeigt.

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. Sie können auch alle Blobs aus einem Container mit folgendem Befehl in das Data Lake-Speicherkonto kopieren:
   
        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        
   
    Beispiel:
   
        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Beim Kopieren aus einem Azure Blob Storage-Konto wird der Kopierauftrag unter Umständen auf der Blob Storage-Seite gedrosselt. Dies beeinträchtigt die Leistung Ihres Kopierauftrags. Weitere Informationen zu den Einschränkungen von Azure Blob Storage finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md) unter „Speichergrenzwerte“.

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Verwenden von AdlCopy (als eigenständiges Tool) zum Kopieren von Daten aus einem anderen Data Lake Store-Konto
Sie können AdlCopy auch verwenden, um Daten von einem Data Lake Store-Konto in ein anderes Data Lake Store-Konto zu kopieren.

1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Verzeichnis, in dem „AdlCopy“ installiert ist, in der Regel `%HOMEPATH%\Documents\adlcopy`.
2. Führen Sie den folgenden Befehl aus, um eine bestimmte Datei aus einem Data Lake Store-Konto in ein anderes Data Lake Store-Konto zu kopieren.
   
        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/
   
    Beispiel:
   
        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/
   
   > [!NOTE]
   > Die oben stehende Syntax gibt die Datei an, die in einen Ordner im Data Lake Store-Zielkonto kopiert werden soll. Das AdlCopy-Tool erstellt einen Ordner, falls der angegebene Ordnername nicht vorhanden ist.
   > 
   > 
   
    Sie werden aufgefordert, die Anmeldeinformationen für das Azure-Abonnement einzugeben, dem Ihr Data Lake-Speicherkonto zugeordnet ist. Eine Ausgabe ähnlich der folgenden wird angezeigt.
   
        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. Der folgende Befehl kopiert alle Dateien aus einem bestimmten Ordner im Data Lake Store-Quellkonto in einen Ordner im Data Lake Store-Zielkonto.
   
        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Wenn Sie AdlCopy als eigenständiges Tool verwenden, wird die Kopie auf gemeinsam genutzten, von Azure verwalteten Ressourcen ausgeführt. Die Leistung in dieser Umgebung ist abhängig von der Systemlast und den verfügbaren Ressourcen. Dieser Modus eignet sich am besten für kleine Ad-hoc-Übertragungen. Wenn Sie AdlCopy als eigenständiges Tool verwenden, müssen keine Parameter optimiert werden.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Verwenden von AdlCopy (mit einem Data Lake Analytics-Konto) zum Kopieren von Daten
Sie können mit Ihrem Data Lake Analytics-Konto auch den AdlCopy-Auftrag zum Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher ausführen. In der Regel verwenden Sie diese Option, wenn die zu verschiebenden Daten im GB- und TB-Bereich liegen, und Sie einen besseren und vorhersagbareren Leistungsdurchsatz wünschen.

Um Ihr Data Lake Analytics-Konto mit AdlCopy zum Kopieren von Daten aus einem Azure Storage-Blob zu verwenden, müssen Sie die Quelle (Azure Storage-Blob) als Datenquelle für Ihr Data Lake Analytics-Konto hinzufügen. Informationen zum Hinzufügen von zusätzlichen Datenquellen zu Ihrem Data Lake Analytics-Konto finden Sie unter [Verwalten der Datenquellen des Data Lake Analytics-Kontos](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

> [!NOTE]
> Wenn Sie Daten unter Verwendung eines Data Lake Analytics-Konto aus einem Azure Data Lake Store-Quellkonto kopieren, müssen Sie das Data Lake Store-Konto nicht dem Data Lake Analytics-Konto zuordnen. Die Anforderung, den Quellspeicher dem Data Lake Analytics-Konto zuzuordnen, gilt nur, wenn es sich bei der Quelle um ein Azure Storage-Konto handelt.
> 
> 

Führen Sie den folgenden Befehl aus, um Daten aus einem Azure Storage-Blob unter Verwendung eines Data Lake Analytics-Kontos in ein Data Lake Store-Konto zu kopieren:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Beispiel:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Führen Sie auf gleiche Weise den folgenden Befehl aus, um Daten aus einem Azure Storage-Blob unter Verwendung eines Data Lake Analytics-Kontos in ein Data Lake Store-Konto zu kopieren:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Überlegungen zur Leistung

Wenn Sie Datenmengen im Terabytebereich kopieren und AdlCopy mit Ihrem eigenen Azure Data Lake Analytics-Konto verwenden, erhalten Sie eine höhere und besser vorhersagbare Leistung. Bei den Parametern empfiehlt sich die Optimierung der Anzahl zu verwendender Azure Data Lake Analytics-Einheiten für den Kopierauftrag. Wenn Sie die Einheitenanzahl erhöhen, erhöht sich auch die Leistung Ihres Kopierauftrags. Jede zu kopierende Datei kann maximal eine Einheit verwenden. Wenn die Anzahl von Einheiten die Anzahl zu kopierender Dateien übersteigt, erhöht sich die Leistung nicht weiter.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Verwenden von AdlCopy zum Kopieren von Daten mithilfe des Musterabgleichs
In diesem Abschnitt erfahren Sie, wie Sie AdlCopy verwenden, um Daten mithilfe des Musterabgleichs aus einem Data Lake Store-Quellkonto (im Beispiel unten wird ein Azure Storage-Blob verwendet) in ein Data Lake Store-Zielkonto zu kopieren. Sie können diese Schritte z.B. verwenden, um alle Dateien mit der Erweiterung „CSV“ aus dem Quellblob in das Ziel zu kopieren.

1. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Verzeichnis, in dem „AdlCopy“ installiert ist, in der Regel `%HOMEPATH%\Documents\adlcopy`.
2. Führen Sie den folgenden Befehl aus, um alle Dateien mit der Erweiterung „CSV“ aus einem bestimmten Blob des Quellcontainers in einen Data Lake Store zu kopieren:
   
        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv
   
    Beispiel:
   
        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Abrechnung
* Bei Verwendung des AdlCopy-Tools als eigenständiges Programm werden Ihnen Ausgangskosten für das Verschieben von Daten berechnet, wenn das Azure Storage-Quellkonto nicht in der gleichen Region wie der Data Lake-Speicher liegt.
* Bei Verwendung des AdlCopy-Tools mit Ihrem Data Lake Analytics-Konto gelten standardmäßige [Data Lake Analytics-Abrechnungsraten](https://azure.microsoft.com/pricing/details/data-lake-analytics/) .

## <a name="considerations-for-using-adlcopy"></a>Überlegungen zur Verwendung von AdlCopy
* AdlCopy (für Version 1.0.5) unterstützt das Kopieren von Daten aus Quellen, die zusammen mehrere Tausend Dateien und Ordner aufweisen. Wenn beim Kopieren eines umfangreichen Datasets jedoch Probleme auftreten, können Sie die Dateien und Ordner in verschiedene Unterordner verteilen und den Pfad zu diesen Unterordnern als Quelle angeben.

## <a name="performance-considerations-for-using-adlcopy"></a>Überlegungen zur Leistung bei Verwendung von AdlCopy

AdlCopy unterstützt das Kopieren von Daten mit Tausenden von Dateien und Ordnern. Sollten beim Kopieren eines großen Datasets jedoch Probleme auftreten, können Sie die Dateien/Ordner auf kleinere Unterordner verteilen. AdlCopy wurde für Ad-hoc-Kopien konzipiert. Wenn Sie wiederholt Daten kopieren möchten, sollten Sie die Verwendung von [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) in Betracht ziehen, da diese Lösung eine umfassende Verwaltung rund um Kopiervorgänge bietet.

## <a name="next-steps"></a>Nächste Schritte
* [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)
* [Verwenden von Azure Data Lake Analytics mit Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Verwenden von Azure HDInsight mit Data Lake-Speicher](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Dec16_HO1-->


