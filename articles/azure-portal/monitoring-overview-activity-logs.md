<properties
	pageTitle="Übersicht über das Azure-Aktivitätsprotokoll | Microsoft Azure"
	description="Es wird beschrieben, was das Azure-Aktivitätsprotokoll ist und wie Sie es nutzen können, um Informationen zu den Ereignissen in Ihrem Azure-Abonnement zu erhalten."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/17/2016"
	ms.author="johnkem"/>

# Übersicht über das Azure-Aktivitätsprotokoll
Das **Azure-Aktivitätsprotokoll** ist ein Protokoll, mit dem Einblicke in Vorgänge möglich sind, die für Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokolle“ oder „Vorgangsprotokolle“ bezeichnet, da es Ereignisse der Steuerungsebene für Ihre Abonnements enthält. Mit dem Aktivitätsprotokoll können Sie die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Außerdem erhalten Sie Informationen zum Status des Vorgangs und zu anderen relevanten Eigenschaften. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET).

Es unterscheidet sich von [Diagnoseprotokollen](./monitoring-overview-of-diagnostic-logs.md), da es sich dabei um alle ausgegebenen Protokolle einer Ressource handelt. Diese Protokolle enthalten Daten zum Betrieb der Ressource und nicht zu den Vorgängen einer Ressource.

Sie können Ereignisse per Azure-Portal, CLI, PowerShell-Cmdlets und Insights-REST-API aus dem Aktivitätsprotokoll abrufen.

## Möglichkeiten mit dem Aktivitätsprotokoll
Hier sind einige Verwendungsmöglichkeiten für das Aktivitätsprotokoll aufgeführt:

