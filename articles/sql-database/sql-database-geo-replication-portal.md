---
title: Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal | Microsoft Docs
description: Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/14/2016
ms.author: sstein

---
# Konfigurieren der Georeplikation für Azure SQL-Datenbank mit dem Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-geo-replication-overview.md)
> * [Azure-Portal](sql-database-geo-replication-portal.md)
> * [PowerShell](sql-database-geo-replication-powershell.md)
> * [T-SQL](sql-database-geo-replication-transact-sql.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie die aktive Georeplikation für eine SQL-Datenbank mit dem [Azure-Portal](http://portal.azure.com) konfigurieren.

Informationen zum Initiieren eines Failovers mit dem Azure-Portal finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank mit dem Azure-Portal](sql-database-geo-replication-failover-portal.md).

> [!NOTE]
> Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert.
> 
> 

Wenn Sie die Georeplikation über das Azure-Portal konfigurieren möchten, benötigen Sie Folgendes:

* Ein Azure-Abonnement.
* Eine Azure SQL-Datenbank: Die primäre Datenbank, die in eine andere geografische Region repliziert werden soll.

## Hinzufügen einer sekundären Datenbank
Mit den folgenden Schritten wird eine neue sekundäre Datenbank in einer Partnerschaft für die Georeplikation erstellt.

Zum Hinzufügen einer sekundären Datenbank müssen Sie der Besitzer des Abonnements oder der Mitbesitzer sein.

Die sekundäre Datenbank hat den gleichen Namen wie die primäre Datenbank und standardmäßig auch den gleichen Servicelevel. Die sekundäre Datenbank kann lesbar oder nicht lesbar und eine Einzeldatenbank oder eine elastische Datenbank sein. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md). Nachdem die sekundäre Datenbank erstellt und das Seeding ausgeführt wurde, beginnt die Replikation der Daten von der primären Datenbank in die neue sekundäre Datenbank.

> [!NOTE]
> Wenn die Partnerdatenbank bereits vorhanden ist (z.B. aufgrund der Beendigung einer vorherigen Georeplikationsbeziehung), tritt für den Befehl ein Fehler auf.
> 
> 

### Sekundäre Datenbank hinzufügen
1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu der Datenbank, die Sie für die Georeplikation einrichten möchten.
2. Wählen Sie auf dem Blatt „SQL-Datenbank“ die Option **Alle Einstellungen** > **Georeplikation**.
3. Wählen Sie die Region für die Erstellung der sekundären Datenbank aus.

    ![Sekundäre Datenbank hinzufügen][1]


1. Konfigurieren Sie den sekundären Typ (**Lesbar** oder **Nicht lesbar**).
2. Wählen Sie den Server für die sekundäre Datenbank aus bzw. konfigurieren Sie ihn.
   
    ![Sekundäre Datenbank erstellen][3]
3. Optional können Sie eine sekundäre Datenbank einem Pool für elastische Datenbanken hinzufügen:
   
       - Klicken Sie auf **Pool für elastische Datenbanken**, und wählen Sie einen Pool auf dem Zielserver aus, in dem die sekundäre Datenbank erstellt werden soll. Auf dem Zielserver muss bereits ein Pool vorhanden sein, da bei diesem Workflow kein neuer Pool erstellt wird.
4. Klicken Sie auf **Erstellen**, um die sekundäre Datenbank hinzuzufügen.
5. Die sekundäre Datenbank wird erstellt, und der Seedingprozess beginnt.
   
    ![Seeding][6]
6. Nach Abschluss des Seedingprozesses wird der Status (Nicht lesbar) für die zweite Datenbank angezeigt.
   
    ![Sekundäre Datenbank bereit][9]

## Entfernen einer sekundären Datenbank
Mit diesem Vorgang wird die Replikation zur sekundären Datenbank dauerhaft beendet, und die Rolle der sekundären Datenbank wird in eine normale Datenbank mit Lese-/Schreibzugriff geändert. Wenn die Verbindung mit der sekundären Datenbank unterbrochen wird, ist der Befehl zwar erfolgreich, aber die sekundäre Datenbank wird erst mit Lese-/ Schreibzugriff versehen, nachdem die Verbindung wiederhergestellt wurde.

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur primären Datenbank in der Georeplikationspartnerschaft.
2. Wählen Sie auf dem Blatt „SQL-Datenbank“ die Option **Alle Einstellungen** > **Georeplikation**.
3. Wählen Sie in der Liste **SEKUNDÄRE DATENBANKEN** die Datenbank aus, die Sie aus der Georeplikationspartnerschaft entfernen möchten.
4. Klicken Sie auf **Replikation beenden**.
   
    ![Sekundäre Datenbank entfernen][7]
5. Wenn Sie auf **Replikation beenden** klicken, wird ein Bestätigungsfenster geöffnet. Klicken Sie auf **Ja**, um die Datenbank aus der Georeplikationspartnerschaft zu entfernen und als Datenbank mit Lese-/Schreibzugriff zu konfigurieren, die nicht Teil einer Replikation ist.

    ![Entfernen bestätigen][8]


## Nächste Schritte
* Weitere Informationen zur aktiven Georeplikation finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

<!--Image references-->
[1]: ./media/sql-database-geo-replication-portal/configure-geo-replication.png
[2]: ./media/sql-database-geo-replication-portal/add-secondary.png
[3]: ./media/sql-database-geo-replication-portal/create-secondary.png
[4]: ./media/sql-database-geo-replication-portal/secondary-type.png
[5]: ./media/sql-database-geo-replication-portal/create.png
[6]: ./media/sql-database-geo-replication-portal/seeding0.png
[7]: ./media/sql-database-geo-replication-portal/remove-secondary.png
[8]: ./media/sql-database-geo-replication-portal/stop-confirm.png
[9]: ./media/sql-database-geo-replication-portal/seeding-complete.png
[10]: ./media/sql-database-geo-replication-portal/failover.png

<!---HONumber=AcomDC_0727_2016-->