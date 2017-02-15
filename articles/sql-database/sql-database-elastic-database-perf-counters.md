---
title: "Leistungsindikatoren für den Shardzuordnungs-Manager"
description: "ShardMapManager-Klasse und datenabhängiges Routing – Leistungsindikatoren"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: b090aba0-2e30-454c-96b3-dffa281f539a
ms.service: sql-database
ms.custom: multiple databases
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: ddove
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7e5d43ca318337626734f9460780764150314f9c


---
# <a name="performance-counters-for-shard-map-manager"></a>Leistungsindikatoren für den Shardzuordnungs-Manager
Sie können die Leistung eines [Shardzuordnungs-Managers](sql-database-elastic-scale-shard-map-management.md) insbesondere bei der Verwendung des [datenabhängigen Routings](sql-database-elastic-scale-data-dependent-routing.md) erfassen. Leistungsindikatoren werden mit Methoden der Klasse „Microsoft.Azure.SqlDatabase.ElasticScale.Client“ erstellt.  

Mit Leistungsindikatoren wird die Leistung der Vorgänge bei [datenabhängigem Routing](sql-database-elastic-scale-data-dependent-routing.md) nachverfolgt. Auf diese Leistungsindikatoren kann im Systemmonitor in der Kategorie „Elastic Database: Shard Management“ (Elastische Datenbank: Shardverwaltung) zugegriffen werden.

**Die neueste Version** erhalten Sie unter [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Siehe auch [Upgrade einer App auf die neueste Clientbibliothek für elastische Datenbanken](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Voraussetzungen
* Damit der Benutzer die Leistungskategorie und -indikatoren erstellen kann, muss er Mitglied der lokalen Gruppe **Administratoren** auf dem Computer mit der Anwendung sein.  
* Damit der Benutzer eine Leistungsindikatorinstanz erstellen und die Leistungsindikatoren aktualisieren kann, muss er entweder Mitglied der Gruppe **Administratoren** oder Mitglied der Gruppe **Leistungsüberwachungsbenutzer** sein. 

## <a name="create-performance-category-and-counters"></a>Erstellen der Leistungskategorie und Leistungsindikatoren
Rufen Sie zum Erstellen der Leistungsindikatoren die CreatePeformanceCategoryAndCounters-Methode der [ShardMapManagmentFactory-Klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx)auf. Nur ein Administrator kann die Methode ausführen: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Sie können auch [dieses](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-Skript zum Ausführen der Methode verwenden. Die Methode erstellt die folgenden Leistungsindikatoren:  

* **Cached mappings**(Zwischengespeicherte Zuordnungen): Anzahl der Zuordnungen, die für die Shardzuordnung zwischengespeichert werden.
* **DDR operations/sec**(DDR-Vorgänge/s): Rate der Vorgänge für datenabhängiges Routing für die Shardzuordnung. Dieser Leistungsindikator wird aktualisiert, wenn ein Aufruf von [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) zu einer erfolgreichen Verbindung mit dem Zielshard führt. 
* **Mapping lookup cache hits/sec**(Cachetreffer bei der Zuordnungssuche/s): Rate der erfolgreichen Cachesuchvorgänge für Zuordnungen in der Shardzuordnung. 
* **Mapping lookup cache misses/sec**(Cachefehler bei der Zuordnungssuche/s): Rate der fehlerhaften Cachsuchvorgänge für Zuordnungen in der Shardzuordnung.
* **Mappings added or updated in cache/sec**(Im Cache hinzugefügte oder aktualisierte Zuordnungen/s): Rate, mit der Zuordnungen im Cache für die Shardzuordnung hinzugefügt oder aktualisiert werden. 
* **Mappings removed from cache/sec**(Aus dem Cache entfernte Zuordnungen/s): Rate, mit der Zuordnungen aus dem Cache für die Shardzuordnung entfernt werden. 

Leistungsindikatoren werden für jede zwischengespeicherte Shardzuordnung pro Prozess erstellt.  

## <a name="notes"></a>Hinweise
Die folgenden Ereignisse lösen die Erstellung der Leistungsindikatoren aus:  

* Initialisierung von [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) mit [Eager Loading](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), wenn der ShardMapManager Shardzuordnungen enthält. Dazu gehören die Methoden [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) und [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx).
* Erfolgreiches Nachschlagen einer Shardzuordnung (unter Verwendung von [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) oder [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 
* Erfolgreiche Erstellung einer Shardzuordnung mit „CreateShardMap()“.

Die Leistungsindikatoren werden von allen Cachevorgängen aktualisiert, die für die Shardzuordnung und Zuordnungen ausgeführt werden. Wird die Shardzuordnung mit „DeleteShardMap()“ erfolgreich entfernt, führt dies zu einer Löschung der Leistungsindikatorinstanz.  

## <a name="best-practices"></a>Bewährte Methoden
* Die Erstellung der Leistungskategorie und -indikatoren sollte nur einmal vor der Erstellung des ShardMapManager-Objekts erfolgen. Jede Ausführung des Befehls „CreatePerformanceCategoryAndCounters()“ löscht die vorherigen Leistungsindikatoren (von allen Instanzen werden Datenverluste gemeldet) und erstellt neue.  
* Leistungsindikatorinstanzen werden pro Prozess erstellt. Jeder Anwendungsabsturz oder jede Entfernung einer Shardzuordnung aus dem Cache führt dazu, dass die Leistungsindikatorinstanzen gelöscht werden.  

### <a name="see-also"></a>Siehe auch 
[Übersicht über Features für elastische Datenbanken](sql-database-elastic-scale-introduction.md)  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->




<!--HONumber=Nov16_HO3-->


