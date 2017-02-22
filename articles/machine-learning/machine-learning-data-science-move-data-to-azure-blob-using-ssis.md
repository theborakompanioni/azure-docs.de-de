---
title: Verschieben von Daten in und aus Azure Blob Storage mithilfe von SSIS-Connectors | Microsoft Docs
description: Verschieben Sie Daten in und aus Azure Blob Storage mithilfe von SSIS-Connectors.
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 96a1b5fb-34d1-4b9b-8d99-2bb8289e0398
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 179cac8856cd42cfad4a4d4aaa6301afb2c58329
ms.openlocfilehash: 6d802148cec5b12004d1d374485f56af8eae64f8


---
# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Verschieben Sie Daten in und aus Azure Blob Storage mithilfe von SSIS-Connectors.
Das [SQL Server Integration Services Feature Pack für Azure](https://msdn.microsoft.com/library/mt146770.aspx) bietet Komponenten zum Herstellen einer Verbindung mit Azure, Übertragen von Daten zwischen Azure und lokalen Datenquellen und Verarbeiten von in Azure gespeicherten Daten.

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

Nachdem Kunden lokale Daten in die Cloud verschoben haben, können sie über sämtliche Azure-Dienste darauf zugreifen, um die ganze Leistungsfähigkeit der Azure-Technologiepalette zu nutzen. Die Daten können beispielsweise in Azure Machine Learning oder einem HDInsight-Cluster verwendet werden.

Dies ist normalerweise der erste Schritt in exemplarischen Vorgehensweisen für [SQL](machine-learning-data-science-process-sql-walkthrough.md) und [HDInsight](machine-learning-data-science-process-hive-walkthrough.md).

Eine Beschreibung gängiger Szenarien, in denen SQL Server Integration Services (SSIS) zum Erfüllen geschäftlicher Anforderungen in hybriden Datenintegrationsszenarien zum Einsatz kommt, finden Sie im Blog [Doing more with SQL Server Integration Services Feature Pack for Azure](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) .

> [!NOTE]
> Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der in diesem Artikel beschriebenen Aufgaben benötigen Sie ein eingerichtetes Azure-Abonnement und Azure-Speicherkonto. Um Daten hoch- und herunterladen zu können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen.

* Informationen zum Einrichten eines **Azure-Abonnements**finden Sie unter [Kostenlose 1-Monat-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Anleitungen zum Erstellen eines **Speicherkontos** und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

Zum Verwenden der **SSIS-Connectors**müssen Sie Folgendes herunterladen:

* **SQL Server 2014 oder 2016 Standard (oder höher)**: Die Installation umfasst SQL Server Integration Services.
* **Microsoft SQL Server 2014 oder 2016 Integration Services Feature Pack für Azure**: Diese Pakete können auf den Seiten [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) und [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) heruntergeladen werden.

> [!NOTE]
> SSIS wird mit SQL Server installiert, ist aber nicht in der Express-Version enthalten. Informationen dazu, welche Anwendungen in verschiedenen Editionen von SQL Server enthalten sind, finden Sie unter [SQL Server-Editionen](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/).
> 
> 

Schulungsmaterialien für SSIS finden Sie unter [Praktische Schulung für SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Informationen zur Inbetriebnahme von SSIS zum Erstellen von ETL-Paketen (Extrahieren, Transformieren, Laden) finden Sie im [SSIS-Tutorial: Erstellen eines einfachen ETL-Pakets](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Herunterladen des Datasets „NYC Taxi Trips“
Das hier beschriebene Beispiel verwendet ein öffentlich verfügbares Dataset mit dem Namen [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/). Das Dataset besteht aus ca. 173 Millionen Taxifahrten in New York im Jahr 2013. Es gibt zwei Arten von Daten: Daten zu Fahrten und Daten zum Fahrpreis. Für jeden Monat gibt es jeweils eine Datei, sodass wir insgesamt 24 Dateien haben, die unkomprimiert jeweils 2 GB groß sind.

## <a name="upload-data-to-azure-blob-storage"></a>Hochladen von Daten in Azure-Blobspeicher
Um Daten mithilfe des SSIS Feature Packs aus lokalem Speicher in Azure-Blobspeicher zu verschieben, verwenden wir eine Instanz der nachstehend gezeigten [**Aufgabe zum Hochladen von Azure-Blobs**](https://msdn.microsoft.com/library/mt146776.aspx):

![configure-data-science-vm](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)

Die Parameter dieser Aufgabe werden nachfolgend beschrieben:

| Feld | Beschreibung |
| --- | --- |
| **AzureStorageConnection** |Gibt einen vorhandenen Azure Storage-Verbindungs-Manager an oder erstellt einen neuen, der auf ein Azure-Speicherkonto verweist, das dorthin zeigt, wo die Blobdateien gehostet werden. |
| **BlobContainer** |Gibt den Namen des Blobcontainers an, der die hochgeladenen Dateien als Blobs enthält. |
| **BlobDirectory** |Gibt das Blobverzeichnis an, in dem die hochgeladene Datei als Blockblob gespeichert wird. Das Blobverzeichnis ist eine virtuelle hierarchische Struktur. Wenn das Blob bereits vorhanden ist, wird es ersetzt. |
| **LocalDirectory** |Gibt das lokale Verzeichnis mit den hochzuladenden Dateien an. |
| **FileName** |Gibt einen Namensfilter an, um Dateien mit dem angegebenen Namensmuster auszuwählen. Beispielsweise enthält „MySheet\*.xls\*“ Dateien wie „MySheet001.xls“ und „MySheetABC.xlsx“. |
| **TimeRangeFrom/TimeRangeTo** |Gibt einen Zeitbereichsfilter an. Nach *TimeRangeFrom* und vor *TimeRangeTo* geänderte Dateien werden eingeschlossen. |

> [!NOTE]
> Die Anmeldeinformationen für **AzureStorageConnection** müssen ordnungsgemäß sein, und der **BlobContainer** muss vorhanden sein, bevor die Übertragung versucht wird.
> 
> 

## <a name="download-data-from-azure-blob-storage"></a>Herunterladen von Daten aus Azure Blob Storage
Um Daten mit SSIS aus Azure-Blobspeicher in den lokalen Speicher herunterzuladen, verwenden Sie eine Instanz der [Aufgabe zum Hochladen von Azure-Blobs](https://msdn.microsoft.com/library/mt146779.aspx).

## <a name="more-advanced-ssis-azure-scenarios"></a>Erweiterte Azure/SSIS-Szenarien
Das SSIS Feature Pack ermöglicht komplexere zu verarbeitende Abläufe, indem diese Aufgaben in einem Paket zusammengeführt werden. Beispielsweise können die Blobdaten direkt in einen HDInsight-Cluster übertragen werden, dessen Ausgabe zunächst zurück in ein Blob und anschließend in lokalen Speicher heruntergeladen werden kann. SSIS kann mithilfe eines zusätzlichen SSIS-Connectors Hive- und Pig-Aufträge auf einem HDInsight-Cluster anwenden:

* Verwenden Sie zum Anwenden eines Hive-Skripts auf einen Azure HDInsight-Cluster mit SSIS die [Hive-Aufgabe in Azure HDInsight](https://msdn.microsoft.com/library/mt146771.aspx).
* Verwenden Sie zum Anwenden eines Pig-Skripts auf einen Azure HDInsight-Cluster mit SSIS die [Pig-Aufgabe in Azure HDInsight](https://msdn.microsoft.com/library/mt146781.aspx).




<!--HONumber=Dec16_HO3-->


