---
title: Konfigurieren einer Firewallregel auf Serverebene für SQL-Datenbank | Microsoft Docs
description: Hier erfahren Sie, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL Server konfigurieren.
services: sql-database
documentationcenter: ''
author: BYHAM
manager: jhubbard
editor: ''

ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/30/2016
ms.author: rickbyh;carlrab

---
# Konfigurieren einer Firewallregel auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-firewall-configure.md)
> * [Azure-Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Der Azure SQL Server verwendet Firewallregeln, um Verbindungen mit Ihren Servern und Datenbanken zu erlauben. Sie können auf Ihrem logischen Server des Azure SQL-Servers auf Serverebene und Datenbankebene Firewalleinstellungen für die Masterdatenbank oder eine Benutzerdatenbank definieren, um den selektiven Zugriff auf die Datenbank zu ermöglichen. In diesem Thema werden Firewallregeln auf Serverebene behandelt.

> [!IMPORTANT]
> Um Anwendungen von Azure die Verbindung mit dem Azure SQL-Server zu ermöglichen, müssen Azure-Verbindungen aktiviert sein. Informationen zur Funktionsweise der Firewallregeln finden Sie unter [So wird's gemacht: Konfigurieren einer Azure SQL Server-Firewall – Übersicht](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Grenzen der Azure-Cloud herstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-Ports öffnen. Weitere Informationen finden Sie im Abschnitt **SQL-Datenbank V12: "Außerhalb" im Vergleich zu "Innerhalb"** im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md)
> 
> 

**Empfehlung:** Verwenden Sie Firewallregeln auf Serverebene für Administratoren, und wenn Sie über viele Datenbanken mit identischen Zugriffsanforderungen verfügen und die Datenbanken nicht einzeln konfigurieren möchten. Microsoft empfiehlt, nach Möglichkeit Firewallregeln auf Datenbankebene zu verwenden, um die Sicherheit und die Portabilität der Datenbank zu verbessern.

[!INCLUDE [Erstellen einer SQL-Datenbank](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Verwalten vorhandener Firewallregeln auf Serverebene über das Azure-Portal
Wiederholen Sie die Schritte zum Verwalten der Firewallregeln auf Serverebene.

* Um den aktuellen Computer hinzuzufügen, klicken Sie auf „Client-IP-Adresse hinzufügen“.
* Um zusätzliche IP-Adressen hinzuzufügen, geben Sie den Regelnamen, die Start-IP-Adresse und die End-IP-Adresse ein.
* Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert.
* Um eine vorhandene Regel zu löschen, zeigen Sie mit Maus auf die Regel, bis ein Kreuz am Ende der Zeile angezeigt wird. Klicken Sie auf das Kreuz, um die Regel zu entfernen.

Klicken Sie zum Speichern der Änderungen auf **Speichern**.

## Nächste Schritte
Einen Artikel mit einer Anleitung zur Verwendung von Transact-SQL zum Erstellen von Firewallregeln auf Serverebene und Datenbankebene finden Sie unter [Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mit T-SQL](sql-database-configure-firewall-settings-tsql.md).

Artikel mit Anleitungen zum Erstellen von Firewallregeln auf Serverebene mit anderen Methoden finden Sie unter:

* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell](sql-database-configure-firewall-settings-powershell.md)
* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Tutorial zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md). Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank über Open Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx). Informationen zum Navigieren zu Datenbanken finden Sie unter [Verwalten von Datenbankzugriff und Anmeldesicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## Zusätzliche Ressourcen
* [Sichern der Datenbank](sql-database-security.md)
* [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!---HONumber=AcomDC_0831_2016-->