---
title: "Erste Schritte mit Tools für elastische Datenbanken | Microsoft-Dokumentation"
description: "Grundlegende Erläuterung des Features mit Tools für elastische Datenbanken von Azure SQL-Datenbank, einschließlich einer einfachen Beispiel-App."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.translationtype: Human Translation
ms.sourcegitcommit: b1d56fcfb472e5eae9d2f01a820f72f8eab9ef08
ms.openlocfilehash: 637463399593f4bc9ff5bfcbf67bf93b816efc7f
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="get-started-with-elastic-database-tools"></a>Erste Schritte mit Tools für elastische Datenbanken
In diesem Dokument werden die Entwicklerfunktionen vorgestellt, indem Ihnen beim Ausführen der Beispiel-App geholfen wird. Mit dem Beispiel wird eine einfache Sharding-Anwendung erstellt, und es werden die wichtigsten Funktionen von Tools für elastische Datenbanken erkundet. Das Beispiel veranschaulicht die Funktionen der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md).

Um die Bibliothek zu installieren, wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Die Bibliothek wird mit der im nachfolgenden Abschnitt beschriebenen Beispiel-App installiert.

## <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2012 oder eine neuere Version mit C#. Laden Sie eine kostenlose Version unter [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)herunter.
* NuGet 2.7 oder eine neuere Version. Die aktuelle Version finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) (Installieren von NuGet).

## <a name="download-and-run-the-sample-app"></a>Herunterladen und Ausführen der Beispiel-App
Die Beispielanwendung unter **Tools für elastische Datenbanken für Azure SQL – erste Schritte** veranschaulicht die wichtigsten Aspekte der Entwicklungsoberfläche für Shardinganwendungen, die Tools für elastische Datenbanken verwenden. Dabei stehen die wichtigsten Anwendungsfälle für [Shardzuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md), [datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md) und [Abfragen mehrerer Shards](sql-database-elastic-scale-multishard-querying.md) im Mittelpunkt. Gehen Sie folgendermaßen vor, um das Beispiel herunterzuladen und auszuführen: 

1. Laden Sie das Beispiel [Elastic DB Tools for Azure SQL - Getting Started (Elastische DB-Tools für Azure SQL – Erste Schritte)](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-a80d8dc6) von MSDN herunter. Entzippen Sie das Beispiel an einem Speicherort Ihrer Wahl.

2. Um ein Projekt zu erstellen, öffnen Sie die Projektmappe **ElasticScaleStarterKit.sln** im Verzeichnis **C#**.

3. Öffnen Sie in der Projektmappe für das Beispielprojekt der Datei **app.config**. Folgen Sie dann den Anweisungen in der Datei, um den Namen des Azure SQL-Datenbank-Servers und Ihre Anmeldeinformationen (Benutzername und Kennwort) hinzuzufügen.

4. Erstellen Sie die Anwendung, und führen Sie sie aus. Ermöglichen Sie Visual Studio bei der entsprechenden Aufforderung, die NuGet-Pakete der Projektmappe wiederherzustellen. Dadurch wird die aktuelle Version der Clientbibliothek für elastische Datenbanken von NuGet heruntergeladen.

5. Experimentieren Sie mit den verschiedenen Optionen, um mehr über die Funktionen der Clientbibliothek zu erfahren. Beachten Sie die Schritte, welche die Anwendung in der Konsolenausgabe ausführt, und erkunden Sie den zugrunde liegenden Code.
   
    ![Status][4]

Herzlichen Glückwunsch! Sie haben Ihre erste Shardinganwendung mit den Tools für elastische Datenbanken in SQL-Datenbank erfolgreich erstellt und ausgeführt. Werfen Sie einen Blick auf die im Beispiel erstellten Shards, indem Sie mithilfe von Visual Studio oder SQL-Server Management Studio eine Verbindung mit Ihrer SQL-Datenbank herstellen. Sie sehen, dass mit dem Beispiel neue Beispiel-Shard-Datenbanken und eine Shard-Map-Managerdatenbank erstellt wurden.

> [!IMPORTANT]
> Wir empfehlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-the-code-sample"></a>Zentrale Elemente des Codebeispiels
* **Verwalten von Shards und Shardzuordnungen:** Der Code in der Datei **ShardManagementUtils.cs** veranschaulicht die Arbeit mit Shards, Bereichen und Zuordnungen. Weitere Informationen finden Sie unter [Horizontales Hochskalieren von Datenbanken mit dem Shardzuordnungs-Manager](http://go.microsoft.com/?linkid=9862595).  

* **Datenabhängiges Routing:** Das Routing von Transaktionen zum richtigen Shard wird in **DataDependentRoutingSample.cs** dargestellt. Weitere Informationen finden Sie unter [Datenabhängiges Routing](http://go.microsoft.com/?linkid=9862596). 

* **Abfragen mehrerer Shards:** Shardübergreifendes Abfragen wird in der Datei **MultiShardQuerySample.cs** veranschaulicht. Weitere Informationen finden Sie unter [Abfragen mehrerer Shards](http://go.microsoft.com/?linkid=9862597).

* **Hinzufügen leerer Shards:** Das iterative Hinzufügen neuer leerer Shards wird mit dem Code in der Datei **CreateShardSample.cs** durchgeführt. Weitere Informationen finden Sie unter [Horizontales Hochskalieren von Datenbanken mit dem Shardzuordnungs-Manager](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Weitere Elastic Scale-Operationen
* **Aufteilen eines vorhandenen Shards:** Die Möglichkeit zum Aufteilen von Shards wird durch das **Split-Merge-Tool** bereitgestellt. Weitere Informationen finden Sie unter [Verschieben von Daten zwischen horizontal hochskalierten Clouddatenbanken](sql-database-elastic-scale-overview-split-and-merge.md).

* **Zusammenführen vorhandener Shards:** Shardzusammenführungen werden ebenfalls mit dem **Split-Merge-Tool** durchgeführt. Weitere Informationen finden Sie unter [Verschieben von Daten zwischen horizontal hochskalierten Clouddatenbanken](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kosten
Die Tools für elastische Datenbanken sind kostenlos. Für die Verwendung der Tools für elastische Datenbanken werden Ihnen keine zusätzlichen Kosten über die Kosten für Ihre Azure-Nutzung hinaus berechnet. 

Die Beispielanwendung erstellt z. B. neue Datenbanken. Die Kosten hierfür richten sich nach der SQL-Datenbank-Edition, die Sie auswählen, sowie nach der Azure-Nutzung Ihrer Anwendung.

Preisinformationen finden Sie unter [SQL-Datenbank – Preisdetails](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Tools für elastische Datenbanken finden Sie auf den folgenden Seiten:

* Codebeispiele: 
  * [Elastic DB Tools for Azure SQL – Getting Started (Tools für elastische Datenbanken in Azure SQL – erste Schritte)](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Elastic DB Tools for Azure SQL - Entity Framework Integration (Tools für elastische Datenbanken in Azure SQL – Entity Framework-Integration)](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Shard-Elastizität im Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [Ankündigung zur elastischen Skalierung](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy-Video: [Implementing Scale-Out Using Sharding with the Elastic Database Client Library](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) (Implementieren der horizontalen Hochskalierung mithilfe von Sharding mit der Clientbibliothek für elastische Datenbanken) 
* Channel 9: [Elastische Skalierung – Übersichtsvideo](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskussionsforum: [Azure SQL-Datenbank – Forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* So messen Sie die Leistung: [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png


