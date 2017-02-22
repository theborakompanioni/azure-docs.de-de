---
title: Nachverfolgen von B2B-Nachrichten im OMS-Portal | Microsoft-Dokumentation
description: Nachverfolgen von B2B-Nachrichten im OMS-Portal
author: padmavc
manager: anneta
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
ms.sourcegitcommit: 53195091ac4b93ed94f432990c84c407615fc03e
ms.openlocfilehash: 9c3855c7fce5a9f38424f0bb6cd03f7a2c8d36be


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
* Ein Integrationskonto. Sie können ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erstellen und die Protokollierung aktivieren. Entsprechende Schritte finden Sie [hier](logic-apps-monitor-b2b-message.md).
* Eine Logik-App. Sie können eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) erstellen und die Protokollierung aktivieren. Entsprechende Schritte finden Sie [hier](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics-and-alerts).

## <a name="adding-logic-apps-b2b-solution-to-oms-portal"></a>Hinzufügen der B2B-Lösung für Logik-Apps zum OMS-Portal

1. Wählen Sie im Portal **Weitere Dienste**, suchen Sie nach **Log Analytics**, und wählen Sie **Log Analytics** .  
![Log Analytics suchen](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)  

2. Wählen Sie Ihre **Log Analytics**.  
![Log Analytics auswählen](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Wählen Sie **OMS-Portal**. Die Startseite des OMS-Portals wird geöffnet.   
![OMS-Portal durchsuchen](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Wählen Sie **Lösungskatalog**.    
![Lösungskatalog wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Wählen Sie **Logic Apps B2B**   .  
![„Logic Apps B2B“ wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Klicken Sie auf **Hinzufügen**, um **Logic Apps B2B Messages** der Startseite hinzuzufügen.  
![„Hinzufügen“ wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

7. Suchen Sie auf der Startseite, um **Logic Apps B2B Messages**  anzuzeigen.  
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

## <a name="tracking-data-in-oms-portal"></a>Nachverfolgen von Daten im OMS-Portal

1. Nachrichtenveröffentlichungsprozess. Die Nachrichtenanzahl auf der Startseite wird aktualisiert.   
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

2. Wenn Sie auf der Startseite **Logic Apps B2B Messages** auswählen, wird der AS2- und X12-Nachrichtenstatus angezeigt.  Die Daten basieren auf dem letzten Tag.
![„Logic Apps B2B Messages“ (Logik-Apps-B2B-Nachrichten) auswählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)



3. Durch die Auswahl von AS2- oder X12-Nachrichten nach Status wird die Nachrichtenliste angezeigt.   
![AS2-Nachrichtenstatus auswählen](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der in den Empfangseinstellungen konfigurierte Gastpartner, oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Hostpartner |
| Receiver | Der in den Empfangseinstellungen konfigurierte Hostpartner, oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Gastpartner |
| Logik-App | Logic App, in der die AS2-Aktionen konfiguriert sind |
| Status | AS2-Nachrichtenstatus. Success = gute AS2-Nachricht empfangen oder gesendet, keine MDN konfiguriert; Success = gute AS2-Nachricht empfangen oder gesendet, MDN konfiguriert und MDN empfangen oder gesendet; Failed = fehlerhafte AS2-Nachricht empfangen, keine MDN konfiguriert; Pending = gute AS2-Nachricht empfangen oder gesendet, MDN; Funktionsbestätigung konfiguriert und wird erwartet; |
| Ack | MDN-Nachrichtenstatus |
| Richtung | Richtung der AS2-Nachricht |
| Korrelations-ID | ID zum Korrelieren aller Trigger und Aktionen innerhalb einer Logic App |
| Nachrichten-ID |  AS2-Nachrichten-ID, aus den Headern der AS2-Nachricht |
| Zeitstempel | Uhrzeit, zu der die AS2-Aktion die Nachricht verarbeitet hat |
|  |  |


![X12-Nachrichtenstatus auswählen](media/logic-apps-track-b2b-messages-omsportal/x12messagelist.png)

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der in den Empfangseinstellungen konfigurierte Gastpartner, oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Hostpartner |
| Receiver | Der in den Empfangseinstellungen konfigurierte Hostpartner, oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Gastpartner |
| Logik-App | Logic App, in der die AS2-Aktionen konfiguriert sind |
| Status | X12-Nachrichtenstatus. Success = gute X12-Nachricht empfangen oder gesendet, keine Funktionsbestätigung konfiguriert; Success = gute X12-Nachricht empfangen oder gesendet, Funktionsbestätigung konfiguriert und empfangen oder gesendet; Failed = fehlerhafte X12-Nachricht empfangen oder gesendet; Pending = gute X12-Nachricht empfangen oder gesendet, Funktionsbestätigung konfiguriert und wird erwartet |
| Ack | Status der Funktionsbestätigung (997).  Accepted = positive Funktionsbestätigung empfangen oder gesendet; Rejected = negative Funktionsbestätigung empfangen oder gesendet; Pending = Funktionsbestätigung wird erwartet, wurde jedoch nicht empfangen; Pending = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden |
| Richtung | Richtung der X12-Nachricht |
| Korrelations-ID | ID zum Korrelieren aller Trigger und Aktionen innerhalb einer Logic App |
| Msg Type |  EDI-X12-Nachrichtentyp |
| ICN | Austauschkontrollnummer der X12-Nachricht |
| TSCN | Transaktionssatzkontrollnummer der X12-Nachricht |
| Zeitstempel | Uhrzeit, zu der die X12-Aktion die Nachricht verarbeitet hat |
| | |

4. Wenn Sie eine Zeile in der AS2- oder X12-Nachrichtenliste auswählen, wird die Protokollsuche angezeigt.  In der Protokollsuche werden alle Aktionen mit derselben **Ausführungs-ID** angezeigt 
![Nachrichtenstatus auswählen](media/logic-apps-track-b2b-messages-omsportal/logsearch.png).

## <a name="queries-in-oms-portal"></a>Abfragen im OMS-Portal

Auf der Suchseite können Sie eine Abfrage erstellen und die Ergebnisse dann bei der Suche anhand von Facetsteuerelementen filtern.

### <a name="how-to-create-a-query"></a>Erstellen einer Abfrage

1. Schreiben Sie in der Protokollsuche eine Abfrage, und wählen Sie **Speichern**.  [Hier](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md) finden Sie die Schritte zum Schreiben einer Abfrage.![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery.png)

2. **Suche speichern** wird geöffnet.  Geben Sie einen **Namen** und eine **Kategorie** ein, und klicken Sie auf **Speichern** .  
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery1.png)

3. Wählen Sie zum Anzeigen der Abfrage **Favoriten**  .  
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery3.png)

    ![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery4.png)

### <a name="how-to-use-a-saved-query"></a>Verwenden einer gespeicherten Abfrage

* Wählen Sie in der Protokollsuche **Favoriten**, um gespeicherte Abfragen anzuzeigen.  Auswählen eines der Abfrageergebnisse ![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery5.png)


## <a name="next-steps"></a>Nächste Schritte
[Benutzerdefiniertes Nachverfolgungsschema](logic-apps-track-integration-account-custom-tracking-schema.md "Learn about Custom Tracking Schema")   
[AS2-Nachverfolgungsschema](logic-apps-track-integration-account-as2-tracking-schemas.md "Learn about AS2 Tracking Schema")    
[X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md "Learn about X12 Tracking Schema")  
[Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 


<!--HONumber=Jan17_HO4-->


