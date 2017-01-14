---
title: "Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank | Microsoft Docs"
description: "In „Ändern der Dienstebene und Leistungsstufe einer Azure SQL-Datenbank“ wird beschrieben, wie Sie die Dienstebene und Leistungsstufe Ihrer SQL-Datenbank zentral hoch- oder herunterstufen. Ändern des Tarifs einer Azure SQL-Datenbank."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: cbd67e88-08d5-40e2-a223-0fb0c718a782
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: NA
ms.date: 10/12/2016
ms.author: sstein
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 09c2332589b1170b411c6f45f4109fb8048887e2
ms.openlocfilehash: a4d68b4365c6c5865b19a0b2e9b78afdb7a04014


---
# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-using-the-azure-portal"></a>Ändern der Dienstebene und Leistungsstufe (Tarif) einer SQL-Datenbank mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [**Azure-Portal**](sql-database-scale-up.md)
> * [PowerShell](sql-database-scale-up-powershell.md)
> 
> 

Dienstebenen und Leistungsstufen beschreiben die für Ihre SQL-Datenbank verfügbaren Funktionen und Ressourcen und können aktualisiert werden, wenn sich die Anforderungen an Ihre Anwendung ändern. Weitere Informationen finden Sie unter [Tarife](sql-database-service-tiers.md).

Beachten Sie, dass durch das Ändern des Diensttarifs und/oder der Leistungsstufe einer Datenbank ein Replikat der ursprünglichen Datenbank mit der neuen Leistungsstufe erstellt wird. Infolge wird die Verbindung zu diesem Replikat umgestellt. Während dieses Vorgangs gehen keine Daten verloren. Allerdings sind die Verbindungen zur Datenbank inaktiv, während kurz auf das Replikat umgestellt wird. Daher werden möglicherweise einige aktive Transaktionen zurückgesetzt. Dieses Zeitfenster variiert, ist aber durchschnittlich kleiner als vier Sekunden und in mehr als 99 % der Fälle kürzer als 30 Sekunden. Nur in seltenen Fällen, z.B. wenn im Moment der Verbindungstrennung viele Transaktionen stattfinden, ist dieses Fenster länger.  

Die gesamte des Hochskalierens beträgt in den meisten Fällen weniger als 5 Minuten. 

Bestimmen Sie anhand der Informationen unter [Dienstebenen für SQL-Datenbank für Einzeldatenbanken und Pools für elastische Datenbanken](sql-database-service-tiers.md) die geeignete Dienstebene und Leistungsstufe für Ihre Azure SQL-Datenbank.

* Für ein Downgrade einer Datenbank sollte die Datenbank kleiner als die in der Zieldienstebene maximal zulässige Größe sein. 
* Beim Aktualisieren einer Datenbank, für die [Georeplikation](sql-database-geo-replication-overview.md) aktiviert ist, müssen Sie vor dem Upgrade der primären Datenbank zunächst die zugehörigen sekundären Datenbanken auf die gewünschte Leistungsstufe aktualisieren.
* Beim Downgraden einer Dienstebene müssen Sie zuerst alle Georeplikationsbeziehungen beenden. 
* Die Angebote des Wiederherstellungsdienstes variieren für die verschiedenen Dienstebenen. Wenn Sie ein Downgrade durchführen, verlieren Sie eventuell die Möglichkeit einer Zeitpunktwiederherstellung, oder der Aufbewahrungszeitraum für Sicherungen verkürzt sich. Weitere Informationen finden Sie unter [Sichern und Wiederherstellen der Azure SQL-Datenbank](sql-database-business-continuity.md).
* Durch das Ändern des Tarifs für Ihre Datenbank ändert sich nicht die maximale Datenbankgröße. Verwenden Sie zum Ändern der maximalen Datenbankgröße [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
* Die neuen Eigenschaften für die Datenbank werden erst angewendet, wenn die Änderungen abgeschlossen sind.

**Damit Sie die Anweisungen in diesem Artikel ausführen können, benötigen Sie Folgendes:**

* Eine Azure SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="change-the-service-tier-and-performance-level-of-your-database"></a>Ändern der Dienstebene und Leistungsstufe Ihrer Datenbank
Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie zentral hoch- oder herunterskalieren möchten:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Datenbanken**.
2. Klicken Sie auf die Datenbank, die Sie ändern möchten.
3. Klicken Sie auf dem Blatt **SQL-Datenbank** auf **Tarif (DTUs skalieren)**:
   
   ![Tarif][1]
4. Wählen Sie eine neue Stufe aus, und klicken Sie auf **Auswählen**:
   
   Durch das Klicken auf **Auswählen** wird eine Skalierungsanforderung zum Ändern des Tarifs gesendet. Je nach Größe Ihrer Datenbank kann es einige Zeit dauern, bis der Skalierungsvorgang abgeschlossen ist (siehe Informationen oben in diesem Artikel).
   
   > [!NOTE]
   > Durch das Ändern des Tarifs für Ihre Datenbank ändert sich nicht die maximale Datenbankgröße. Verwenden Sie zum Ändern der maximalen Datenbankgröße [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
   > 
   > 
   
   ![Tarif auswählen][2]
5. Klicken Sie auf das Benachrichtigungssymbol (Glocke), in der oberen rechten Ecke:
   
   ![Benachrichtigungen][3]
   
   Klicken Sie auf den Benachrichtigungstext, um im Detailbereich öffnen, der den Status der Anforderung anzeigt.

## <a name="next-steps"></a>Nächste Schritte
* Ändern Sie die maximale Datenbankgröße mit [Transact-SQL (T-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) oder [PowerShell](https://msdn.microsoft.com/library/mt619433.aspx).
* [Horizontal hoch- und herunterskalieren](sql-database-elastic-scale-get-started.md)
* [Herstellen einer Verbindung mit einer SQL-Datenbank und Abfragen dieser Datenbank mit SSMS](sql-database-connect-query-ssms.md)
* [Exportieren einer Azure SQL-Datenbank](sql-database-export.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!--Image references-->
[1]: ./media/sql-database-scale-up/new-tier.png
[2]: ./media/sql-database-scale-up/choose-tier.png
[3]: ./media/sql-database-scale-up/scale-notification.png
[4]: ./media/sql-database-scale-up/new-tier.png



<!--HONumber=Dec16_HO1-->


