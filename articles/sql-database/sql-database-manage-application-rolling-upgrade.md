<properties
   pageTitle="Cloudbasierte Lösungen für die Notfallwiederherstellung – Aktive Georeplikation in SQL-Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie Sie die Georeplikation von Azure SQL-Datenbank verwenden, um Onlineupgrades Ihrer Cloudanwendung zu unterstützen."
   services="sql-database"
   documentationCenter=""
   authors="anosov1960"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/16/2016"
   ms.author="sashan"/>

# Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank


> [AZURE.NOTE] [Active Geo-Replication](sql-database-geo-replication-overview.md) ist jetzt für alle Datenbanken in allen Tarifen verfügbar.


Erfahren Sie, wie Sie die [Georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank verwenden, um parallele Upgrades Ihrer Cloudanwendung zu aktivieren. Da ein Upgrade einen unterbrechenden Vorgang darstellt, sollte es Teil der Planung und des Designs Ihrer Geschäftskontinuität sein. In diesem Artikel betrachten wir zwei unterschiedliche Methoden zum Orchestrieren des Upgradevorgangs und erörtern die Vor- und Nachteile jeder Option. Für die Zwecke dieses Artikels verwenden wir eine einfache Anwendung, die aus einer mit einer Einzeldatenbank als Datenebene verbundenen Website besteht. Unser Ziel ist es, die Version 1 der Anwendung auf die Version 2 zu aktualisieren, ohne dass sich dies deutlich auf die Endbenutzererfahrung auswirkt.

Beim Evaluieren der Upgradeoptionen sollten Sie die folgenden Faktoren berücksichtigen:

+ Die Auswirkung auf die Verfügbarkeit der Anwendung während Upgrades. Wie lange die Anwendungsfunktion möglicherweise eingeschränkt oder beeinträchtigt ist.
+ Die Möglichkeit, bei einem Upgradefehler ein Rollback durchzuführen.
+ Das Sicherheitsrisiko für die Anwendung, wenn während des Upgrades ein schwerwiegender Fehler auftritt, der nicht in Zusammenhang mit dem Upgrade steht.
+ Die insgesamt anfallenden Kosten. Dies schließt zusätzliche Redundanz und Mehrkosten der vom Upgradevorgang verwendeten temporären Komponenten ein. 

## Upgraden von Anwendungen, die sich auf Datenbanksicherungen für die Notfallwiederherstellung verlassen 

Wenn sich Ihre Anwendung auf automatische Datenbanksicherungen verlässt und die Geowiederherstellung zur Notfallwiederherstellung verwendet, wird sie normalerweise in einer einzelnen Azure-Region bereitgestellt. In diesem Fall umfasst der Upgradevorgang das Erstellen einer Sicherungsbereitstellung aller am Upgrade beteiligter Anwendungskomponenten. Nutzen Sie Azure Traffic Manager mit dem Failoverprofil, um die Endnutzerunterbrechung zu minimieren. Das folgende Diagramm veranschaulicht die Betriebsumgebung vor dem Upgradevorgang. Der Endpunkt <i>contoso-1.azurewebsites.net</i> stellt einen Produktionsslot der Anwendung dar, die upgegradet werden muss. Sie müssen einen Stagingslot mit einer vollständig synchronisierten Kopie der Anwendung erstellen, um ein Rollback des Upgrades zu ermöglichen. Die folgenden Schritte sind erforderlich, um die Anwendung für das Upgrade vorzubereiten:

1.  Erstellen Sie einen Stagingslot für das Upgrade. Hierzu erstellen Sie eine sekundäre Datenbank (1) und stellen eine identische Website in derselben Azure-Region bereit. Überwachen Sie die sekundäre Datenbank, um festzustellen, ob der Seedingprozess abgeschlossen ist.
3.  Erstellen Sie ein Failoverprofil in Azure Traffic Manager mit <i>contoso-1.azurewebsites.net</i> als Onlineendpunkt und <i>contoso-2.azurewebsites.net</i> als Offlineendpunkt. 

> [AZURE.NOTE] Beachten Sie, dass sich die Vorbereitungsschritte nicht auf die Anwendung im Produktionsslot auswirken und diese im Vollzugriffsmodus funktionieren kann.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Wenn die Vorbereitungsschritte abgeschlossen sind, ist die Anwendung bereit für das tatsächliche Upgrade. Das folgende Diagramm veranschaulicht die Schritte, die der Upgradevorgang umfasst.

1. Legen Sie für die primäre Datenbank im Produktionsslot den schreibgeschützten Modus fest (3). Dadurch wird sichergestellt, dass die Produktionsinstanz der Anwendung (V1) während des Upgrades schreibgeschützt bleibt, sodass Datenabweichungen zwischen V1- und V2-Datenbankinstanzen vermieden werden.  
2. Trennen Sie die sekundäre Datenbank mithilfe des Modus für die geplante Beendigung (4). Dieser erstellt eine vollständig synchronisierte, unabhängige Kopie der primären Datenbank. Diese Datenbank wird upgegradet.
3. Ändern Sie den Modus der primären Datenbank in den Lese-/Schreibmodus, und führen Sie das Upgradeskript im Stagingslot aus (5).     

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Wenn das Upgrade erfolgreich abgeschlossen ist, können die Endbenutzer jetzt zu der bereitgestellten Kopie der Anwendung wechseln. Diese wird nun zum Produktionsslot der Anwendung. Dies umfasst ein paar mehr Schritte, wie im folgenden Diagramm dargestellt.

1. Ändern Sie den Onlineendpunkt im Azure Traffic Manager-Profil in <i>contoso-2.azurewebsites.net</i>, der auf die V2-Version der Website verweist (6). Dieser wird nun zum Produktionsslot mit der V2-Anwendung, und der Endbenutzerdatenverkehr wird an ihn umgeleitet.  
2. Wenn Sie die V1-Anwendungskomponenten nicht mehr benötigen, können Sie diese sicher entfernen (7).   

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Wenn der Upgradevorgang nicht erfolgreich ist, z.B. aufgrund eines Fehlers im Upgradeskript, sollte der Stagingslot als gefährdet betrachtet werden. Legen Sie für die Anwendung im Produktionsslot einfach wieder „Vollzugriff“ fest, um sie auf den Status vor dem Upgrade zurückzusetzen. Die Schritte hierzu werden im nächsten Diagramm angezeigt.

1. Legen Sie für die Datenbankkopie den Lese-/Schreibmodus fest (8). Dadurch wird die vollständige V1 im Produktionsslot wiederhergestellt.
2. Führen Sie die Fehlerursachenanalyse durch, und entfernen Sie die gefährdeten Komponenten im Stagingslot (9). 

An dieser Stelle ist die Anwendung voll funktionsfähig, und die Upgradeschritte können wiederholt werden.

> [AZURE.NOTE] Das Rollback erfordert keine Änderungen im Azure Traffic Manager-Profil, da es bereits zu <i>contoso-1.azurewebsites.net</i> als aktiven Endpunkt verweist.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

Der wichtigste **Vorteil** dieser Option ist, dass Sie eine Anwendung in einer einzelnen Region mithilfe einer Reihe von einfachen Schritten upgraden können. Die anfallenden Kosten für das Upgrade sind relativ gering. Der wichtigste **Nachteil** ist, dass im Fall eines schwerwiegenden Fehlers während des Upgrades die Wiederherstellung auf den Status vor dem Upgrade beinhaltet, dass die Anwendung in einer anderen Region erneut bereitgestellt und die Datenbank mithilfe der Geowiederherstellung aus einer Sicherung wiederhergestellt wird. Dieser Prozess führt zu erheblicher Downtime.

## Upgraden von Anwendungen, die sich auf die Datenbank-Georeplikation für die Notfallwiederherstellung verlassen

Wenn Ihre Anwendung für die Geschäftskontinuität die Georeplikation nutzt, wird sie in mindestens zwei verschiedenen Regionen bereitgestellt. In der primären Region erfolgt eine aktive Bereitstellung und in der Sicherungsregion eine Standby-Bereitstellung. Zusätzlich zu den weiter oben genannten Faktoren muss der Upgradevorgang Folgendes garantieren:

+ Die Anwendung bleibt jederzeit während des Upgrades vor schwerwiegenden Fehlern geschützt
+ Die georedundanten Komponenten der Anwendung werden parallel mit den aktiven Komponenten upgegradet

Zum Erreichen dieser Ziele nutzen Sie Azure Traffic Manager, indem Sie das Failoverprofil mit einem aktiven Endpunkt und drei Sicherungsendpunkten verwenden. Das folgende Diagramm veranschaulicht die Betriebsumgebung vor dem Upgradevorgang. Die Websites <i>contoso-1.azurewebsites.net</i> und <i>contoso-dr.azurewebsites.net</i> stellen einen Produktionsslot der Anwendung mit vollständiger geografischer Redundanz dar. Sie müssen einen Stagingslot mit einer vollständig synchronisierten Kopie der Anwendung erstellen, um ein Rollback des Upgrades zu ermöglichen. Der Stagingslot muss ebenfalls georedundant sein, da Sie sicherstellen müssen, dass die Anwendung im Fall eines schwerwiegenden Fehlers während des Upgradevorgangs schnell wiederhergestellt werden kann. Die folgenden Schritte sind erforderlich, um die Anwendung für das Upgrade vorzubereiten:

1.  Erstellen Sie einen Stagingslot für das Upgrade. Hierzu erstellen Sie eine sekundäre Datenbank (1) und stellen eine identische Kopie der Website in derselben Azure-Region bereit. Überwachen Sie die sekundäre Datenbank, um festzustellen, ob der Seedingprozess abgeschlossen ist.
2.  Erstellen Sie im Stagingslot eine georedundante sekundäre Datenbank, indem Sie eine Georeplikation der sekundären Datenbank auf die Sicherungsregion durchführen (dies wird „verkettete Georeplikation“ genannt). Überwachen Sie die sekundäre Sicherungsdatenbank, um festzustellen, ob der Seedingprozess abgeschlossen ist (3).
3.  Erstellen Sie eine Standby-Kopie der Website in der Sicherungsregion, und verknüpfen Sie diese mit der georedundanten sekundären Datenbank (4).  
4.  Fügen Sie dem Failoverprofil in Azure Traffic Manager die zusätzlichen Endpunkte <i>contoso-2.azurewebsites.net</i> und <i>contoso-3.azurewebsites.net</i> als Offlineendpunkte hinzu (5). 

> [AZURE.NOTE] Beachten Sie, dass sich die Vorbereitungsschritte nicht auf die Anwendung im Produktionsslot auswirken und diese im Vollzugriffsmodus funktionieren kann.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Wenn die Vorbereitungsschritte abgeschlossen sind, ist der Stagingslot bereit für das Upgrade. Das folgende Diagramm veranschaulicht die Upgradeschritte.

1. Legen Sie für die primäre Datenbank im Produktionsslot den schreibgeschützten Modus fest (6). Dadurch wird sichergestellt, dass die Produktionsinstanz der Anwendung (V1) während des Upgrades schreibgeschützt bleibt, sodass Datenabweichungen zwischen V1- und V2-Datenbankinstanzen vermieden werden.  
2. Trennen Sie die sekundäre Datenbank in derselben Region mithilfe des Modus für die geplante Beendigung (7). Dieser erstellt eine vollständig synchronisierte, unabhängige Kopie der primären Datenbank, die nach Abschluss automatisch zu einer primären Datenbank wird. Diese Datenbank wird upgegradet.
3. Ändern Sie den Modus der primären Datenbank im Stagingslot in den Lese-/Schreibmodus, und führen Sie das Upgradeskript aus (8).    

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Wenn das Upgrade erfolgreich abgeschlossen ist, können die Endbenutzer jetzt zu der V2-Version der Anwendung wechseln. Das folgende Diagramm veranschaulicht die dafür notwendigen Schritte.

1. Ändern Sie den aktiven Endpunkt im Azure Traffic Manager-Profil in <i>contoso-2.azurewebsites.net</i>, der nun auf die V2-Version der Website verweist (9). Dieser wird nun zu einem Produktionsslot mit der V2-Anwendung, und der Endbenutzerdatenverkehr wird an ihn umgeleitet. 
2. Wenn Sie die V1-Anwendung nicht mehr benötigen, können Sie diese sicher entfernen (10 und 11).  

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Wenn der Upgradevorgang nicht erfolgreich ist, z.B. aufgrund eines Fehlers im Upgradeskript, sollte der Stagingslot als gefährdet betrachtet werden. Verwenden Sie die Anwendung im Produktionsslot einfach wieder mit dem Vollzugriff, um sie auf den Status vor dem Upgrade zurückzusetzen. Die Schritte hierzu werden im nächsten Diagramm angezeigt.

1. Legen Sie für die Kopie der primären Datenbank im Produktionsslot den Lese-/Schreibmodus fest (12). Dadurch wird die vollständige V1 im Produktionsslot wiederhergestellt.
2. Führen Sie die Fehlerursachenanalyse durch, und entfernen Sie die gefährdeten Komponenten im Stagingslot (13 und 14). 

An dieser Stelle ist die Anwendung voll funktionsfähig, und die Upgradeschritte können wiederholt werden.

> [AZURE.NOTE] Das Rollback erfordert keine Änderungen im Azure Traffic Manager-Profil, da es bereits zu <i>contoso-1.azurewebsites.net</i> als aktiven Endpunkt verweist.

![Konfiguration der SQL-Datenbank-Georeplikation Cloudbasierte Notfallwiederherstellung](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

Der wichtigste **Vorteil** dieser Option ist, dass Sie die Anwendung und ihre georedundante Kopie parallel upgraden können, ohne Ihre Geschäftskontinuität während des Upgrades zu beeinträchtigen. Der wichtigste **Nachteil** ist, dass die Option von jeder Anwendungskomponente die doppelte Redundanz erfordert und daher höhere Kosten anfallen. Sie umfasst auch einen komplexeren Workflow.

## Zusammenfassung

Die beiden in diesem Artikel beschriebenen Upgrademethoden unterscheiden sich hinsichtlich der Komplexität und der anfallenden Kosten. Beide legen jedoch den Fokus auf die Minimierung der Zeit, in der die Endbenutzer auf schreibgeschützte Vorgänge eingeschränkt sind. Diese Zeit wird direkt durch die Dauer des Upgradeskripts definiert. Sie hängt nicht von der Datenbankgröße, der gewählten Dienstebene, der Websitekonfiguration oder anderen Faktoren ab, die Sie nicht einfach steuern können. Das liegt daran, dass alle Vorbereitungsschritte von den Upgradeschritten entkoppelt sind und ohne Auswirkungen auf die Produktionsanwendung durchgeführt werden können. Die Effizienz des Upgradeskripts ist der wichtigste Faktor, der die Endbenutzerfreundlichkeit während Upgrades bestimmt. Die beste Möglichkeit diese zu verbessern ist daher, sich darauf zu konzentrieren, das Upgradeskript so effizient wie möglich zu gestalten.


## Nächste Schritte
Auf den folgenden Seiten können Sie sich über die speziellen Vorgänge informieren, die zum Implementieren des Upgradeworkflows erforderlich sind:

- [Hinzufügen einer sekundären Datenbank ](https://msdn.microsoft.com/library/azure/mt603689.aspx) 
- [Failover database to secondary (Failover für die Datenbank in eine sekundäre Datenbank)](https://msdn.microsoft.com/library/azure/mt619393.aspx)
- [Trennen der Georeplikationsverbindung mit der sekundären Datenbank](https://msdn.microsoft.com/library/azure/mt603457.aspx)
- [Geo-restore database (Geowiederherstellung der Datenbank)](https://msdn.microsoft.com/library/azure/mt693390.aspx) 
- [Drop database (Verwerfen der Datenbank)](https://msdn.microsoft.com/library/azure/mt619368.aspx)
- [Copy database (Kopieren der Datenbank)](https://msdn.microsoft.com/library/azure/mt603644.aspx)
- [Festlegen des schreibgeschützten bzw. des Lese-/Schreibmodus für die Datenbank](https://msdn.microsoft.com/library/bb522682.aspx)

## Weitere Ressourcen

- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0622_2016-->