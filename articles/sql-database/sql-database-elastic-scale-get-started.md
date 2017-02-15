---
title: "Erste Schritte mit Tools für elastische Datenbanken"
description: "Grundlegende Erläuterung der Funktion „Tools für elastische Datenbanken“ von Azure SQL, einschließlich einer einfachen Beispiel-App."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: CarlRabeler
ms.assetid: b6911f8d-2bae-4d04-9fa8-f79a3db7129d
ms.service: sql-database
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a42f6ed3bce42cbd5841f241c2f735b87252cb3f


---
# <a name="get-started-with-elastic-database-tools"></a>Erste Schritte mit Tools für elastische Datenbanken
In diesem Dokument werden die Entwicklerfunktionen durch Ausführen der Beispiel-App vorgestellt. Mit dem Beispiel wird eine einfache Sharding-Anwendung erstellt, und es werden die wichtigsten Funktionen von Tools für elastische Datenbanken erkundet. Das Beispiel veranschaulicht die Funktionen der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md)

Um die Bibliothek zu installieren, wechseln Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Die Bibliothek wird mit der unten beschriebenen Beispiel-App installiert.

## <a name="prerequisites"></a>Voraussetzungen
1. Visual Studio 2012 oder höher mit C# wird benötigt. Laden Sie eine kostenlose Version unter [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)herunter.
2. NuGet 2.7 oder höher. Die aktuelle Version finden Sie unter [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Herunterladen und Ausführen der Beispiel-App
Die Beispielanwendung unter **Elastische Datenbank mit Azure SQL – Erste Schritte** veranschaulicht die wichtigsten Aspekte der Entwicklungsoberfläche für Sharding-Anwendungen, die Tools für elastische Datenbanken in Azure SQL verwenden. Dabei stehen die wichtigsten Anwendungsfälle für [Shardzuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md), [datenabhängiges Routing](sql-database-elastic-scale-data-dependent-routing.md) und [Abfragen mehrerer Shards](sql-database-elastic-scale-multishard-querying.md) im Mittelpunkt. Gehen Sie folgendermaßen vor, um das Beispiel herunterzuladen und auszuführen: 

1. Öffnen Sie Visual Studio, und wählen Sie **Datei -> Neu -> Projekt** aus.
2. Klicken Sie im Dialogfeld auf **Online**.
   
    ![Neues Projekt > Online][2]
3. Klicken Sie anschließend unter **Beispiele** auf **Visual C#**.
   
    ![Auf Visual C klicken#][3]
4. Geben Sie im Suchfeld die Zeichenfolge **elastic db** ein, um nach dem Beispiel zu suchen. Der Titel **Elastic DB Tools for Azure SQL - Getting Started** wird angezeigt.
   
    ![Suchfeld][1]
5. Wählen Sie das Beispiel aus, wählen Sie einen Namen und einen Speicherort für das neue Projekt, und drücken Sie **OK** , um das Projekt zu erstellen.
6. Öffnen Sie die Datei **app.config** in der Lösung des Beispielprojekts, und folgen Sie den Anweisungen in der Datei, um den Namen des Azure SQL-Datenbankservers und Ihre Anmeldeinformationen (Benutzername und Kennwort) hinzuzufügen.
7. Erstellen Sie die Anwendung, und führen Sie sie aus. Gestatten Sie Visual Studio bei der entsprechenden Aufforderung, die NuGet-Pakete der Lösung wiederherzustellen. Dadurch wird die aktuelle Version der Clientbibliothek für elastische Datenbanken von NuGet heruntergeladen.
8. Spielen Sie mit den verschiedenen Optionen, um mehr über die Funktionen der Clientbibliothek zu erfahren. Beachten Sie die Schritte, welche die Anwendung in der Konsolenausgabe ausführt, und erkunden Sie den zugrunde liegenden Code.
   
    ![Fortschritt][4]

Herzlichen Glückwunsch! Sie haben Ihre erste Sharding-Anwendung mit den Tools für elastische Datenbanken in Azure SQL-Datenbank erfolgreich erstellt und ausgeführt. Werfen Sie einen Blick auf die Shards, die im Beispiel durch Herstellen einer Verbindung zwischen Visual Studio oder SQL-Server Management Studio und Ihrem Azure-Datenbankserver erstellt wurden. Sie sehen, dass mit dem Beispiel neue Beispiel-Shard-Datenbanken und eine Shard-Map-Managerdatenbank erstellt wurden.

> [!IMPORTANT]
> Es wird empfohlen, immer die neueste Version von Management Studio zu verwenden, damit Sie mit Updates von Microsoft Azure und SQL-Datenbank synchron sind. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="key-pieces-of-the-code-sample"></a>Zentrale Elemente des Codebeispiels
1. **Verwalten von Shards und Shardzuordnungen**: Der Code in der Datei **ShardMapManagerSample.cs** veranschaulicht die Arbeit mit Shards, Bereichen und Zuordnungen. Weitere Informationen über dieses Thema finden Sie hier: [Shard Map-Verwaltung](http://go.microsoft.com/?linkid=9862595).  
2. **Datenabhängiges Routing**: Das Routing von Transaktionen zum richtigen Shard wird in **DataDependentRoutingSample.cs** dargestellt. Weitere Einzelheiten finden Sie unter [Datenabhängiges Routing](http://go.microsoft.com/?linkid=9862596). 
3. **Abfragen mehrerer Shards**: shardübergreifendes Abfragen wird in der Datei **MultiShardQuerySample.cs** veranschaulicht. Weitere Einzelheiten finden Sie unter [Abfragen mehrerer Shards](http://go.microsoft.com/?linkid=9862597).
4. **Hinzufügen leerer Shards**: Das iterative Hinzufügen neuer leerer Shards wird mit dem Code in der Datei **AddNewShardsSample.cs** durchgeführt. Einzelheiten zu diesem Thema werden hier behandelt: [Shard Map-Verwaltung](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Weitere Elastic Scale-Operationen
1. **Aufteilen eines vorhandenen Shards**: Die Möglichkeit zum Aufteilen von Shards wird über das **Split-Merge-Tool** bereitgestellt. Weitere Informationen zu diesem Tool finden Sie hier: [Übersicht über das Split-Merge-Tool](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Zusammenführen vorhandener Shards**: Shardzusammenführungen werden ebenfalls mit dem **Split-Merge-Tool** durchgeführt. Weitere Informationen finden Sie unter: [Übersicht über das Split-Merge-Tool](sql-database-elastic-scale-overview-split-and-merge.md).   

## <a name="cost"></a>Kosten
Die Tools für elastische Datenbanken sind kostenlos. Durch Tools für elastische Datenbanken entstehen keine zusätzlichen Gebühren zu den Kosten der Azure-Nutzung. 

Die Beispielanwendung erstellt z. B. neue Datenbanken. Die Kosten richten sich nach der Azure SQL DB-Datenbankedition, die Sie wählen, sowie nach der Azure-Nutzung Ihrer Anwendung.

Preisinformationen finden Sie in der [SQL-Datenbank – Preisdetails](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Tools für elastische Datenbanken finden Sie unter:

* [Dokumentationsübersicht zu Tools für elastische Datenbanken](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
* Codebeispiele: 
  * [Elastische Datenbank mit Azure SQL – Erste Schritte](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
  * [Elastische Datenbank mit Azure SQL – Integration in Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
  * [Shard-Elastizität im Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
* Blog: [Elastic Scale-Ankündigung](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
* Microsoft Virtual Academy-Video: [Implementing Scale-Out Using Sharding with the Elastic Database Client Library](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554?l=lWyQhF1fC_6306218965) (Implementieren der horizontalen Hochskalierung mithilfe von Sharding mit der Clientbibliothek für elastische Datenbanken) 
* Channel 9: [Elastic Scale – Übersichtsvideo](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
* Diskussionsforum: [Azure SQL-Datenbank – Forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
* So messen Sie die Leistung: [Leistungsindikatoren für den Shardzuordnungs-Manager](sql-database-elastic-database-client-library.md)

<!--Anchors-->
[Die Elastic Scale-Beispielanwendung]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Kosten]: #Cost
[Nächste Schritte]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png




<!--HONumber=Nov16_HO3-->


