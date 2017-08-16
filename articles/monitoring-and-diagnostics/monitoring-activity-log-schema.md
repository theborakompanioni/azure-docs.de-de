---
title: "Ereignisschema des Azure-Aktivitätsprotokolls | Microsoft-Dokumentation"
description: "Informieren Sie sich über das Ereignisschema für Daten, die in das Aktivitätsprotokoll ausgegeben werden"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: bfd49ea68c597b109a2c6823b7a8115608fa26c3
ms.openlocfilehash: 5c96a1cfa56d1535549cb15d5a7bcf03bd11e723
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="azure-activity-log-event-schema"></a>Ereignisschema des Azure-Aktivitätsprotokolls
Das **Azure-Aktivitätsprotokoll** ist ein Protokoll, das einen Einblick in alle Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dieser Artikel beschreibt das Ereignisschema pro Datenkategorie.

## <a name="administrative"></a>Administrative
Diese Kategorie enthält die Datensätze aller Erstellungs-, Aktualisierungs-, Lösch- und Aktionsvorgänge, die über Resource Manager ausgeführt wurden. Zu den Ereignissen in dieser Kategorie gehört das Erstellen eines virtuellen Computers und das Löschen einer Netzwerksicherheitsgruppe. Jede Aktion, die von einem Benutzer oder einer Anwendung mithilfe von Resource Manager ausgeführt wird, wird als Vorgang für einen bestimmten Ressourcentyp modelliert. Wenn der Vorgangstyp „Schreiben“, „Löschen“ oder „Aktion“ ist, werden die Datensätze zum Start und zum Erfolg oder Fehler dieses Vorgangs in der Kategorie „Administration“ aufgezeichnet. Die Kategorie „Administration“ enthält außerdem alle Änderungen an der rollenbasierten Zugriffssteuerung in einem Abonnement.

### <a name="sample-event"></a>Beispielereignis
```json
{
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
}
```

### <a name="property-descriptions"></a>Beschreibungen der Eigenschaften
| Elementname | Beschreibung |
| --- | --- |
| authorization |Blob mit RBAC-Eigenschaften des Ereignisses. Enthält normalerweise die Eigenschaften „action“, „role“ und „scope“. |
| caller |E-Mail-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch auf Grundlage der Verfügbarkeit ausgeführt hat. |
| channels |Einer der folgenden Werte: „Admin“, „Operation“ |
| claims |Das JWT-Token, das von Active Directory zum Authentifizieren des Benutzers oder der Anwendung zur Ausführung dieses Vorgangs in Resource Manager verwendet wird. |
| correlationId |Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die über die gleiche correlationId verfügen, gehören zu derselben übergeordneten Aktion. |
| Beschreibung |Statische Beschreibung eines Ereignisses in Textform. |
| eventDataId |Eindeutiger Bezeichner eines Ereignisses. |
| httpRequest |Blob, das die HTTP-Anforderung beschreibt. Umfasst üblicherweise „clientRequestId“, „clientIpAddress“ und „method“ (HTTP-Methode, z.B. PUT). |
| Einstellung |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| ResourceGroupName |Name der Ressourcengruppe für die betroffene Ressource. |
| resourceProviderName |Name des Ressourcenanbieters für die betroffene Ressource. |
| resourceId |Ressourcen-ID der betroffenen Ressource. |
| operationId |Eine GUID, die von den Ereignissen eines einzelnen Vorgangs gemeinsam genutzt wird. |
| operationName |Name des Vorgangs. |
| Eigenschaften |Satz mit `<Key, Value>`-Paaren (Wörterbuch), die Details des Ereignisses beschreiben. |
| status |Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus |Normalerweise der HTTP-Statuscode des entsprechenden REST-Aufrufs, kann aber auch andere Zeichenfolgen zum Beschreiben eines Unterstatus enthalten, z.B. diese häufigen Werte: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504). |
| eventTimestamp |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| submissionTimestamp |Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war. |
| subscriptionId |Azure-Abonnement-ID |

