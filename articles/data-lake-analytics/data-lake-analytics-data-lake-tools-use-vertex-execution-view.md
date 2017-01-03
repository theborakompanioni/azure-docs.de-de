---
title: "Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio | Microsoft Docs"
description: "Erfahren Sie, wie Scheitelpunktausführungsansicht zum Prüfen der Data Lake Analytics-Aufträge verwenden können."
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5366d852-e7d6-44cf-a88c-e9f52f15f7df
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/13/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: ac5a64b759376c06e058ae015b73f1b73b7d1e7b
ms.openlocfilehash: b2acf4fd95c1611ecd580b508339d1305010fdd8


---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Verwenden der Scheitelpunktausführungsansicht in Data Lake-Tools für Visual Studio
Erfahren Sie, wie Scheitelpunktausführungsansicht zum Prüfen der Data Lake Analytics-Aufträge verwenden können.

## <a name="prerequisites"></a>Voraussetzungen
* Grundlegende Kenntnisse der Verwendung von Data Lake-Tools für Visual Studio zum Entwickeln von U-SQL-Skript.  Weitere Informationen finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Öffnen Sie die Scheitelpunktausführungsansicht
Für ein bestimmtes Projekt können Sie auf den Link „Scheitelpunktausführungsansicht“ in der unteren linken Ecke klicken. Sie werden möglicherweise aufgefordert, zuerst Profile zu laden. Dies kann abhängig von Ihrer Netzwerkkonnektivität einige Zeit in Anspruch nehmen.

![Scheitelpunkausführungsansicht der Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Verstehen der Scheitelpunktausführungsansicht
Nachdem Sie die Scheitelpunktausführungsansicht eingegeben haben, gibt es drei Teile:

![Scheitelpunkausführungsansicht der Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

* Vertex selector (Scheitelpunktauswahl): Diese befindet sich auf der linken Seite.  Sie können die Scheitelpunkte nach Features auswählen, (z.B. nach den Top 10 der gelesenen Daten oder nach Phase).
  
    Einer der am häufigsten verwendeten Filter ist der für die Scheitelpunkte auf dem kritischen Pfad. Der kritische Pfad ist der längste Pfad eines U-SQL-Auftrags. Für die Optimierung Ihres Auftrags ist es hilfreich, zu überprüfen, welcher Scheitelpunkt die meiste Zeit benötigt.
* Der Bereich oben in der Mitte:
  
    ![Scheitelpunkausführungsansicht der Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)
  
    Diese Ansicht zeigt auch den Ausführungsstatus aller Scheitelpunkte. Die Zeit wird entsprechend Ihres lokalen Computers konvertiert ,und es werden verschiedene Status in unterschiedlichen Farben gezeigt.
* Der Bereich unten in der Mitte:
  
    ![Scheitelpunkausführungsansicht der Data Lake Analytics-Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)
  
  * Process Name (Prozessname): Der Name der Scheitelpunktinstanz. Er besteht aus verschiedenen Teilen in StageName|VertexName|VertexRunInstance. Beispielsweise steht der Scheitelpunkt SV7_Split[62].v1 für die zweite, sich in Ausführung befindliche Instanz (.v1, der Index beginnt ab 0) der Scheitelpunktnummer 62 in der Phase SV7_Split.
  * Total Data Read/Written (Gelesene/geschriebene Daten gesamt): Die Daten wurden von diesem Scheitelpunkt gelesen/geschrieben.
  * State/Exit Status (Phasen/Exitstatus): Der finale Status wenn der Scheitelpunkt beendet ist.
  * Exit Code/Failure Type (Exitcode/Fehlertyp): Der Fehler, wenn der Scheitelpunkt fehlgeschlagen ist.
  * Creation Reason (Erstellungsgrund): Warum der Scheitelpunkt erstellt wurde.
  * Resource Latency/Process Latency/PN Queue Latency (Ressourcenlatenz/Prozesslatenz/PN-Schlangenlatenz): Die Zeit, die der Scheitelpunkt benötigt, um auf Ressourcen zu warten, um Daten zu verarbeiten und in der Schlange zu bleiben.
  * Process/Creator GUID (Prozess/Ersteller-GUID): GUID für den aktuell ausgeführten Scheitelpunkt oder dessen Ersteller.
  * Version (Version): die n-te Instanz des ausgeführten Scheitelpunkts (das System plant möglicherweise neue Instanzen für einen Scheitelpunkt aus verschiedenen Gründen, z.B. Failover, Berechnungsredundanz usw.).
  * Version Created Time (Erstellungszeit der Version).
  * Process Create Start Time/Process Queued Time/Process Start Time/Process Complete Time (Startzeit der Erstellung durch den Prozess / Zeit des Prozesses in der Warteschlange / Startzeit des Prozesses / Abschlusszeit des Prozesses): wenn der Scheitelpunktprozess erstellt wird, wenn sich der Scheitelpunktprozess in der Warteschlange befindet, wenn der bestimmte Scheitelpunktprozess startet, wenn der bestimmte Scheitelpunktprozess beendet wird.

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht über Data Lake Analytics finden Sie unter [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md).
* Informationen zu den ersten Schritten der Entwicklung von U-SQL-Anwendungen finden Sie unter [Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Informationen zum Erlernen von U-SQL finden Sie unter [Erste Schritte mit der Sprache U-SQL für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informationen zu Verwaltungsaufgaben finden Sie unter [Verwalten von Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-manage-use-portal.md).
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Accessing diagnostic logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Zum Anzeigen von Auftragsdetails gehen Sie unter [Use Job Browser and Job View for Azure Data lake Analytics jobs (Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge)](data-lake-analytics-data-lake-tools-view-jobs.md).
* Informationen über Data Lake-Tools für Visual Studio-Code finden Sie unter [Use the Azure Data Lake Tools for Visual Studio Code (Verwenden der Azure Data Lake-Tools für Visual Studio-Code)](data-lake-analytics-data-lake-tools-for-vscode.md).



<!--HONumber=Nov16_HO4-->


