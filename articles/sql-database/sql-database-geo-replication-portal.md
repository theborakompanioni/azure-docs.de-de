<properties 
    pageTitle="Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal | Microsoft Azure" 
    description="Konfigurieren der Georeplikation für eine Azure SQL-Datenbank mit dem Azure-Portal" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="04/27/2016"
    ms.author="sstein"/>

# Konfigurieren der Georeplikation für die Azure SQL-Datenbank mit dem Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [Transact-SQL](sql-database-geo-replication-transact-sql.md)


In diesem Artikel wird beschrieben, wie Sie die Georeplikation für eine SQL-Datenbank mit dem [Azure-Portal](http://portal.azure.com) konfigurieren.

Informationen zum Initiieren eines Failovers finden Sie unter [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Die aktive Georeplikation (lesbare sekundäre Datenbanken) ist jetzt für alle Datenbanken in allen Diensttarifen verfügbar. Im April 2017 wird der nicht lesbare sekundäre Typ eingestellt, und vorhandene nicht lesbare Datenbanken werden automatisch auf lesbare sekundäre Datenbanken aktualisiert.

Sie können bis zu vier lesbare sekundäre Datenbanken in derselben oder in verschiedenen Standorten von Rechenzentren (Regionen) konfigurieren. Sekundäre Datenbanken stehen zur Verfügung, wenn ein Datencenter ausgefallen ist oder keine Verbindung mit der primären Datenbank möglich ist.

Zum Konfigurieren der Georeplikation benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Eine Azure SQL-Datenbank: Die primäre Datenbank, die in eine andere geografische Region repliziert werden soll.



## Hinzufügen einer sekundären Datenbank

Mit den folgenden Schritten wird eine neue sekundäre Datenbank in einer Partnerschaft für die Georeplikation erstellt.

Zum Hinzufügen einer sekundären Datenbank müssen Sie der Besitzer des Abonnements oder der Mitbesitzer sein.

Die sekundäre Datenbank hat den gleichen Namen wie die primäre Datenbank und standardmäßig auch den gleichen Servicelevel. Die sekundäre Datenbank kann lesbar oder nicht lesbar und eine Einzeldatenbank oder eine elastische Datenbank sein. Weitere Informationen finden Sie unter [Dienstebenen](sql-database-service-tiers.md). Nachdem die sekundäre Datenbank erstellt und das Seeding ausgeführt wurde, beginnt die Replikation der Daten von der primären Datenbank in die neue sekundäre Datenbank.

> [AZURE.NOTE] Wenn die Partnerdatenbank bereits vorhanden ist (z. B. aufgrund der Beendigung einer vorherigen Georeplikationsbeziehung), tritt für den Befehl ein Fehler auf.




### Sekundäre Datenbank hinzufügen

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu der Datenbank, die Sie für die Georeplikation einrichten möchten.
2. Wählen Sie auf dem Blatt „SQL-Datenbank“ die Option **Alle Einstellungen** > **Georeplikation**.
3. Wählen Sie die Region für die Erstellung der sekundären Datenbank aus.


    ![Sekundäre Datenbank hinzufügen][1]


4. Konfigurieren Sie den **sekundären Typ** (**Lesbar** oder **Nicht lesbar**)
5. Wählen Sie den Server für die sekundäre Datenbank aus bzw. konfigurieren Sie ihn.

    ![Sekundäre Datenbank erstellen][3]

5. Optional können Sie eine sekundäre Datenbank einem Pool für elastische Datenbanken hinzufügen:

       - Klicken Sie auf **Pool für elastische Datenbanken**, und wählen Sie einen Pool auf dem Zielserver aus, in dem die sekundäre Datenbank erstellt werden soll. Auf dem Zielserver muss bereits ein Pool vorhanden sein, da bei diesem Workflow kein neuer Pool erstellt wird.

6. Klicken Sie auf **Erstellen**, um die sekundäre Datenbank hinzuzufügen.
 
6. Die sekundäre Datenbank wird erstellt, und der Seedingprozess beginnt.
 
    ![Seeding][6]

7. Nach Abschluss des Seedingprozesses wird der Status (Nicht lesbar) für die zweite Datenbank angezeigt.

    ![Sekundäre Datenbank bereit][9]



## Entfernen einer sekundären Datenbank

Mit diesem Vorgang wird die Replikation zur sekundären Datenbank dauerhaft beendet, und die Rolle der sekundären Datenbank wird in eine normale Datenbank mit Lese-/Schreibzugriff geändert. Wenn die Verbindung mit der sekundären Datenbank unterbrochen wird, ist der Befehl zwar erfolgreich, aber die sekundäre Datenbank wird erst mit Lese-/ Schreibzugriff versehen, nachdem die Verbindung wiederhergestellt wurde.

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zur primären Datenbank in der Georeplikationspartnerschaft.
2. Wählen Sie auf dem Blatt „SQL-Datenbank“ die Option **Alle Einstellungen** > **Georeplikation**.
3. Wählen Sie in der Liste **SEKUNDÄRE DATENBANKEN** die Datenbank aus, die Sie aus der Georeplikationspartnerschaft entfernen möchten.
4. Klicken Sie auf **Replikation beenden**.

    ![Sekundäre Datenbank entfernen][7]


5. Wenn Sie auf **Replikation beenden** klicken, wird ein Bestätigungsfenster geöffnet. Klicken Sie auf **Ja**, um die Datenbank aus der Georeplikationspartnerschaft zu entfernen (Festlegung auf eine Datenbank mit Lese-/Schreibzugriff, die nicht Teil einer Replikation ist).


    ![Entfernen bestätigen][8]



   

## Nächste Schritte

- [Initiieren eines geplanten oder ungeplanten Failovers für die Azure SQL-Datenbank](sql-database-geo-replication-failover-portal.md)
- [Entwerfen von Cloudanwendungen zum Sicherstellen der Geschäftskontinuität mithilfe der Georeplikation](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Warnungen zur Notfallwiederherstellung](sql-database-disaster-recovery-drills.md)


## Zusätzliche Ressourcen

- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [Spotlight auf die neuen Georeplikationsfunktionen](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)


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

<!---HONumber=AcomDC_0504_2016-->