## <a name="service-health"></a>Dienstintegrität
Diese Kategorie enthält Datensätze zu allen Incidents im Zusammenhang mit der Dienstintegrität, die in Azure aufgetreten sind. Ein Beispiel für ein Ereignis in dieser Kategorie ist „Ausfallzeiten bei SQL Azure in der Region „USA, Osten““. Für Ereignisse zur Dienstintegrität gibt es fünf Varianten: Aktion erforderlich, unterstützte Wiederherstellung, Incident, Wartung, Information oder Sicherheit. Sie werden nur angezeigt, wenn eine Ressource in Ihrem Abonnement von dem Ereignis betroffen wäre.

### <a name="sample-event"></a>Beispielereignis
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited to App Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows to mitigate the impact. The next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Beschreibungen der Eigenschaften
Elementname | Beschreibung
-------- | -----------
channels | Einer der folgenden Werte: „Admin“, „Operation“
correlationId | Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die zur gleichen Uber-Aktion gehören, haben normalerweise die gleiche correlationId.
Beschreibung | Beschreibung des Ereignisses.
eventDataId | Der eindeutige Bezeichner eines Ereignisses.
eventName | Der Titel des Ereignisses.
Einstellung | Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose
resourceProviderName | Name des Ressourcenanbieters für die betroffene Ressource. Wenn nicht bekannt, ist der Wert null.
resourceType| Der Ressourcentyp der betroffenen Ressource. Wenn nicht bekannt, ist der Wert null.
subStatus | Ist in der Regel bei Ereignissen zur Dienstintegrität null.
eventTimestamp | Zeitstempel des Zeitpunkts, zu dem das Protokollereignis generiert und an das Aktivitätsprotokoll übermittelt wurde.
submissionTimestamp |   Zeitstempel des Zeitpunkts, ab dem das Ereignis im Aktivitätsprotokoll verfügbar war.
subscriptionId | Das Azure-Abonnement, in dem dieses Ereignis protokolliert wurde.
status | Zeichenfolge, die den Status des Vorgangs beschreibt. Einige häufig verwendete Werte sind: Aktiv, Aufgelöst.
operationName | Name des Vorgangs. In der Regel „Microsoft.ServiceHealth/incident/action“.
category | „ServiceHealth“
resourceId | Ressourcen-ID der betroffenen Ressource, sofern bekannt. Andernfalls wird die Abonnement-ID angegeben.
Properties.title | Der lokalisierte Titel für diese Kommunikation. Englisch ist die Standardsprache.
Properties.communication | Die lokalisierten Details der Kommunikation mit HTML-Markup. Englisch ist der Standard.
Properties.incidentType | Mögliche Werte: AssistedRecovery, ActionRequired, Information, Incident, Maintenance, Security
Properties.trackingId | Identifiziert den Incident, dem dieses Ereignis zugeordnet ist. Verwenden Sie diese Option, um die Ereignisse im Zusammenhang mit einem Vorfall zu korrelieren.
Properties.impactedServices | Ein JSON-Blob mit Escapezeichen, das die Dienste und Regionen beschreibt, die vom Incident betroffen sind. Eine Liste der Dienste, von denen jeder über einen ServiceName und eine Liste von ImpactedRegions verfügt, von denen jede einen RegionName hat.
Properties.defaultLanguageTitle | Die Kommunikation in Englisch.
Properties.defaultLanguageContent | Die Kommunikation in Englisch, entweder als HTML-Markup oder Nur-Text.
Properties.stage | Mögliche Werte für AssistedRecovery, ActionRequired, Information, Incident, Security: Active, Resolved. Für die Wartung lauten sie: Active, Planned, InProgress, Canceled, Rescheduled, Resolved, Complete.
Properties.communicationId | Die Kommunikation dieses Ereignisses ist zugeordnet.

