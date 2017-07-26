---
title: "Verwenden der Scheitelpunktausführungsansicht in Data Lake Tools für Visual Studio | Microsoft Docs"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: b788e7bc8ded86ebd49cc0be73e5b4e1bcbeaba3
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="use-the-vertex-execution-view-in-data-lake-tools-for-visual-studio"></a>Verwenden der Scheitelpunktausführungsansicht in Data Lake Tools für Visual Studio
Erfahren Sie, wie Scheitelpunktausführungsansicht zum Prüfen der Data Lake Analytics-Aufträge verwenden können.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen Grundkenntnisse der Verwendung von Data Lake Tools für Visual Studio zum Entwickeln von U-SQL-Skripts.  Weitere Informationen finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

## <a name="open-the-vertex-execution-view"></a>Öffnen Sie die Scheitelpunktausführungsansicht
Öffnen Sie in Data Lake Tools für Visual Studio einen U-SQL-Auftrag. Klicken Sie links unten auf **Vertexausführungsansicht**. Sie werden möglicherweise aufgefordert, zuerst Profile zu laden. Dies kann abhängig von Ihrer Netzwerkkonnektivität einige Zeit in Anspruch nehmen.

![Vertexausführungsansicht in Data Lake Analytics Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-open-vertex-execution-view.png)

## <a name="understand-vertex-execution-view"></a>Grundlegendes zur Vertexausführungsansicht
Die Vertexausführungsansicht hat drei Teile:

![Vertexausführungsansicht in Data Lake Analytics Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view.png)

In der **Vertexauswahl** links können Sie Vertices nach Features auswählen (z.B. die zehn am meisten gelesenen Datenobjekte oder nach Phase). Einer der am häufigsten verwendeten Filter ist der für die **Vertices auf dem kritischen Pfad**. Der **kritischen Pfad** ist die längste Kette von Vertices eines U-SQL-Auftrags. Das Verstehen des kritischen Pfads ist nützlich für das Optimieren Ihrer Aufträge, indem geprüft wird, welcher Vertex die meiste Zeit benötigt.
  
![Vertexausführungsansicht in Data Lake Analytics Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane2.png)

Der obere mittlere Bereich zeigt den **Ausführungsstatus aller Vertices**.
  
![Vertexausführungsansicht in Data Lake Analytics Tools](./media/data-lake-analytics-data-lake-tools-use-vertex-execution-view/data-lake-tools-vertex-execution-view-pane3.png)

Der untere mittlere Bereich zeigt Informationen zu jedem Vertex:
* Process Name (Prozessname): Der Name der Vertexinstanz. Er besteht aus verschiedenen Teilen in StageName|VertexName|VertexRunInstance. Beispielsweise steht der Vertex SV7_Split[62].v1 für die zweite sich in Ausführung befindliche Instanz (.v1, der Index beginnt ab 0) der Vertexnummer 62 in der Phase SV7_Split.
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
* Informationen zum Protokollieren von Diagnoseinformationen finden Sie unter [Accessing diagnostic logs for Azure Data Lake Analytics](data-lake-analytics-diagnostic-logs.md)
* Eine komplexere Abfrage finden Sie unter [Analysieren von Websiteprotokollen mit Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Zum Anzeigen von Auftragsdetails gehen Sie unter [Use Job Browser and Job View for Azure Data lake Analytics jobs (Verwenden des Auftragsbrowsers und der Auftragsansicht für Azure Data Lake Analytics-Aufträge)](data-lake-analytics-data-lake-tools-view-jobs.md).

