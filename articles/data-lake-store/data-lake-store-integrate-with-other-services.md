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
ms.date: 10/28/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ce6a611db7be7f1ae5f5027a77727e54e5e538ae


---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrieren von Data Lake-Speicher in andere Azure-Dienste
Azure Data Lake-Speicher kann zusammen mit anderen Azure-Diensten verwendet werden, um mehr Szenarios zu ermöglichen. Im folgende Artikel werden die Dienste aufgeführt, in die Data Lake-Speicher integriert werden kann.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Verwenden von Data Lake-Speicher mit Azure HDInsight
Sie können einen [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) -Cluster bereitstellen, der Data Lake-Speicher als HDFS-kompatiblen Speicher verwendet. In dieser Version können Sie für Hadoop- und Storm-Cluster unter Windows und Linux Data Lake-Speicher nur als Zusatzspeicher verwenden. Solche Cluster verwenden weiterhin Azure Storage (WASB) als Standardspeicher. Sie können jedoch für HBase-Cluster unter Windows und Linux Data Lake-Speicher als Standardspeicher oder als Zusatzspeicher verwenden.

Anleitungen zum Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher finden Sie unter:

* [Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Bereitstellen eines HDInsight-Clusters mit Data Lake-Speicher mithilfe von Azure PowerShell](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Bevorzugen Sie Videos?**  Folgen Sie den unten aufgeführten Links, um Videos mit Anleitungen zur Verwendung des Data Lake-Speichers mit HDInsight-Clustern anzusehen.

* [Erstellen eines HDInsight-Clusters mit Zugriff auf den Data Lake-Speicher](https://mix.office.com/watch/l93xri2yhtp2)
*  [Zugreifen auf Daten im Data Lake-Speicher mit Hive- und Pig-Skripts](https://mix.office.com/watch/1n9g5w0fiqv1q)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Verwenden von Data Lake-Speicher mit Azure Data Lake Analytics
[Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) ermöglicht das Arbeiten mit Big Data in der Cloud. Es stellt Ressourcen dynamisch bereit und ermöglicht Ihnen die Durchführung von Analysen von Terabytes oder sogar Exabytes an Daten, die in einer Reihe von unterstützten Datenquellen gespeichert sein können, auch in Data Lake-Speicher. Data Lake-Analytics ist speziell für die Arbeit mit Azure Data Lake-Speicher optimiert – so erzielen Sie Höchstwerte bei Leistung, Durchsatz und Parallelisierung bei Big Data-Workloads.

Anleitung zum Verwenden von Data Lake Analytics mit Data Lake-Speicher finden Sie unter [Erste Schritte mit Data Lake Analytics unter Verwendung von Data Lake-Speicher](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Bevorzugen Sie Videos?**  Folgen Sie den unten aufgeführten Links, um Videos mit Anleitungen zur Verwendung des Data Lake-Speichers mit HDInsight-Clustern anzusehen.

* [Verbinden von Azure Data Lake Analytics mit dem Azure Data Lake-Speicher](https://mix.office.com/watch/qwji0dc9rx9k)
* [Zugreifen auf den Azure Data Lake-Speicher mit Azure Data Lake Analytics](https://mix.office.com/watch/1n0s45up381a8)

## <a name="use-data-lake-store-with-azure-data-factory"></a>Verwenden von Data Lake-Speicher mit Azure Data Factory
Sie können [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) zum Erfassen von Daten aus Azure-Tabellen, Azure SQL-Datenbank, Azure SQL Data Warehouse, Azure Storage-Blobs und lokalen Datenbanken verwenden. Als wichtige Komponente im Azure-Ökosystem kann Azure Data Factory auch zum Orchestrieren der Erfassung von Daten aus diesen Quelle in Azure Data Lake-Speicher verwendet werden.

Anleitungen zum Verwenden von Azure Data Factory mit Data Lake-Speicher finden Sie unter [Verschieben von Daten in und aus Data Lake-Speicher mit Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

**Verfügbare Videos:** Siehe [Datenorchestrierung mithilfe von Azure Data Factory für den Azure Data Lake-Speicher](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher
Azure Data Lake-Speicher bietet ein Befehlszeilentool namens AdlCopy, mit dem Sie Daten aus Azure Blob Storage in ein Data Lake-Speicherkonto kopieren können. Weitere Informationen finden Sie unter [Kopieren von Daten aus Azure Storage-Blobs in den Data Lake-Speicher](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Kopieren von Daten zwischen Azure SQL-Datenbank und Data Lake-Speicher
Sie können Apache Sqoop verwenden, um Daten zwischen Azure SQL-Datenbank und Data Lake-Speicher zu importieren und zu exportieren. Weitere Informationen finden Sie unter [Kopieren von Daten zwischen Data Lake-Speicher und Azure SQL-Datenbank mithilfe von Sqoop](data-lake-store-data-transfer-sql-sqoop.md).

**Sehen Sie sich dieses Video** über das [Verwenden von Apache Sqoop zum Verschieben von Daten zwischen relationalen Quellen und dem Azure Data Lake-Speicher](https://mix.office.com/watch/1butcdjxmu114)an.

## <a name="use-data-lake-store-with-stream-analytics"></a>Verwenden des Data Lake-Speichers mit Stream Analytics
Sie können den Data Lake-Speicher als eine der Ausgaben zum Speichern von Daten verwenden, die mit Azure Stream Analytics gestreamt werden. Weitere Informationen finden Sie unter [Streamen von Daten aus Azure Storage-Blob in Data Lake-Speicher mit Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Verwenden des Data Lake-Speichers mit Power BI
Sie können Power BI zum Importieren von Daten aus einem Data Lake-Speicherkonto verwenden, um die Daten zu analysieren und zu visualisieren. Weitere Informationen finden Sie unter [Analysieren von Daten im Data Lake-Speicher mithilfe von Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Verwenden des Data Lake-Speichers mit Data Catalog
Sie können Daten aus dem Data Lake-Speicher in Azure Data Catalog registrieren, damit die Daten in der gesamten Organisation auffindbar sind. Weitere Informationen finden Sie unter [Registrieren von Daten aus dem Data Lake-Speicher in Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="see-also"></a>Siehe auch
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe des Portals](data-lake-store-get-started-portal.md)
* [Erste Schritte mit Data Lake-Speicher mithilfe von PowerShell](data-lake-store-get-started-powershell.md)  




<!--HONumber=Nov16_HO3-->