## <a name="alert"></a>Warnung
Diese Kategorie enthält die Datensätze zu allen Aktivierungen von Azure-Warnungen. Ein Beispiel für ein Ereignis in dieser Kategorie ist „CPU-Auslastung auf „myVM“ lag in den letzten 5 Minuten über 80“. Eine Vielzahl von Azure-Systemen weist ein Konzept für Warnungen auf: Sie können eine Regel definieren und erhalten eine Benachrichtigung, wenn die Bedingungen mit der Regel übereinstimmen. Jedes Mal, wenn ein unterstützter Azure-Warnungstyp „aktiviert“ wird oder die Bedingungen erfüllt sind, sodass eine Benachrichtigung generiert wird, wird ein Datensatz der Aktivierung auch in dieser Kategorie des Aktivitätsprotokolls abgelegt.

### <a name="sample-event"></a>Beispielereignis

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in the last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Beschreibungen der Eigenschaften
| Elementname | Beschreibung |
| --- | --- |
| caller | Immer „Microsoft.Insights/alertRules“ |
| channels | Immer „Admin, Operation“ |
| claims | JSON-Blob mit dem Dienstprinzipalnamen (Service Principal Name, SPN) oder dem Ressourcentyp des Warnungsmoduls. |
| correlationId | Eine GUID im Zeichenfolgenformat. |
| Beschreibung |Statische Beschreibung des Warnungsereignisses. |
| eventDataId |Eindeutiger Bezeichner des Warnungsereignisses. |
| Einstellung |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| ResourceGroupName |Bei einer Metrikwarnung der Name der Ressourcengruppe für die betroffene Ressource. Bei anderen Warnungstypen ist dies der Name der Ressourcengruppe, die die Warnung selbst enthält. |
| resourceProviderName |Bei einer Metrikwarnung der Name des Ressourcenanbieters für die betroffene Ressource. Bei anderen Warnungstypen ist dies der Name des Ressourcenanbieters für die Warnung selbst. |
| resourceId | Bei einer Metrikwarnung der Name der Ressourcen-ID für die betroffene Ressource. Bei anderen Warnungstypen ist dies die Ressourcen-ID der Warnungsressource selbst. |
| operationId |Eine GUID, die von den Ereignissen eines einzelnen Vorgangs gemeinsam genutzt wird. |
| operationName |Name des Vorgangs. |
| Eigenschaften |Satz mit `<Key, Value>`-Paaren (Wörterbuch), die Details des Ereignisses beschreiben. |
| status |Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | In der Regel für Warnungen null. |
| eventTimestamp |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| submissionTimestamp |Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war. |
| subscriptionId |Azure-Abonnement-ID |

### <a name="properties-field-per-alert-type"></a>Feld „properties“ pro Warnungstyp
Das Feld „properties“ enthält abhängig von der Quelle des Warnungsereignisses unterschiedliche Werte. Zwei allgemeine Ereignisanbieter für Warnungen sind Aktivitätsprotokollwarnungen und Metrikwarnungen.

#### <a name="properties-for-activity-log-alerts"></a>Eigenschaften für Aktivitätsprotokollwarnungen
| Elementname | Beschreibung |
| --- | --- |
| properties.subscriptionId | Die Abonnement-ID aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.eventDataId | Die Ereignisdaten-ID aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.resourceGroup | Die Ressourcengruppe aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.resourceId | Die Ressourcen-ID aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.eventTimestamp | Der Ereigniszeitstempel aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.operationName | Der Vorgangsname aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde. |
| properties.status | Der Status aus dem Aktivitätsprotokollereignis, das verursacht hat, dass diese Warnungsregel des Aktivitätsprotokolls aktiviert wurde.|

#### <a name="properties-for-metric-alerts"></a>Eigenschaften für Metrikwarnungen
| Elementname | Beschreibung |
| --- | --- |
| properties.RuleUri | Ressourcen-ID der Metrikwarnungsregel selbst. |
| properties.RuleName | Der Name der Metrikwarnungsregel. |
| properties.RuleDescription | Die Beschreibung der Metrikwarnungsregel (wie in der Warnungsregel definiert). |
| properties.Threshold | Der Schwellenwert, der bei der Auswertung der Metrikwarnungsregel verwendet wird. |
| properties.WindowSizeInMinutes | Die Fenstergröße, die bei der Auswertung der Metrikwarnungsregel verwendet wird. |
| properties.Aggregation | Der Aggregationstyp, der in der Metrikwarnungsregel definiert ist. |
| properties.Operator | Der bedingte Operator, der bei der Auswertung der Metrikwarnungsregel verwendet wird. |
| properties.MetricName | Der Metrikname der Metrik, die bei der Auswertung der Metrikwarnungsregel verwendet wird. |
| properties.MetricUnit | Die Metrikeinheit für die Metrik, die bei der Auswertung der Metrikwarnungsregel verwendet wird. |

