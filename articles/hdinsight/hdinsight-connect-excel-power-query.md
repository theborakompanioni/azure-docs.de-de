---
title: "Verbinden von Excel mit Hadoop mithilfe von Power Query – Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Business Intelligence-Komponenten nutzen und wie Sie mithilfe von Power Query für Excel auf Daten in Hadoop in HDInsight zugreifen können."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 01ad2f90-7520-44d9-8c16-4d936faaff9b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 6a15efb091b4faaa7305bb8faa362c62fac595cb
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="connect-excel-to-hadoop-by-using-power-query"></a>Verbinden von Excel mit Hadoop mithilfe von Power Query
Eine der wichtigsten Features der Big Data-Lösung von Microsoft ist die Integration von Microsoft Business Intelligence (BI)-Komponenten in Hadoop-Cluster in Azure HDInsight. Ein gutes Beispiel ist die Möglichkeit, Excel mithilfe von Microsoft Power Query für Excel-Add-Ins mit dem Azure Storage-Konto zu verbinden, das die dem Hadoop-Cluster zugeordneten Daten enthält. Dieser Artikel beschreibt die Einrichtung und Verwendung von Power Query für die Abfrage von Daten aus einem mit HDInsight verwalteten Hadoop-Cluster.

### <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

* **Einen HDInsight-Cluster**. Hinweise zu dessen Konfiguration finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
* **Eine Arbeitsstation** mit Windows 7, Windows Server 2008 R2 oder einem späteren Betriebssystem.
* **Office 2016, Office 2013 Professional Plus, Office 365 ProPlus, Excel 2013 Standalone oder Office 2010 Professional Plus**.

## <a name="install-power-query"></a>Installieren von Power Query
Power Query kann Daten importieren, die von einem Hadoop-Auftrag, der in einem HDInsight-Cluster ausgeführt wird, ausgegeben bzw. generiert wurden.

Power Query wurde in Excel 2016 in das Menüband „Daten“ im Abschnitt zum Abrufen und Transformieren integriert. Laden Sie bei älteren Excel-Versionen Microsoft Power Query für Excel aus dem [Microsoft Download Center][powerquery-download] herunter, und installieren Sie es.

## <a name="import-hdinsight-data-into-excel"></a>Importieren von HDInsight-Daten in Excel
Das Power Query für Excel-Add-In erleichtert den Import von Daten aus Ihrem HDInsight-Cluster in Excel. Business Intelligence-Tools wie z. B. PowerPivot und Power Map dienen zur Inspektion, Analyse und Darstellung der Daten.

**So importieren Sie Daten aus einem HDInsight-Cluster**

1. Öffnen Sie Excel.
2. Erstellen Sie eine neue leere Arbeitsmappe.
3. Führen Sie je nach Excel-Version die folgenden Schritte aus:

    - Excel 2016

        - Klicken Sie auf das Menü **Daten**, anschließend auf dem Menüband **Daten abrufen und transformieren** auf **Daten abrufen**, dann auf **Aus Azure** und schließlich auf **Aus Azure HDInsight(HDFS)**.

        ![HDI.PowerQuery.SelectHdiSource](./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.excel2016.png)

    - Excel 2013/2010

        - Klicken Sie auf das Menü **Power Query**, klicken Sie auf **Von Azure**, und klicken Sie dann auf **Von Microsoft Azure HDInsight**.
   
        ![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]
       
        **Hinweis**: Falls das **Power Query**-Menü nicht angezeigt wird, öffnen Sie **Datei** > **Optionen** > **Add-Ins** und wählen Sie **COM-Add-Ins** im Dropdownfeld **Verwalten** im unteren Bereich der Seite aus. Klicken Sie auf die Schaltfläche **Los...** , und vergewissern Sie sich, dass das Optionsfeld des Power Query für Excel-Add-Ins markiert ist.
       
        **Hinweis**: Mit Power Query können Sie außerdem Daten aus HDFS durch Klicken auf **Aus anderen Quellen** importieren.
4. Geben Sie den **Kontonamen** des mit Ihrem Cluster verbundenen Azure-Blobspeicherkontos ein, und klicken Sie dann auf **OK**. Bei diesem Konto kann es sich um das [Standardspeicherkonto](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) oder ein verknüpftes Speicherkonto handeln.  Das Format lautet *https://&lt;Speicherkontoname>.blob.core.windows.net/*.
5. Geben Sie den **Kontoschlüssel** für das Blobspeicherkonto ein, und klicken Sie dann auf **Speichern**. (Sie müssen die Kontoinformationen nur beim ersten Zugriff auf diesen Speicher eingeben.)
6. Doppelklicken Sie im **Navigationsbereich** links vom Abfrage-Editor auf den Namen des Blobspeichercontainers. Der Containername ist standardmäßig derselbe Name wie der Clustername.
7. Suchen Sie **HiveSampleData.txt** in der Spalte **Name** (der Pfad zum Ordner lautet **../hive/warehouse/hivesampletable/**) und klicken Sie links von HiveSampleData.txt auf **Binär**. „HiveSampleData.txt“ enthält alle Cluster. Sie können optional auch Ihre eigene Datei verwenden.
   
    ![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]
8. Bei Bedarf können Sie die Spaltennamen ändern. Klicken Sie auf, wenn Sie fertig sind, **Schließen und laden**.  Die Daten wurden in Ihre Arbeitsmappe geladen.
   
    ![HDI.PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Sie Daten aus HDInsight mithilfe von Power Query in Excel verwenden können. Auf ähnliche Weise können Sie Daten aus HDInsight für die Azure SQL-Datenbank abrufen. Außerdem können Sie Daten nach HDInsight hochladen. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verbinden von Excel mit HDInsight mithilfe des Microsoft Hive ODBC-Treibers][hdinsight-ODBC]
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.selecthdisource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importdata.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/hdi.powerquery.importedtable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689

