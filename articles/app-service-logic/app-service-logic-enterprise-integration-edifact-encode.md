<properties 
	pageTitle="Informationen zum Enterprise Integration Pack-Connector zur Codierung von EDIFACT-Nachrichten | Microsoft Azure App Service | Microsoft Azure" 
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

# Erste Schritte mit der Codierung von EDIFACT-Nachrichten

Überprüfung von EDI- und partnerspezifischen Eigenschaften

## Erstellen der Verbindung

### Voraussetzungen

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Für die Verwendung des Connectors zur Codierung von EDIFACT-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](./app-service-logic-enterprise-integration-create-integration-account.md), zu [Partnern](./app-service-logic-enterprise-integration-partners.md) und zur [EDIFACT-Vereinbarung](./app-service-logic-enterprise-integration-edifact.md).

### Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von EDIFACT-Nachrichten her:

1. Ein Beispiel finden Sie unter [Erstellen einer Logik-App](./app-service-logic-create-a-logic-app.md).

2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten, wie beispielsweise einen Anforderungstrigger. Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu. Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „EDIFACT“ in das Suchfeld ein. Wählen Sie entweder „Encode EDIFACT Message by agreement name“ (EDIFACT-Nachricht nach Vereinbarungsname codieren) oder „EDIFACT message by identities“ (EDIFACT-Nachricht nach Identitäten codieren) aus.

	![Suchen nach EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)

3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben.

	![Herstellen einer Integrationskontoverbindung](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactencodeimage1.png)

4. Geben Sie die Details des Integrationskontos ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.

	| Eigenschaft | Details |
	| -------- | ------- |
	| Verbindungsname* | Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
	| Integrationskonto* | Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. 

	Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:

	![Integrationskontoverbindung](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactencodeimage2.png)
	
5. Klicken Sie auf **Erstellen**.
	
6. Beachten Sie, dass die Verbindung erstellt wurde.

	![Verbindungsdetails des Integrationskontos](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactencodeimage4.png)
	
#### Encode EDIFACT Message by agreement name (EDIFACT-Nachricht nach Vereinbarungsname codieren)

7.	Geben Sie den EDIFACT-Vereinbarungsnamen und die zu codierende XML-Nachricht an.

	![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactencodeimage6.png)
	
#### Encode EDIFACT Message by identities (EDIFACT-Nachricht nach Identitäten codieren)

7. Geben Sie die Senderqualifizierer und -bezeichner und Empfängerqualifizierer und -bezeichner wie in der EDIFACT-Vereinbarung konfiguriert an. Wählen Sie die zu codierende XML-Nachricht aus.

	![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactencodeimage7.png)
	
## Die EDIFACT-Codierung führt die folgenden Schritte aus:

* Auflösen der Vereinbarung durch Abgleich von Senderqualifizierer und -bezeichner sowie von Empfängerqualifizierer und -bezeichner
* Serialisieren des EDI-Austauschs, Konvertieren von XML-codierten Nachrichten in EDI-Transaktionssätze im Austausch
* Anwenden von Transaktionssatz-Header und Nachspannsegmenten
* Generieren einer Austauschkontrollnummer, einer Gruppenkontrollnummer und einer Transaktionssatz-Kontrollnummer für jeden ausgehenden Austausch
* Ersetzen von Trennzeichen in den Nutzlastdaten
* Überprüfen von EDI- und partnerspezifischen Eigenschaften
	* Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
	* EDI-Überprüfung für Transaktionssatz-Datenelemente
	* Erweiterte Überprüfung für Transaktionssatz-Datenelemente
* Generieren eines XML-Dokuments für jeden Transaktionssatz
* Generieren einer technischen Bestätigung (Kontrollbestätigung) und/oder einer Funktionsbestätigung (sofern konfiguriert)
	* Als technische Bestätigung kennzeichnet die Kontrollbestätigung den Empfang eines Austauschvorgangs.
	* Als Funktionsbestätigung dient die Kontrollbestätigung dazu, die Annahme oder Ablehnung des empfangenen Austauschs, der Gruppe oder der Nachricht mit einer Liste von Fehlern oder nicht unterstützter Funktionen anzugeben.

## Nächste Schritte

[Weitere Informationen zum Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0824_2016-->