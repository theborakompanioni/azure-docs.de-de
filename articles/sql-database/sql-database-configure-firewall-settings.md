---
title: "Azure-Portal: Firewallregeln auf Serverebene für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie Firewallregeln auf Serverebene für IP-Adressen konfigurieren, mit denen über das Azure-Portal auf Azure SQL-Server zugegriffen wird."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
ms.assetid: c3b206b5-af6e-41af-8306-db12ecfc1b5d
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 11/28/2016
ms.author: rickbyh;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2a85b3dc1078bad9e5e2fc0ce0bec7e994b29150
ms.openlocfilehash: 36938e642853d587cd28405f5e518ac9ac0697ac


---
# <a name="create-and-manage-azure-sql-database-server-level-firewall-rules-using-the-azure-portal"></a>Erstellen und Verwalten von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-firewall-configure.md)
> * [Azure-Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 

Mithilfe von Firewallregeln auf Serverebene können Administratoren über eine bestimmte IP-Adresse oder über einen IP-Adressbereich auf einen SQL-Datenbank-Server zugreifen. Firewallregeln auf Serverebene können auch für Benutzer verwendet werden, falls Sie über viele Datenbanken mit identischen Zugriffsanforderungen verfügen und die Datenbanken nicht einzeln konfigurieren möchten. Microsoft empfiehlt, nach Möglichkeit Firewallregeln auf Datenbankebene zu verwenden, um die Sicherheit und die Portabilität der Datenbank zu verbessern. Eine Übersicht über SQL-Datenbank-Firewalls finden Sie unter [Konfigurieren von Firewallregeln für Azure SQL-Datenbank – Übersicht](sql-database-firewall-configure.md).

> [!Note]
> Informationen zu portablen Datenbanken im Kontext der Geschäftskontinuität finden Sie unter [Authentifizierungsanforderungen für die Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
>

[!INCLUDE [Create SQL Database firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Verwalten vorhandener Firewallregeln auf Serverebene über das Azure-Portal
Wiederholen Sie die Schritte zum Verwalten der Firewallregeln auf Serverebene.

* Um den aktuellen Computer hinzuzufügen, klicken Sie auf „Client-IP-Adresse hinzufügen“.
* Um zusätzliche IP-Adressen hinzuzufügen, geben Sie den Regelnamen, die Start-IP-Adresse und die End-IP-Adresse ein.
* Um eine vorhandene Regel zu ändern, klicken Sie auf eines der Felder in der Regel, und ändern Sie den betreffenden Wert.
* Um eine vorhandene Regel zu löschen, zeigen Sie mit Maus auf die Regel, bis ein Kreuz am Ende der Zeile angezeigt wird. Klicken Sie auf das Kreuz, um die Regel zu entfernen.

Klicken Sie zum Speichern der Änderungen auf **Speichern** .

## <a name="next-steps"></a>Nächste Schritte

- Ein Tutorial mit ersten Schritten finden Sie unter [SQL-Datenbank-Tutorial: Erstellen eines Servers, einer Firewallregel auf Serverebene, einer Beispieldatenbank und einer Firewallregel auf Datenbankebene und Herstellen der Verbindung mit SQL Server Management Studio](sql-database-get-started.md).
- Ein Tutorial mit ersten Sicherheitsschritten finden Sie unter [SQL-Datenbank-Tutorial: Erste Schritte für die Sicherheit](sql-database-control-access-sql-authentication-get-started.md).
- Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank über Open Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Um zu erfahren, wie zusätzliche Benutzer erstellt werden, die eine Verbindung mit Datenbanken herstellen können, gehen Sie unter [SQL-Datenbank-Authentifizierung und -Autorisierung: Gewähren von Zugriff](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Sichern der Datenbank](sql-database-security-overview.md)   
* [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)   






<!--HONumber=Jan17_HO3-->


