---
title: "Erläuterungen zu dem in Aktivitätsprotokollwarnungen verwendeten Webhookschema | Microsoft-Dokumentation"
description: "Lernen Sie das Schema des JSON-Codes kennen, der beim Aktivieren einer Aktivitätsprotokollwarnung an einen Webhook-URL gesendet wird."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 75c71bcd16573d4f4dd3377c623aa9b414aa3906
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Webhooks für Azure-Aktivitätsprotokollwarnungen
Als Teil der Definition einer Aktionsgruppe können Sie Webhookendpunkte für den Empfang von Aktivitätsprotokollwarnungs-Benachrichtigungen konfigurieren. Mithilfe von Webhooks können Sie diese Benachrichtigung zur Nachbearbeitung oder Ausführung benutzerdefinierter Aktionen an andere Systeme weiterleiten. In diesem Artikel erfahren Sie, wie die Nutzlast für die HTTP POST-Methode für einen Webhook aussieht.

Weitere Informationen zu Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md).

Weitere Informationen zu Aktionsgruppen, finden Sie unter [Erstellen und Verwalten von Aktionsgruppen im Azure-Portal](monitoring-action-groups.md).

## <a name="authenticate-the-webhook"></a>Authentifizieren des Webhooks
Der Webhook kann optional tokenbasierte Autorisierung zur Authentifizierung verwenden. Der Webhook-URI wird mit einer Token-ID gespeichert, z.B. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Nutzlast und Schema
Die JSON-Nutzlast des POST-Vorgangs variiert in Abhängigkeit vom Feld „data.context.activityLog.eventSource“ der Nutzlast.

###<a name="common"></a>Common
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Administrative
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

Spezifische Schemainformationen zu Dienstintegritätsbenachrichtigungs-Aktivitätsprotokollwarnungen finden Sie unter [Dienstintegritätsbenachrichtigungen](monitoring-service-notifications.md).

Spezifische Schemainformationen zu allen anderen Aktivitätsprotokollwarnungen finden Sie unter [Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll](monitoring-overview-activity-logs.md).

| Elementname | Beschreibung |
| --- | --- |
| status |Wird für Metrikwarnungen verwendet. Muss für Aktivitätsprotokollwarnungen immer auf „aktiviert“ festgelegt werden. |
| context |Der Kontext des Ereignisses. |
| resourceProviderName |Der Ressourcenanbieter der betroffenen Ressource. |
| conditionType |Muss immer „Event“ lauten. |
| name |Der Name der Warnungsregel. |
| id |Die Ressourcen-ID der Warnung. |
| Beschreibung |Warnungsbeschreibung, die beim Erstellen der Warnung festgelegt wird. |
| subscriptionId |Die Azure-Abonnement-ID. |
| timestamp |Die Zeit, zu der das Ereignis durch den Azure-Dienst generiert wurde, der die Anforderung verarbeitet hat. |
| resourceId |Ressourcen-ID der betroffenen Ressource. |
| ResourceGroupName |Name der Ressourcengruppe für die betroffene Ressource. |
| properties |Eine Gruppe von `<Key, Value>`-Paaren (`Dictionary<String, String>`) mit Details zum Ereignis |
| event |Element, das Metadaten zum Ereignis enthält. |
| authorization |Die Eigenschaften der rollenbasierten Zugriffssteuerung des Ereignisses. Zu diesen Eigenschaften zählen üblicherweise Aktion, Rolle und Bereich. |
| category |Kategorie des Ereignisses. Unterstützte Werte: „Administration“, „Warnung“, „Sicherheit“, „Dienstintegrität“ und „Empfehlung“. |
| caller |Die E-Mail-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar). Kann für bestimmte Systemaufrufe NULL sein. |
| correlationId |Üblicherweise eine GUID in Zeichenfolgenformat. Ereignisse mit „correlationId“ gehören zur gleichen übergeordneten Aktion und besitzen üblicherweise den gleichen correlationId-Wert. |
| eventDescription |Statische Beschreibung des Ereignisses. |
| eventDataId |Eindeutiger Bezeichner für das Ereignis. |
| eventSource |Der Name des Azure-Diensts oder der Infrastruktur, der bzw. die das Ereignis generiert hat. |
| httpRequest |Die Anforderung umfasst üblicherweise clientRequestId, clientIpAddress und HTTP-Methode (z.B. PUT). |
| level |Einer der folgenden Werte: „Kritisch“, „Fehler“, „Warnung“, „Information“ und „Ausführlich“. |
| operationId |Üblicherweise eine gemeinsame GUID für alle Ereignisse des gleichen Vorgangs. |
| operationName |Name des Vorgangs. |
| properties |Die Eigenschaften des Ereignisses. |
| status |Eine Zeichenfolge. Der Status des Vorgangs. Gängige Werte: „Gestartet“, „In Bearbeitung“, „Erfolgreich“, „Fehler“, „Aktiv“ und „Aufgelöst“. |
| subStatus |Enthält üblicherweise den HTTP-Statuscode des zugehörigen REST-Aufrufs. Es können auch weitere Zeichenfolgen enthalten sein, die einen Unterstatus beschreiben. Gängige subStatus-Werte: „OK“ (HTTP-Statuscode: 200), „Erstellt“ (HTTP-Statuscode: 201), „Akzeptiert“ (HTTP-Statuscode: 202), „Kein Inhalt“ (HTTP-Statuscode: 204), „Ungültige Anforderung“ (HTTP-Statuscode: 400), „Nicht gefunden“ (HTTP-Statuscode: 404), „Konflikt“ (HTTP-Statuscode: 409), „Interner Serverfehler“ (HTTP-Statuscode: 500), „Dienst nicht verfügbar“ (HTTP-Statuscode: 503) und „Gatewaytimeout“ (HTTP-Statuscode: 504). |

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Aktivitätsprotokoll](monitoring-overview-activity-logs.md).
* [Ausführen von Azure Automation-Skripts (Runbooks) für Azure-Warnungen](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Senden einer SMS über Twilio auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* [Senden einer Slack-Nachricht auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
* [Senden einer Nachricht an eine Azure-Warteschlange auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.

