<properties
	pageTitle="Konfigurieren von Webhooks für Azure-Aktivitätsprotokollwarnungen | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie Webhooks mithilfe von Aktivitätsprotokollwarnungen aufrufen. "
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
	ms.date="09/15/2016"
	ms.author="ashwink"/>

# Konfigurieren eines Webhooks für eine Azure-Aktivitätsprotokollwarnung

Mithilfe von Webhooks können Benutzer eine Azure-Warnbenachrichtigung zur Nachbearbeitung oder Ausführung benutzerdefinierter Aktionen an andere Systeme weiterleiten. Sie können einen Webhook für eine Warnung verwenden, um sie an Dienste weiterzuleiten, die SMS-Nachrichten versenden, Fehler protokollieren, ein Team per Chat-/Messagingdienst benachrichtigen oder beliebige andere Aktionen ausführen. In diesem Artikel erfahren Sie, wie Sie einen Webhook für eine Azure-Aktivitätsprotokollwarnung festlegen und wie die Nutzlast für den an einen Webhook gerichteten HTTP POST-Vorgang aussieht. Informationen zur Einrichtung und zum Schema einer Azure-Metrikwarnung finden Sie auf [dieser Seite](./insights-webhooks-alerts.md). Sie können auch eine Aktivitätsprotokollwarnung einrichten, bei deren Aktivierung eine E-Mail gesendet wird.

>[AZURE.NOTE] Da sich dieses Feature momentan in der Vorschauphase befindet, ist noch mit Qualitäts- und Leistungsschwankungen zu rechnen.

Aktivitätsprotokollwarnungen können mithilfe von [Azure PowerShell-Cmdlets](./insights-powershell-samples.md#create-alert-rules), mithilfe der [plattformübergreifenden Befehlszeilenschnittstelle](./insights-cli-samples.md#work-with-alerts) oder mithilfe der [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn933805.aspx) eingerichtet werden.

## Authentifizieren des Webhooks
Der Webhook kann mithilfe folgender Methoden authentifiziert werden:

1. **Tokenbasierte Autorisierung:** Der Webhook-URI wird mit einer Token-ID gespeichert. Beispiel: `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2.	**Einfache Autorisierung:** Der Webhook-URI wird mit einem Benutzernamen und Kennwort gespeichert. Beispiel: `https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## Nutzlast und Schema
Der POST-Vorgang enthält für alle aktivitätsprotokollbasierten Warnungen die folgende JSON-Nutzlast und das folgende Schema. Dieses Schema ähnelt dem Schema metrikbasierter Warnungen.

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
|status |Wird für Metrikwarnungen verwendet. Muss für Aktivitätsprotokollwarnungen immer auf „activated“ festgelegt werden.|
|context|Der Kontext des Ereignisses.|
|resourceProviderName|Der Ressourcenanbieter der betroffenen Ressource.|
|conditionType |Muss immer „Event“ lauten.|
|name |Der Name der Warnungsregel.|
|id |Die Ressourcen-ID der Warnung.|
|description|	Die bei der Erstellung der Warnung festgelegte Warnungsbeschreibung.|
|subscriptionId |Die Azure-Abonnement-ID.|
|timestamp|	Die Zeit, zu der das Ereignis durch den Azure-Dienst generiert wurde, der die Anforderung verarbeitet hat.|
|resourceId |Ressourcen-ID der betroffenen Ressource.|
|ResourceGroupName|Der Name der Ressourcengruppe für die betroffene Ressource.|
|properties |Eine Gruppe von `<Key, Value>`-Paaren (`Dictionary<String, String>`) mit Details zum Ereignis.|
|event|Element mit Metadaten zum Ereignis.|
|authorization|Die RBAC-Eigenschaften des Ereignisses. Hierzu zählen üblicherweise „action“, „role“ und „scope“.|
|category | Kategorie des Ereignisses. Unterstützte Werte: „Administrative“, „Alert“, „Security“, „ServiceHealth“, „Recommendation“.|
|caller|Die E-Mail-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar). Kann für bestimmte Systemaufrufe NULL sein.|
|correlationId|	Üblicherweise eine GUID in Zeichenfolgenformat. Ereignisse mit „correlationId“ gehören zur gleichen übergeordneten Aktion und besitzen üblicherweise den gleichen correlationId-Wert.|
|eventDescription |Statische Beschreibung des Ereignisses.|
|eventDataId|Eindeutiger Bezeichner für das Ereignis.|
|eventSource |Der Name des Azure-Diensts oder der Infrastruktur, der bzw. die das Ereignis generiert hat.|
|httpRequest|	Umfasst üblicherweise „clientRequestId“, „clientIpAddress“ und „method“ (HTTP-Methode, z. B. PUT).|
|level|Einer der folgenden Werte: „Critical“, „Error“, „Warning“, „Informational“ oder „Verbose“.|
|operationId|Üblicherweise eine gemeinsame GUID für alle Ereignisse des gleichen Vorgangs.|
|operationName|Name des Vorgangs.|
|Eigenschaften |Die Eigenschaften des Ereignisses.|
|status|Eine Zeichenfolge. Der Status des Vorgangs. Gängige Werte: „Started“, „In Progress“, „Succeeded“, „Failed“, „Active“, „Resolved“.|
|subStatus|	Enthält üblicherweise den HTTP-Statuscode des zugehörigen REST-Aufrufs. Es können auch weitere Zeichenfolgen enthalten sein, die einen Unterstatus beschreiben. Gängige subStatus-Werte: „OK“ (HTTP-Statuscode: 200), „Erstellt“ (HTTP-Statuscode: 201), „Akzeptiert“ (HTTP-Statuscode: 202), „Kein Inhalt“ (HTTP-Statuscode: 204), „Ungültige Anforderung“ (HTTP-Statuscode: 400), „Nicht gefunden“ (HTTP-Statuscode: 404), „Konflikt“ (HTTP-Statuscode: 409), „Interner Serverfehler“ (HTTP-Statuscode: 500), „Dienst nicht verfügbar“ (HTTP-Statuscode: 503), „Gatewaytimeout“ (HTTP-Statuscode: 504)|

## Nächste Schritte
- [Weitere Informationen zum Aktivitätsprotokoll](./monitoring-overview-activity-logs.md)
- [Ausführen von Azure Automation-Skripts (Runbooks) für Azure-Warnungen](http://go.microsoft.com/fwlink/?LinkId=627081)
- [Senden einer SMS über Twilio auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
- [Senden einer Slack-Nachricht auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.
- [Senden einer Nachricht an eine Azure-Warteschlange auf der Grundlage einer Azure-Warnung mithilfe einer Logik-App](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Dieses Beispiel ist eigentlich für Metrikwarnungen konzipiert, kann jedoch für Aktivitätsprotokollwarnungen angepasst werden.

<!---HONumber=AcomDC_0921_2016-->