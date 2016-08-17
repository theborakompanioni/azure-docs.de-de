<properties
	pageTitle="Verwalten von Azure SQL-Datenbanken mit PowerShell | Microsoft Azure"
	description="Verwalten von Azure SQL-Datenbank mit PowerShell."
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="sql-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/07/2016"
	ms.author="sstein"/>

# Verwalten von Azure SQL-Datenbanken mit PowerShell


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-manage-portal.md)
- [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

In diesem Thema werden PowerShell-Befehle vorgestellt, mit denen viele Azure SQL-Datenbank-Aufgaben ausgeführt werden können.

[AZURE.INCLUDE [Starten der PowerShell-Sitzung](../../includes/sql-database-powershell.md)]


## Erstellen einer Ressourcengruppe

Erstellen Sie die Ressourcengruppe, die den Server enthalten soll. Sie können den nächsten Befehl für einen beliebigen gültigen Standort anpassen.

Um eine Liste der gültigen Standorte für SQL-Datenbankserver abzurufen, führen Sie das folgende Cmdlet aus:

	$AzureSQLLocations = (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Sql'}).Locations

Wenn Sie bereits über eine Ressourcengruppe verfügen, können Sie mit dem nächsten Abschnitt („Erstellen eines Servers“) fortfahren, oder Sie können den folgenden Befehl bearbeiten und ausführen, um eine neue Ressourcengruppe zu erstellen:

	New-AzureRmResourceGroup -Name "resourcegroupJapanWest" -Location "Japan West"

## Erstellen eines Servers

Um einen neuen Server der Version 12 zu erstellen, verwenden Sie das Cmdlet [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx). Ersetzen Sie *server12* durch den Namen Ihres Servers. Es wird eine Fehlermeldung angezeigt, wenn der Servername bereits vergeben ist. Sie sollten auch berücksichtigen, dass dieser Befehl mehrere Minuten in Anspruch nehmen kann. Die Serverdetails und die PowerShell-Eingabeaufforderung werden angezeigt, nachdem der Server erfolgreich erstellt wurde. Sie können den Befehl für einen beliebigen gültigen Speicherort anpassen.

	New-AzureRmSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -Location "Japan West" -ServerVersion "12.0"

Wenn Sie diesen Befehl ausführen, werden Sie zum Eingeben des Benutzernamens und Kennworts aufgefordert. Geben Sie Ihre Azure-Anmeldeinformationen hier nicht ein. Geben Sie stattdessen die Kombination aus Benutzername und Kennwort ein, die Sie als Anmeldeinformationen für das Administratorkonto des neuen Servers erstellen möchten.

## Erstellen einer Server-Firewallregel

Um eine Firewallregel für den Zugriff auf den Server zu erstellen, verwenden Sie den Befehl [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx). Führen Sie den folgenden Befehl aus, wobei Sie die Start- und End-IP-Adresse durch gültige Werte für Ihren Client ersetzen.

Wenn Ihr Server den Zugriff auf andere Azure-Dienste zulassen muss, fügen Sie den Schalter **-AllowAllAzureIPs** hinzu. Damit wird eine spezielle Firewallregel hinzugefügt und dem gesamten Azure-Datenverkehr wird der Zugriff auf den Server gestattet.

	New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -FirewallRuleName "clientFirewallRule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

Weitere Informationen finden Sie unter [Firewall für die Azure SQL-Datenbank](https://msdn.microsoft.com/library/azure/ee621782.aspx).

## Erstellen einer SQL-Datenbank

Um eine Datenbank zu erstellen, verwenden Sie den Befehl [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx). Sie benötigen einen Server, um eine Datenbank zu erstellen. Im folgenden Beispiel wird eine SQL-Datenbank mit dem Namen "TestDB12" erstellt. Die Datenbank wird als Standard-S1-Datenbank erstellt.

	New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S1"


## Ändern der Leistungsebene einer SQL-Datenbank

Sie können Ihre Datenbank mit dem Befehl [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) hoch- oder herunterskalieren. Im folgenden Beispiel wird eine SQL-Datenbank mit dem Namen "TestDB12" aus der aktuellen Leistungsebene auf eine Standard-S3-Ebene hochskaliert.

	Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S3"


## Löschen einer SQL-Datenbank

Sie können eine SQL-Datenbank mit dem Befehl [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) löschen. Im folgenden Beispiel wird eine SQL-Datenbank mit dem Namen "TestDB12" gelöscht.

	Remove-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12"

## Löschen eines Servers

Sie können auch einen Server mit dem Befehl [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) löschen. Im folgenden Beispiel wird ein Server mit dem Namen "server12" gelöscht.


>[AZURE.NOTE]  Der Löschvorgang ist asynchron und kann einige Zeit dauern. Vergewissern Sie sich, dass der Vorgang abgeschlossen ist, bevor Sie weitere Vorgänge ausführen, für die der Server vollständig gelöscht sein muss – beispielsweise, wenn Sie einen neuen Server mit dem gleichen Namen erstellen möchten.


	Remove-AzureRmSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12"




## Nächste Schritte

Kombinieren und Automatisieren von Befehlen. Um beispielsweise einen Server, eine Firewallregel und eine Datenbank zu erstellen, ersetzen Sie den gesamten Text innerhalb der Anführungszeichen, einschließlich der Zeichen „<“ und „>“, durch Ihre Werte:


    New-AzureRmResourceGroup -Name "<resourceGroupName>" -Location "<Location>"
    New-AzureRmSqlServer -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -Location "<Location>" -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -FirewallRuleName "<firewallRuleName>" -StartIpAddress "<192.168.0.198>" -EndIpAddress "<192.168.0.199>"
    New-AzureRmSqlDatabase -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -DatabaseName "<databaseName>" -Edition <Standard> -RequestedServiceObjectiveName "<S1>"

## Verwandte Informationen

- [Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx)

<!---HONumber=AcomDC_0803_2016-->