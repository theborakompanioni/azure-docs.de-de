<properties
   pageTitle="Geschäftskontinuität für die Cloud – Datenbankwiederherstellung – SQL-Datenbank | Microsoft Azure"
   description="Erfahren Sie, wie Azure SQL-Datenbank die Geschäftskontinuität in der Cloud sowie die Datenbankwiederherstellung unterstützt und dafür sorgt, dass geschäftskritische Cloudanwendungen in Betrieb gehalten werden."
   keywords="Geschäftskontinuität,Geschäftskontinuität in der Cloud,Notfallwiederherstellung von Datenbanken,Datenbankwiederherstellung"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/20/2016"
   ms.author="carlrab"/>

# Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank

Diese Übersicht beschreibt die Funktionen, die Azure SQL-Datenbank für Geschäftskontinuität und Notfallwiederherstellung bereitstellt. Dieser Artikel bietet Optionen, Empfehlungen und Tutorials für die Wiederherstellung nach Störungen, die Datenverluste nach sich ziehen oder dazu führen könnten, dass Ihre Datenbank und Ihre Anwendungen nicht verfügbar sind. Es wird auch erläutert, was zu tun ist, wenn ein Benutzer- oder Anwendungsfehler die Datenintegrität gefährdet, eine Azure-Region ausfällt oder Wartungsaufgaben für Ihre Anwendung ausgeführt werden müssen.

## SQL-Datenbankfunktionen zum Sicherstellen der Geschäftskontinuität

SQL-Datenbank bietet verschiedene Funktionen für die Sicherstellung der Geschäftskontinuität, einschließlich automatisierter Sicherungen und optionaler Datenbankreplikation. Jede Funktion weist unterschiedliche Eigenschaften für die geschätzte Wiederherstellungszeit (Estimated Recovery Time, ERT) sowie für mögliche Datenverluste bei kürzlich durchgeführten Transaktionen auf. Wenn Sie diese Optionen kennen, können Sie die richtigen Optionen auswählen und in den meisten Szenarien auch miteinander kombinieren. Wenn Sie Ihren Plan für die Geschäftskontinuität entwickeln, müssen Sie ermitteln, wie viel Zeit maximal vergehen darf, bis die Anwendung nach einer Störung vollständig wiederhergestellt ist – diese Zeitspanne ist Ihre RTO (Recovery Time Objective). Sie müssen auch herausfinden, wie viele kürzlich durchgeführte Datenupdates (in einem bestimmten Zeitraum) verloren gehen dürfen, wenn die Anwendung nach einer Störung wiederhergestellt wird – diese Zeitspanne ist Ihre RPO (Recovery Point Objective).

### Verwenden von Datenbanksicherungen zum Wiederherstellen einer Datenbank

SQL-Datenbank führt automatisch eine Kombination aus wöchentlichen vollständigen Datenbanksicherungen, stündlichen differenziellen Datenbanksicherungen sowie Transaktionsprotokollsicherungen im Abstand von 5 Minuten durch, um Ihr Unternehmen vor Datenverlusten zu schützen. Diese Sicherungen werden in lokal redundantem Speicher gespeichert – für Datenbanken der Tarife „Standard“ und „Premium“ 35 Tage lang, für Datenbanken im Tarif „Basic“ 7 Tage lang. Informationen zu Dienstebenen finden Sie unter [Tarife](sql-database-service-tiers.md). Wenn der Aufbewahrungszeitraum für Ihren Tarif Ihre Geschäftsanforderungen nicht erfüllt, können Sie diesen verlängern, indem Sie [den Tarif wechseln](sql-database-scale-up.md). Die vollständigen und differenziellen Datenbanksicherungen werden auch an ein [gekoppeltes Rechenzentrum](../best-practices-availability-paired-regions.md) repliziert, um weiteren Schutz vor Rechenzentrumsausfällen zu bieten. Informationen hierzu finden Sie unter [Automatische Datenbanksicherungen](sql-database-automated-backups.md).

