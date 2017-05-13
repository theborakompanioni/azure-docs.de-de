---
title: "Logic Apps B2B-Integrationskonto für Notfallwiederherstellung – Azure App Service | Microsoft-Dokumentation"
description: Logic Apps B2B-Notfallwiederherstellung
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
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: e28c1410145d8da168a73e74251ac037997d1752
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="logic-apps-b2b-cross-region-disaster-recovery"></a>Logic Apps B2B – regionsübergreifende Notfallwiederherstellung
B2B-Workloads umfassen Geldtransaktionen wie Aufträge und Rechnungen. Bei Notfallereignissen ist für Unternehmen eine schnelle Wiederherstellung von maßgeblicher Bedeutung, um die mit den Partnern getroffenen Vereinbarungen zum Servicelevel auf Unternehmensebene zu erfüllen. In diesem Artikel wird die Erstellung eines Geschäftskontinuitätsplans für B2B-Workloads beschrieben. 

* Vorbereitungen für die Notfallwiederherstellung 
* Durchführen eines Failovers auf die sekundäre Region während eines Notfallereignisses 
* Durchführen eines Fallbacks auf die primäre Region nach dem Notfallereignis

## <a name="disaster-recovery-readiness"></a>Vorbereitungen für die Notfallwiederherstellung  

1. Identifizieren Sie eine sekundäre Region, und erstellen Sie ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) in der sekundären Region. 

2. Fügen Sie Partner, Schemas und Vereinbarungen für den erforderlichen Nachrichtenfluss hinzu, bei dem der Ausführungsstatus im Integrationskonto der sekundären Region repliziert werden muss.

    > [!Tip]
    > Stellen Sie sicher, dass die Integrationskontoartefakte in den Regionen konsistent benannt sind. 
    > 
    > 

3. Erstellen Sie eine Logik-App in der sekundären Region, um den Ausführungsstatus per Pull-Vorgang von der primären Region abzurufen.  Sie muss einen **Trigger** und eine **Aktion** enthalten.  Dabei muss der Trigger mit dem Integrationskonto der primären Region und die Aktion mit dem Integrationskonto der sekundären Region verbunden werden.  Basierend auf dem angegebenen Zeitintervall fragt der Trigger die Ausführungsstatustabelle der primären Region ab und ruft gegebenenfalls die neuen Datensätze per Pull-Vorgang ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Auf diese Weise wird der inkrementelle Laufzeitstatus der primären Region in die sekundäre Region übertragen.

4. Die Geschäftskontinuität im Logic Apps-Integrationskonto unterstützt X12, AS2 und EDIFACT basierend auf B2B-Protokollen.  Ausführliche Anweisungen erhalten Sie durch Auswählen der entsprechenden Links.

5. Es wird empfohlen, alle Ressourcen der primären Region auch in einer sekundären Region bereitzustellen. Zu den Ressourcen der primären Region gehören Azure SQL-Datenbank oder Azure DocumentDB, Azure Service Bus/Azure Event Hubs für Messaging, Azure API Management sowie die Logik-Apps-Funktion von Azure App Service.   

6. Stellen Sie eine Verbindung zwischen der primären Region und der sekundären Region her. Erstellen Sie eine Logik-App in einer sekundären Region, um den Ausführungsstatus per Pull von der primären Region abzurufen. Sie muss einen Trigger und eine Aktion enthalten. Der Trigger muss mit dem Integrationskonto der primären Region verbunden werden. Die Aktion muss mit dem Integrationskonto der sekundären Region verbunden werden. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die Ausführungsstatustabelle der primären Region ab und ruft gegebenenfalls die neuen Datensätze per Pull ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Mit diesem Prozess wird der inkrementelle Laufzeitstatus der primären Region in die sekundäre Region übertragen.

