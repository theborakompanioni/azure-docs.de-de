---
title: "Logic Apps B2B-Integrationskonto für Notfallwiederherstellung – Azure Logic Apps | Microsoft-Dokumentation"
description: Azure Logic Apps B2B-Notfallwiederherstellung
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 7f469fb309f92b86dbf289d3a0462ba9042af48a
ms.openlocfilehash: 0ebaa553f360cc7deb52139fe9adaaca39a2ee23
ms.lasthandoff: 04/13/2017


---

# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B – regionsübergreifende Notfallwiederherstellung
B2B-Workloads umfassen Geldtransaktionen, z.B. Aufträge oder Rechnungen.  Für Unternehmen ist eine schnelle Wiederherstellung im Fall von Notfallereignissen von maßgeblicher Bedeutung, um die mit den Partnern getroffenen Vereinbarungen zum Servicelevel auf Unternehmensebene zu erfüllen.  In diesem Thema wird die Erstellung eines Geschäftskontinuitätsplans für B2B-Workloads beschrieben.  Dies umfasst Folgendes:

* Erstellen eines Integrationskontos in der sekundären Region
* Herstellen einer Verbindung zwischen der primären Region und der sekundären Region 
* Durchführen eines Failovers auf die sekundäre Region während eines Notfallereignisses
* Durchführen eines Fallbacks auf die primäre Region nach dem Notfallereignis

## <a name="create-an-integration-account-in-secondary-region"></a>Erstellen eines Integrationskontos in der sekundären Region
1. Wählen Sie eine sekundäre Region aus, und erstellen Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).  

2. Fügen Sie Partner, Schemas und Vereinbarungen für den erforderlichen Nachrichtenfluss hinzu, bei dem der Ausführungsstatus im Integrationskonto der sekundären Region repliziert werden muss.

    > [!Tip]
    > Stellen Sie sicher, dass die Integrationskontoartefakte in den Regionen konsistent benannt sind. 
    > 
    > 

3. Es empfiehlt sich, alle Ressourcen der primären Region (z.B. SQL Azure- oder DocumentDB-Datenbanken oder für Nachrichten, APIM oder Logik-Apps verwendete Service Bus/Event Hubs) auch in der sekundären Region bereitzustellen.  

## <a name="establish-a-connection-from-primary-to-secondary"></a>Herstellen einer Verbindung zwischen der primären Region und der sekundären Region 
Erstellen Sie eine Logik-App in der sekundären Region, um den Ausführungsstatus per Pull-Vorgang von der primären Region abzurufen.  Sie muss einen **Trigger** und eine **Aktion** enthalten.  Dabei muss der Trigger mit dem Integrationskonto der primären Region und die Aktion mit dem Integrationskonto der sekundären Region verbunden werden.  Basierend auf dem angegebenen Zeitintervall fragt der Trigger die Ausführungsstatustabelle der primären Region ab und ruft gegebenenfalls die neuen Datensätze per Pull-Vorgang ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Auf diese Weise wird der inkrementelle Laufzeitstatus der primären Region in die sekundäre Region übertragen.

Die Geschäftskontinuität im Logic Apps-Integrationskonto unterstützt X12, AS2 und EDIFACT basierend auf B2B-Protokollen.  Ausführliche Anweisungen erhalten Sie durch Auswählen der entsprechenden Links.

* [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12)
* [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2)
* EDIFACT (in Kürze verfügbar)

## <a name="fail-over-to-secondary-region-during-a-disaster-event"></a>Durchführen eines Failovers auf die sekundäre Region während eines Notfallereignisses
Wenn während eines Notfallereignisses die primäre Region für die Geschäftskontinuität nicht verfügbar ist, wird der Datenverkehr an die sekundäre Region weitergeleitet. Aufgrund der vorhandenen sekundären Region können Geschäftsfunktionen rasch wiederhergestellt und so die mit den Partnern vereinbarten Recovery Time/Point Objectives (RPO/RTO) erfüllt werden.  Zudem wird dadurch der Aufwand zum Durchführen eines Failovers auf eine andere Region auf ein Minimum beschränkt. 

