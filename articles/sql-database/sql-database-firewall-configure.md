<properties
   pageTitle="Konfigurieren einer SQL Server-Firewall (Übersicht) | Microsoft Azure"
   description="Erfahren Sie, wie eine SQL-Datenbankfirewall mit der Firewallregeln auf Server- und Datenbankebene zum Verwalten des Zugriffs konfigurieren."
   keywords="Datenbankfirewall"
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor="cgronlun"
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/06/2016"
   ms.author="rickbyh"/>

# Konfigurieren von Firewallregeln für Azure SQL-Datenbank – Übersicht


> [AZURE.SELECTOR]
- [Übersicht](sql-database-firewall-configure.md)
- [Azure-Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST-API](sql-database-configure-firewall-settings-rest.md)


Die Microsoft Azure SQL-Datenbank bietet einen relationalen Datenbankdienst für Azure und andere internetbasierte Anwendungen. Zum Schutz Ihrer Daten verhindern Firewalls jeglichen Zugriff auf Ihren Datenbankserver, bis Sie angeben, welche Computer zugriffsberechtigt sind. Die Firewall gewährt den Datenbankzugriff auf der Grundlage der Ursprungs-IP-Adresse der jeweiligen Anforderung.

Zum Konfigurieren der Firewall erstellen Sie Firewallregeln, die Bereiche zulässiger IP-Adressen festlegen. Sie können Firewallregeln auf Server- und Datenbankebene erstellen.

- **Firewallregeln auf Serverebene:** Diese Regeln ermöglichen es Clients, auf den gesamten Azure SQL Server zuzugreifen (also auf alle Datenbanken innerhalb des gleichen logischen Servers). Diese Regeln sind in der **master**-Datenbank gespeichert. Firewallregeln auf Serverebene können über das Portal oder mithilfe von Transact-SQL-Anweisungen konfiguriert werden.
- **Firewallregeln auf Datenbankebene:** Diese Regeln ermöglichen es Clients, auf einzelne Datenbanken innerhalb des Azure SQL-Datenbankservers zuzugreifen. Sie können diese Regeln für jede Datenbank erstellen, und sie werden in den einzelnen Datenbanken gespeichert. (Sie können Firewallregeln auf Datenbankebene für die **Masterdatenbank** erstellen.) Diese Regeln können beim Einschränken des Zugriffs auf bestimmte (sichere) Datenbanken innerhalb desselben logischen Servers hilfreich sein. Firewallregeln auf Datenbankebene können nur mithilfe von Transact-SQL-Anweisungen konfiguriert werden.

**Empfehlung:** Microsoft empfiehlt, nach Möglichkeit Firewallregeln auf Datenbankebene zu verwenden, um die Sicherheit und die Portabilität der Datenbank zu verbessern. Verwenden Sie Firewallregeln auf Serverebene für Administratoren und wenn Sie über viele Datenbanken mit identischen Zugriffsanforderungen verfügen und die Datenbanken nicht einzeln konfigurieren möchten.


## Firewallübersicht

Anfänglich wird jeglicher Transact-SQL-Zugriff auf Ihren Azure SQL Server durch die Firewall blockiert. Damit Sie Ihre Azure SQL Server-Instanz verwenden können, müssen Sie zum Azure-Portal wechseln und eine oder mehrere Firewallregeln auf Serverebene angeben, die Zugriff auf Ihre Azure SQL Server-Instanz ermöglichen. Geben Sie mit den Firewallregeln an, welche IP-Adressbereiche aus dem Internet zulässig sind, und ob Azure-Anwendungen versuchen können, eine Verbindung mit Ihrer Azure SQL Server-Instanz herzustellen.

Um selektiv Zugriff auf nur eine der Datenbanken in Ihrer Azure SQL Server-Instanz zu gewähren, müssen Sie eine Regel auf Datenbankebene für die erforderliche Datenbank erstellen. Geben Sie in der Firewallregel auf Datenbankebene einen IP-Adressbereich an, der außerhalb des Bereichs liegt, der in der Firewallregel auf Serverebene angegeben ist, und stellen Sie sicher, dass die IP-Adresse des Clients innerhalb des auf Datenbankebene angegebenen Bereichs liegt.