- Abfragen und Anzeigen des Protokolls im **Azure-Portal**
- Abfragen per REST-API, PowerShell-Cmdlet oder CLI
- [Erstellen einer E-Mail- oder Webhookwarnung, die ein Aktivitätsprotokoll auslöst](./insights-auditlog-to-webhook-email.md)
- [Speichern unter einem **Speicherkonto** zur Archivierung oder manuellen Untersuchung](./monitoring-archive-activity-log.md) Sie können die Aufbewahrungsdauer (in Tagen) mithilfe von **Protokollprofilen** angeben.
- Analysieren in PowerBI mit dem [**Power BI-Inhaltspaket**](https://powerbi.microsoft.com/de-DE/documentation/powerbi-content-pack-azure-audit-logs/)
- [Streamen zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an einen **Event Hub**](./monitoring-stream-activity-logs-event-hubs.md)

## Exportieren des Aktivitätsprotokolls mit Protokollprofilen
Mit einem **Protokollprofil** wird gesteuert, wie das Aktivitätsprotokoll exportiert wird. Mit einem Protokollprofil können Sie Folgendes konfigurieren:

- Wohin das Aktivitätsprotokoll gesendet wird (Speicherkonto oder Event Hubs)
- Welche Ereigniskategorien gesendet werden sollen (z.B. „Write“, „Delete“, „Action“)
- Welche Regionen (Standorte) exportiert werden sollen
- Wie lange das Aktivitätsprotokoll in einem Speicherkonto aufbewahrt werden sollen. Bei Angabe einer Aufbewahrungsdauer von null Tagen werden die Protokolle dauerhaft aufbewahrt. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden. Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (wenn z.B. nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.

Diese Einstellungen können im Portal auf dem Blatt „Aktivitätsprotokoll“ mit der Option „Exportieren“ oder programmgesteuert [mit der REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShell-Cmdlets oder der CLI konfiguriert werden. Ein Abonnement kann nur über ein Protokollprofil verfügen.

### Konfigurieren von Protokollprofilen mit dem Azure-Portal
Sie können das Aktivitätsprotokoll an einen Event Hub streamen oder in einem Speicherkonto speichern, indem Sie im Azure-Portal die Option „Exportieren“ verwenden.

1. Navigieren Sie zum Blatt **Aktivitätsprotokoll**, indem Sie das Menü auf der linken Seite des Portals verwenden.

    ![Im Portal zum Aktivitätsprotokoll navigieren](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Exportieren**.

    ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem angezeigten Blatt können Sie Folgendes auswählen: die Regionen, für die Sie Ereignisse exportieren möchten, das Speicherkonto, in dem Sie Ereignisse speichern möchten (und die Anzahl von Tagen für die Aufbewahrung dieser Ereignisse im Speicher. Bei der Angabe von 0 Tagen werden die Protokolle unbegrenzt gespeichert.), und den Service Bus-Namespace, unter dem ein Event Hub zum Streamen dieser Ereignisse erstellt werden soll.

    ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.

### Konfigurieren von Protokollprofilen mit den Azure PowerShell-Cmdlets
#### Abrufen eines vorhandenen Protokollprofils
```
Get-AzureRmLogProfile
```

#### Hinzufügen eines Protokollprofils
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name | Ja | Name des Protokollprofils. |
| StorageAccountId | Nein | Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId | Nein | Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Locations | Ja | Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays | Ja | Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Categories | Nein | Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### Entfernen eines Protokollprofils
```
Remove-AzureRmLogProfile -name my_log_profile
```

### Konfigurieren von Protokollprofilen mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle
#### Abrufen eines vorhandenen Protokollprofils
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Die `name`-Eigenschaft sollte den Namen des Protokollprofils enthalten.

#### Hinzufügen eines Protokollprofils
``` 
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Name | Ja | Name des Protokollprofils. |
| storageId | Nein | Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId | Nein | Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| locations | Ja | Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| retentionInDays | Ja | Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| categories | Nein | Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### Entfernen eines Protokollprofils
```
azure insights logprofile delete --name my_log_profile
```

## Ereignisschema
Jedes Ereignis im Aktivitätsprotokoll verfügt über ein JSON-Blob dieser Art:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Elementname | Beschreibung |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| authorization | Blob mit RBAC-Eigenschaften des Ereignisses. Enthält normalerweise die Eigenschaften „action“, „role“ und „scope“. |
| caller | E-Mail-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch auf Grundlage der Verfügbarkeit ausgeführt hat. |
| channels | Einer der folgenden Werte: „Admin“, „Operation“ |
| correlationId | Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die über die gleiche correlationId verfügen, gehören zu derselben übergeordneten Aktion. |
| description | Statische Beschreibung eines Ereignisses in Textform. |
| eventDataId | Eindeutiger Bezeichner eines Ereignisses. |
| eventSource | Name des Azure-Diensts oder der Infrastruktur, die das Ereignis generiert hat. |
| httpRequest | Blob, das die HTTP-Anforderung beschreibt. Umfasst üblicherweise „clientRequestId“, „clientIpAddress“ und „method“ (HTTP-Methode, z. B. PUT). |
| level | Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| ResourceGroupName | Name der Ressourcengruppe für die betroffene Ressource. |
| resourceProviderName | Name des Ressourcenanbieters für die betroffene Ressource. |
| resourceUri | Ressourcen-ID der betroffenen Ressource. |
| operationId | Eine GUID, die von den Ereignissen eines einzelnen Vorgangs gemeinsam genutzt wird. |
| operationName | Name des Vorgangs. |
| properties | Satz mit `<Key, Value>`-Paaren (Wörterbuch), die die Details des Ereignisses beschreiben. |
| status | Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | Normalerweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch andere Zeichenfolgen zum Beschreiben eines Unterstatus enthalten, z.B. diese häufigen Werte: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504). |
| eventTimestamp | Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| submissionTimestamp | Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war. |
| subscriptionId | Azure-Abonnement-ID |
| nextLink | Fortsetzungstoken zum Abrufen des nächsten Satzes mit Ergebnissen, wenn diese in mehrere Antworten unterteilt werden. Dies ist normalerweise der Fall, wenn mehr als 200 Datensätze vorhanden sind. |

## Nächste Schritte
- [Weitere Informationen zum Aktivitätsprotokoll (bisher „Überwachungsprotokolle“)](../resource-group-audit.md)
- [Stream the Azure Activity Log to Event Hubs (Streamen des Azure-Aktivitätsprotokolls auf Event Hubs)](./monitoring-stream-activity-logs-event-hubs.md)

<!---HONumber=AcomDC_0907_2016-->