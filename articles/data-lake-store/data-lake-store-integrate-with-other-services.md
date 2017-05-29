---
title: Integrieren von Data Lake Store in andere Azure-Dienste | Microsoft Docs
description: Erfahren Sie, wie Sie Data Lake-Speicher in andere Azure-Dienste integrieren.
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 750540d0866e69ce043133e9c668c8a2e09b9d1c
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrieren von Data Lake-Speicher in andere Azure-Dienste
Azure Data Lake-Speicher kann zusammen mit anderen Azure-Diensten verwendet werden, um mehr Szenarios zu ermöglichen. Im folgende Artikel werden die Dienste aufgeführt, in die Data Lake-Speicher integriert werden kann.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Verwenden von Data Lake-Speicher mit Azure HDInsight
Sie können einen [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) -Cluster bereitstellen, der Data Lake-Speicher als HDFS-kompatiblen Speicher verwendet. In dieser Version können Sie für Hadoop- und Storm-Cluster unter Windows und Linux Data Lake-Speicher nur als Zusatzspeicher verwenden. Solche Cluster verwenden weiterhin Azure Storage (WASB) als Standardspeicher. Sie können jedoch für HBase-Cluster unter Windows und Linux Data Lake-Speicher als Standardspeicher oder als Zusatzspeicher verwenden.

Anleitungen zum Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher finden Sie unter:

* [Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Erstellen von HDInsight-Clustern mit Data Lake Store als Standardspeicher mithilfe von PowerShell](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [Erstellen eines HDInsight-Clusters mit Data Lake Store (als zusätzlichem Speicher) mithilfe von Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Verwenden von Data Lake-Speicher mit Azure Data Lake Analytics
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) ermöglicht das Arbeiten mit Big Data in der Cloud. Es stellt Ressourcen dynamisch bereit und ermöglicht Ihnen die Durchführung von Analysen von Terabytes oder sogar Exabytes an Daten, die in einer Reihe von unterstützten Datenquellen gespeichert sein können, auch in Data Lake-Speicher. Data Lake-Analytics ist speziell für die Arbeit mit Azure Data Lake-Speicher optimiert – so erzielen Sie Höchstwerte bei Leistung, Durchsatz und Parallelisierung bei Big Data-Workloads.

Anleitung zum Verwenden von Data Lake Analytics mit Data Lake-Speicher finden Sie unter [Erste Schritte mit Data Lake Analytics unter Verwendung von Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Verwenden von Data Lake-Speicher mit Azure Data Factory
Sie können [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) zum Erfassen von Daten aus Azure-Tabellen, Azure SQL-Datenbank, Azure SQL Data Warehouse, Azure Storage-Blobs und lokalen Datenbanken verwenden. Als wichtige Komponente im Azure-Ökosystem kann Azure Data Factory auch zum Orchestrieren der Erfassung von Daten aus diesen Quelle in Azure Data Lake-Speicher verwendet werden.

Anleitungen zum Verwenden von Azure Data Factory mit Data Lake-Speicher finden Sie unter [Verschieben von Daten in und aus Data Lake-Speicher mit Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher
Azure Data Lake-Speicher bietet ein Befehlszeilentool namens AdlCopy, mit dem Sie Daten aus Azure Blob Storage in ein Data Lake-Speicherkonto kopieren können. Weitere Informationen finden Sie unter [Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopieren von Daten zwischen Azure SQL-Datenbank und Data Lake-Speicher
Sie können Apache Sqoop verwenden, um Daten zwischen Azure SQL-Datenbank und Data Lake-Speicher zu importieren und zu exportieren. Weitere Informationen finden Sie unter [Kopieren von Daten zwischen Data Lake-Speicher und Azure SQL-Datenbank mithilfe von Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Verwenden des Data Lake-Speichers mit Stream Analytics
Sie können den Data Lake-Speicher als eine der Ausgaben zum Speichern von Daten verwenden, die mit Azure Stream Analytics gestreamt werden. Weitere Informationen finden Sie unter [Streamen von Daten aus Azure Storage-Blob in Data Lake-Speicher mit Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Verwenden des Data Lake-Speichers mit Power BI
Sie können Power BI zum Importieren von Daten aus einem Data Lake-Speicherkonto verwenden, um die Daten zu analysieren und zu visualisieren. Weitere Informationen finden Sie unter [Analysieren von Daten im Data Lake-Speicher mithilfe von Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Verwenden des Data Lake-Speichers mit Data Catalog
Sie können Daten aus dem Data Lake-Speicher in Azure Data Catalog registrieren, damit die Daten in der gesamten Organisation auffindbar sind. Weitere Informationen finden Sie unter [Registrieren von Daten aus dem Data Lake-Speicher in Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Verwenden von Data Lake Store mit SQL Server Integration Services (SSIS)
Sie können den Azure Data Lake Store-Verbindungs-Manager in SSIS verwenden, um für ein SSIS-Paket eine Verbindung mit Azure Data Lake Store herzustellen. Weitere Informationen finden Sie unter [Azure Data Lake Store Connection Manager](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager) (Azure Data Lake Store-Verbindungs-Manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Verwenden von Data Lake Store mit SQL Data Warehouse
Sie können PolyBase verwenden, um Daten aus Azure Data Lake Store in SQL Data Warehouse zu laden. Weitere Informationen finden Sie unter [Laden von Daten aus Azure Data Lake Store in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Weitere Informationen
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe des Portals](data-lake-store-get-started-portal.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe von PowerShell](data-lake-store-get-started-powershell.md)  


