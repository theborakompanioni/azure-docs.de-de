<properties 
   pageTitle="Notfallwiederherstellungsverfahren für SQL-Datenbank | Microsoft Azure" 
   description="Hier erhalten Sie Anleitungen und bewährte Methoden für die Verwendung von Azure SQL-Datenbank zum Ausführen von Notfallwiederherstellungsverfahren, mit denen Sie wichtige Geschäftsanwendungen vor Fehlern und Ausfällen schützen." 
   services="sql-database" 
   documentationCenter="" 
   authors="mihaelablendea" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="06/16/2016"
   ms.author="mihaelab"/>

#Ausführen von Notfallwiederherstellungsverfahren

Es wird empfohlen, in regelmäßigen Abständen Überprüfungen der Anwendungsbereitschaft für den Wiederherstellungsworkflow durchzuführen. Überprüfungen des Anwendungsverhaltens und der Auswirkungen von Datenverlusten und/oder Unterbrechungen durch Failover gehören zu einem angemessenen Entwicklungsverfahren. Solche Überprüfungen werden auch für die meisten Branchenstandards im Zuge von Zertifizierungen zur Geschäftskontinuität vorausgesetzt.

Notfallwiederherstellungsverfahren umfassen Folgendes:

- Simulieren von Ausfällen auf Datenebene
- Wiederherstellen 
- Überprüfen der Anwendungsintegrität nach der Wiederherstellung

Abhängig vom [Entwurf Ihrer Anwendung für Geschäftskontinuität](sql-database-business-continuity.md) kann der verbundene Workflow variieren. Im Folgenden beschreiben wir bewährte Methoden zur Ausführung von Notfallwiederherstellungen im Kontext von Azure SQL-Datenbank.

##Geografische Wiederherstellung

Um Datenverluste beim Durchführen von Notfallwiederherstellungsverfahren zu verhindern, empfehlen wir die Verwendung einer Testumgebung, indem Sie eine Kopie der Produktionsumgebung erstellen und diese zum Überprüfen des Failoverworkflows der Anwendung verwenden.
 
####Simulieren von Ausfällen

Um die Ausfallzeit zu simulieren, können Sie die Quelldatenbank löschen oder umbenennen. Dadurch wird ein Verbindungsfehler für die Anwendung verursacht.

####Wiederherstellen

- Führen Sie die geografische Wiederherstellung der Datenbank auf einem anderen Server aus, wie [hier](sql-database-disaster-recovery.md) beschrieben. 
- Ändern Sie die Anwendungskonfiguration zum Herstellen einer Verbindung mit den wiederhergestellten Datenbanken, und folgen Sie der Anleitung unter [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) zum Abschließen der Wiederherstellung.

####Überprüfen

- Führen Sie das Verfahren aus, indem Sie die Anwendungsintegrität nach der Wiederherstellung sicherstellen (Verbindungszeichenfolgen, Anmeldungen, grundlegende Funktionstests oder andere Überprüfungen, die Teil der Standardabmeldeverfahren von Anwendungen sind).

##Georeplikation

Für eine durch Georeplikation geschützte Datenbank beinhaltet das Verfahren ein geplantes Failover auf die sekundäre Datenbank. Durch das geplante Failover wird sichergestellt, dass die primäre und die sekundäre Datenbank beim Wechseln der Rollen synchronisiert bleiben. Im Gegensatz zum ungeplanten Failover führt dieser Vorgang nicht zu Datenverlust, daher kann er auch in einer Produktionsumgebung ausgeführt werden.

####Simulieren von Ausfällen

Um die Ausfallzeit zu simulieren, können Sie die Web-Anwendung oder den virtuellen Computer deaktivieren, die bzw. der mit der Datenbank verbunden ist. Dies führt zu Verbindungsfehlern für die Webclients.

####Wiederherstellen

- Stellen Sie sicher, dass die Anwendungskonfiguration in der Notfallwiederherstellungsregion auf die vorherige sekundäre Datenbank verweist. Diese Datenbank wird zur neuen primären Datenbank, auf die vollständig zugegriffen werden kann. 
- Ausführen eines [geplanten Failovers](sql-database-geo-replication-powershell.md#initiate-a-planned-failover), um die sekundäre Datenbank als neue primäre Datenbank festzulegen
- Führen Sie die Anweisungen unter [Konfigurieren einer Datenbank nach der Wiederherstellung](sql-database-disaster-recovery.md) aus, um die Wiederherstellung abzuschließen.

####Überprüfen

- Führen Sie das Verfahren aus, indem Sie die Anwendungsintegrität nach der Wiederherstellung sicherstellen (Verbindungszeichenfolgen, Anmeldungen, grundlegende Funktionstests oder andere Überprüfungen, die Teil der Standardabmeldeverfahren von Anwendungen sind).


## Nächste Schritte

- Informationen zur Verwendung und Konfiguration der aktiven Georeplikation für die Notfallwiederherstellung finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zur Verwendung der Geowiederherstellung für die Notfallwiederherstellung finden Sie unter [Geowiederherstellung](sql-database-geo-restore.md).

## Zusätzliche Ressourcen

- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0622_2016-->