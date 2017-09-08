---
title: Verbindungsarchitektur von Azure SQL-Datenbank | Microsoft-Dokumentation
description: "In diesem Artikel wird die Verbindungsarchitektur von Azure-SQLDB aus Azure oder von außerhalb von Azure erläutert."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 524804c972ee3a5e97ebc756628dbf7ef5ab720d
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---
# <a name="azure-sql-database-connectivity-architecture"></a>Verbindungsarchitektur der Azure SQL-Datenbank 

In diesem Artikel wird die Verbindungsarchitektur der Azure SQL-Datenbank erläutert. Zudem wird dargelegt, wie die verschiedenen Komponenten Datenverkehr an Ihre Instanz von Azure SQL-Datenbank leiten. Diese Verbindungskomponenten von Azure SQL-Datenbank leiten Netzwerkdatenverkehr mithilfe von Clients, die von innerhalb und von außerhalb von Azure verbunden sind, an die Azure-Datenbank. Dieser Artikel bietet auch Skriptbeispiele zum Ändern der Verbindungsart und Überlegungen, die beim Ändern der Standardverbindungseinstellungen berücksichtigt werden können. Wenn Sie noch weitere Fragen haben, nachdem Sie diesen Artikel gelesen haben, wenden Sie sich an Dhruv unter dmalik@microsoft.com. 

## <a name="connectivity-architecture"></a>Verbindungsarchitektur

Das folgende Diagramm bietet einen allgemeinen Überblick über die Verbindungsarchitektur von Azure SQL-Datenbank. 

![Architekturübersicht](./media/sql-database-connectivity-architecture/architecture-overview.png)


In den folgenden Schritte wird beschrieben, wie eine Verbindung zu einer Azure SQL-Datenbank mithilfe des Software Load Balancers (SLB) von Azure SQL-Datenbank und des Gateways von Azure SQL-Datenbank hergestellt wird.

- Clients innerhalb von Azure oder außerhalb von Azure stellen eine Verbindung mit dem SLB her, der über eine öffentliche IP-Adresse verfügt und auf dem Port 1433 lauscht.
- Der SLB leitet Datenverkehr an das Azure SQL-Datenbank-Gateway.
- Das Gateway leitet den Datenverkehr an die korrekte Proxy-Middleware.
- Die Proxy-Middleware leitet den Datenverkehr an die entsprechende Azure SQL-Datenbank.

> [!IMPORTANT]
> In jede dieser Komponenten ist Denial-of Service-Schutz (DDoS) auf Ebene des Netzwerks und der App integriert.
>

## <a name="connectivity-from-within-azure"></a>Verbindung aus Azure

Wenn Sie eine Verbindung aus Azure herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie **Redirect**. Die Verbindungsrichtlinie **Redirect** bedeutet, dass Verbindungen nach TCP-Sitzungen mit der Azure-Datenbank hergestellt werden. Anschließend wird die Clientsitzung an die Proxy-Middleware weitergeleitet. Dabei wird die virtuelle Ziel-IP von der des Gateways der Azure SQL-Datenbank in die der Proxy-Middleware geändert. Danach fließen alle nachfolgenden Pakete direkt über die Proxy-Middleware, wobei das Gateway für die Azure SQL-Datenbank umgangen wird. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Verbindung von außerhalb von Azure

Wenn Sie von außerhalb von Azure eine Verbindung herstellen, verfügen Ihre Verbindungen standardmäßig über die Verbindungsrichtlinie **Proxy**. Die Richtlinie **Proxy** bedeutet, dass die TCP-Sitzung über das Gateway von Azure SQL-Datenbank hergestellt wird, und dass alle nachfolgenden Pakete über das Gateway fließen. Das folgende Diagramm veranschaulicht diesen Datenverkehrfluss.

