---
title: Analysieren von Sensordaten mit Hive und Hadoop | Microsoft Docs
description: "Erfahren Sie, wie Sie Sensordaten mithilfe der Hive-Abfragekonsole in HDInsight (Hadoop) analysieren und die Daten anschließend in Microsoft Excel mit Power View visualisieren können."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.translationtype: Human Translation
ms.sourcegitcommit: a3ca1527eee068e952f81f6629d7160803b3f45a
ms.openlocfilehash: 6588d2865928a64f6e172d0779fd43d35e7d9a6a
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analysieren von Sensordaten mit der Hive-Abfragekonsole für Hadoop in HDInsight

Erfahren Sie, wie Sie Sensordaten mithilfe der Hive-Abfragekonsole in HDInsight (Hadoop) analysieren und die Daten anschließend in Microsoft Excel mithilfe von Power View visualisieren können.

> [!IMPORTANT]
> Die Schritte in diesem Dokument funktionieren nur mit einem Windows-basierten HDInsight-Cluster. HDInsight ist unter Windows nur für HDInsight-Versionen vor 3.4 verfügbar. Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).


In diesem Beispiel verwenden Sie Hive, um Verlaufsdaten zu verarbeiten und Probleme mit Heizungs- und Klimaanlagen zu ermitteln. Insbesondere ermitteln Sie Systeme, die nicht in der Lage sind, eine festgelegte Temperatur beizubehalten, indem Sie die folgenden Aufgaben ausführen:

* Erstellen von HIVE-Tabellen zum Abfragen von Daten in CSV-Dateien.
* Erstellen von HIVE-Abfragen zum Analysieren der Daten
* Zum Abrufen der analysierten Daten stellen Sie mit Microsoft Excel eine Verbindung zu HDInsight her.
* Verwenden Sie Power View zum Visualisieren der Daten.

![Ein Diagramm der Lösungsarchitektur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Voraussetzungen

* Ein HDInsight-Cluster (Hadoop): Unter [Erstellen von Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) finden Sie weitere Informationen zur Erstellung von Clustern.
* Microsoft Excel 2013 (PowerPivot für Datenanalysten – Microsoft Excel 2010)

  > [!NOTE]
  > Microsoft Excel dient zur Visualisierung von Daten mithilfe von [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Microsoft Hive ODBC-Treiber](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Ausführen des Beispiels

1. Navigieren Sie in einem Browser zur folgenden URL: 

         https://<clustername>.azurehdinsight.net

    Ersetzen Sie `<clustername>` durch den Namen Ihres HDInsight-Clusters.

    Geben Sie den Benutzernamen und das Kennwort des Administrators ein, den Sie bei der Clusterbereitstellung verwendet haben, wenn Sie dazu aufgefordert werden.

2. Klicken Sie auf der Webseite, die sich daraufhin öffnet, auf die Registerkarte **Erste Schritte mit dem Katalog** und anschließend in der Kategorie **Lösungen mit Beispieldaten** auf das Beispiel **Sensordatenanalyse**.

    ![Abbildung zu „Erste Schritte mit dem Katalog“](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Folgen Sie den Anweisungen auf der Webseite, um das Beispiel abzuschließen.

