---
title: Informationen zum Enterprise Integration Pack-Connector zur Decodierung von X12-Nachrichten | Microsoft Docs
description: Erfahren Sie, wie Sie Partner mit dem Enterprise Integration Pack und Logik-Apps verwenden
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: erikre
editor: ''

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: padmavc

---
# Erste Schritte mit der Decodierung von X12-Nachrichten
Überprüfung von EDI- und partnerspezifischen Eigenschaften, Generieren eines XML-Dokuments für jeden Transaktionssatz und einer Bestätigung für die verarbeitete Transaktion

## Erstellen der Verbindung
### Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Für die Verwendung des Connectors zur Decodierung von X12-Nachrichten ist ein Integrationskonto erforderlich. Hier finden Sie Details zum Erstellen eines [Integrationskontos](app-service-logic-enterprise-integration-create-integration-account.md), zu [Partnern](app-service-logic-enterprise-integration-partners.md) und zur [X12-Vereinbarung](app-service-logic-enterprise-integration-x12.md).

### Stellen Sie mithilfe der folgenden Schritte eine Verbindung mit der Decodierung von X12-Nachrichten her:
1. Ein Beispiel finden Sie unter [Erstellen einer Logik-App](app-service-logic-create-a-logic-app.md).
2. Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten, wie beispielsweise einen Anforderungstrigger. Fügen Sie im Designer für Logik-Apps einen Trigger und anschließend eine Aktion hinzu. Wählen Sie in der Dropdownliste „Von Microsoft verwaltete APIs anzeigen“ aus, und geben Sie dann „x12“ in das Suchfeld ein. Wählen Sie „X12 – Decode X12 Message“ (X12 – X12-Nachricht decodieren) aus.
   
    ![Suchen von x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)
3. Wenn Sie zuvor noch keine Verbindungen mit dem Integrationskonto hergestellt haben, werden Sie aufgefordert, die Details der Verbindung anzugeben.
   
    ![Integrationskontoverbindung](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)
4. Geben Sie die Details des Integrationskontos ein. Mit einem Sternchen gekennzeichnete Eigenschaften müssen angegeben werden.
   
   | Eigenschaft | Details |
   | --- | --- |
   | Verbindungsname* |Geben Sie einen beliebigen Namen für Ihre Verbindung ein. |
   | Integrationskonto* |Geben Sie den Namen des Integrationskontos ein. Ihr Integrationskonto und Ihre Logik-App müssen sich am gleichen Azure-Standort befinden. |
   
    Nach Abschluss des Vorgangs sehen Ihre Verbindungsdetails in etwa wie folgt aus:
   
    ![Integrationskontoverbindung erstellt](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png)
5. Klicken Sie auf **Erstellen**.
6. Beachten Sie, dass die Verbindung erstellt wurde.
   
    ![Verbindungsdetails des Integrationskontos](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png)
7. Wählen Sie die zu decodierende X12-Flatfile-Nachricht aus.
   
    ![Pflichtfelder ausfüllen](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png)

## Die X12-Decodierung führt die folgenden Schritte aus:
* Überprüfen des Umschlags anhand der Handelspartnervereinbarung
* Generieren eines XML-Dokuments für jeden Transaktionssatz
* Überprüfen von EDI- und partnerspezifischen Eigenschaften
  * Strukturelle EDI-Überprüfung und erweiterte Schemaüberprüfung
  * Überprüfung der Struktur des Austauschumschlags
  * Schemaüberprüfung des Umschlags anhand des Kontrollschemas
  * Schemaüberprüfung der Transaktionssatz-Datenelemente in Bezug auf das Nachrichtenschema
  * EDI-Überprüfung für Transaktionssatz-Datenelemente
* Sicherstellen, dass der Austausch, die Gruppe und die Transaktionssatz-Kontrollnummern keine Duplikate sind
  * Überprüfen der Austauschkontrollnummer in Bezug auf zuvor empfangene Austauschvorgänge
  * Überprüfen der Gruppenkontrollnummer in Bezug auf andere Gruppenkontrollnummern im Austausch
  * Überprüfen der Transaktionssatz-Kontrollnummer in Bezug auf andere Transaktionssatz-Kontrollnummern in dieser Gruppe
* Konvertieren des gesamten Austauschs in XML
  * Austausch in Transaktionssätze trennen – Transaktionssatz bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung nur diese Transaktionssätze an.
  * Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten: Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung den gesamten Austausch an.
  * Austausch beibehalten – Transaktionssätze bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Die X12-Decodierung hält nur die Transaktionssätze an, die die Überprüfung nicht bestehen, während alle weiteren Transaktionssätze verarbeitet werden.
  * Austausch beibehalten – Austausch bei Fehler anhalten: Erstellt ein XML-Dokument für den gesamten Batchaustausch. Wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht, hält die X12-Decodierung den gesamten Austausch an.
* Generieren einer technischen Bestätigung und/oder einer Funktionsbestätigung (sofern konfiguriert)
  * Als Ergebnis der Headerüberprüfung wird eine technische Bestätigung generiert. Die technische Bestätigung meldet den Status der Verarbeitung eines Austauschheaders und -nachspanns durch den Adressempfänger.
  * Eine Funktionsbestätigung wird als Ergebnis der Textüberprüfung generiert. Die Funktionsbestätigung meldet jeden Fehler, der während der Verarbeitung des empfangenen Dokuments aufgetreten ist.

## Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")

<!---HONumber=AcomDC_0824_2016-->