Verbindungsversuche über das Internet und Azure müssen zunächst die Firewall passieren, bevor sie Ihren Azure SQL Server oder Ihre SQL-Datenbank erreichen (wie im folgenden Diagramm dargestellt).

   ![Diagramm zur Beschreibung der Firewallkonfiguration][1]

## Herstellen einer Verbindung über das Internet

Wenn ein Computer versucht, über das Internet eine Verbindung mit Ihrem Datenbankserver herzustellen, prüft die Firewall zuerst anhand des vollständigen Satzes von Firewallregeln die ursprüngliche IP-Adresse der Anforderung:

- Liegt die IP-Adresse der Anforderung innerhalb eines der in den Firewallregeln auf Serverebene angegebenen Bereiche, wird die Verbindung mit dem Azure SQL-Datenbankserver gewährt.

- Liegt die IP-Adresse der Anforderung nicht innerhalb eines der in den Firewallregeln auf Serverebene angegebenen Bereiche, werden die Firewallregeln auf Datenbankebene geprüft. Liegt die IP-Adresse der Anforderung innerhalb eines der in den Firewallregeln auf Datenbankebene angegebenen Bereiche, wird nur die Verbindung mit der Datenbank gewährt, die eine entsprechende Regel auf Datenbankebene aufweist.

- Liegt die IP-Adresse der Anforderung nicht innerhalb der in den Firewallregeln auf Serverebene oder Datenbankebene angegebenen Bereiche, schlägt die Verbindungsanforderung fehl.

> [AZURE.NOTE] Um von Ihrem lokalen Computer aus auf die Azure SQL-Datenbank zuzugreifen, stellen Sie sicher, dass die Firewall im Netzwerk und auf dem lokalen Computer eine ausgehende Kommunikation an TCP-Port 1433 zulässt.


## Herstellen einer Verbindung über Azure

Wenn eine Azure-Anwendung versucht, eine Verbindung mit dem Datenbankserver herzustellen, prüft die Firewall, ob Azure-Verbindungen zulässig sind. Eine Firewalleinstellung, bei der die Start- und Endadresse den Wert 0.0.0.0 aufweist, gibt an, dass diese Verbindungen zulässig sind. Ist der Verbindungsversuch nicht zulässig, gelangt die Anforderung nicht zum Azure SQL-Datenbankserver.

> [AZURE.IMPORTANT] Diese Option konfiguriert die Firewall derart, dass alle von Azure ausgehenden Verbindungen zugelassen werden, einschließlich Verbindungen von den Abonnements anderer Kunden. Wenn Sie diese Option auswählen, stellen Sie sicher, dass die Anmelde- und die Benutzerberechtigungen den Zugriff nur auf autorisierte Benutzer beschränken.

Sie können Verbindungen über Azure auf zwei Arten ermöglichen:

