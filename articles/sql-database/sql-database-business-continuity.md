---
title: "Geschäftskontinuität in der Cloud – Datenbankwiederherstellung – SQL-Datenbank | Microsoft Docs"
description: "Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden."
keywords: "Geschäftskontinuität,Geschäftskontinuität in der Cloud,Notfallwiederherstellung von Datenbanken,Datenbankwiederherstellung"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/25/2017
ms.author: sashan
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 4963598837b71e812ad3750aad9d20c8460111d9
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank

Diese Übersicht beschreibt die Funktionen, die Azure SQL-Datenbank für Geschäftskontinuität und Notfallwiederherstellung bereitstellt. Erfahren Sie etwas über Optionen, Empfehlungen und Tutorials für die Wiederherstellung nach Störungen, die Datenverluste nach sich ziehen oder dazu führen können, dass Ihre Datenbank und Ihre Anwendungen nicht mehr verfügbar sind. Erfahren Sie, was zu tun ist, wenn ein Benutzer- oder Anwendungsfehler die Datenintegrität gefährdet, eine Azure-Region ausfällt oder Wartungsaufgaben für Ihre Anwendung ausgeführt werden müssen.

## <a name="sql-database-features-that-you-can-use-to-provide-business-continuity"></a>Funktionen von SQL-Datenbank zum Sicherstellen der Geschäftskontinuität

SQL-Datenbank bietet verschiedene Funktionen für die Sicherstellung der Geschäftskontinuität, einschließlich automatisierter Sicherungen und optionaler Datenbankreplikation. Jede Funktion weist unterschiedliche Eigenschaften für die geschätzte Wiederherstellungszeit (Estimated Recovery Time, ERT) sowie für mögliche Datenverluste bei kürzlich durchgeführten Transaktionen auf. Wenn Sie diese Optionen kennen, können Sie die richtigen Optionen auswählen und in den meisten Szenarien auch miteinander kombinieren. Wenn Sie Ihren Plan für die Geschäftskontinuität entwickeln, müssen Sie ermitteln, wie viel Zeit maximal vergehen darf, bis die Anwendung nach einer Störung vollständig wiederhergestellt ist – diese Zeitspanne ist Ihre RTO (Recovery Time Objective). Sie müssen auch herausfinden, wie viele kürzlich durchgeführte Datenupdates (in einem bestimmten Zeitraum) verloren gehen dürfen, wenn die Anwendung nach einer Störung wiederhergestellt wird – diese Zeitspanne ist Ihre RPO (Recovery Point Objective).

Die folgende Tabelle vergleicht ERT und RPO für die drei häufigsten Szenarien.

| Funktion | Basic-Tarif | Standard-Tarif | Premium-Tarif |
| --- | --- | --- | --- |
| Point-in-Time-Wiederherstellung von Sicherung |Jeder Wiederherstellungspunkt innerhalb von 7 Tagen |Jeder Wiederherstellungspunkt innerhalb von 35 Tagen |Jeder Wiederherstellungspunkt innerhalb von 35 Tagen |
| Geowiederherstellung von georeplizierten Sicherungen |Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h |Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h |Geschätzte Wiederherstellungszeit < 12 h, RPO < 1 h |
| Wiederherstellen von Daten aus dem Azure-Sicherungstresor |Geschätzte Wiederherstellungszeit < 12 Stunden, RPO < 1 Woche |Geschätzte Wiederherstellungszeit < 12 Stunden, RPO < 1 Woche |Geschätzte Wiederherstellungszeit < 12 Stunden, RPO < 1 Woche |
| Aktive Georeplikation |Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s |Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s |Geschätzte Wiederherstellungszeit < 30 s, RPO < 5 s |

### <a name="use-database-backups-to-recover-a-database"></a>Verwenden von Datenbanksicherungen zum Wiederherstellen einer Datenbank

