---
title: "Azure Monitoring-REST-API – exemplarische Vorgehensweise | Microsoft Docs"
description: Authentifizieren von Anforderungen und Verwenden der Azure Monitor-REST-API.
author: mcollier
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 565e6a88-3131-4a48-8b82-3effc9a3d5c6
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: mcollier
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fcf9cc661da0d8e65b385bfddeded0a3e5d0d3e2
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
<a id="azure-monitoring-rest-api-walkthrough" class="xliff"></a>

# Exemplarische Vorgehensweise für die Azure Monitor-REST-API
In diesem Artikel erfahren Sie, wie Sie die Authentifizierungen so durchführen, dass Ihr Code die [REST-API-Referenz für Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx)nutzen kann.         

Mit der Azure Monitor-API können Sie die verfügbaren Standardmetrikdefinitionen (den Metriktyp, z.B. CPU-Zeit, Anforderungen etc.), Granularität und Metrikwerte programmgesteuert abrufen. Nach dem Abrufen können die Daten in einem separaten Datenspeicher abgelegt werden, z.B. in Azure SQL-Datenbank, Azure Cosmos DB oder Azure Data Lake. Von dort aus können ggf. weitere Analysen durchgeführt werden.

Außer dem Arbeiten mit verschiedenen Metrikdatenpunkten – wie in diesem Artikel beschrieben – können Sie mit der Monitor-API auch Warnungsregeln auflisten, Aktivitätsprotokolle anzeigen und vieles mehr. Eine vollständige Liste aller verfügbaren Vorgänge finden Sie in der [REST-API-Referenz für Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).

<a id="authenticating-azure-monitor-requests" class="xliff"></a>

## Authentifizieren von Azure Monitor-Anforderungen
Zunächst muss die Anforderung authentifiziert werden.

Alle für die Azure Monitor-API ausgeführten Aufgaben verwenden das Azure Resource Manager-Authentifizierungsmodell. Daher müssen alle Anforderungen mithilfe von Azure Active Directory (Azure AD) authentifiziert werden. Ein Ansatz für die Authentifizierung der Clientanwendung ist die Erstellung eines Azure AD-Dienstprinzipals und das Abrufen des Authentifizierungstokens (JWT). Das folgende Beispielskript zeigt, wie ein Azure AD-Dienstprinzipal mithilfe von PowerShell erstellt werden kann. Eine ausführlichere Anleitung finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-password). Es ist außerdem möglich, [einen Dienstprinzipal über das Azure-Portal zu erstellen](../azure-resource-manager/resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Für die Abfrage der Azure Monitor-API sollte die Clientanwendung für die Authentifizierung den vorher erstellten Dienstprinzipal verwenden. Das folgende beispielhafte PowerShell-Skript zeigt einen Ansatz, wie die [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) verwendet werden kann, um das JWT-Authentifizierungstoken zu erhalten. Das JWT-Token wird als Teil eines HTTP-Autorisierungsparameters bei Anforderungen an die Azure Monitor-REST-API weitergegeben.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.microsoftonline.com/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)

$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Sobald die Authentifizierungseinrichtung abgeschlossen ist, können Abfragen für die Azure Monitor-REST-API ausgeführt werden. Es stehen zwei hilfreiche Abfragen zur Verfügung:

1. Auflisten der Metrikdefinitionen für eine Ressource
2. Abrufen der Metrikwerte

<a id="retrieve-metric-definitions" class="xliff"></a>

## Abrufen der Metrikdefinitionen
> [!NOTE]
> Zum Abrufen der Metrikdefinitionen mithilfe der Azure Monitor-REST-API verwenden Sie die API-Version 2016-03-01.
>
>

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Die Metrikdefinitionen für eine Azure Logik-App würden in etwa wie im folgenden Screenshot aussehen:

![Alt "JSON-Ansicht der Metrikdefinitionsantwort"](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Weitere Informationen finden Sie in der Dokumentation zum [Auflisten der Metrikdefinitionen für eine Ressource in der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/mt743621.aspx) .

<a id="retrieve-metric-values" class="xliff"></a>

## Abrufen von Metrikwerten
Sobald die verfügbaren Metrikdefinitionen bekannt sind, können die entsprechenden Metrikwerte abgerufen werden. Für sämtliche Filteranforderungen verwenden Sie den Metriknamen „value“ (nicht „localizedValue“; z.B. zum Abrufen der Metrikdatenpunkte „CpuTime“ und „Requests“). Werden keine Filter verwendet, wird die Standardmetrik zurückgegeben.

> [!NOTE]
> Zum Abrufen der Metrikwerte mithilfe der Azure Monitor-REST-API verwenden Sie die API-Version 2016-06-01.
>
>

**Methode**: GET

**Anforderungs-URI**: https://management.azure.com/subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/*{resource-provider-namespace}*/*{resource-type}*/*{resource-name}*/providers/microsoft.insights/metrics?$filter=*{filter}*&api-version=*{apiVersion}*

Um zum Beispiel RunsSucceeded-Metrikdatenpunkte für einen bestimmten Zeitraum und ein Aggregationsintervall von einer Stunde abzurufen, sähe die Anforderung wie folgt aus:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Das Ergebnis ähnelt dem folgenden Screenshot:

![Alt "JSON-Ansicht mit dem Metrikwert für die durchschnittliche Antwortzeit"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Um mehrere Daten- oder Aggregationspunkte abzurufen, fügen Sie dem Filter die Metrikdefinitionsnamen und Aggregationstypen hinzu, wie im folgenden Beispiel dargestellt:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<a id="use-armclient" class="xliff"></a>

### Verwenden von ARMClient
Eine Alternative zur Verwendung von PowerShell (wie oben dargestellt) ist die Verwendung von [ARMClient](https://github.com/projectkudu/ARMClient) auf Ihrem Windows-Computer. ARMClient führt die Azure AD-Authentifizierung automatisch durch (und verwendet das entsprechende JWT-Token). Die folgenden Schritte beschreiben die Nutzung von ARMClient zum Abrufen von Metrikdaten:

1. Installieren Sie [Chocolatey](https://chocolatey.org/) und [ARMClient](https://github.com/projectkudu/ARMClient).
2. Geben Sie in einem Terminalfenster *armclient.exe login*ein. Danach werden Sie dazu aufgefordert, sich bei Azure anzumelden.
3. Geben Sie *armclient GET [Ihre_Ressourcen_ID]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01* ein.
4. Geben Sie *armclient GET [Ihre_Ressourcen_ID]/providers/microsoft.insights/metrics?api-version=2016-06-01* ein.

![Alt "Verwenden von ARMClient zum Arbeiten mit der Azure Monitor-REST-API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)

<a id="retrieve-the-resource-id" class="xliff"></a>

## Abrufen der Ressourcen-ID
Die Verwendung der REST-API kann Ihnen dabei helfen, die verfügbaren Metrikdefinitionen, die Granularität und die entsprechenden Werte zu verstehen. Diese Informationen sind für die Nutzung der [Azure-Verwaltungsbibliothek](https://msdn.microsoft.com/library/azure/mt417623.aspx)hilfreich.

Für den oben angegebenen Code muss als Ressourcen-ID der vollständige Pfad zur gewünschten Azure-Ressource angegeben werden. Zum Beispiel würde die Ressourcen-ID für die Abfrage einer Azure Web-App wie folgt lauten:

*/subscriptions/{abonnement-id}/resourceGroups/{ressourcengruppenname}/providers/Microsoft.Web/sites/{sitename}/*

Die folgende Liste enthält einige Beispiele für Ressourcen-ID-Formate für verschiedene Azure-Ressourcen:

* **IoT Hub** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Devices/IotHubs/*{iot-hub-name}*
* **Elastischer SQL-Pool** - /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{pool-db}*/elasticpools/*{sql-pool-name}*
* **SQL-Datenbank (v12)** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Sql/servers/*{server-name}*/databases/*{database-name}*
* **Service Bus** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.ServiceBus/*{namespace}*/*{servicebus-name}*
* **VM-Skalierungsgruppen** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachineScaleSets/*{vm-name}*
* **VMs** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.Compute/virtualMachines/*{vm-name}*
* **Event Hubs** – /subscriptions/*{subscription-id}*/resourceGroups/*{resource-group-name}*/providers/Microsoft.EventHub/namespaces/*{eventhub-namespace}*

Es gibt alternative Ansätze für das Abrufen von Ressourcen-ID – darunter die Verwendung von Azure-Ressourcen-Explorers – und für das Anzeigen der gewünschten Ressource im Azure-Portal, über PowerShell oder die Azure-CLI.

<a id="azure-resource-explorer" class="xliff"></a>

### Azure-Ressourcen-Explorer
Ein hilfreicher Ansatz zum Finden der Ressourcen-ID für die gewünschte Ressource ist die Verwendung des [Azure-Ressourcen-Explorers](https://resources.azure.com) . Navigieren Sie zur gewünschten Ressource, und sehen Sie sich dort die angezeigte ID an, wie im folgenden Screenshot dargestellt:

![Alt "Azure-Ressourcen-Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

<a id="azure-portal" class="xliff"></a>

### Azure-Portal
Die Ressourcen-ID finden Sie auch im Azure-Portal. Navigieren Sie dazu zur gewünschten Ressource, und wählen Sie „Eigenschaften“. Die Ressourcen-ID wird auf dem Blatt „Eigenschaften“ angezeigt, wie im folgenden Screenshot dargestellt:

![Alt "Ressourcen-ID wird auf dem Blatt „Eigenschaften“ im Azure-Portal angezeigt"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

<a id="azure-powershell" class="xliff"></a>

### Azure PowerShell
Die Ressourcen-ID kann auch mithilfe von Azure PowerShell-Cmdlets abgerufen werden. Um zum Beispiel die Ressourcen-ID für eine Azure-Web-App abzurufen, führen Sie das Cmdlet Get-AzureRmWebApp aus, wie im folgenden Screenshot dargestellt:

![Alt "Ressourcen-ID wird über PowerShell abgerufen"](./media/monitoring-rest-api-walkthrough/resourceid_powershell.png)

<a id="azure-cli" class="xliff"></a>

### Azure-Befehlszeilenschnittstelle
Um die Ressourcen-ID mithilfe der Azure-CLI abzurufen, führen Sie den Befehl „azure webapp show“ aus, und geben Sie die Option „--json“ an, wie im folgenden Screenshot dargestellt:

![Alt "Ressourcen-ID wird über PowerShell abgerufen"](./media/monitoring-rest-api-walkthrough/resourceid_azurecli.png)

<a id="retrieve-activity-log-data" class="xliff"></a>

## Abrufen von Aktivitätsprotokolldaten
Außer Metrikdefinitionen und den entsprechenden Werten können auch zusätzliche interessante Informationen über Ihre Azure-Ressourcen abgerufen werden. So können beispielsweise [Aktivitätsprotokolldaten](https://msdn.microsoft.com/library/azure/dn931934.aspx) abgefragt werden. Das folgende Muster zeigt die Verwendung der Azure Monitor-REST-API, um Aktivitätsprotokolldaten innerhalb eines bestimmten Zeitraums für Ihr Azure-Abonnement abzufragen:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Lesen Sie die [Übersicht über die Überwachung](monitoring-overview.md).
* Lesen Sie [Unterstützte Metriken von Azure Monitor](monitoring-supported-metrics.md).
* Lesen Sie die [REST-API-Referenz zu Microsoft Azure Monitor](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Schlagen Sie in der [Azure-Verwaltungsbibliothek](https://msdn.microsoft.com/library/azure/mt417623.aspx)nach.