Sie können diese automatischen Datenbanksicherungen verwenden, um eine Datenbank nach verschiedenen Störungen und Unterbrechungen wiederherzustellen, sowohl innerhalb Ihres eigenen Rechenzentrums als auch in einem anderen Rechenzentrum. Bei automatischen Datenbanksicherungen hängt die geschätzte Wiederherstellungszeit von verschiedenen Faktoren ab, beispielsweise von der Gesamtzahl von Datenbanken, die gleichzeitig in der gleichen Region wiederhergestellt werden müssen, von der Größe der Datenbank und der Transaktionsprotokolle sowie von der Netzwerkbandbreite. In den meisten Fällen beträgt die Wiederherstellungszeit weniger als 12 Stunden. Bei der Wiederherstellung in eine andere Datenregion ist der potenzielle Datenverlust aufgrund der georedundanten Speicherung der stündlichen differenziellen Datensicherungen auf eine Stunde begrenzt.

> [AZURE.IMPORTANT] Um eine Wiederherstellung mithilfe von automatisierten Sicherungen durchzuführen, müssen Sie Mitglied der Rolle „SQL Server-Mitwirkender“ oder Besitzer des Abonnements sein. Informationen hierzu finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md). Sie können das Azure-Portal, PowerShell oder die REST-API zur Wiederherstellung verwenden. Die Nutzung von Transact-SQL ist nicht möglich.

Verwenden Sie automatisierte Sicherungen als Mechanismus für Geschäftskontinuität und Wiederherstellung, wenn Folgendes auf Ihre Anwendung zutrifft:

- Es handelt sich nicht um eine geschäftskritische Anwendung.
- Es ist keine bindende SLA vorhanden, sodass eine Ausfallzeit von 24 Stunden oder länger keine Kosten aufgrund finanzieller Haftung nach sich zieht.
- Daten ändern sich langsam (nur wenige Transaktionen pro Stunde), und der Verlust aller innerhalb einer Stunde erfolgten Datenänderungen ist akzeptabel.
- Die Kosten spielen eine große Rolle.

Wenn Sie eine schnellere Wiederherstellung benötigen, verwenden Sie die [aktive Georeplikation](sql-database-geo-replication-overview.md) (im nächsten Abschnitt beschrieben). Wenn Sie in der Lage sein müssen, Daten aus einem Zeitraum wiederherzustellen, der länger als 35 Tage zurückliegt, sollten Sie Ihre Datenbank regelmäßig in eine BACPAC-Datei sichern (eine komprimierte Datei, die Ihr Datenbankschema und die zugehörigen Daten enthält), die entweder in einem Azure-Blobspeicher oder einem anderen Speicherort Ihrer Wahl gespeichert wird. Weitere Informationen zum Erstellen eines transaktionskonsistenten Datenbankarchivs finden Sie unter [Erstellen einer Datenbankkopie](sql-database-copy.md) und [Exportieren der Datenbankkopie](sql-database-export.md).

### Verwenden der aktiven Georeplikation zum Verkürzen der Wiederherstellungszeit und Begrenzen von mit einer Wiederherstellung verknüpften Datenverlusten

Zusätzlich zu Datenbanksicherungen zur Wiederherstellung von Datenbanken nach einer Unterbrechung des Geschäftsbetriebs können Sie die [aktive Georeplikation](sql-database-geo-replication-overview.md) verwenden, um eine Datenbank so zu konfigurieren, dass sie über bis zu vier lesbare sekundäre Datenbanken in den Regionen Ihrer Wahl verfügt. Diese sekundären Datenbanken werden mithilfe eines asynchronen Replikationsmechanismus mit der primären Datenbank synchronisiert. Diese Funktion dient zum Schutz vor Unterbrechungen des Geschäftsbetriebs bei einem Rechenzentrumsausfall oder während eines Anwendungsupgrades. Die aktive Georeplikation kann auch verwendet werden, um die Leistung schreibgeschützter Abfragen für geografisch verteilte Benutzer zu verbessern.

Wenn die primäre Datenbank unerwartet offline geschaltet wird oder Sie sie für Wartungsaktivitäten offline schalten müssen, können Sie schnell eine sekundäre Datenbank zur primären Datenbank hochstufen (dies wird als Failover bezeichnet) und Anwendungen so konfigurieren, dass sie eine Verbindung mit dieser hochgestuften Datenbank herstellen. Bei einem geplanten Failover entstehen keine Datenverluste. Bei einem ungeplanten Failover gehen aufgrund der Natur der asynchronen Replikation einige wenige Daten aus Transaktionen verloren, die vor sehr kurzer Zeit erfolgt sind. Nach einem Failover können Sie zu einem späteren Zeitpunkt ein Failback durchführen – entweder gemäß einem definierten Zeitplan oder sobald das Rechenzentrum wieder online ist. In allen Fällen können die Benutzer die Datenbank für einen kurzen Zeitraum nicht nutzen und müssen die Verbindung erneut herstellen.

