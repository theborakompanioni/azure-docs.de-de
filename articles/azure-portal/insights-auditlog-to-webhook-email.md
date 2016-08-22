<properties
	pageTitle="Azure Insights: Verwenden von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights | Microsoft Azure"
	description="Erfahren Sie, wie Sie in Azure Insights Einträge im Dienstüberwachungsprotokoll dazu nutzen können, um Web-URLs aufzurufen oder E-Mail-Benachrichtigungen zu senden. "
	authors="kamathashwin"
	manager=""
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="ashwink"/>

# Verwenden von Überwachungsprotokollen zum Senden von E-Mail- und Webhook-Warnbenachrichtigungen in Azure Insights

Dieser Artikel zeigt, wie Sie das Nutzlastschema verwenden können, wenn ein Überwachungsprotokollereignis einen Webhook auslöst. Außerdem wird beschrieben, wie Sie mit demselben Ereignis E-Mails senden können.

>[AZURE.NOTE] Dieses Feature steht derzeit als Vorschau zur Verfügung. Infrastruktur und Leistung für das Feature „Warnung bei Ereignissen“ werden in den nächsten Monaten verbessert. In dieser Vorschau kann nur unter Verwendung von Azure PowerShell und CLI auf dieses Feature zugegriffen werden. Der Zugriff auf das Feature über das Azure-Portal ist für einen späteren Zeitpunkt geplant.

## Webhooks
Mithilfe von Webhooks können Benutzer Azure-Warnbenachrichtigungen für die Nachbearbeitung oder benutzerdefinierte Benachrichtigungen an andere Systeme weiterleiten. Beispielsweise können Warnungen an Dienste weitergeleitet werden, die eine eingehende Webanforderung zum Senden einer SMS, zum Protokollieren von Fehlern oder zum Benachrichtigen anderer Personen über Chat-/Messagingdienste verarbeiten können. Der URI des Webhooks muss ein gültiger HTTP- oder HTTPS-Endpunkt sein.

## E-Mail
E-Mails können an eine beliebige gültige E-Mail-Adresse gesendet werden. Administratoren und Co-Administratoren des Abonnements, in der die Regel ausgeführt wird, werden ebenfalls benachrichtigt.

### Beispiel-E-Mail-Regel
Sie müssen eine E-Mail-Regel und eine Webhookregel einrichten und die Regeln so konfigurieren, dass sie bei Auftreten eines Protokollereignisses gestartet werden. Eine Beispielregel unter Verwendung von PowerShell finden Sie unter [Azure Insights PowerShell quickstart samples](insights-powershell-samples.md#alert-on-audit-log-event) (Azure Insights – PowerShell-Schnellstartbeispiele).


## Authentifizierung
Es gibt zwei Arten von Authentifizierungs-URIs:

1. Bei der tokenbasierten Authentifizierung wird der Webhook-URI mit einer Token-ID als Abfrageparameter gespeichert. Beispiel: https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue
2. Bei der Standardauthentifizierung werden eine Benutzer-ID und ein Kennwort verwendet. Beispiel: https://userid:password@mysamplealert/webcallback?someparamater=somevalue&parameter=value

## Benachrichtigung bei Überwachungsprotokollereignissen mit dem Webhook-Nutzlastschema
Wenn ein neues Ereignis verfügbar ist, führt das Feature „Warnung bei Überwachungsprotokollereignissen“ den konfigurierten Webhook mit Ereignismetadaten in der Webhooknutzlast aus. Das folgende Beispiel zeigt das Webhook-Nutzlastschema:

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

|Elementname|	Beschreibung|
|---|---|
|status |Immer auf „activated“ festgelegt|
|context|Kontext für das Ereignis|
|resourceProviderName|Ressourcenanbieter der betroffenen Ressource|
|conditionType |Ereignis|
|Name |Name der Warnungsregel|
|id |Ressourcen-ID der Warnung|
|description|	Beschreibung, die vom Ersteller der Warnung festgelegt wurde|
|subscriptionId |Azure-Abonnement-GUID|
|timestamp|	Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat|
|Ressourcen-ID |URI der Ressourcen-ID, der die Ressource eindeutig identifiziert|
|ResourceGroupName|Ressourcengruppenname der betroffenen Ressource|
|Eigenschaften |Eine Reihe von Schlüssel-Wert-Paaren (d.h. Wörterbuch<Zeichenfolge, Zeichenfolge>), die Details zum Ereignis enthalten|
|event|Element, das Metadaten zum Ereignis enthält|
|Autorisierung|Erfasst die RBAC-Eigenschaften des Ereignisses. Hierzu zählen üblicherweise „action“, „role“ und „scope“.|
|category | Kategorie des Ereignisses. Unterstützte Werte: Administrative, Alert, Security, ServiceHealth, Recommendation|
|caller|E-Mail-Adresse des Benutzers, der den Vorgang ausgeführt hat, UPN-Anspruch oder SPN-Anspruch auf Grundlage der Verfügbarkeit Kann für bestimmte Systemaufrufe NULL sein.|
|correlationId|	Üblicherweise eine GUID in Zeichenfolgenformat. Ereignisse mit Korrelations-ID gehören derselben größeren Aktion an und verwenden üblicherweise dieselbe Korrelations-ID.|
|eventDescription |Ein statischer Text, der ein Ereignis beschreibt|
|eventDataId|Eindeutiger Bezeichner eines Elements|
|eventSource |Name des Azure-Diensts oder der Infrastruktur, die das Ereignis generiert hat|
|httpRequest|	Umfasst üblicherweise „clientRequestId“, „clientIpAddress“ und „method“ (HTTP-Methode, z. B. PUT).|
|level|Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose|
|operationId|Üblicherweise eine GUID, die von allen Ereignissen gemeinsam verwendet wird, die demselben Vorgang angehören|
|operationName|Name des Vorgangs|
|Eigenschaften |Das Element innerhalb des Ereigniselelements enthält Eigenschaften zum Ereignis|
|status|Zeichenfolge, die den Status des Vorgangs beschreibt. Gängige Werte sind: Started, In Progress, Succeeded, Failed, Active, Resolved|
|subStatus|	Enthält üblicherweise den HTTP-Statuscode des zugehörigen REST-Aufrufs. Es können auch weitere Zeichenfolgen enthalten sein, die einen Unterstatus beschreiben. Gängige Werte für den Unterstatus sind z. B.: OK (HTTP-Statuscode: 200), Erstellt (HTTP-Statuscode: 201), Akzeptiert (HTTP-Statuscode: 202), Kein Inhalt (HTTP-Statuscode: 204), Ungültige Anforderung (HTTP-Statuscode: 400), Nicht gefunden (HTTP-Statuscode: 404), Konflikt (HTTP-Statuscode: 409), Interner Serverfehler (HTTP-Statuscode: 500), Dienst nicht verfügbar (HTTP-Statuscode: 503), Gatewaytimeout (HTTP-Statuscode: 504)|

<!---HONumber=AcomDC_0810_2016-->