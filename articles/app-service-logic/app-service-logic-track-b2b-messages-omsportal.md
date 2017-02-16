---
title: Nachverfolgen von B2B-Nachrichten im OMS-Portal | Microsoft-Dokumentation
description: Nachverfolgen von B2B-Nachrichten im OMS-Portal
author: padmavc
manager: erikre
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 7f64fe625b7c3b7e58f86154b9d601eff03e31d3
ms.openlocfilehash: 854db453a3af459c0398c4b88a6789c87747e920


---
# <a name="tracking-b2b-messages-in-oms-portal"></a>Nachverfolgen von B2B-Nachrichten im OMS-Portal
Die B2B-Kommunikation umfasst den Nachrichtenaustausch zwischen zwei laufenden Geschäftsprozessen oder Anwendungen. Bei der Nachverfolgung von B2B-Nachrichten im OMS-Portal werden umfangreiche webbasierte Nachverfolgungsfunktionen bereitgestellt, mit denen angezeigt werden kann, ob Nachrichten richtig verarbeitet werden.  Sie können Folgendes nachverfolgen:

* Anzahl und Status von Nachrichten
* Bestätigungsstatus
* Korrelieren von Nachrichten mit Bestätigungen
* Ausführliche Fehlerbeschreibung für Fehler
* Suchfunktionen

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein Integrationskonto. Sie können ein [Integrationskonto](app-service-logic-enterprise-integration-create-integration-account.md) erstellen und die Protokollierung aktivieren. Entsprechende Schritte finden Sie [hier](app-service-logic-monitor-b2b-message.md).
* Eine Logik-App. Sie können eine [Logik-App](app-service-logic-create-a-logic-app.md) erstellen und die Protokollierung aktivieren. Entsprechende Schritte finden Sie [hier](app-service-logic-monitor-your-logic-apps.md#azure-diagnostics-and-alerts).

## <a name="adding-logic-apps-b2b-solution-to-oms-portal"></a>Hinzufügen der B2B-Lösung für Logik-Apps zum OMS-Portal

1. Wählen Sie im Portal **More Services** (Weitere Dienste), suchen Sie nach **Log Analytics**, und wählen Sie **Log Analytics**
![Log Analytics suchen](./media/app-service-logic-track-b2b-messages-omsportal/browseloganalytics.png).  

2. Wählen Sie **Log Analytics**
![Log Analytics auswählen](./media/app-service-logic-track-b2b-messages-omsportal/selectla.png).

3. Wählen Sie **OMS-Portal**. Die Startseite des OMS-Portals wird geöffnet![OMS-Portal durchsuchen](./media/app-service-logic-track-b2b-messages-omsportal/omsportalpage.png).

4. Wählen Sie **Lösungskatalog**.    
![Lösungskatalog wählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage1.png)

5. Wählen Sie **Logic Apps B2B** (Logik-Apps-B2B)
![„Logic Apps B2B“ (Logik-Apps-B2B) auswählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage2.png).

6. Klicken Sie auf **Hinzufügen**, um **Logic Apps B2B Messages** (Logik-Apps-B2B-Nachrichten) zur Startseite hinzuzufügen![„Hinzufügen“ auswählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage3.png).

7. Suchen Sie auf der Startseite, um **Logic Apps B2B Messages** (Logik-Apps-B2B-Nachrichten) anzuzeigen
![Startseite auswählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage4.png).

## <a name="tracking-data-in-oms-portal"></a>Nachverfolgen von Daten im OMS-Portal

1. Nachrichtenveröffentlichungsprozess. Die Nachrichtenanzahl auf der Startseite wird aktualisiert ![Startseite auswählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage6.png).

2. Wenn Sie auf der Startseite **Logic Apps B2B Messages** (Logik-Apps-B2B-Nachrichten) auswählen, wird der AS2- und X12-Nachrichtenstatus angezeigt.  Die Daten basieren auf dem letzten Tag.
![„Logic Apps B2B Messages“ (Logik-Apps-B2B-Nachrichten) auswählen](./media/app-service-logic-track-b2b-messages-omsportal/omshomepage5.png)

3. Durch die Auswahl von AS2- oder X12-Nachrichten nach Status wird die Nachrichtenliste angezeigt ![AS2-Nachrichtenstatus auswählen](./media/app-service-logic-track-b2b-messages-omsportal/as2messagelist.png)

    ![X12-Nachrichtenstatus auswählen](./media/app-service-logic-track-b2b-messages-omsportal/x12messagelist.png)

4. Wenn Sie eine Zeile in der AS2- oder X12-Nachrichtenliste auswählen, wird die Protokollsuche angezeigt.  In der Protokollsuche werden alle Aktionen mit derselben **Ausführungs-ID** angezeigt 
![Nachrichtenstatus auswählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearch.png).

## <a name="queries-in-oms-portal"></a>Abfragen im OMS-Portal

Auf der Suchseite können Sie eine Abfrage erstellen und die Ergebnisse dann bei der Suche anhand von Facetsteuerelementen filtern.

### <a name="how-to-create-a-query"></a>Erstellen einer Abfrage

1. Schreiben Sie in der Protokollsuche eine Abfrage, und wählen Sie **Speichern**.  [Hier](app-service-logic-track-b2b-messages-omsportal-query-filter-control-number.md) finden Sie die Schritte zum Schreiben einer Abfrage.![Startseite wählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearchaddquery.png)

2. **Suche speichern** wird geöffnet.  Geben Sie einen **Namen** und eine **Kategorie** ein, und klicken Sie auf **Speichern** .  
![Startseite wählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearchaddquery1.png)

3. Wählen Sie zum Anzeigen der Abfrage **Favoriten**  .  
![Startseite wählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearchaddquery3.png)

    ![Startseite wählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearchaddquery4.png)

### <a name="how-to-use-a-saved-query"></a>Verwenden einer gespeicherten Abfrage

1. Wählen Sie in der Protokollsuche **Favoriten**, um gespeicherte Abfragen anzuzeigen.  Auswählen eines der Abfrageergebnisse ![Startseite wählen](./media/app-service-logic-track-b2b-messages-omsportal/logsearchaddquery5.png)


## <a name="next-steps"></a>Nächste Schritte
[Benutzerdefiniertes Nachverfolgungsschema](app-service-logic-track-integration-account-custom-tracking-shema.md "Learn about Custom Tracking Schema")   
[AS2-Nachverfolgungsschema](app-service-logic-track-integration-account-as2-tracking-shemas.md "Learn about AS2 Tracking Schema")    
[X12-Nachverfolgungsschema](app-service-logic-track-integration-account-x12-tracking-shemas.md "Learn about X12 Tracking Schema")  
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 


<!--HONumber=Nov16_HO3-->


