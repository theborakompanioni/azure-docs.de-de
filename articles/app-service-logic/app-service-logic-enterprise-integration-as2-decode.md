<properties 
	pageTitle="Informationen zum Enterprise Integration Pack-Connector zur Decodierung von AS2-Nachrichten | Microsoft Azure App Service | Microsoft Azure" 
	description="Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden" 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="padmavc" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/15/2016" 
	ms.author="padmavc"/>

# Erste Schritte mit der Decodierung von AS2-Nachrichten

Stellen Sie eine Verbindung mit der Decodierung von AS2-Nachrichten her, um eine sichere und zuverlässige Nachrichtenübertragung zu gewährleisten. Sie ermöglicht die digitale Signierung, Entschlüsselung und Bestätigungen über Benachrichtigungen über den Nachrichtenstatus (Message Disposition Notifications, MDNs).

## Erstellen der Verbindung

### Voraussetzungen

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Für die Verwendung des Connectors zur Decodierung von AS2-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](./app-service-logic-enterprise-integration-create-integration-account.md), zu [Partnern](./app-service-logic-enterprise-integration-partners.md) und zur [AS2-Vereinbarung](./app-service-logic-enterprise-integration-as2.md).

### Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von AS2-Nachrichten her:

1. Ein Beispiel finden Sie unter [Erstellen einer Logik-App](./app-service-logic-create-a-logic-app.md).

2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten, wie beispielsweise einen Anforderungstrigger. Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu. Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „AS2“ in das Suchfeld ein. Wählen Sie „AS2 – Decode AS2 Message“ (AS2 – AS2-Nachricht decodieren) aus.

	![Suchen von AS2](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben.

	![Erstellen der Integrationsverbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Geben Sie die Details des Integrationskontos ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

	| Eigenschaft | Details |
	| --------   | ------- |
	| Verbindungsname* | Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
	| Integrationskonto* | Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |

  	Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:

  	![Integrationsverbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Klicken Sie auf **Erstellen**.
	
6. Beachten Sie, dass die Verbindung erstellt wurde. Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort.

	![Integrationsverbindung erstellt](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png)

7. Wählen Sie bei den Ausgaben der Anforderung den Text und die Header aus.

	![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png)

## Die AS2-Decodierung führt folgende Schritte aus:

* Verarbeiten der AS2/HTTP-Header
* Überprüfen der Signatur (sofern konfiguriert)
* Entschlüsseln der Nachrichten (falls konfiguriert)
* Dekomprimieren der Nachrichten (falls konfiguriert)
* Abgleichen einer empfangenen MDN mit der ursprünglichen ausgehenden Nachricht
* Aktualisieren und Korrelieren von Datensätzen in der Nichtabstreitbarkeits-Datenbank
* Schreiben von Datensätzen für AS2-Statusberichte
* Der Inhalt der Ausgabenutzlast ist base64-codiert.
* Bestimmen, ob eine MDN erforderlich ist und ob die MDN synchron oder asynchron sein soll (ausgehend von der Konfiguration in der AS2-Vereinbarung)
* Generieren einer synchronen oder asynchronen MDN (ausgehend von der Konfiguration in der Vereinbarung)
* Festlegen der Korrelationstoken und Eigenschaften für die MDN

##Probieren Sie es selbst aus

Versuchen Sie es doch einfach mal. Klicken Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive), um Ihre eigene voll funktionsfähige Logik-App mit den AS2-Features von Logic Apps bereitzustellen.

## Nächste Schritte

[Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0824_2016-->