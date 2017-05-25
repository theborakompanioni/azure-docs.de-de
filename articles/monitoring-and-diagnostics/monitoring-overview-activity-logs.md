---
title: "Übersicht über das Azure-Aktivitätsprotokoll | Microsoft Docs"
description: "Es wird beschrieben, was das Azure-Aktivitätsprotokoll ist und wie Sie es nutzen können, um Informationen zu den Ereignissen in Ihrem Azure-Abonnement zu erhalten."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.translationtype: Human Translation
ms.sourcegitcommit: f41fbee742daf2107b57caa528e53537018c88c6
ms.openlocfilehash: c123b76b0e4c95cfebcc79063fb1c3a27efc8646
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017


---
# <a name="overview-of-the-azure-activity-log"></a>Übersicht über das Azure-Aktivitätsprotokoll
Das **Azure-Aktivitätsprotokoll** bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokolle“ oder „Vorgangsprotokolle“ bezeichnet, da es Ereignisse der Steuerungsebene für Ihre Abonnements enthält. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden.

![Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Abbildung 1: Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen

Das Aktivitätsprotokoll unterscheidet sich von [Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md). Aktivitätsprotokolle enthalten Daten zu den Vorgängen an einer Ressource von außen. Diagnoseprotokolle werden von einer Ressource ausgegeben und enthalten Informationen zu den Vorgängen dieser Ressource.

Sie können Ereignisse per Azure-Portal, Befehlszeilenschnittstelle, PowerShell-Cmdlets und Azure Monitor-REST-API aus dem Aktivitätsprotokoll abrufen.


> [!WARNING]
> Das Azure-Aktivitätsprotokoll ist in erster Linie für Aktivitäten bestimmt, die in Azure Resource Manager stattfinden. Es verfolgt keine Ressourcen nach, die das klassische Modell/RDFE-Modell verwenden. Einige klassische Ressourcentypen weisen einen Proxyressourcenanbieter in Azure Resource Manager auf (z.B. Microsoft.ClassicCompute). Wenn Sie mithilfe dieser Proxyressourcenanbieter über Azure Resource Manager mit einem klassischen Ressourcentyp interagieren, werden die Vorgänge im Aktivitätsprotokoll aufgeführt. Wenn Sie mit einem klassischen Ressourcentyp im klassischen Portal oder an anderer Stelle außerhalb der Azure Resource Manager-Proxys interagieren, werden Ihre Aktionen nur in das Vorgangsprotokoll aufgenommen. Auf das Vorgangsprotokoll kann nur im klassischen Portal zugegriffen werden.
>
>

Sehen Sie sich dieses Video zum Aktivitätsprotokoll an.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="what-you-can-do-with-the-activity-log"></a>Möglichkeiten mit dem Aktivitätsprotokoll
Hier sind einige Verwendungsmöglichkeiten für das Aktivitätsprotokoll aufgeführt:

![Azure-Aktivitätsprotokoll](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* [Erstellen Sie eine Warnung, die ein Aktivitätsprotokollereignis auslöst.](monitoring-activity-log-alerts.md)
* Streamen zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI [an einen **Event Hub**](monitoring-stream-activity-logs-event-hubs.md).
* Analysieren in Power BI mit dem [**Power BI-Inhaltspaket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)
* [Speichern unter einem **Speicherkonto** zur Archivierung oder manuellen Untersuchung](monitoring-archive-activity-log.md) Sie können die Aufbewahrungsdauer (in Tagen) mithilfe von **Protokollprofilen** angeben.
* Abfragen und Anzeigen des Protokolls im **Azure-Portal**
* Fragen Sie es per PowerShell-Cmdlet, CLI oder REST-API ab.


Sie können ein Speicherkonto oder Event Hub-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

## <a name="export-the-activity-log-with-log-profiles"></a>Exportieren des Aktivitätsprotokolls mit Protokollprofilen
Mit einem **Protokollprofil** wird gesteuert, wie das Aktivitätsprotokoll exportiert wird. Mit einem Protokollprofil können Sie Folgendes konfigurieren:

* Wohin das Aktivitätsprotokoll gesendet wird (Speicherkonto oder Event Hubs)
* Welche Ereigniskategorien gesendet werden sollen („Write“, „Delete“, „Action“) *„Kategorie“ hat in Protokollprofilen und Aktivitätsprotokollen eine unterschiedliche Bedeutung. Im Protokollprofil ist „Kategorie“ der Vorgangstyp („Write“, „Delete“, „Action“). In einem Aktivitätsprotokoll stellt die Eigenschaft „Kategorie“ die Quelle oder den Typ des Ereignisses dar (z.B. „Administration“, „ServiceHealth“, „Alert“ usw.).*
* Welche Regionen (Standorte) exportiert werden sollen
* Wie lange das Aktivitätsprotokoll in einem Speicherkonto beibehalten werden soll.
    - Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

Diese Einstellungen können über die Option „Exportieren“ auf dem Blatt „Aktivitätsprotokoll“ im Portal konfiguriert werden. Sie können auch [mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx), über PowerShell-Cmdlets oder über die Befehlszeilenschnittstelle programmgesteuert konfiguriert werden. Ein Abonnement kann nur über ein Protokollprofil verfügen.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurieren von Protokollprofilen mit dem Azure-Portal
Sie können das Aktivitätsprotokoll an einen Event Hub streamen oder in einem Speicherkonto speichern, indem Sie im Azure-Portal die Option „Exportieren“ verwenden.

1. Navigieren Sie zum Blatt **Aktivitätsprotokoll** , indem Sie das Menü auf der linken Seite des Portals verwenden.

    ![Navigation zum Aktivitätsprotokoll im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Exportieren** .

    ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem daraufhin angezeigten Blatt können Sie Folgendes auswählen:  
  * Regionen, die für die Ereignisse exportiert werden sollen
  * das Speicherkonto, in dem Sie Ereignisse speichern möchten
  * die Anzahl der Tage, die diese Ereignisse im Speicher aufbewahrt werden sollen. Bei einer Einstellung von 0 Tagen werden die Protokolle unbegrenzt aufbewahrt.
  * den Service Bus-Namespace, in dem Sie einen Event Hub für das Streamen dieser Ereignisse erstellen möchten

     ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurieren von Protokollprofilen mit den Azure PowerShell-Cmdlets
#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| StorageAccountId |Nein |Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId |Nein |Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Standorte |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Categories |Nein |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfigurieren von Protokollprofilen mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle
#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Die `name` -Eigenschaft sollte den Namen des Protokollprofils enthalten.

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| storageId |Nein |Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId |Nein |Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| locations |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| categories |Nein |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Ereignisschema
Jedes Ereignis im Aktivitätsprotokoll verfügt über ein JSON-Blob ähnlich diesem Beispiel:

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
| --- | --- |
| authorization |Blob mit RBAC-Eigenschaften des Ereignisses. Enthält normalerweise die Eigenschaften „action“, „role“ und „scope“. |
| caller |E-Mail-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch auf Grundlage der Verfügbarkeit ausgeführt hat. |
| channels |Einer der folgenden Werte: „Admin“, „Operation“ |
| correlationId |Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die über die gleiche correlationId verfügen, gehören zu derselben übergeordneten Aktion. |
| Beschreibung |Statische Beschreibung eines Ereignisses in Textform. |
| eventDataId |Eindeutiger Bezeichner eines Ereignisses. |
| eventSource |Name des Azure-Diensts oder der Infrastruktur, die das Ereignis generiert hat. |
| httpRequest |Blob, das die HTTP-Anforderung beschreibt. Umfasst üblicherweise „clientRequestId“, „clientIpAddress“ und „method“ (HTTP-Methode, z.B. PUT). |
| Einstellung |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| ResourceGroupName |Name der Ressourcengruppe für die betroffene Ressource. |
| resourceProviderName |Name des Ressourcenanbieters für die betroffene Ressource. |
| resourceUri |Ressourcen-ID der betroffenen Ressource. |
| operationId |Eine GUID, die von den Ereignissen eines einzelnen Vorgangs gemeinsam genutzt wird. |
| operationName |Name des Vorgangs. |
| Eigenschaften |Satz mit `<Key, Value>`-Paaren (Wörterbuch), die Details des Ereignisses beschreiben. |
| status |Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus |Normalerweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch andere Zeichenfolgen zum Beschreiben eines Unterstatus enthalten, z.B. diese häufigen Werte: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504). |
| eventTimestamp |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| submissionTimestamp |Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war. |
| subscriptionId |Azure-Abonnement-ID |
| nextLink |Fortsetzungstoken zum Abrufen des nächsten Satzes mit Ergebnissen, wenn diese in mehrere Antworten unterteilt werden. Normalerweise erforderlich, wenn mehr als 200 Datensätze vorhanden sind. |

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Aktivitätsprotokoll (bisher „Überwachungsprotokolle“)](../azure-resource-manager/resource-group-audit.md)
* [Stream the Azure Activity Log to Event Hubs (Streamen des Azure-Aktivitätsprotokolls auf Event Hubs)](monitoring-stream-activity-logs-event-hubs.md)