- Aktivieren Sie im [Microsoft Azure-Portal](https://portal.azure.com/) beim Erstellen eines neuen Servers das Kontrollkästchen **Azure-Diensten Zugriff auf den Server erlauben**.

- Klicken Sie im [klassischen Portal](http://go.microsoft.com/fwlink/p/?LinkID=161793) auf der Registerkarte **Konfigurieren** eines Servers im Abschnitt **Zulässige Dienste** für **Microsoft Azure-Dienste** auf **Ja**.

## Erstellen der ersten Firewallregel auf Serverebene

Die erste Firewalleinstellung auf Serverebene kann mit dem [Azure-Portal](https://portal.azure.com/) oder programmgesteuert mithilfe der REST-API oder Azure PowerShell erstellt werden. Nachfolgende Firewallregeln auf Serverebene können anhand dieser Methoden sowie über Transact-SQL erstellt und verwaltet werden. Weitere Informationen zu Firewallregeln auf Serverebene finden Sie unter [Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md).

## Erstellen von Firewallregeln auf Datenbankebene

Nachdem Sie die erste Firewall auf Serverebene konfiguriert haben, können Sie den Zugriff auf bestimmte Datenbanken einschränken. Wenn Sie in der Firewallregel auf Datenbankebene einen IP-Adressbereich angeben, der außerhalb des Bereichs liegt, der in der Firewallregel auf Serverebene angegeben ist, können nur die Clients auf die Datenbank zugreifen, die IP-Adressen in dem auf Datenbankebene angegebenen Bereich aufweisen. Es können maximal 128 Firewallregeln auf Datenbankebene für eine Datenbank verwendet werden. Firewallregeln auf Datenbankebene für Master- und Benutzerdatenbanken können über Transact-SQL erstellt und verwaltet werden. Weitere Informationen zum Konfigurieren von Firewallregeln auf Datenbankebene finden Sie unter [sp\_set\_database\_firewall\_rule (Azure SQL-Datenbank)](https://msdn.microsoft.com/library/dn270010.aspx).

## Programmgesteuertes Verwalten von Firewallregeln

Außer im Azure-Portal können Firewallregeln programmgesteuert mithilfe von Transact-SQL, REST-API und Azure PowerShell verwaltet werden. Die folgenden Tabellen beschreiben den Satz von Befehlen, die für jede Methode verfügbar sind.


### Transact-SQL

| Katalogsicht oder gespeicherte Prozedur | Ebene | Beschreibung |
|--------------------------------------------------------------------------------------------|-----------|------------------------------------------------------|
| [sys.firewall\_rules](https://msdn.microsoft.com/library/dn269980.aspx) | Server | Zeigt die aktuellen Firewallregeln auf Serverebene an. |
| [sp\_set\_firewall\_rule](https://msdn.microsoft.com/library/dn270017.aspx) | Server | Erstellt oder aktualisiert Firewallregeln auf Serverebene. |
| [sp\_delete\_firewall\_rule](https://msdn.microsoft.com/library/dn270024.aspx) | Server | Entfernt Firewallregeln auf Serverebene. |
| [sys.database\_firewall\_rules](https://msdn.microsoft.com/library/dn269982.aspx) | Datenbank | Zeigt die aktuellen Firewallregeln auf Datenbankebene an. |
| [sp\_set\_database\_firewall\_rule](https://msdn.microsoft.com/library/dn270010.aspx) | Datenbank | Erstellt oder aktualisiert Firewallregeln auf Datenbankebene. |
| [sp\_delete\_database\_firewall\_rule](https://msdn.microsoft.com/library/dn270030.aspx) | Datenbanken | Entfernt Firewallregeln auf Datenbankebene. |

### REST-API

| API | Ebene | Beschreibung |
|----------------------|--------|------------------------------------------------------------------|
| [List Firewall Rules](https://msdn.microsoft.com/library/azure/dn505715.aspx) | Server | Zeigt die aktuellen Firewallregeln auf Serverebene an. |
| [Create Firewall Rule](https://msdn.microsoft.com/library/azure/dn505712.aspx) | Server | Erstellt oder aktualisiert Firewallregeln auf Serverebene. |
| [Set Firewall Rule](https://msdn.microsoft.com/library/azure/dn505707.aspx) | Server | Aktualisiert die Eigenschaften einer vorhandenen Firewallregel auf Serverebene. |
| [Delete Firewall Rule](https://msdn.microsoft.com/library/azure/dn505706.aspx) | Server | Entfernt Firewallregeln auf Serverebene. |


### Azure PowerShell

| Cmdlet | Ebene | Beschreibung |
|-----------------------------------------------------------------------------------------------------|--------|------------------------------------------------------------------|
| [Get-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546731.aspx) | Server | Gibt die aktuellen Firewallregeln auf Serverebene zurück. |
| [New-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546724.aspx) | Server | Erstellt eine neue Firewallregel auf Serverebene. |
| [Set-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546739.aspx) | Server | Aktualisiert die Eigenschaften einer vorhandenen Firewallregel auf Serverebene. |
| [Remove-AzureSqlDatabaseServerFirewallRule](https://msdn.microsoft.com/library/azure/dn546727.aspx) | Server | Entfernt Firewallregeln auf Serverebene. |

> [AZURE.NOTE] Es kann bis zu fünf Minuten dauern, bis die Änderungen der Firewalleinstellungen wirksam werden.

## Problembehandlung der Datenbankfirewall

Wenn der Zugriff auf den Microsoft Azure SQL-Datenbankdienst nicht das erwartete Verhalten aufweist, sind folgende Punkte zu beachten:

- **Lokale Firewallkonfiguration:** Bevor Ihr Computer auf Azure SQL-Datenbank zugreifen kann, müssen Sie möglicherweise eine Firewallausnahme auf Ihrem Computer für TCP-Port 1433 erstellen. Wenn Sie innerhalb der Grenzen der Azure-Cloud Verbindungen herstellen, müssen Sie möglicherweise zusätzliche Ports öffnen. Weitere Informationen finden Sie im Abschnitt **SQL-Datenbank V12: „Außerhalb“ im Vergleich zu „Innerhalb“** im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Netzwerkadressübersetzung (Network Address Translation, NAT):** Aufgrund von NAT kann sich die IP-Adresse, die Ihr Computer zur Verbindung mit Azure SQL-Datenbank verwendet, möglicherweise von der IP-Adresse unterscheiden, die in den IP-Konfigurationseinstellungen des Computers angezeigt wird. Um die IP-Adresse anzuzeigen, mit der der Computer die Verbindung mit Azure herstellt, melden Sie sich beim Portal an, und navigieren Sie zur Registerkarte **Konfigurieren** des Servers, der Ihre Datenbank hostet. Im Abschnitt **Zulässige IP-Adressen** wird die aktuelle Client-IP-Adresse angezeigt. Klicken Sie zum Hinzufügen zu **Zulässige IP-Adressen** auf **Hinzufügen**, um diesem Computer den Zugriff auf den Server zu ermöglichen.

- **Änderungen an der Zulassungsliste sind noch nicht wirksam:** Änderungen der Firewallkonfiguration für die Azure SQL-Datenbank werden möglicherweise erst nach fünf Minuten wirksam.

- **Die Anmeldung ist nicht autorisiert, oder ein falsches Kennwort wurde verwendet:** Wenn eine Anmeldung über keine Berechtigungen für den Azure SQL-Datenbankserver verfügt, oder das verwendete Kennwort falsch ist, wird die Verbindung mit dem Azure SQL-Datenbankserver verweigert. Durch das Erstellen einer Firewalleinstellung wird Clients lediglich die Möglichkeit gegeben, eine Verbindung mit dem Server herzustellen. Jeder Client muss die erforderlichen Sicherheitsanmeldeinformationen bereitstellen. Weitere Informationen zum Vorbereiten von Anmeldungen finden Sie unter "Verwalten von Datenbanken, Anmeldungen und Benutzern in der Azure SQL-Datenbank.

- **Dynamische IP-Adresse:** Wenn Sie über eine Internetverbindung mit dynamischer IP-Adressierung verfügen und Probleme beim Passieren der Firewall auftreten, können Sie eine der folgenden Lösungen ausprobieren:

 - Fragen Sie Ihren Internetdienstanbieter (ISP) nach dem IP-Adressbereich, der den Clientcomputern zugewiesen ist, mit denen auf den Azure SQL-Datenbankserver zugegriffen wird, und fügen Sie dann den IP-Adressbereich als eine Firewallregel hinzu.

 - Verwenden Sie stattdessen die statische IP-Adressierung für die Clientcomputer, und fügen Sie dann die IP-Adressen als Firewallregeln hinzu.

## Nächste Schritte

Artikel mit Anleitungen zum Erstellen von Firewallregeln auf Serverebene und auf Datenbankebene finden Sie unter:

- [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
- [Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mithilfe von T-SQL](sql-database-configure-firewall-settings-tsql.md)
- [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Tutorial zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md). Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank über Open Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx). Informationen zum Navigieren zu Datenbanken finden Sie unter [Verwalten von Datenbankzugriff und Anmeldesicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).



## Zusätzliche Ressourcen

- [Sichern der Datenbank](sql-database-security.md)
- [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png

<!---HONumber=AcomDC_0907_2016-->