## <a name="autoscale"></a>Autoscale
Diese Kategorie enthält Datensätze von Ereignissen im Zusammenhang mit automatischen Skalierungsvorgängen basierend auf den Einstellungen für die automatische Skalierung, die Sie in Ihrem Abonnement definiert haben. Ein Beispiel für Ereignisse in dieser Kategorie ist „Fehler beim automatischen zentralen Hochskalieren“. Mit der automatischen Skalierung können Sie die Anzahl der Instanzen eines unterstützten Ressourcentyps basierend auf der Tageszeit und/oder Lastdaten (Metrik) mithilfe einer Einstellung für die automatische Skalierung automatisch horizontal hoch- oder herunterskalieren. Wenn die Bedingungen zum zentralen Hoch- oder Herunterskalieren erfüllt sind, werden Ereignisse zum Start und zum Erfolg oder Fehler in dieser Kategorie aufgezeichnet.

### <a name="sample-event"></a>Beispielereignis
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "The autoscale engine attempting to scale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count to 2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Beschreibungen der Eigenschaften
| Elementname | Beschreibung |
| --- | --- |
| caller | Immer „Microsoft.Insights/autoscaleSettings“ |
| channels | Immer „Admin, Operation“ |
| claims | JSON-Blob mit dem Dienstprinzipalnamen (Service Principal Name, SPN) oder dem Ressourcentyp der automatischen Skalierung. |
| correlationId | Eine GUID im Zeichenfolgenformat. |
| Beschreibung |Statische Beschreibung des Ereignisses der automatischen Skalierung. |
| eventDataId |Eindeutiger Bezeichner des Ereignisses der automatischen Skalierung. |
| Einstellung |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| ResourceGroupName |Name der Ressourcengruppe der Einstellung für die automatische Skalierung. |
| resourceProviderName |Name des Ressourcenanbieters der Einstellung für die automatische Skalierung. |
| resourceId |Ressourcen-ID der Einstellung für die automatische Skalierung. |
| operationId |Eine GUID, die von den Ereignissen eines einzelnen Vorgangs gemeinsam genutzt wird. |
| operationName |Name des Vorgangs. |
| Eigenschaften |Satz mit `<Key, Value>`-Paaren (Wörterbuch), die Details des Ereignisses beschreiben. |
| properties.Description | Detaillierte Beschreibung des Vorgangs der automatischen Skalierung. |
| properties.ResourceName | Ressourcen-ID der betroffenen Ressource (die Ressource, für die die Skalierungsaktion ausgeführt wurde) |
| properties.OldInstancesCount | Die Anzahl von Instanzen, bevor die automatische Skalierung wirksam war. |
| properties.NewInstancesCount | Die Anzahl von Instanzen, nachdem die automatische Skalierung wirksam war. |
| properties.LastScaleActionTime | Der Zeitstempel des Zeitpunkts, zu dem die automatische Skalierung aufgetreten ist. |
| status |Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind z.B.: Started, In Progress, Succeeded, Failed, Active, Resolved. |
| subStatus | In der Regel für die automatische Skalierung null. |
| eventTimestamp |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| submissionTimestamp |Zeitstempel des Zeitpunkts, ab dem das Ereignis für Abfragen verfügbar war. |
| subscriptionId |Azure-Abonnement-ID |

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Aktivitätsprotokoll (bisher „Überwachungsprotokolle“)](monitoring-overview-activity-logs.md)
* [Stream the Azure Activity Log to Event Hubs (Streamen des Azure-Aktivitätsprotokolls auf Event Hubs)](monitoring-stream-activity-logs-event-hubs.md)