Beim Kopieren von Kontrollnummern von der primären in die sekundäre Region ist eine Latenzzeit zu erwarten.  Um zu vermeiden, dass während eines Notfallereignisses doppelt generierte Kontrollnummern an Partner gesendet werden, wird empfohlen, Kontrollnummern in den **Vereinbarungen für die sekundäre Region** mithilfe von [PowerShell-Cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery) zu erhöhen.

## <a name="fall-back-to-primary-region-post-disaster-event"></a>Durchführen eines Fallbacks auf die primäre Region nach dem Notfallereignis
Wenn die primäre Region verfügbar ist, wird mit den folgenden Schritten ein Fallback auf die primäre Region durchgeführt.     
* Festlegen, dass Nachrichten von Partnern in der **sekundären Region** nicht mehr akzeptiert werden   
* Erhöhen der generierten Kontrollnummern für alle **Vereinbarungen für die primäre Region** mithilfe von [PowerShell-Cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery)   
* Weiterleiten des Datenverkehrs von der sekundären zur primären Region   
* Überprüfen, ob die in der sekundären Region erstellte Logik-App aktiviert ist, die den Ausführungsstatus von der primären Region per Pull-Vorgang abruft    

## <a name="x12"></a>X12 
Die Geschäftskontinuität für EDI X12-Dokumente basiert auf Kontrollnummern.   
* Von Partnern empfangene Kontrollnummern (eingehende Nachrichten)  
* Generierte und an Partner gesendete Kontrollnummern (ausgehende Nachrichten)  
    
    > [!Tip]
    > Sie können Logik-Apps auch mithilfe der [X12-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) erstellen.  Als Voraussetzung für die Verwendung der Vorlage müssen Integrationskonten für die primäre und die sekundäre Region erstellt werden.  Mit der Vorlage können zwei Logik-Apps erstellt werden, eine für empfangene Kontrollnummern und eine zweite für generierte Kontrollnummern.  In den Logik-Apps werden die entsprechenden Trigger und Aktionen erstellt. Der Trigger wird mit dem Integrationskonto der primären Region und die Aktion mit dem Integrationskonto der sekundären Region verbunden.
    > 
    >

### <a name="control-numbers-received-from-the-partners"></a>Von den Partnern empfangene Kontrollnummern
1. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in der sekundären Region.   

2. Suchen Sie **X12**, und wählen Sie **X12 – Bei Änderung einer empfangenen Kontrollnummer**   aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN1.png)

3. Durch Auswählen des Triggers erfolgt die Aufforderung zum Herstellen einer Verbindung mit dem Integrationskonto. Der Trigger muss mit dem Integrationskonto der primären Region verbunden werden.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf „Erstellen“.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN2.png)

4. Optional kann ein DateTime-Wert zum Starten der Synchronisierung der Kontrollnummern angegeben werden.  Die Häufigkeit kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.    
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN3.png)

5. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen**  .  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN4.png)

6. Suchen Sie **X12**, und wählen Sie **X12 –Empfangene Kontrollnummer hinzufügen oder aktualisieren**  aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN5.png)

7. Die Aktion muss mit dem Integrationskonto der sekundären Region verbunden werden.  Wählen Sie **Verbindung ändern** aus. Unter **Neue Verbindung hinzufügen** werden dann die verfügbaren Integrationskonten aufgelistet.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf „Erstellen“.     
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN6.png)

8. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN7.png)

9. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die von der primären Region empfangene Kontrollnummerntabelle ab und ruft die neuen Datensätze per Pull-Vorgang ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region.  Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status „Übersprungen“ angezeigt.
    
    > [!Tip]
    > Durch Aktivieren der Prüfung auf Duplikate in den Empfangseinstellungen für Vereinbarungen wird der Laufzeitstatus der empfangenen Kontrollnummern beibehalten. Zudem werden die Trigger in regelmäßigen Intervallen ausgelöst.
    > 
    >

![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN8.png)


### <a name="control-numbers-generated-and-send-to-the-partners"></a>Generierte und an die Partner gesendete Kontrollnummern
1. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in der sekundären Region.  

2. Suchen Sie **X12**, und wählen Sie **X12 – Bei Änderung einer generierten Kontrollnummer**  aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN1.png)