> [AZURE.IMPORTANT] Um die aktive Georeplikation verwenden zu können, müssen Sie entweder der Besitzer des Abonnements sein oder über Administratorberechtigungen in SQL Server verfügen. Zum Konfigurieren und Durchführen des Failovers können Sie das Azure-Portal, PowerShell oder die REST-API mit Berechtigungen des Abonnements verwenden. Alternativ dazu können Sie Transact-SQL-Berechtigungen in SQL Server verwenden.

Verwenden Sie die aktive Georeplikation, wenn Ihre Anwendung die folgenden Kriterien erfüllt:

- Sie ist geschäftskritisch.
- Es ist eine Vereinbarung zum Servicelevel (SLA) vorhanden, die keine Ausfallzeit von 24 Stunden oder länger erlaubt.
- Ausfallzeiten ziehen Kosten aufgrund finanzieller Haftung nach sich.
- Daten ändern sich sehr schnell, und der Verlust aller innerhalb einer Stunde erfolgten Datenänderungen ist nicht akzeptabel.
- Die zusätzlichen Kosten für die Georeplikation sind niedriger als die potenziellen Kosten aufgrund der finanziellen Haftung und die damit einhergehenden Geschäftsverluste.

## Wiederherstellen einer Datenbank nach einem Benutzer- oder Anwendungsfehler

Niemand ist perfekt! Es kann passieren, dass ein Benutzer versehentlich Daten, eine wichtige Tabelle oder sogar eine ganze Datenbank löscht. Oder eine Anwendung überschreibt aufgrund eines Anwendungsfehlers unbeabsichtigt gültige Daten mit ungültigen Daten.

In einem solchen Szenario stehen Ihnen die nachfolgend beschriebenen Wiederherstellungsoptionen zur Verfügung.

### Durchführen einer Point-in-Time-Wiederherstellung

Sie können die automatisierten Sicherungen verwenden, um eine Kopie Ihrer Datenbank auf einen bekanntermaßen fehlerfreien Zeitpunkt wiederherzustellen, vorausgesetzt, dieser Zeitpunkt liegt innerhalb des Aufbewahrungszeitraums für die Datenbank. Nach der Wiederherstellung der Datenbank können Sie entweder die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen oder die benötigten Daten aus der wiederhergestellten Datenbank in die ursprüngliche Datenbank kopieren. Wenn die Datenbank die aktive Georeplikation verwendet, empfiehlt es sich, die erforderlichen Daten aus der wiederhergestellten Kopie in die ursprüngliche Datenbank zu kopieren. Wenn Sie die ursprüngliche Datenbank durch die wiederhergestellte Datenbank ersetzen, müssen Sie die aktive Georeplikation neu konfigurieren und synchronisieren (was bei großen Datenbanken sehr lange dauern kann).

Weitere Informationen und die detaillierten Schritte zum Wiederherstellen einer Datenbank auf einen bestimmten Zeitpunkt über das Azure-Portal oder mithilfe von PowerShell finden Sie unter [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore). Transact-SQL kann zur Wiederherstellung nicht verwendet werden.

### Wiederherstellen einer gelöschten Datenbank

Wenn eine Datenbank gelöscht wurde, der logische Server aber noch vorhanden ist, können Sie die gelöschte Datenbank auf den Punkt wiederherstellen, an dem sie gelöscht wurde. Dadurch wird eine Datenbanksicherung auf dem gleichen logischen SQL-Server wiederhergestellt, von dem sie gelöscht wurde. Sie können die Datenbank mit dem ursprünglichen Namen wiederherstellen oder der wiederhergestellten Datenbank einen neuen Namen geben.

