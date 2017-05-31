---
title: Tutorial mit Adventure Works von Azure Analysis Services | Microsoft-Dokumentation
description: "Einführung des Tutorials mit Adventure Works für Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: b20adfa6c69488b848d4ca5bee2cb4baeca806b8
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
# <a name="azure-analysis-services---adventure-works-tutorial"></a>Azure Analysis Services – Tutorial mit Adventure Works

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In diesem Tutorial erhalten Sie Lektionen zum Erstellen und Bereitstellen eines tabellarischen Modells auf Kompatibilitätsgrad 1400 mit [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  

Wenn Sie mit Analysis Services und tabellarischer Modellierung noch nicht vertraut sind, können Sie in diesem Tutorial am schnellsten erfahren, wie Sie ein einfaches tabellarisches Modell erstellen und an einen Server mit Analysis Services bereitstellen können. Sobald Sie alle Voraussetzungen erfüllt haben, nimmt das Tutorial etwa zwei bis drei Stunden in Anspruch.  
  
## <a name="what-youll-learn"></a>Sie lernen Folgendes   
  
-   Wie Sie ein neues Projekt für ein tabellarisches Modell mit dem **Kompatibilitätsgrad 1400** in SSDT erstellen können.
  
-   Wie Sie Daten aus einer relationalen Datenbank in ein Projekt für ein tabellarisches Modell importieren können.  
  
-   Wie Sie Beziehungen zwischen den einzelnen Tabellen im Modell verwalten können.  
  
-   Wie Sie berechnete Spalten, Maßnahmen und Key Performance Indicators erstellen können, die Benutzern dabei helfen, kritische Geschäftsmetrik zu analysieren.  
  
-   Wie Sie Perspektiven und Hierarchien erstellen und verwalten können, die Benutzern dabei helfen, Modelldaten einfacher zu durchsuchen, indem sie geschäfts- und anwendungsspezifische Blickpunkte bereitstellen.  
  
-   Wie Sie Partitionen erstellen können, die Tabellendaten in kleiner logische Teile aufteilen, die unabhängig von anderen Partitionen verarbeitet werden können.  
  
-   Wie Sie Modellobjekte und -daten sichern können, indem Sie Rollen mit Benutzern erstellen.  
  
-   Wie Sie ein tabellarischen Modell an einen Server mit **Azure Analysis Services** oder an einen lokalen Server mit SQL Server 2017 Analysis Services bereitstellen können.  
  
## <a name="prerequisites"></a>Voraussetzungen  
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:  
  
-   Eine Instanz von Azure Analysis Services oder SQL Server 2017 Analysis Services, für die Sie Ihr Modell bereitstellen können. Registrieren Sie sich für eine kostenlose [Textversion von Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) und [erstellen Sie einen Server](../analysis-services-create-server.md). Oder registrieren Sie sich und laden Sie [SQL Server 2017 Community Technology Preview](https://www.microsoft.com/evalcenter/evaluate-sql-server-vnext-ctp) herunter. 

-   Eine Datenbank von SQL Server oder Azure SQL mit der [Beispieldatenbank „AdventureWorksDW2014“](http://go.microsoft.com/fwlink/?LinkID=335807). Diese Beispieldatenbank beinhaltet die für den Abschluss dieses Tutorials erforderlichen Daten. Laden Sie [kostenlose Versionen von SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) herunter. Oder registrieren Sie sich für eine kostenlose [Testversion von Azure SQL Database](https://azure.microsoft.com/services/sql-database/). 

    **Wichtig**: Wenn Sie die Beispieldatenbank auf einem lokalen SQL-Server installieren, und Sie Ihr Modell an einen Server mit Azure Analysis Services bereitstellen, benötigen Sie ein [lokales Datengateway](../analysis-services-gateway.md).

-   Die aktuelle Version von [SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).

-   Die aktuelle Version von [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).    

-   Eine Client-Anwendung wie z.B. [Power BI Desktop](https://powerbi.microsoft.com/desktop/) oder Excel. 

## <a name="scenario"></a>Szenario  
Dieses Tutorial basiert auf Adventure Works Cycles, einem fiktiven Unternehmen. Adventure Works ist ein großes, multinationales Unternehmen, das Metall- und Verbundwerkstoff-Fahrräder herstellt und diese auf den nordamerikanischen, europäischen und asiatischen Handelsmärkten vertreibt. Der Hauptsitz des Unternehmens befindet sich in Bothell, Washington, und das Unternehmen beschäftigt 500 Angestellte. Zusätzlich beschäftigt Adventure Works mehrere regionale Verkaufsteams, die über das gesamte Marktgebiet verteilt sind. Sie werden beauftragt, ein tabellarisches Modell für Benutzer aus dem Vertrieb und Marketing zu erstellen zur Analyse von Internetverkaufsdaten in der Beispieldatenbank „AdventureWorksDW“.  
  
Sie müssen verschiedene Lektionen abschließen, um das Tutorial abschließen zu können. Jede Lektion setzt sich aus verschiedenen Aufgaben zusammen. Sie müssen die Aufgaben nacheinander durchführen, um die Lektion abzuschließen. Während sich in einer Lektion möglicherweise mehrere Aufgaben befinden, die zu einem ähnlichen Ergebnis führen, unterscheiden sich die Aufgaben bei der Art und Weise der Ausführung. So wird verdeutlicht, dass es oft mehr als eine Möglichkeit gibt, eine bestimmte Aufgabe durchzuführen. Außerdem sollen Sie Fähigkeiten einsetzen, die Sie in vorherigen Lektionen und Aufgaben gelernt haben.  
  
Zweck der Lektionen soll es sein, Sie beim Schreiben eines einfachen tabellarischen Modells mithilfe von vielen in SSDT beinhalteten Funktion zu begleiten. Da jede Lektion auf der vorherigen aufbaut, sollten Sie diese in der richtigen Reihenfolge abschließen. Wenn Sie alle Lektionen abgeschlossen haben, haben Sie das tabellarische Beispielmodell „Adventure Works Internet Sales“ erstellt und auf einem Server mit Analysis Services bereitgestellt.  
  
In diesem Tutorial erhalten Sie allerdings keine Lektionen oder Informationen zum Verwalten eines Servers mit Azure Analysis Services im Azure-Portal, zum Verwalten eines Servers bzw. Bereitstellen einer Datenbank mit SSMS oder zum Verwenden einer Berichterstellungsclientanwendung zum Verbinden mit einem bereitgestellten Modell, um Modelldaten zu durchsuchen.  


## <a name="lessons"></a>Lektionen  
Dieses Lernprogramm enthält die folgenden Lektionen:  
  
|Lektion|Geschätzter Zeitaufwand|  
|----------|------------------------------|  
|[Lektion 1: Erstellen eines neuen tabellarischen Modellprojekts](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)|10 Minuten|  
|[Lektion 2: Abrufen von Daten](../tutorials/aas-lesson-2-get-data.md)|10 Minuten|  
|[Lektion 3: Markieren als Datumstabelle](../tutorials/aas-lesson-3-mark-as-date-table.md)|3 Minuten|  
|[Lektion 4: Erstellen von Beziehungen](../tutorials/aas-lesson-4-create-relationships.md)|10 Minuten|  
|[Lektion 5: Erstellen berechneter Spalten](../tutorials/aas-lesson-5-create-calculated-columns.md)|15 Minuten|
|[Lektion 6: Erstellen von Kennzahlen](../tutorials/aas-lesson-6-create-measures.md)|30 Minuten|  
|[Lektion 7: Erstellen von Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md)|15 Minuten|  
|[Lektion 8: Erstellen von Perspektiven](../tutorials/aas-lesson-8-create-perspectives.md)|5 Minuten|  
|[Lektion 9: Erstellen von Hierarchien](../tutorials/aas-lesson-9-create-hierarchies.md)|20 Minuten|  
|[Lektion 10: Erstellen von Partitionen](../tutorials/aas-lesson-10-create-partitions.md)|15 Minuten|  
|[Lektion 11: Erstellen von Rollen](../tutorials/aas-lesson-11-create-roles.md)|15 Minuten|  
|[Lektion 12: Analysieren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md)|5 Minuten| 
|[Lektion 13: Bereitstellen](../tutorials/aas-lesson-13-deploy.md)|5 Minuten|  
  
## <a name="supplemental-lessons"></a>Ergänzende Lektionen  
Diese Lektionen sind nicht erforderlich, um das Tutorial abzuschließen. Allerdings tragen sie dazu bei, dass Sie erweiterte Schreibfunktionen für tabellarische Modelle besser verstehen.  
  
|Lektion|Geschätzter Zeitaufwand|  
|----------|------------------------------|  
|[Detailzeilen](../tutorials/aas-supplemental-lesson-detail-rows.md)|10 Minuten|
|[Dynamische Sicherheit](../tutorials/aas-supplemental-lesson-dynamic-security.md)|30 Minuten|
|[Unregelmäßige Hierarchien](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)|20 Minuten| 

  
## <a name="next-steps"></a>Nächste Schritte  
Die ersten Schritte finden Sie unter [Lektion 1: Erstellen eines neuen tabellarischen Modellprojekts](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
  
  