3. Durch Auswählen des Triggers erfolgt die Aufforderung zum Herstellen einer Verbindung mit dem Integrationskonto. Der Trigger muss mit dem Integrationskonto der primären Region verbunden werden.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf „Erstellen“.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN2.png) 

4. Optional kann ein DateTime-Wert zum Starten der Synchronisierung der Kontrollnummern angegeben werden.  Die Häufigkeit kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN3.png)  

5. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen** .  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN4.png)

6. Suchen Sie **X12**, und wählen Sie **X12 –Generierte Kontrollnummer hinzufügen oder aktualisieren**  aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN5.png)

7. Die Aktion muss mit dem Integrationskonto der sekundären Region verbunden werden.  Wählen Sie **Verbindung ändern** aus. Unter **Neue Verbindung hinzufügen** werden dann die verfügbaren Integrationskonten aufgelistet.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf „Erstellen“.    
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN6.png)

8. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN7.png)

9. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die von der primären Region empfangene Kontrollnummerntabelle ab und ruft die neuen Datensätze per Pull-Vorgang ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region.  Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status „Übersprungen“ angezeigt.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN8.png)

Basierend auf dem angegebenen Zeitintervall wird der inkrementelle Laufzeitstatus der primären Region in der sekundären Region repliziert.  Wenn während eines Notfallereignisses die primäre Region nicht verfügbar ist, wird der Datenverkehr zur Geschäftskontinuität an die sekundäre Region weitergeleitet. 

## <a name="as2"></a>AS2 
Die Geschäftskontinuität für Dokumente, die das AS2-Protokoll verwenden, basiert auf der Nachrichten-ID und dem MIC-Wert.

> [!Tip]
    > Sie können Logik-Apps auch mithilfe der [AS2-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/pull/3302) erstellen.  Als Voraussetzung für die Verwendung der Vorlage müssen Integrationskonten für die primäre und die sekundäre Region erstellt werden.  Mit der Vorlage kann eine Logik-App mit einem Trigger und einer Aktion erstellt werden.  Es wird eine Verbindung zwischen dem Trigger und dem Integrationskonto der primären Region sowie zwischen der Aktion und dem Integrationskonto der sekundären Region hergestellt.
    > 
    >

1. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in der sekundären Region.  

2. Suchen Sie **AS2**, und wählen Sie **AS2 – When a MIC value is created**  (AS2 – Beim Erstellen eines MIC-Werts) aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid1.png)

3. Durch Auswählen des Triggers erfolgt die Aufforderung zum Herstellen einer Verbindung mit dem Integrationskonto. Der Trigger muss mit dem Integrationskonto der primären Region verbunden werden.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf „Erstellen“.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid2.png)

4. Optional kann ein DateTime-Wert zum Starten der Synchronisierung des MIC-Werts angegeben werden.  Die Häufigkeit kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid3.png)

5. Klicken Sie auf **Neuer Schritt** und anschließend auf **Aktion hinzufügen** .  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid4.png)

6. Suchen Sie **AS2**, und wählen Sie **AS2 – Add or update a MIC**  (MIC-Wert hinzufügen oder aktualisieren) aus.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid5.png)

7. Die Aktion muss mit dem Integrationskonto der sekundären Region verbunden werden.  Wählen Sie **Verbindung ändern** aus. Unter **Neue Verbindung hinzufügen** werden dann die verfügbaren Integrationskonten aufgelistet.  Geben Sie einen Verbindungsnamen an, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf „Erstellen“.    
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid6.png)

8. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App.   
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid7.png)

9. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die Tabelle der primären Region ab und ruft die neuen Datensätze per Pull-Vorgang ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region.  Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status „Übersprungen“ angezeigt.  
![](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid8.png)

Basierend auf dem angegebenen Zeitintervall wird der inkrementelle Laufzeitstatus der primären Region in der sekundären Region repliziert.  Wenn während eines Notfallereignisses die primäre Region nicht verfügbar ist, wird der Datenverkehr zur Geschäftskontinuität an die sekundäre Region weitergeleitet. 

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich über das [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).   