Weitere Informationen und die detaillierten Schritte zum Wiederherstellen einer gelöschten Datenbank über das Azure-Portal oder mithilfe von PowerShell finden Sie unter [Wiederherstellen einer gelöschten Datenbank](sql-database-recovery-using-backups.md#deleted-database-restore). Transact-SQL kann zur Wiederherstellung nicht verwendet werden.

> [AZURE.IMPORTANT] Wenn der logische Server gelöscht wurde, kann eine gelöschte Datenbank nicht wiederhergestellt werden.

### Importieren aus einem Datenbankarchiv

Wenn der Datenverlust außerhalb des aktuellen Aufbewahrungszeitraums für automatisierte Sicherungen aufgetreten ist und Sie die Datenbank archiviert haben, können Sie eine [archivierte BACPAC-Datei in eine neue Datenbank importieren](sql-database-import.md). Zu diesem Zeitpunkt können Sie entweder die ursprüngliche Datenbank durch die importierte Datenbank ersetzen oder die benötigten Daten aus der importierten Datenbank in die ursprüngliche Datenbank kopieren.

## Wiederherstellen einer Datenbank in einer anderen Region nach dem Ausfall eines Rechenzentrums in einer Azure-Region

<!-- Explain this scenario -->

Es kommt zwar sehr selten vor, aber es ist möglich, dass ein Azure-Rechenzentrum ausfällt. Ein solcher Ausfall kann den Geschäftsbetrieb einige wenige Minuten oder mehrere Stunden unterbrechen.

- Eine Möglichkeit ist, einfach zu warten, bis die Datenbank wieder online ist, wenn der Rechenzentrumsausfall behoben wurde. Dies funktioniert bei Anwendungen, bei denen die Datenbank nicht notwendigerweise online sein muss. Beispiele hierfür sind Entwicklungsprojekte oder kostenlose Testversionen, mit denen Sie nicht ständig arbeiten müssen. Wenn ein Rechenzentrum ausfällt, wissen Sie nicht, wie lange der Ausfall dauern wird, daher ist diese Option nur dann in Erwägung zu ziehen, wenn Sie Ihre Datenbank eine Zeitlang nicht benötigen.
- Eine andere Möglichkeit besteht darin, ein Failover in eine andere Datenregion auszuführen (wenn Sie die aktive Georeplikation verwenden) oder die Datenbank mithilfe georedundanter Datenbanksicherungen wiederherzustellen (Geowiederherstellung). Ein Failover dauert nur wenige Sekunden, die Wiederherstellung aus Sicherungen nimmt mehrere Stunden in Anspruch.

Die Dauer einer Wiederherstellung und die Menge an verlorenen Daten bei einem Rechenzentrumsausfall richten sich danach, wie Sie die in diesem Artikel beschriebenen Funktionen für die Geschäftskontinuität in Ihrer Anwendung einsetzen. Sie könnten z.B. – je nach Anforderungen Ihrer Anwendung – eine Kombination aus Datenbanksicherungen und aktiver Georeplikation verwenden. Überlegungen zum Anwendungsentwurf für eigenständige Datenbanken und zum Einsatz elastischer Pools mit diesen Funktionen für die Geschäftskontinuität finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

Die folgenden Abschnitte bieten einen Überblick über die Schritte, die zur Wiederherstellung mithilfe von Datenbanksicherungen oder der aktiven Georeplikation erforderlich sind. Weitere Schritte einschließlich der Planungsanforderungen und Schritte nach der Wiederherstellung sowie Informationen zum Simulieren eines Ausfalls, um eine Strategie für die Notfallwiederherstellung zu entwickeln, finden Sie unter [Notfallwiederherstellung für SQL-Datenbank](sql-database-disaster-recovery.md).

### Vorbereiten auf einen Ausfall

Unabhängig von der Funktion für die Geschäftskontinuität, die Sie einsetzen werden, müssen Sie folgende Aktivitäten ausführen:

- Ermitteln und Vorbereiten des Zielservers, einschließlich Firewallregeln auf Serverebene, Anmeldedaten und Berechtigungen auf Ebene der Masterdatenbank.
- Bestimmen, wie Clients und Clientanwendungen an den neuen Server umgeleitet werden sollen.
- Dokumentieren weiterer Abhängigkeiten wie z.B. Überwachungseinstellungen und Warnungen.
 
Wenn Planung und Vorbereitung nicht sorgfältig durchgeführt werden, dauert es länger, Anwendungen nach einem Failover oder einer Wiederherstellung wieder online zu schalten. Zudem werden voraussichtlich weitere Problembehandlungsmaßnahmen zu einem Zeitpunkt erforderlich sein, zu dem Sie bereits unter Stress stehen – eine ungünstige Kombination.

### Failover auf eine georeplizierte sekundäre Datenbank 

Wenn Sie die aktive Georeplikation als Wiederherstellungsmechanismus verwenden, [erzwingen Sie ein Failover auf eine georeplizierte sekundäre Datenbank](sql-database-disaster-recovery.md#failover-to-geo-replicated-secondary-database). Die sekundäre Datenbank wird innerhalb weniger Sekunden zur neuen primären Datenbank hochgestuft und kann neue Transaktionen aufzeichnen und auf Abfragen antworten. Hierbei entsteht ein Datenverlust von nur wenigen Sekunden für die Daten, die noch nicht repliziert wurden. Informationen zum Automatisieren des Failoverprozesses finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [AZURE.NOTE] Wenn das Rechenzentrum wieder online geschaltet wird, können Sie ein Failback auf die ursprüngliche primäre Datenbank ausführen (nicht zwingend erforderlich).

### Ausführen einer Geowiederherstellung 

Wenn Sie automatisierte Sicherungen mit georedundanter Speicherreplikation als Wiederherstellungsmechanismus verwenden, [stellen Sie die Datenbank per Geowiederherstellung wieder her](sql-database-disaster-recovery.md#recover-using-geo-restore). Die Wiederherstellung erfolgt in den meisten Fällen innerhalb von 12 Stunden. Es entsteht ein Datenverlust von bis zu einer Stunde – je nachdem, wann die letzte differenzielle Sicherung durchgeführt und repliziert wurde. Solange die Wiederherstellung nicht abgeschlossen ist, kann die Datenbank keine Transaktionen aufzeichnen oder auf Abfragen antworten.

> [AZURE.NOTE] Wenn das Rechenzentrum wieder online ist, bevor Sie Ihre Anwendung auf die wiederhergestellte Datenbank umstellen, können Sie die Wiederherstellung einfach abbrechen.

### Ausführen von Aufgaben nach dem Failover bzw. nach der Wiederherstellung 

Nach einer Wiederherstellung müssen Sie unabhängig vom Wiederherstellungsmechanismus folgende zusätzliche Aufgaben ausführen, damit Ihre Anwendungen wieder vollständig einsatzfähig sind und von Ihren Benutzern wieder in vollem Umfang verwendet können:

- Umleiten von Clients und Clientanwendungen an den neuen Server
- Sicherstellen, dass geeignete Firewallregeln auf Serverebene vorhanden sind, damit Benutzer eine Verbindung herstellen können (oder Verwenden von [Firewallregeln auf Datenbankebene](sql-database-firewall-configure.md#creating-database-level-firewall-rules))
- Sicherstellen, dass die geeigneten Anmeldeinformationen und Berechtigungen auf Ebene der Masterdatenbank vorhanden sind (oder Verwenden von [eigenständigen Benutzern](https://msdn.microsoft.com/library/ff929188.aspx))
- Konfigurieren der erforderlichen Überwachung
- Konfigurieren der erforderlichen Warnungen

## Durchführen eines Anwendungsupgrades mit minimalen Ausfallzeiten

Zuweilen muss eine Anwendung aufgrund geplanter Wartungsmaßnahmen offline geschaltet werden – beispielsweise für Anwendungsupgrades. Der Artikel [Verwalten von Anwendungsupgrades](sql-database-manage-application-rolling-upgrade.md) beschreibt, wie Sie die aktive Georeplikation verwenden, um parallele Upgrades Ihrer Cloudanwendung zu ermöglichen und so Ausfallzeiten während Upgrades zu minimieren. Gleichzeitig können Sie mit dieser Funktion einen Wiederherstellungspfad bereitstellen, falls etwas schiefgeht. In diesem Artikel werden zwei unterschiedliche Methoden zum Orchestrieren des Upgradevorgangs betrachtet und die Vor- und Nachteile jeder Option erörtert.

## Nächste Schritte

Überlegungen zum Anwendungsentwurf für eigenständige Datenbanken und elastische Pools finden Sie unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) und [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

<!---HONumber=AcomDC_0727_2016-->