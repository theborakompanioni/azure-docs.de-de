---
title: "Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell | Microsoft Docs"
description: "Sie erfahren, wie Sie die Firewall für IP-Adressen mit Zugriff auf Azure SQL-Datenbanken konfigurieren."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 30dcea72-61c1-48b6-8e1d-b1db2eb61567
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 1f7a1f6f4cc970cf12851e48d0f1b3c6ae46279c


---
# <a name="configure-azure-sql-database-server-level-firewall-rules-by-using-powershell"></a>Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe von PowerShell
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-firewall-configure.md)
> * [Azure-Portal](sql-database-configure-firewall-settings.md)
> * [TSQL](sql-database-configure-firewall-settings-tsql.md)
> * [PowerShell](sql-database-configure-firewall-settings-powershell.md)
> * [REST-API](sql-database-configure-firewall-settings-rest.md)
> 
> 

Azure SQL-Datenbank verwendet Firewallregeln, um Verbindungen mit Ihren Servern und Datenbanken zu erlauben. Sie können auf Ihrem SQL-Datenbank-Server Firewalleinstellungen für die Masterdatenbank oder eine Benutzerdatenbank auf Server- und Datenbankebene definieren, um den selektiven Zugriff auf die Datenbank zu ermöglichen.

> [!IMPORTANT]
> Um Anwendungen von Azure die Verbindung mit dem Datenbankserver zu ermöglichen, müssen Azure-Verbindungen aktiviert sein. Weitere Informationen zu Firewallregeln und dem Aktivieren von Verbindungen aus Azure finden Sie unter [Firewall für Azure SQL-Datenbank](sql-database-firewall-configure.md). Wenn Sie Verbindungen innerhalb der Grenzen der Azure-Cloud herstellen möchten, müssen Sie möglicherweise einige zusätzliche TCP-Ports öffnen. Weitere Informationen finden Sie im Abschnitt „SQL-Datenbank V12: ,Außerhalb‘ im Vergleich zu ,Innerhalb‘“ im Artikel [Andere Ports als 1433 für ADO.NET 4.5 und SQL-Datenbank V12](sql-database-develop-direct-route-ports-adonet-v12.md).
> 
> 

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-server-firewall-rules"></a>Erstellen von Serverfirewallregel
Firewallregeln auf Serverebene können mit Azure PowerShell erstellt, aktualisiert und gelöscht werden.

Um eine neue Firewallregel auf Serverebene zu erstellen, führen Sie das Cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) aus. Im folgenden Beispiel wird ein Bereich von IP-Adressen auf dem Server "Contoso" aktiviert.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' -ServerName 'Contoso' -FirewallRuleName "ContosoFirewallRule" -StartIpAddress '192.168.1.1' -EndIpAddress '192.168.1.10'        

Um eine vorhandene Firewallregel auf Serverebene zu ändern, führen Sie das Cmdlet [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx) aus. Im folgenden Beispiel wird der Bereich zulässiger IP-Adressen für die Regel mit dem Namen "ContosoFirewallRule" geändert.

    Set-AzureRmSqlServerFirewallRule -ResourceGroupName 'resourcegroup1' –StartIPAddress 192.168.1.4 –EndIPAddress 192.168.1.10 –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'

Um eine vorhandene Firewallregel auf Serverebene zu löschen, führen Sie das Cmdlet [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx) aus. Im folgenden Beispiel wird die Regel mit dem Namen "ContosoFirewallRule" gelöscht.

    Remove-AzureRmSqlServerFirewallRule –FirewallRuleName 'ContosoFirewallRule' –ServerName 'Contoso'


## <a name="manage-firewall-rules-by-using-powershell"></a>Verwalten von Firewallregeln mithilfe von PowerShell
Sie können auch PowerShell verwenden, um Firewallregeln zu verwalten. Weitere Informationen finden Sie in den folgenden Themen:

* [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx)
* [Remove-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603588\(v=azure.300\).aspx)
* [Set-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603789\(v=azure.300\).aspx)
* [Get-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603586\(v=azure.300\).aspx)

## <a name="next-steps"></a>Nächste Schritte
Informationen zur Verwendung von Transact-SQL zum Erstellen von Firewallregeln auf Server- und Datenbankebene finden Sie unter [Konfigurieren von Firewallregeln auf Serverebene und Datenbankebene für Azure SQL-Datenbank mithilfe von T-SQL](sql-database-configure-firewall-settings-tsql.md).

Informationen zum Erstellen von Firewallregeln auf Serverebene mit anderen Methoden finden Sie unter:

* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe des Azure-Portals](sql-database-configure-firewall-settings.md)
* [Konfigurieren von Firewallregeln auf Serverebene für Azure SQL-Datenbank mithilfe der REST-API](sql-database-configure-firewall-settings-rest.md)

Ein Tutorial zum Erstellen einer Datenbank finden Sie unter [Erstellen einer SQL-Datenbank in Minuten mit dem Azure-Portal](sql-database-get-started.md).
Hilfe beim Herstellen einer Verbindung mit einer Azure SQL-Datenbank über Open-Source-Anwendungen oder Anwendungen von Drittanbietern finden Sie unter [Clientcodebeispiele für die ersten Schritte mit SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee336282.aspx).
Informationen zum Navigieren zu Datenbanken finden Sie unter [Verwalten von Datenbankzugriff und Anmeldesicherheit](https://msdn.microsoft.com/library/azure/ee336235.aspx).

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Sichern der Datenbank](sql-database-security-overview.md)
* [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589)

<!--Image references-->
[1]: ./media/sql-database-configure-firewall-settings/AzurePortalBrowseForFirewall.png
[2]: ./media/sql-database-configure-firewall-settings/AzurePortalFirewallSettings.png
<!--anchors-->



<!--HONumber=Dec16_HO4-->


