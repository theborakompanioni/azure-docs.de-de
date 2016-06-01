<properties
	pageTitle="Behandeln von häufigen Verbindungsproblemen mit Azure SQL-Datenbank"
	description="Schritte zum Ermitteln und Behandeln von häufigen Verbindungsproblemen für Azure SQL-Datenbank."
	services="sql-database"
	documentationCenter=""
	authors="dalechen"
	manager="felixwu"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/10/2016"
	ms.author="daleche"/>

# Behandeln von häufigen Verbindungsproblemen mit Azure SQL-Datenbank
Verbindungsprobleme mit Azure SQL-Datenbank können wie folgt grob klassifiziert werden:

- [Vorübergehende Fehler (kurzlebige oder zeitweilige Fehler)](#troubleshoot-the-transient-errors)
- [Dauerhafte oder nicht vorübergehende Fehler (Fehler, die sich regelmäßig wiederholen)](#troubleshoot-the-persistent-errors-non-transient-errors)

Führen Sie bei Verbindungsproblemen die in diesem Artikel beschriebenen Schritte zur Problembehandlung aus.
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## Problembehandlung bei vorübergehenden Fehlern
Wenn in Ihrer Anwendung vorübergehende Fehler auftreten, lesen Sie die folgenden Themen. Darin finden Sie Tipps, wie Sie bei der Problembehandlung vorgehen und die Häufigkeit dieser Fehler reduzieren können:

- [Problembehandlung bei Fehlern wie „Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist nicht verfügbar“ (Fehler: 40613)](sql-database-troubleshoot-connection.md)
- [Durchführen der Problembehandlung, Diagnose und Verhinderung von SQL-Verbindungsfehlern und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>
## Problembehandlung bei dauerhaften Fehlern (nicht vorübergehende Fehler)
Wenn die Anwendung dauerhaft keine Verbindung mit SQL Azure-Datenbank herstellen kann, weist dies in der Regel auf ein Problem mit Folgendem hin:

- Firewall-Konfiguration Die Firewall von Azure SQL-Datenbank oder eine clientseitige Firewall blockiert Verbindungen mit Azure SQL-Datenbank.
- Netzwerkneukonfiguration auf Clientseite: z.B. eine neue IP-Adresse oder ein Proxyserver.
- Benutzerfehler: z.B. falsch geschriebene Verbindungsparameter wie der Servername in der Verbindungszeichenfolge.

### Schritte zum Beheben dauerhafter Verbindungsprobleme
1.	Richten Sie [Firewallregeln](sql-database-configure-firewall-settings.md) so ein, dass die IP-Adresse des Clients zugelassen wird.
2.	Stellen Sie für alle Firewalls zwischen Client und Internet sicher, dass Port 1433 für ausgehende Verbindungen geöffnet ist. Lesen Sie [Konfigurieren der Windows-Firewall für den SQL Server-Zugriff](https://msdn.microsoft.com/library/cc646023.aspx), um weitere Anhaltspunkte zu erhalten.
3.	Überprüfen Sie die Verbindungszeichenfolge und andere Verbindungseinstellungen. Siehe im Thema [Verbindungsprobleme](sql-database-connectivity-issues.md#connections-to-azure-sql-database) den Abschnitt „Verbindungszeichenfolge“.
4.	Überprüfen Sie im Dashboard den Dienststatus. Wenn Sie glauben, dass es sich um einen regionalen Ausfall handelt, finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) Schritte zum Wiederherstellen in einer neuen Region.

<!----HONumber=AcomDC_0511_2016-->