SQL-Datenbank führt automatisch eine Kombination aus wöchentlichen vollständigen Datenbanksicherungen, stündlichen differenziellen Datenbanksicherungen sowie Transaktionsprotokollsicherungen im Abstand von fünf bis zehn Minuten durch, um Ihr Unternehmen vor Datenverlusten zu schützen. Diese Sicherungen werden in georedundantem Speicher gespeichert – für Datenbanken in den Tarifen „Standard“ und „Premium“ 35 Tage lang, für Datenbanken im Tarif „Basic“ sieben Tage lang. Weitere Informationen finden Sie unter [Tarife](sql-database-service-tiers.md). Wenn der Aufbewahrungszeitraum für Ihren Tarif Ihre Geschäftsanforderungen nicht erfüllt, können Sie diesen verlängern, indem Sie [den Tarif wechseln](sql-database-service-tiers.md). Die vollständigen und differenziellen Datenbanksicherungen werden auch in ein [gekoppeltes Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert, um weiteren Schutz bei Rechenzentrumsausfällen zu bieten. Weitere Informationen finden Sie unter [Automatische Datenbanksicherungen](sql-database-automated-backups.md).

Wenn die integrierte Aufbewahrungsdauer für Ihre Anwendung nicht ausreicht, können Sie diese verlängern, indem Sie eine langfristige Aufbewahrungsrichtlinie für Datenbanken konfigurieren. Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

Sie können diese automatischen Datenbanksicherungen verwenden, um eine Datenbank nach verschiedenen Störungen und Unterbrechungen wiederherzustellen, sowohl innerhalb Ihres eigenen Rechenzentrums als auch in einem anderen Rechenzentrum. Bei automatischen Datenbanksicherungen hängt die geschätzte Wiederherstellungszeit von verschiedenen Faktoren ab, beispielsweise von der Gesamtzahl von Datenbanken, die gleichzeitig in der gleichen Region wiederhergestellt werden müssen, von der Größe der Datenbank und der Transaktionsprotokolle sowie von der Netzwerkbandbreite. Die Wiederherstellungszeit beträgt für gewöhnlich weniger als 12 Stunden. Bei der Wiederherstellung in eine andere Datenregion ist der potenzielle Datenverlust aufgrund der georedundanten Speicherung der stündlichen differenziellen Datensicherungen auf eine Stunde begrenzt.

> [!IMPORTANT]
> Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ oder Besitzer des Abonnements sein. Informationen hierzu finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Sie können das Azure-Portal, PowerShell oder die REST-API zur Wiederherstellung verwenden. Die Nutzung von Transact-SQL ist nicht möglich.
>

Verwenden Sie automatisierte Sicherungen als Mechanismus für Geschäftskontinuität und Wiederherstellung, wenn Folgendes auf Ihre Anwendung zutrifft:

* Es handelt sich nicht um eine geschäftskritische Anwendung.
* Es ist keine bindende SLA vorhanden – eine Ausfallzeit von 24 Stunden oder länger zieht keine finanziellen Verpflichtungen nach sich.
* Daten ändern sich langsam (nur wenige Transaktionen pro Stunde), und der Verlust aller innerhalb einer Stunde erfolgten Datenänderungen ist akzeptabel.
* Die Kosten spielen eine große Rolle.

Wenn Sie eine schnellere Wiederherstellung benötigen, verwenden Sie die [aktive Georeplikation](sql-database-geo-replication-overview.md) (im nächsten Abschnitt beschrieben). Wenn Sie Daten, die älter sind als 35 Tage, wiederherstellen müssen, verwenden Sie die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-to-reduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Verwenden von aktiver Georeplikation und automatischen Failovergruppen (in der Vorschau) zum Verkürzen der Wiederherstellungszeit und Begrenzen von Datenverlusten im Zusammenhang mit einer Wiederherstellung

Zusätzlich zu Datenbanksicherungen zur Wiederherstellung von Datenbanken nach einer Unterbrechung des Geschäftsbetriebs können Sie die [aktive Georeplikation](sql-database-geo-replication-overview.md) verwenden, um eine Datenbank so zu konfigurieren, dass sie über bis zu vier lesbare sekundäre Datenbanken in den Regionen Ihrer Wahl verfügt. Diese sekundären Datenbanken werden mithilfe eines asynchronen Replikationsmechanismus mit der primären Datenbank synchronisiert. Dieses Feature dient zum Schutz vor Unterbrechungen des Geschäftsbetriebs bei einem Rechenzentrumsausfall oder während eines Anwendungsupgrades. Die aktive Georeplikation kann auch verwendet werden, um die Leistung schreibgeschützter Abfragen für geografisch verteilte Benutzer zu verbessern.

Zum Aktivieren des automatisierten und transparenten Failovers organisieren Sie Ihre georeplizierten Datenbanken unter Verwendung des SQL Database-Features [Automatische Failovergruppen](sql-database-geo-replication-overview.md) (in der Vorschau) in Gruppen.

Wenn die primäre Datenbank unerwartet offline geschaltet wird oder Sie sie für Wartungsaktivitäten offline schalten müssen, können Sie schnell eine sekundäre Datenbank zur primären hochstufen (dies wird als Failover bezeichnet) und Anwendungen so konfigurieren, dass sie eine Verbindung mit dieser hochgestuften Datenbank herstellen. Wenn Ihre Anwendung über den Failovergruppenlistener eine Verbindung mit den Datenbanken herstellt, brauchen Sie die Konfiguration der SQL-Verbindungszeichenfolge nach dem Failover nicht zu ändern. Bei einem geplanten Failover entstehen keine Datenverluste. Bei einem ungeplanten Failover können aufgrund der Natur der asynchronen Replikation einige wenige Daten aus Transaktionen verloren gehen, die vor sehr kurzer Zeit erfolgt sind. Mit automatischen Failovergruppen (in der Vorschau) können Sie die Failoverrichtlinie anpassen, um potenzielle Datenverluste zu minimieren. Nach einem Failover können Sie zu einem späteren Zeitpunkt ein Failback durchführen – entweder gemäß einem definierten Zeitplan oder sobald das Rechenzentrum wieder online ist. In allen Fällen können die Benutzer die Datenbank für einen kurzen Zeitraum nicht nutzen und müssen die Verbindung erneut herstellen.

> [!IMPORTANT]
> Um die aktive Georeplikation und automatische Failovergruppen (in der Vorschau) verwenden zu können, müssen Sie entweder der Besitzer des Abonnements sein oder über Administratorberechtigungen in SQL Server verfügen. Zum Konfigurieren und Durchführen des Failovers können Sie das Azure-Portal, PowerShell oder die REST-API mit den Berechtigungen des Azure-Abonnements oder Transact-SQL mit SQL Server-Berechtigungen verwenden.
> 

Verwenden Sie die aktive Georeplikation und automatische Failovergruppen (in der Vorschau), wenn Ihre Anwendung einen Teil der folgenden Kriterien erfüllt:

* Sie ist geschäftskritisch.
* Es ist eine Vereinbarung zum Servicelevel (SLA) vorhanden, die keine Ausfallzeit von 24 Stunden oder länger erlaubt.
* Ausfallzeiten können Kosten aufgrund finanzieller Haftung nach sich ziehen.
* Daten ändern sich sehr schnell, und der Verlust aller innerhalb einer Stunde erfolgten Datenänderungen ist nicht akzeptabel.
* Die zusätzlichen Kosten für die Georeplikation sind niedriger als die potenziellen Kosten aufgrund der finanziellen Haftung und die damit einhergehenden Geschäftsverluste.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Wiederherstellen einer Datenbank nach einem Benutzer- oder Anwendungsfehler
*Niemand ist perfekt! Es kann passieren, dass ein Benutzer versehentlich Daten, eine wichtige Tabelle oder sogar eine ganze Datenbank löscht. Es kann auch vorkommen, dass eine Anwendung aufgrund eines Anwendungsfehlers unbeabsichtigt gültige Daten mit ungültigen Daten überschreibt.

In einem solchen Szenario stehen Ihnen die nachfolgend beschriebenen Wiederherstellungsoptionen zur Verfügung.

### <a name="perform-a-point-in-time-restore"></a>Durchführen einer Point-in-Time-Wiederherstellung
Sie können die automatisierten Sicherungen verwenden, um eine Kopie Ihrer Datenbank auf einen bekanntermaßen fehlerfreien Zeitpunkt wiederherzustellen, vorausgesetzt, dieser Zeitpunkt liegt innerhalb des Aufbewahrungszeitraums für die Datenbank. Nach der Wiederherstellung der Datenbank können Sie entweder die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen oder die benötigten Daten aus der wiederhergestellten Datenbank in die ursprüngliche Datenbank kopieren. Wenn die Datenbank die aktive Georeplikation verwendet, empfiehlt es sich, die erforderlichen Daten aus der wiederhergestellten Kopie in die ursprüngliche Datenbank zu kopieren. Wenn Sie die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen, müssen Sie die aktive Georeplikation neu konfigurieren und synchronisieren (was bei großen Datenbanken sehr lange dauern kann). Beim Wiederherstellen einer Datenbank zum letzten verfügbaren Zeitpunkt wird die Wiederherstellung der Geo-Sekundärdatenbank zu einem beliebigen Zeitpunkt derzeit nicht unterstützt.

Weitere Informationen und die detaillierten Schritte zum Wiederherstellen einer Datenbank auf einen bestimmten Zeitpunkt über das Azure-Portal oder mithilfe von PowerShell finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore). Transact-SQL kann zur Wiederherstellung nicht verwendet werden.

