---
title: Verwenden von Power BI mit SQL Data Warehouse | Microsoft Docs
description: "Tipps zur Verwendung von Power BI in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: b12bee87-2268-40c2-81bf-ab27588b32e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 201efc5225d6c12028b48ccaf6764c60b44dce58


---
# <a name="use-power-bi-with-sql-data-warehouse"></a>Verwenden von Power BI mit SQL Data Warehouse
Wie bei Azure SQL-Datenbank ermöglicht auch SQL Data Warehouse Direct Connect leistungsfähige logische Pushdowns in Verbindung mit den Analysefunktionen von Power BI.  Abfragen werden mit Direct Connect in Echtzeit zurück an Ihr Azure SQL Data Warehouse gesendet, während Sie die Daten durchsuchen.  Dadurch haben Benutzer in Kombination mit der Skalierung von SQL Data Warehouse die Möglichkeit, in Minuten dynamische Berichte für Terabytes an Daten zu erstellen.  Darüber hinaus ermöglicht die Einführung der Schaltfläche "Open in Power BI" Benutzern, direkt eine Verbindung zwischen Power BI und SQL Data Warehouse herzustellen, ohne dass Informationen aus anderen Teilen von Azure gesammelt werden müssen.

Beachten Sie bei der Verwendung von Direct Connect Folgendes:

* Geben Sie beim Herstellen einer Verbindung den vollqualifizierten Servernamen an (weitere Informationen im Folgenden)
* Stellen Sie sicher, dass die Firewallregeln für die Datenbank mit "Zugriff auf Azure-Dienste erlauben" konfiguriert sind.
* Für jede Aktion, wie z.B. das Auswählen einer Spalte oder das Hinzufügen eines Filters, wird das Data Warehouse direkt abgefragt.
* Kacheln werden ca. alle 15 Minuten aktualisiert (die Aktualisierung muss nicht geplant werden).
* Fragen und Antworten sind nicht für Direct Connect-DataSets verfügbar.
* Schemaänderungen werden nicht automatisch übernommen.
* Für alle Direct Connect-Abfragen gilt ein Timeout von 2 Minuten.

Diese Einschränkungen und Hinweise können sich ändern, da wir die Umgebung weiter verbessern. Die Schritte zum Herstellen der Verbindung werden nachfolgend beschrieben.  

## <a name="using-the-open-in-power-bi-button"></a>Mithilfe der Schaltfläche "Open in Power BI"
Die einfachste Möglichkeit zum Navigieren zwischen Ihrem SQL Data Warehouse und Power BI ist über die Schaltfläche "Open in Power BI". Mit dieser Schaltfläche können Sie nahtlos beginnen, neue Dashboards in Power BI zu erstellen.  

1. Navigieren Sie zunächst zu Ihrer SQL Data Warehouse-Instanz im klassischen Azure-Portal.
2. Klicken Sie auf die Schaltfläche „Open in Power BI“.
3. Wenn Sie nicht direkt angemeldet werden können oder wenn Sie nicht über ein Power BI-Konto verfügen, müssen Sie sich anmelden.  
4. Sie werden auf die SQL Data Warehouse-Verbindungsseite weitergeleitet, die bereits mit den Informationen aus dem SQL Data Warehouse ausgefüllt ist.
5. Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, ist die Verbindung mit SQL Data Warehouse vollständig hergestellt.

## <a name="connecting-through-the-power-bi-portal"></a>Herstellen einer Verbindung über das Power BI-Portal
Zusätzlich zur Schaltfläche "Open in Power BI" können Benutzer auch über das Power BI-Portal eine Verbindung mit dem SQL Data Warehouse herstellen.

1. Klicken Sie unten im Navigationsbereich auf „Get Data“.
2. Wählen Sie „Databases“ aus.
3. Wenn die Seite „Databases“ angezeigt wird, wählen Sie „Azure SQL Data Warehouse“ aus, und klicken Sie dann auf „Connect“.
4. Geben Sie die erforderlichen Verbindungsinformationen ein.  Den Namen des Servers und der Datenbank finden Sie im Azure-Portal.
5. Sie gelangen zurück zur Hauptseite von Power BI, und nachdem die Verbindung hergestellt ist, wird unter „Datasets“ ein neuer Eintrag mit dem Namen Ihrer Instanz angezeigt.  
6. Sie können auf das neue Dataset klicken, um alle Tabellen und Sichten in der Datenbank zu untersuchen. Durch die Auswahl einer Spalte wird eine Abfrage zurück an die Quelle gesendet und dynamisch ein visuelles Element erstellt. Diese visuellen Elemente können in einem neuen Bericht gespeichert und wieder an Ihr Dashboard angeheftet werden.

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]:  ./sql-data-warehouse-overview-develop/
[SQL Data Warehouse integration overview]:  ./sql-data-warehouse-overview-integration/

<!--MSDN references-->

<!--Other Web references-->



<!--HONumber=Feb17_HO3-->


