---
title: "Konfigurieren von Webhooks für Azure-Metrikwarnungen | Microsoft Docs"
description: Umleiten von Azure-Warnungen an andere Azure-fremde Systeme.
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 7282de704a1053e2052a189990fb2b30b2adad6f
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017

---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>Konfigurieren eines Webhooks für eine Azure-Metrikwarnung
Mithilfe von Webhooks können Benutzer eine Azure-Warnbenachrichtigung zur Nachbearbeitung oder Ausführung benutzerdefinierter Aktionen an andere Systeme weiterleiten. Sie können einen Webhook für eine Warnung verwenden, um sie an Dienste weiterzuleiten, die SMS-Nachrichten versenden, Fehler protokollieren, ein Team per Chat-/Messagingdienst benachrichtigen oder beliebige andere Aktionen ausführen. In diesem Artikel erfahren Sie, wie Sie einen Webhook für eine Azure-Metrikwarnung festlegen und wie die Nutzlast für den an einen Webhook gerichteten HTTP POST-Vorgang aussieht. Informationen zur Einrichtung und zum Schema einer Azure-Aktivitätsprotokollwarnung (Warnung für Ereignisse) finden Sie auf [dieser Seite](insights-auditlog-to-webhook-email.md).

Azure-Warnungen geben die Inhalte von Warnungen mittels HTTP POST im JSON-Format (Schema siehe weiter unten) an einen Webhook-URI weiter, den Sie beim Erstellen der Warnung angeben. Dieser URI muss ein gültiger HTTP- oder HTTPS-Endpunkt sein. Bei Aktivierung einer Warnung veröffentlicht Azure jeweils einen Eintrag pro Anforderung.

## <a name="configuring-webhooks-via-the-portal"></a>Konfigurieren von Webhooks über das Portal
Den Webhook-URI können Sie im [Portal](https://portal.azure.com/)im Bildschirm zum Erstellen/Aktualisieren von Warnungen hinzufügen oder aktualisieren.

![Hinzufügen einer Warnungsregel](./media/insights-webhooks-alerts/Alertwebhook.png)

Sie können auch [Azure PowerShell-Cmdlets](insights-powershell-samples.md#create-alert-rules), die [plattformübergreifende Befehlszeilenschnittstelle](insights-cli-samples.md#work-with-alerts) oder die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx) verwenden, um eine Warnung mit Veröffentlichung an einen Webhook-URI zu konfigurieren.

## <a name="authenticating-the-webhook"></a>Authentifizieren des Webhooks
Die Authentifizierung des Webhooks kann mithilfe der Token-basierten Autorisierung erfolgen. Der Webhook-URI wird z.B. mit einer Token-ID gespeichert. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Nutzlast und Schema
Der POST-Vorgang enthält für alle metrikbasierten Warnungen die folgende JSON-Nutzlast und das folgende Schema.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Feld | Erforderlich | Feste Gruppe von Werten | Hinweise |
|:--- |:--- |:--- |:--- |
| status |J |"Activated", "Resolved" |Status der Warnung auf der Grundlage der festgelegten Bedingungen. |
| context |J | |Der Warnungskontext. |
| timestamp |J | |Der Zeitpunkt, an dem die Warnung ausgelöst wurde. |
| id |J | |Jeder Warnungsregel ist eine eindeutige ID zugewiesen. |
| name |J | |Der Name der Warnung. |
| description |J | |Beschreibung der Warnung. |
| conditionType |J |"Metric", "Event" |Zwei Arten von Warnungen werden unterstützt. Warnungen auf der Grundlage einer Metrikbedingung und Warnungen auf der Grundlage eines Ereignisses im Aktivitätsprotokoll. Überprüfen Sie mithilfe dieses Werts, ob es sich um eine metrik- oder um eine ereignisbasierte Warnung handelt. |
| condition |J | |Die spezifischen Felder, die basierend auf „conditionType“ überprüft werden. |
| metricName |für Warnungen des Typs "Metric" | |Der Name der Metrik, die definiert, welche Elemente mit der Regel überwacht werden. |
| metricUnit |für Warnungen des Typs "Metric" |„Bytes“, „BytesPerSecond“, „Count“, „CountPerSecond“, „Percent“, „Seconds“ |Die in der Metrik zulässige Einheit. Zulässige Werte finden Sie [hier](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |für Warnungen des Typs "Metric" | |Der tatsächliche Wert der Metrik, die die Warnung ausgelöst hat. |
| threshold |für Warnungen des Typs "Metric" | |Der Schwellenwert, bei dem die Warnung aktiviert wird. |
| windowSize |für Warnungen des Typs "Metric" | |Der Zeitraum, in dem die Aktivität der Warnung basierend auf dem Schwellenwert überwacht wird. Muss zwischen 5 Minuten und 1 Tag liegen. Format der Zeitspanne nach ISO 8601. |
| timeAggregation |für Warnungen des Typs "Metric" |„Average“, „Last“, „Maximum“, „Minimum“, „None“, „Total“ |Legt fest, wie die erfassten Daten im Zeitverlauf kombiniert werden sollen. Der Standardwert ist "Average". Zulässige Werte finden Sie [hier](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| operator |für Warnungen des Typs "Metric" | |Der Operator, der verwendet wird, um die aktuellen Metrikdaten mit dem festgelegten Schwellenwert zu vergleichen. |
| subscriptionId |J | |Die Azure-Abonnement-ID. |
| resourceGroupName |J | |Name der Ressourcengruppe für die betroffene Ressource. |
| resourceName |J | |Der Ressourcenname der betroffenen Ressource. |
| resourceType |J | |Der Ressourcentyp der betroffenen Ressource. |
| resourceId |J | |Ressourcen-ID der betroffenen Ressource. |
| resourceRegion |J | |Die Region oder der Standort der betroffenen Ressource. |
| portalLink |J | |Direkter Link zur Ressourcenzusammenfassungsseite des Portals. |
| properties |N |Optional |Eine Gruppe von `<Key, Value>`-Paaren (`Dictionary<String, String>`) mit Details zum Ereignis. Das Feld "properties" ist optional. Auf einer angepassten Benutzeroberfläche oder in einem Logik-App-basierenden Workflow können Benutzer Schlüssel und Werte eingeben, die über die Nutzlast übergeben werden können. Alternativ können benutzerdefinierte Eigenschaften direkt über den Webhook-URI an den Webhook zurückgegeben werden (als Abfrageparameter). |

> [!NOTE]
> Das Feld „properties“ kann nur mithilfe der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx) festgelegt werden.
>
>

## <a name="next-steps"></a>Nächste Schritte
* Im Video [Integrate Azure Alerts with PagerDuty](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Ausführen von Azure Automation-Skripts (Runbooks) für Azure-Warnungen](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Senden einer SMS über Twilio auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Senden einer Slack-Nachricht auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Senden einer Nachricht an eine Azure-Warteschlange auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