Die Geschäftskontinuität im Logic Apps-Integrationskonto bietet Unterstützung basierend auf den B2B-Protokollen X12, AS2 und EDIFACT. Detaillierte Schritte zur Verwendung von X12 und AS2 finden Sie unter [X12](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#x12) und [AS2](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md#as2) in diesem Artikel.

## <a name="fail-over-to-a-secondary-region-during-a-disaster-event"></a>Durchführen eines Failovers auf eine sekundäre Region während eines Notfallereignisses
Wenn während eines Notfallereignisses die primäre Region für die Geschäftskontinuität nicht verfügbar ist, wird der Datenverkehr an die sekundäre Region weitergeleitet. Mit einer sekundären Region können Unternehmen Funktionen schnell wiederherstellen, um die mit ihren Partnern vereinbarten RPO/RTO zu erfüllen. Zudem wird dadurch der Aufwand zum Durchführen eines Failovers von einer Region auf eine andere Region auf ein Minimum beschränkt. 

Beim Kopieren von Kontrollnummern von der primären Region in eine sekundäre Region ist eine Latenzzeit zu erwarten. Um zu vermeiden, dass während eines Notfallereignisses doppelt generierte Kontrollnummern an Partner gesendet werden, wird empfohlen, Kontrollnummern in den Vereinbarungen für die sekundäre Region mithilfe von [PowerShell-Cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery) zu erhöhen.

## <a name="fall-back-to-a-primary-region-post-disaster-event"></a>Durchführen eines Fallbacks auf eine primäre Region nach dem Notfallereignis
Um ein Fallback auf eine primäre Region durchzuführen, sobald sie verfügbar ist, gehen Sie folgendermaßen vor:

1. Legen Sie fest, dass Nachrichten von Partnern in der sekundären Region nicht mehr akzeptiert werden.  

2. Erhöhen Sie die generierten Kontrollnummern für alle Vereinbarungen für die primäre Region mithilfe von [PowerShell-Cmdlets](https://blogs.msdn.microsoft.com/david_burgs_blog/2017/03/09/fresh-of-the-press-new-azure-powershell-cmdlets-for-upcoming-x12-connector-disaster-recovery).  

3. Leiten Sie Datenverkehr von der sekundären Region an die primäre Region weiter.

4. Überprüfen, ob die in der sekundären Region erstellte Logik-App aktiviert ist, die den Ausführungsstatus von der primären Region per Pull abruft.

## <a name="x12"></a>X12 
Die Geschäftskontinuität für EDI X12-Dokumente basiert auf Kontrollnummern:
* Von Partnern empfangene Kontrollnummern (eingehende Nachrichten)  
* Generierte und an Partner gesendete Kontrollnummern (ausgehende Nachrichten) 
    
    > [!Tip]
    > Sie können Logik-Apps auch mithilfe der [X12-Schnellstartvorlage](https://azure.microsoft.com/documentation/templates/201-logic-app-x12-disaster-recovery-replication/) erstellen. Als Voraussetzung für die Verwendung der Vorlage müssen Integrationskonten für die primäre und die sekundäre Region erstellt werden. Mit der Vorlage können zwei Logik-Apps erstellt werden, eine für empfangene Kontrollnummern und eine zweite für generierte Kontrollnummern. In den Logik-Apps werden die entsprechenden Trigger und Aktionen erstellt. Der Trigger wird mit dem Integrationskonto der primären Region und die Aktion mit dem Integrationskonto der sekundären Region verbunden.
    > 
    >

### <a name="control-numbers-received-from-partners"></a>Von Partnern empfangene Kontrollnummern

1. Ermöglichen Sie doppelte Überprüfungen in den Empfangseinstellungen der Vereinbarung.   
![X12-Suche](./media/logic-apps-enterprise-integration-b2b-business-continuity/dupcheck.png)  

2. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in einer sekundären Region. 

3. Suchen Sie nach **X12**, und wählen Sie **X12 – Bei Änderung einer empfangenen Kontrollnummer** aus.   
![X12-Suche](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN1.png)

4. Der Trigger fordert Sie zum Herstellen einer Verbindung mit einem Integrationskonto auf. Der Trigger muss mit dem Integrationskonto einer primären Region verbunden werden. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf **Erstellen**.  
![Name des Integrationskontos der primären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN2.png)

5. Die Einstellung **Anfangsdatum und -uhrzeit für die Abfrage von Änderungen** ist optional. Die **Häufigkeit** kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.  
![Datum/Uhrzeit und Häufigkeit](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN3.png)

6. Wählen Sie **Nächster Schritt** > **Aktion hinzufügen** aus.    
![Hinzufügen einer Aktion](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN4.png)

7. Suchen Sie nach **X12**, und wählen Sie **X12 – Empfangene Kontrollnummer hinzufügen oder aktualisieren** aus.   
![Änderung der empfangenen Kontrollnummer](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN5.png)

8. Um eine Aktion mit dem Integrationskonto einer sekundären Region zu verbinden, wählen Sie **Verbindung ändern** > **Neue Verbindung hinzufügen** aus, um eine Liste der verfügbaren Integrationskonten anzuzeigen. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf **Erstellen**.   
![Name des Integrationskontos der sekundären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN6.png)

9. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App. 
![Dynamischer Inhalt](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN7.png)

10. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die von der primären Region empfangene Kontrollnummerntabelle ab und ruft gegebenenfalls die neuen Datensätze per Pull ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status **Übersprungen** angezeigt.
![Kontrollnummerntabelle](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12recevicedCN8.png)

### <a name="control-numbers-generated-and-sent-to-partners"></a>Generierte und an Partner gesendete Kontrollnummern
1. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in einer sekundären Region.

2. Suchen Sie nach **X12**, und wählen Sie **X12 – Bei Änderung einer generierten Kontrollnummer** aus.  
![Änderung der generierten Kontrollnummer](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN1.png)

3. Der Trigger fordert Sie zum Herstellen einer Verbindung mit einem Integrationskonto auf. Der Trigger muss mit dem Integrationskonto einer primären Region verbunden werden. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf **Erstellen**.   
![Name des Integrationskontos der primären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN2.png) 

4. Die Einstellung **Anfangsdatum und -uhrzeit für die Abfrage von Änderungen** ist optional. Die **Häufigkeit** kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.  
![Datum/Uhrzeit und Häufigkeit](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN3.png)  

5. Wählen Sie **Nächster Schritt** > **Aktion hinzufügen** aus.  
![Hinzufügen einer Aktion](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN4.png)

6. Suchen Sie nach **X12**, und wählen Sie **X12 – Generierte Kontrollnummer hinzufügen oder aktualisieren** aus.   
![Hinzufügen oder Aktualisieren der generierten Kontrollnummer](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN5.png)

7. Um eine Aktion mit dem Integrationskonto einer sekundären Region zu verbinden, wählen Sie **Verbindung ändern** > **Neue Verbindung hinzufügen** aus, um eine Liste der verfügbaren Integrationskonten anzuzeigen. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf **Erstellen**.   
![Name des Integrationskontos der sekundären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN6.png)

8. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App. 
![Dynamischer Inhalt](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN7.png)

9. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die von der primären Region empfangene Kontrollnummerntabelle ab und ruft gegebenenfalls die neuen Datensätze per Pull ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status **Übersprungen** angezeigt.  
![Kontrollnummerntabelle](./media/logic-apps-enterprise-integration-b2b-business-continuity/X12generatedCN8.png)

Basierend auf dem angegebenen Zeitintervall wird der inkrementelle Laufzeitstatus einer primären Region in einer sekundären Region repliziert. Wenn während eines Notfallereignisses die primäre Region nicht verfügbar ist, wird der Datenverkehr zur Geschäftskontinuität an die sekundäre Region weitergeleitet. 

## <a name="as2"></a>AS2 
Die Geschäftskontinuität für Dokumente, die das AS2-Protokoll verwenden, basiert auf der Nachrichten-ID und dem MIC-Wert.

> [!Tip]
    > Sie können Logik-Apps auch mithilfe der [AS2-Schnellstartvorlage](https://github.com/Azure/azure-quickstart-templates/pull/3302) erstellen. Als Voraussetzung für die Verwendung der Vorlage müssen Integrationskonten für die primäre und die sekundäre Region erstellt werden. Mit der Vorlage kann eine Logik-App mit einem Trigger und einer Aktion erstellt werden. Die Logik-App erstellt eine Verbindung zwischen einem Trigger und dem Integrationskonto einer primären Region sowie zwischen einer Aktion und dem Integrationskonto einer sekundären Region.
    > 
    >

1. Erstellen Sie eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) in der sekundären Region.  

2. Suchen Sie nach **AS2**, und wählen Sie **AS2 – When a MIC value is created** (AS2 – Beim Erstellen eines MIC-Werts) aus.   
![AS2-Suche](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid1.png)

3. Ein Trigger fordert Sie zum Herstellen einer Verbindung mit einem Integrationskonto auf. Der Trigger muss mit dem Integrationskonto einer primären Region verbunden werden. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der primären Region** aus, und klicken Sie auf **Erstellen**.   
![Name des Integrationskontos der primären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid2.png)

4. Die Einstellung **DateTime to start MIC value sync** (DateTime-Wert zum Starten der Synchronisierung des MIC-Werts) ist optional. Die **Häufigkeit** kann mit einem Intervall auf **Tag**, **Stunde**, **Minute** oder **Sekunde** festgelegt werden.   
![Datum/Uhrzeit und Häufigkeit](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid3.png)

5. Wählen Sie **Nächster Schritt** > **Aktion hinzufügen** aus.  
![Hinzufügen einer Aktion](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid4.png)

6. Suchen Sie nach **AS2**, und wählen Sie **AS2 – Add or update a MIC** (MIC-Wert hinzufügen oder aktualisieren) aus.  
![Hinzufügen oder Aktualisieren eines MIC-Werts](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid5.png)

7. Um eine Aktion mit dem Integrationskonto einer sekundären Region zu verbinden, wählen Sie **Verbindung ändern** > **Neue Verbindung hinzufügen** aus, um eine Liste der verfügbaren Integrationskonten anzuzeigen. Geben Sie einen Verbindungsnamen ein, wählen Sie in der Liste das **Integrationskonto der sekundären Region** aus, und klicken Sie auf **Erstellen**.    
![Name des Integrationskontos der sekundären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid6.png)

8. Wählen Sie den dynamischen Inhalt aus, und speichern Sie die Logik-App.   
![Dynamischer Inhalt](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid7.png)

9. Basierend auf dem angegebenen Zeitintervall fragt der Trigger die Tabelle der primären Region ab und ruft gegebenenfalls die neuen Datensätze per Pull ab. Die Aktion aktualisiert sie im Integrationskonto der sekundären Region. Wenn keine Aktualisierungen vorhanden sind, wird für den Trigger der Status **Übersprungen** angezeigt.  
![Tabelle der primären Region](./media/logic-apps-enterprise-integration-b2b-business-continuity/AS2messageid8.png)

Basierend auf dem angegebenen Zeitintervall wird der inkrementelle Laufzeitstatus der primären Region in der sekundären Region repliziert. Wenn während eines Notfallereignisses die primäre Region nicht verfügbar ist, wird der Datenverkehr zur Geschäftskontinuität an die sekundäre Region weitergeleitet. 

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich über das [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).   