### <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank
Wenn eine Datenbank gelöscht wurde, der logische Server aber noch vorhanden ist, können Sie die gelöschte Datenbank auf den Punkt wiederherstellen, an dem sie gelöscht wurde. Dadurch wird eine Datenbanksicherung auf dem gleichen logischen SQL-Server wiederhergestellt, von dem sie gelöscht wurde. Sie können die Datenbank mit dem ursprünglichen Namen wiederherstellen oder der wiederhergestellten Datenbank einen neuen Namen geben.

Weitere Informationen und die detaillierten Schritte zum Wiederherstellen einer gelöschten Datenbank über das Azure-Portal oder mithilfe von PowerShell finden Sie unter [Wiederherstellen einer gelöschten Datenbank](sql-database-recovery-using-backups.md#deleted-database-restore). Transact-SQL kann zur Wiederherstellung nicht verwendet werden.

> [!IMPORTANT]
> Wenn der logische Server gelöscht wurde, kann eine gelöschte Datenbank nicht wiederhergestellt werden.
>
>

### <a name="restore-from-azure-backup-vault"></a>Wiederherstellen von Daten aus dem Azure-Sicherungstresor
Wenn der Datenverlust außerhalb des aktuellen Aufbewahrungszeitraums für automatisierte Sicherungen aufgetreten und Ihre Datenbank für die langfristige Aufbewahrung konfiguriert ist, können Sie die Daten aus einer wöchentlichen Sicherung im Azure Backup-Tresor in eine neue Datenbank wiederherstellen. Zu diesem Zeitpunkt können Sie entweder die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen oder die benötigten Daten aus der wiederhergestellten Datenbank in die ursprüngliche Datenbank kopieren. Wenn Sie vor einem größeren Anwendungsupgrade eine alte Version Ihrer Datenbank abrufen oder Prüfanforderungen bzw. rechtliche Vorgaben erfüllen müssen, können Sie mithilfe einer im Azure Backup-Tresor gespeicherten vollständigen Sicherung eine Datenbank erstellen.  Weitere Informationen finden Sie unter [Langfristige Aufbewahrung](sql-database-long-term-retention.md).

## <a name="recover-a-database-to-another-region-from-an-azure-regional-data-center-outage"></a>Wiederherstellen einer Datenbank in einer anderen Region nach dem Ausfall eines Rechenzentrums in einer Azure-Region
<!-- Explain this scenario -->

Es kommt zwar sehr selten vor, aber es ist möglich, dass ein Azure-Rechenzentrum ausfällt. Ein solcher Ausfall kann den Geschäftsbetrieb einige wenige Minuten oder mehrere Stunden unterbrechen.

* Eine Möglichkeit ist, einfach zu warten, bis die Datenbank wieder online ist, wenn der Rechenzentrumsausfall behoben wurde. Dies funktioniert bei Anwendungen, bei denen die Datenbank nicht notwendigerweise online sein muss. Beispiele hierfür sind Entwicklungsprojekte oder kostenlose Testversionen, mit denen Sie nicht ständig arbeiten müssen. Wenn ein Rechenzentrum ausfällt, wissen Sie nicht, wie lange der Ausfall dauern kann, daher ist diese Option nur dann in Erwägung zu ziehen, wenn Sie Ihre Datenbank eine Zeit lang nicht benötigen.
* Eine andere Möglichkeit besteht darin, ein Failover in eine andere Datenregion auszuführen (wenn Sie die aktive Georeplikation verwenden) oder eine Datenbank mithilfe georedundanter Datenbanksicherungen wiederherzustellen (Geowiederherstellung). Ein Failover dauert nur wenige Sekunden, die Datenbank-Wiederherstellung aus Sicherungen nimmt mehrere Stunden in Anspruch.

Die Dauer einer Wiederherstellung und die Menge an verlorenen Daten richten sich danach, wie Sie diese Features für die Geschäftskontinuität in Ihrer Anwendung einsetzen. Sie könnten z.B. je nach Anforderungen Ihrer Anwendung eine Kombination aus Datenbanksicherungen und aktiver Georeplikation verwenden. Überlegungen zum Anwendungsentwurf für eigenständige Datenbanken und zum Einsatz elastischer Pools mit diesen Funktionen für die Geschäftskontinuität finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Die nachstehenden Abschnitte bieten eine Übersicht über die Schritte, die zur Wiederherstellung mithilfe von Datenbanksicherungen oder der aktiven Georeplikation erforderlich sind. Weitere Schritte einschließlich der Planungsanforderungen und Schritte nach der Wiederherstellung sowie Informationen zum Simulieren eines Ausfalls, um eine Strategie für die Notfallwiederherstellung zu entwickeln, finden Sie unter [Notfallwiederherstellung für SQL-Datenbank](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Vorbereiten auf einen Ausfall
Unabhängig von der Funktion für die Geschäftskontinuität, die Sie einsetzen werden, müssen Sie folgende Aktivitäten ausführen:

* Ermitteln und Vorbereiten des Zielservers, einschließlich Firewallregeln auf Serverebene, Anmeldedaten und Berechtigungen auf Ebene der Masterdatenbank.
* Bestimmen, wie Clients und Clientanwendungen an den neuen Server umgeleitet werden sollen.
* Dokumentieren weiterer Abhängigkeiten wie z.B. Überwachungseinstellungen und Warnungen.

Wenn die Vorbereitung nicht sorgfältig durchgeführt wird, dauert es länger, Anwendungen nach einem Failover oder einer Datenbank-Wiederherstellung wieder online zu schalten. Zudem werden voraussichtlich weitere Problembehandlungsmaßnahmen zu einem Zeitpunkt erforderlich sein, zu dem Sie bereits unter Stress stehen – eine ungünstige Kombination.

### <a name="fail-over-to-a-geo-replicated-secondary-database"></a>Failover auf eine georeplizierte sekundäre Datenbank
Wenn Sie aktive Georeplikation und automatische Failovergruppen (in der Vorschau) als Wiederherstellungsmechanismus verwenden, können Sie eine Richtlinie für automatisches Failover konfigurieren oder [manuelles Failover](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-database) verwenden. Sobald das Failover initiiert wurde, bewirkt es, dass die sekundäre Datenbank zur neuen primären Datenbank wird und neue Transaktionen aufzeichnen sowie auf Abfragen antworten kann. Hierbei entsteht ein minimaler Datenverlust für noch nicht replizierte Daten. Informationen zum Entwerfen des Failoverprozesses finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Wenn das Rechenzentrum wieder online geschaltet wird, werden die alten primären Datenbanken automatisch wieder mit der neuen primären Datenbank verbunden und damit zu sekundären Datenbanken. Wenn Sie die primäre Datenbank wieder in den ursprünglichen Bereich verschieben müssen, können Sie manuell ein geplantes Failover initiieren (Failback). 
> 

### <a name="perform-a-geo-restore"></a>Ausführen einer Geowiederherstellung
Wenn Sie automatisierte Sicherungen mit georedundanter Speicherreplikation als Wiederherstellungsmechanismus verwenden, [stellen Sie die Datenbank per Geowiederherstellung wieder her](sql-database-disaster-recovery.md#recover-using-geo-restore). Die Wiederherstellung erfolgt in den meisten Fällen innerhalb von 12 Stunden. Es entsteht ein Datenverlust von bis zu einer Stunde – je nachdem, wann die letzte differenzielle Sicherung durchgeführt und repliziert wurde. Solange die Wiederherstellung nicht abgeschlossen ist, kann die Datenbank keine Transaktionen aufzeichnen oder auf Abfragen antworten. Beim Wiederherstellen einer Datenbank zum letzten verfügbaren Zeitpunkt wird die Wiederherstellung der Geo-Sekundärdatenbank zu einem beliebigen Zeitpunkt derzeit nicht unterstützt.

> [!NOTE]
> Wenn das Rechenzentrum wieder online ist, bevor Sie Ihre Anwendung auf die wiederhergestellte Datenbank umstellen, können Sie die Wiederherstellung abbrechen.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Ausführen von Aufgaben nach dem Failover bzw. nach der Wiederherstellung
Nach einer Wiederherstellung müssen Sie unabhängig vom Wiederherstellungsmechanismus folgende zusätzliche Aufgaben ausführen, damit Ihre Anwendungen wieder vollständig einsatzfähig sind und von Ihren Benutzern wieder in vollem Umfang verwendet werden können:

* Umleiten von Clients und Clientanwendungen an den neuen Server
* Sicherstellen, dass geeignete Firewallregeln auf Serverebene vorhanden sind, damit Benutzer eine Verbindung herstellen können (oder Verwenden von [Firewallregeln auf Datenbankebene](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Sicherstellen, dass die geeigneten Anmeldeinformationen und Berechtigungen auf Ebene der Masterdatenbank vorhanden sind (oder Verwenden von [eigenständigen Benutzern](https://msdn.microsoft.com/library/ff929188.aspx))
* Konfigurieren der erforderlichen Überwachung
* Konfigurieren der erforderlichen Warnungen

## <a name="upgrade-an-application-with-minimal-downtime"></a>Durchführen eines Anwendungsupgrades mit minimalen Ausfallzeiten
Zuweilen muss eine Anwendung aufgrund geplanter Wartungsmaßnahmen offline geschaltet werden – beispielsweise für Anwendungsupgrades. Der Artikel [Verwalten von Anwendungsupgrades](sql-database-manage-application-rolling-upgrade.md) beschreibt, wie Sie mithilfe der aktiven Georeplikation parallele Upgrades Ihrer Cloudanwendung ermöglichen und so Ausfallzeiten während Upgrades minimieren. Gleichzeitig können Sie mit dieser Funktion einen Wiederherstellungspfad bereitstellen, falls etwas schiefgeht. 

## <a name="next-steps"></a>Nächste Schritte
Überlegungen zum Anwendungsentwurf für eigenständige Datenbanken und elastische Pools finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für die Notfallwiederherstellung mit elastischen Pools](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

