---
title: "Upgrade auf die neueste Clientbibliothek für elastische Datenbanken | Microsoft Docs"
description: Aktualisieren von Apps und Bibliotheken mithilfe von Nuget
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 0a546510-76e7-465e-9271-f15ff0cfa959
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: ddove
ms.translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 0bbbc4f61c37fe4d296221c372bfdf6dd429a32b
ms.contentlocale: de-de
ms.lasthandoff: 03/07/2017


---
# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Upgrade einer App auf die neueste Clientbibliothek für elastische Datenbanken
Neue Versionen der [Clientbibliothek für elastische Datenbanken](sql-database-elastic-database-client-library.md) stehen über NuGet und die NuGetPackage Manager-Schnittstelle in Visual Studio zur Verfügung. Upgrades bieten Fehlerkorrekturen und Unterstützung für die neuen Funktionen der Clientbibliothek.

**Die neueste Version** erhalten Sie unter [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Erstellen Sie die Anwendung mit der neuen Bibliothek neu, und ändern Sie Ihre vorhandenen Shard Map Manager-Metadaten, die in Ihren Azure SQL-Datenbanken gespeichert sind, damit neue Features unterstützt werden.

Durch Ausführen dieser Schritte in dieser Reihenfolge wird sichergestellt, dass alte Versionen der Clientbibliothek nicht mehr in Ihrer Umgebung vorhanden sind, wenn Metadatenobjekte aktualisiert werden. Dies bedeutet, dass Metadatenobjekte der alten Version nach dem Upgrade nicht mehr erstellt werden.   

## <a name="upgrade-steps"></a>Upgradeschritte
**1. Aktualisieren Sie Ihre Anwendungen.** Laden Sie in Visual Studio die neueste Version der Clientbibliothek herunter, und verweisen Sie in allen Ihren Entwicklungsprojekten, die die Bibliothek verwenden, darauf. Führen Sie dann die Schritte zur Neuerstellung und Bereitstellung aus. 

* Wählen Sie in Ihrer Visual Studio-Projektmappe **Extras** --> **NuGet Package Manager** -->  **NuGet-Pakete für Projektmappe verwalten** aus. 
* (Visual Studio 2013) Wählen Sie im linken Bereich **Updates** aus, und klicken Sie dann für das Paket **Azure SQL Database Elastic Scale Client Library**, das im Fenster angezeigt wird, auf die Schaltfläche **Aktualisieren**.
* (Visual Studio 2015) Legen Sie das Filterfeld auf **Upgrade verfügbar**fest. Wählen Sie das zu aktualisierende Paket aus, und klicken Sie auf die Schaltfläche **Aktualisieren** .
* (Visual Studio 2017) Wählen Sie im oberen Bereich des Dialogfelds **Updates** aus. Wählen Sie das zu aktualisierende Paket aus, und klicken Sie auf die Schaltfläche **Aktualisieren** .
* Führen Sie die Schritte zur Erstellung und Bereitstellung aus. 

**2. Aktualisieren Sie Ihre Skripts.** Wenn Sie **PowerShell**-Skripts zum Verwalten von Shards verwenden, [laden Sie die neue Bibliotheksversion herunter](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), und kopieren Sie sie in das Verzeichnis, in dem Sie Skripts ausführen. 

**3. Aktualisieren Sie den Split-Merge-Dienst.** Wenn Sie das Split-Merge-Tool für elastische Datenbanken zum Reorganisieren von Shardingdaten verwenden, [laden Sie die neueste Version des Tools herunter und stellen diese bereit](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Detaillierte Upgradeschritte für den Dienst finden Sie [hier](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. Aktualisieren Sie Ihre Shardzuordnungs-Manager-Datenbanken**. Aktualisieren Sie die Metadaten, die Ihre Shard Maps in der Azure SQL-Datenbank unterstützen.  Es gibt hierfür zwei Möglichkeiten: PowerShell oder C#. Beide Optionen werden nachstehend vorgestellt.

***Option 1: Aktualisieren von Metadaten mithilfe von PowerShell***

1. Laden Sie das neueste Befehlszeilendienstprogramm für NuGet [hier](http://nuget.org/nuget.exe) herunter, und speichern Sie es in einem Ordner. 
2. Öffnen Sie eine Eingabeaufforderung, navigieren Sie zum selben Ordner, und geben Sie folgenden Befehl aus: `nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`
3. Navigieren Sie zum Unterordner mit der neuen Client-DLL-Version, die Sie gerade heruntergeladen haben, z. B.: `cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`
4. Laden Sie das Scriptlet für das Clientupgrade der elastischen Datenbank aus dem [Script Center](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)herunter, und speichern Sie es in dem Ordner, der bereits die DLL enthält.
5. Führen Sie in diesem Ordner an der Befehlszeile "PowerShell.\upgrade.ps1" aus, und folgen Sie den Anweisungen.

***Option 2: Aktualisieren von Metadaten mithilfe von C#***

Erstellen Sie alternativ eine Visual Studio-Anwendung, die Ihren ShardMapManager öffnet, alle Shards durchläuft und die Aktualisierung der Metadaten durch Aufrufen der Methoden [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) und [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) durchführt, wie im folgenden Beispiel gezeigt: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 

    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Diese Techniken für die Metadatenaktualisierung können ohne Probleme mehrmals angewendet werden. Wenn beispielsweise eine ältere Clientversion versehentlich einen Shard erstellt, den Sie bereits aktualisiert haben, können Sie die Aktualisierung für alle Shards wiederholen, um sicherzustellen, dass die neueste Metadatenversion in der gesamten Infrastruktur vorhanden ist. 

**Hinweis:** Bis dato veröffentlichte neue Versionen der Clientbibliothek funktionieren weiterhin mit früheren Versionen der Shardzuordnungs-Manager-Metadaten für Azure SQL-Datenbank und umgekehrt.   Doch um einige der neuen Features im neuesten Client nutzen zu können, müssen Metadaten aktualisiert werden.   Beachten Sie, dass Aktualisierungen von Metadaten keine benutzer- oder anwendungsspezifischen Daten betreffen, sondern nur Objekte, die von Shard Map Manager erstellt und verwendet werden.  Zudem setzen Anwendungen ihren Betrieb während der zuvor beschriebenen Upgradesequenz fort. 

## <a name="elastic-database-client-version-history"></a>Elastischer Datenbank-Client – Versionsverlauf
Einen Versionsverlauf erhalten Sie unter [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png