![Architekturübersicht](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-Adressen vom Gateway von Azure SQL-Datenbank

Zur Verbindung mit einer Azure SQL-Datenbank von einer lokalen Ressource aus müssen Sie ausgehenden Netzwerkdatenverkehr auf dem Gateway von Azure SQL-Datenbank für Ihre Azure-Region zulassen. Ihre Verbindungen erfolgen nur über das Gateway, wenn Sie eine Verbindung im Proxymodus herstellen. Dies ist die Standardeinstellung bei der Verbindung von lokalen Ressourcen aus.

Die folgende Tabelle enthält die primäre und sekundäre IP-Adressen des Gateways von Azure SQL-Datenbank für alle Datenregionen. Für einige Regionen gibt es zwei IP-Adressen. In diesen Regionen ist die primäre IP-Adresse die aktuelle IP-Adresse des Gateways, und die zweite IP-Adresse ist eine Failover-IP-Adresse. Die Failoveradresse ist die Adresse, wohin wir möglicherweise Ihren Server verschieben, um die Verfügbarkeit des Dienst hoch zu halten. Für diese Regionen wird empfohlen, dass Sie ausgehenden Verkehr an beide IP-Adressen zulassen. Die zweite IP-Adresse ist Eigentum von Microsoft und belauscht keine Dienste von Azure SQL-Datenbank. Dies macht sie erst, wenn Sie zulassen, dass Azure SQL-Datenbank Verbindungen annimmt.

| Name der Region | Primäre IP-Adresse | Sekundäre IP-Adresse |
| --- | --- |--- |
| Australien, Osten | 191.238.66.109 | 13.75.149.87 |
| Australien, Südosten | 191.239.192.109 | 13.73.109.251 |
| Brasilien Süd | 104.41.11.5 | |    
| Kanada, Mitte | 40.85.224.249 | |    
| Kanada, Osten | 40.86.226.166 | |
| USA (Mitte) | 23.99.160.139 | 13.67.215.62 |
| Ostasien | 191.234.2.139 | 52.175.33.150 |
| US, Osten 1 | 191.238.6.43 | 40.121.158.30 |
| USA (Ost) 2 | 191.239.224.107 | 40.79.84.180 |
| Indien, Mitte | 104.211.96.159  | |   
| Indien, Süden | 104.211.224.146  | |
| Indien, Westen | 104.211.160.80 | |
| Japan Ost | 191.237.240.43 | 13.78.61.196 |
| Japan (Westen) | 191.238.68.11 | 104.214.148.156 |
| Korea, Mitte | 52.231.32.42 | |
| Korea, Süden | 52.231.200.86 |  |
| USA Nord Mitte | 23.98.55.75 | 23.96.178.199 |
| Nordeuropa | 191.235.193.75 | 40.113.93.91 |
| USA (Mitte/Süden) | 23.98.162.75 | 13.66.62.124 |
| Südostasien | 23.100.117.95 | 104.43.15.0 |
| Großbritannien, Norden | 13.87.97.210 | |
| Vereinigtes Königreich, Süden 1 | 51.140.184.11 | |    
| Großbritannien, Süden 2 | 13.87.34.7 | |
| UK, Westen | 51.141.8.11  | |
| USA, Westen-Mitte | 13.78.145.25 | |
| Westeuropa | 191.237.232.75 | 40.68.37.158 |
| USA, Westen 1 | 23.99.34.75 | 104.42.238.205 |
| USA, Westen 2 | 13.66.226.202  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a>Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank

Um die Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbankserver zu ändern, verwenden die [REST-API](https://msdn.microsoft.com/library/azure/mt604439.aspx). 

- Wenn Ihre Verbindungsrichtlinie auf **Proxy** festgelegt ist, fließen alle Netzwerkpakete über das Gateway von Azure SQL-Datenbank. Um dies einzustellen, lassen Sie ausgehenden Datenverkehr nur für Gateway-IPs von Azure SQL-Datenbank zu. Die Einstellung **Proxy** hat längere Wartezeiten als die Einstellung **Redirect**. 
- Wenn die Einstellung der Verbindungsrichtlinie **Redirect** ist, fließen alle Netzwerkpakete direkt an den Middleware-Proxy. Um dies einzustellen, lassen Sie ausgehenden Datenverkehr an mehrere IP-Adressen zu. 

## <a name="script-to-change-connection-settings"></a>Skript zum Ändern der Verbindungseinstellungen

> [!IMPORTANT]
> Dieses Skript erfordert das [Azure PowerShell-Modul](/powershell/azure/install-azurerm-ps).
>

Das folgende PowerShell-Skript veranschaulicht, wie Sie die Verbindungsrichtlinie ändern können.

```powershell
Add-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <Subscription Name>

# Azure Active Directory ID
$tenantId = "<Azure Active Directory GUID>"
$authUrl = "https://login.microsoftonline.com/$tenantId"

# Subscription ID
$subscriptionId = "<Subscription GUID>"

# Create an App Registration in Azure Active Directory.  Ensure the application type is set to NATIVE
# Under Required Permissions, add the API:  Windows Azure Service Management API

# Specify the redirect URL for the app registration
$uri = "<NATIVE APP - REDIRECT URI>"

# Specify the application id for the app registration
$clientId = "<NATIVE APP - APPLICATION ID>"

# Logical SQL Server Name
$serverName = "<LOGICAL DATABASE SERVER - NAME>"

# Resource Group where the SQL Server is located
$resourceGroupName= "<LOGICAL DATABASE SERVER - RESOURCE GROUP NAME>"


# Login and acquire a bearer token
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$result = $AuthContext.AcquireToken(
"https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto
)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#Get current connection Policy
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#Set connection policy to Proxy
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

# Apply Changes
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Ändern der Verbindungsrichtlinie von Azure SQL-Datenbank für einen Azure SQL-Datenbankserver finden Sie unter [Create or Update Server Connection Policy using the REST API (Erstellen oder Aktualisieren der Serververbindungsrichtlinie mit der REST-API)](https://msdn.microsoft.com/library/azure/mt604439.aspx).
- Weitere Informationen zum Verbindungsverhalten von Azure SQL-Datenbank für Kunden, die ADO.NET 4.5 oder höher verwenden, finden Sie unter [Andere Ports als 1433 für ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Einen allgemeinen Überblick über die Anwendungsentwicklung finden Sie unter [SQL-Datenbankanwendungsentwicklung – Übersicht](sql-database-develop-overview.md).

