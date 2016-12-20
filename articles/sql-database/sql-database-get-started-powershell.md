---
title: Einrichten einer neuen SQL-Datenbank mit PowerShell | Microsoft Docs
description: "Es wird beschrieben, wie Sie eine SQL-Datenbank mithilfe von PowerShell erstellen. Allgemeine Datenbankeinrichtungsaufgaben können mithilfe von PowerShell-Cmdlets verwaltet werden."
keywords: Erstellen einer neuen SQL-Datenbank, Datenbankeinrichtung
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 08/19/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: daf63815baaf0843f56668b8f310642dbaaf0e54


---
# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Erstellen einer SQL-Datenbank und Ausführen gängiger Datenbankeinrichtungsaufgaben mithilfe von PowerShell-Cmdlets
> [!div class="op_single_selector"]
> * [Azure-Portal](sql-database-get-started.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> * [C#](sql-database-get-started-csharp.md)
> 
> 

Hier erfahren Sie, wie Sie eine SQL-Datenbank mit PowerShell-Cmdlets erstellen. (Informationen zum Erstellen elastischer Datenbanken finden Sie unter [Erstellen eines neuen Pools für elastische Datenbanken mit PowerShell](sql-database-elastic-pool-create-powershell.md).)

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Datenbankeinrichtung: Erstellen einer Ressourcengruppe, eines Servers und einer Firewallregel
Nachdem Sie Zugriff auf Cmdlets für Ihr ausgewähltes Azure-Abonnement haben, können Sie im nächsten Schritt die Ressourcengruppe mit dem Server einrichten, auf dem die Datenbank erstellt wird. Sie können den nächsten Befehl für einen beliebigen gültigen Speicherort anpassen. Führen Sie **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** aus, um eine Liste mit den gültigen Speicherorten abzurufen.

Führen Sie den folgenden Befehl aus, um eine Ressourcengruppe zu erstellen:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Erstellen eines Servers
SQL-Datenbanken werden innerhalb von Azure SQL-Datenbankservern erstellt. Führen Sie [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx) aus, um einen Server zu erstellen. Der Name des Servers muss für alle Azure SQL-Datenbankserver eindeutig sein. Es wird eine Fehlermeldung angezeigt, wenn der Servername bereits vergeben ist. Sie sollten auch berücksichtigen, dass dieser Befehl mehrere Minuten in Anspruch nehmen kann. Sie können den Befehl bearbeiten und einen beliebigen gültigen Speicherort auswählen. Verwenden Sie hier aber denselben Speicherort wie für die im vorhergehenden Schritt erstellte Ressourcengruppe.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wenn Sie diesen Befehl ausführen, werden Sie zum Eingeben des Benutzernamens und Kennworts aufgefordert. Geben Sie Ihre Azure-Anmeldeinformationen nicht ein. Geben Sie stattdessen den Benutzernamen mit dem dazugehörigen Kennwort ein, der für den Serveradministrator erstellt werden soll. Mit dem Skript am Ende dieses Artikels wird veranschaulicht, wie Sie die Serveranmeldeinformationen im Code festlegen.

Die Serverdetails werden angezeigt, nachdem der Server erfolgreich erstellt wurde.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfigurieren einer Serverfirewallregel für den Zugriff auf den Server
Für den Zugriff auf den Server müssen Sie eine Firewallregel einrichten. Führen Sie den Befehl [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) aus, der die Start- und End-IP-Adressen durch gültige Werte für Ihren Computer ersetzt.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Die Details der Firewallregel werden angezeigt, nachdem die Regel erfolgreich erstellt wurde.

Damit andere Azure-Dienste auf den Server zugreifen können, fügen Sie eine Firewallregel hinzu, und setzen Sie sowohl „StartIpAddress“ als auch „EndIpAddress“ auf „0.0.0.0“. Diese Regel ermöglicht es dem Azure-Datenverkehr, aus jedem Azure-Abonnement auf den Server zuzugreifen.

Weitere Informationen finden Sie unter [Firewall für die Azure SQL-Datenbank](sql-database-firewall-configure.md).

## <a name="create-a-sql-database"></a>Erstellen einer SQL-Datenbank
Sie haben jetzt eine Ressourcengruppe, einen Server und eine Firewallregel konfiguriert, sodass Sie auf den Server zugreifen können.

Mit dem folgenden [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx)-Befehl wird eine (leere) SQL-Datenbank auf der Standard-Dienstebene mit der Leistungsstufe S1 erstellt:

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Die Datenbankdetails werden angezeigt, nachdem die Datenbank erfolgreich erstellt wurde.

## <a name="create-a-sql-database-powershell-script"></a>Erstellen eines SQL-Datenbank-PowerShell-Skripts
Mit dem folgenden PowerShell-Skript wird eine SQL-Datenbank mit allen abhängigen Ressourcen erstellt. Ersetzen Sie alle `{variables}` durch die Werte für Ihr Abonnement und Ihre Ressourcen. (Entfernen Sie beim Festlegen der Werte die geschweiften Klammern **{}**).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation

    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"

    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds

    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip

    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp


    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"

    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo


    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine SQL-Datenbank erstellt und grundlegende Datenbankeinrichtungsaufgaben ausgeführt haben, sind Sie für Folgendes bereit:

* [Verwalten von SQL-Datenbanken mit PowerShell](sql-database-manage-powershell.md)
* [Herstellen einer Verbindung mit einer Azure SQL-Datenbank mit SQL Server Management Studio und Ausführen einer T-SQL-Beispielabfrage](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Azure SQL-Datenbank](https://azure.microsoft.com/documentation/services/sql-database/)




<!--HONumber=Dec16_HO2-->


