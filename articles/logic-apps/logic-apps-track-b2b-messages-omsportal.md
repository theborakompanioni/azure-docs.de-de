---
title: "Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal – Azure | Microsoft-Dokumentation"
description: Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal
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
ms.sourcegitcommit: 2aa32d692da81dc34fafbc9cb6ea72a806f31e5c
ms.openlocfilehash: a4068c70a01f80086ff3fb55f8e101c7dce95dea
ms.lasthandoff: 02/27/2017


---
# <a name="track-b2b-messages-in-the-operations-management-suite-portal"></a>Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal
Die B2B-Kommunikation umfasst den Nachrichtenaustausch zwischen zwei laufenden Geschäftsprozessen oder Anwendungen. Verwenden Sie die folgenden webbasierten Nachverfolgungsfunktionen im Operations Management Suite-Portal, um zu bestätigen, ob Nachrichten richtig verarbeitet werden:

* Anzahl und Status von Nachrichten
* Bestätigungsstatus
* Korrelieren von Nachrichten mit Bestätigungen
* Ausführliche Fehlerbeschreibung für Fehler
* Suchfunktionen

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein Integrationskonto. Sie können ein [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) erstellen und die Protokollierung einrichten. Informationen zum Einrichten der Protokollierung finden Sie unter [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md).
* Eine Logik-App. Sie können eine [Logik-App](../logic-apps/logic-apps-create-a-logic-app.md) erstellen und die Protokollierung einrichten. Informationen zum Einrichten der Protokollierung finden Sie unter [Azure-Diagnose und -Warnungen](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics-and-alerts).

## <a name="add-logic-apps-b2b-solution-to-the-operations-management-suite-portal"></a>Hinzufügen der B2B-Lösung für Logic Apps zum Operations Management Suite-Portal

1. Wählen Sie im Azure-Portal **More Services** (Weitere Dienste), suchen Sie nach Log Analytics und wählen Sie **Log Analytics** aus.   
![Log Analytics suchen](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)  

2. Wählen Sie Ihre **Log Analytics**.  
![Log Analytics auswählen](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Wählen Sie **OMS-Portal**. Das Operations Management Suite-Portal wird angezeigt.   
![Suchen im Microsoft Operations Management Suite-Portal](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Wählen Sie **Lösungskatalog**.    
![Lösungskatalog wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Wählen Sie **Logic Apps B2B**.     
![„Logic Apps B2B“ wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Wählen Sie **Hinzufügen**, um **Logic Apps B2B**-Nachrichten der Startseite hinzuzufügen.  
![„Hinzufügen“ wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

7. **Logic Apps B2B Messages** wird auf der Startseite angezeigt.   
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

## <a name="track-data-in-the-operations-management-suite-portal"></a>Nachverfolgen von Daten im Operations Management Suite-Portal

1. Nach dem Verarbeiten der Nachrichten wird die aktualisierte Nachrichtenanzahl angezeigt.   
![Aktualisierte Nachrichten](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

2. Wählen Sie auf der Startseite **Logic Apps B2B Messages**, um den AS2- und X12-Nachrichtenstatus anzuzeigen.  Die Daten basieren auf einem einzelnen Tag.
![„Logic Apps B2B Messages“ (Logik-Apps-B2B-Nachrichten) auswählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

3. Wählen Sie eine AS2 oder X12-Nachricht nach Status und fahren Sie mit der Nachrichtenliste fort. Im nächsten Screenshot wird der AS2-Nachrichtenstatus angezeigt. Die Eigenschaftsbeschreibungen des AS2- und X12-Nachrichtenstatus finden Sie unter „Beschreibungen der Nachrichtenlisteneigenschaften“.  
![AS2-Nachrichtenstatus auswählen](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)
4. Wählen Sie eine Zeile in der AS2- oder X12-Nachrichtenliste aus, um die Protokollsuche anzuzeigen.  In der Protokollsuche werden alle Aktionen mit derselben Ausführungs-ID angezeigt.
![Nachrichtenstatus auswählen](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

## <a name="message-list-property-descriptions"></a>Beschreibungen der Nachrichtenlisteneigenschaften

#### <a name="as2-message-list-property-descriptions"></a>Beschreibungen der AS2-Nachrichtenlisteneigenschaften

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der in den Empfangseinstellungen konfigurierte Gastpartner oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Hostpartner |
| Receiver | Der in den Empfangseinstellungen konfigurierte Hostpartner oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Gastpartner |
| Logik-App | Logik-App, in der die AS2-Aktionen konfiguriert sind |
| Status | AS2-Nachrichtenstatus <br>Erfolg = Gute AS2-Nachricht empfangen oder gesendet, keine MDN konfiguriert <br>Erfolg = Gute AS2-Nachricht empfangen oder gesendet, MDN konfiguriert und empfangen oder MDN gesendet <br>Fehler = Fehlerhafte AS2-Nachricht empfangen, keine MDN konfiguriert <br>Ausstehend = Gute AS2-Nachricht empfangen oder gesendet, MDN konfiguriert und Funktionsbestätigung erwartet |
| Ack | MDN-Nachrichtenstatus <br>Akzeptiert = Positive MDN empfangen oder gesendet <br>Ausstehend = Warten auf Empfangen oder Senden einer MDN <br>Abgelehnt = Negative MDN empfangen oder gesendet <br>Nicht erforderlich = MDN ist in der Vereinbarung nicht konfiguriert |
| Richtung | Richtung der AS2-Nachricht |
| Korrelations-ID | ID zum Korrelieren aller Trigger und Aktionen innerhalb einer Logik-App |
| Nachrichten-ID |  AS2-Nachrichten-ID aus den Headern der AS2-Nachricht |
| Zeitstempel | Uhrzeit, zu der die AS2-Aktion die Nachricht verarbeitet hat |

#### <a name="x12-message-list-property-descriptions"></a>Beschreibungen der X12-Nachrichtenlisteneigenschaften

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der in den Empfangseinstellungen konfigurierte Gastpartner oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Hostpartner |
| Receiver | Der in den Empfangseinstellungen konfigurierte Hostpartner oder der in den Sendeeinstellungen für eine AS2-Vereinbarung konfigurierte Gastpartner |
| Logik-App | Logik-App, in der die AS2-Aktionen konfiguriert sind |
| Status | X12-Nachrichtenstatus <br>Erfolg = Gute X12-Nachricht empfangen oder gesendet, keine Funktionsbestätigung konfiguriert <br>Erfolg = Gute X12-Nachricht empfangen oder gesendet, Funktionsbestätigung konfiguriert und empfangen oder Funktionsbestätigung gesendet <br>Fehler = Fehlerhafte X12-Nachricht empfangen oder gesendet <br>Ausstehend = Gute X12-Nachricht empfangen oder gesendet, Funktionsbestätigung konfiguriert und Funktionsbestätigung erwartet. |
| Ack | Status der Funktionsbestätigung (997) <br>Akzeptiert = Positive Funktionsbestätigung empfangen oder gesendet <br>Abgelehnt = Negative Funktionsbestätigung empfangen oder gesendet <br>Ausstehend = Eine Funktionsbestätigung wird erwartet, wurde aber noch nicht empfangen <br>Ausstehend = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden <br>Nicht erforderlich = Funktionsbestätigung ist nicht konfiguriert |
| Richtung | Richtung der X12-Nachricht |
| Korrelations-ID | ID zum Korrelieren aller Trigger und Aktionen innerhalb einer Logik-App |
| Msg type |  EDI-X12-Nachrichtentyp |
| ICN | Austauschkontrollnummer der X12-Nachricht |
| TSCN | Transaktionssatzkontrollnummer der X12-Nachricht |
| Zeitstempel | Uhrzeit, zu der die X12-Aktion die Nachricht verarbeitet hat |

## <a name="queries-in-the-operations-management-suite-portal"></a>Abfragen im Operations Management Suite-Portal

Auf der Suchseite können Sie eine Abfrage erstellen. Bei der Suche können Sie die Ergebnisse anhand von Facetsteuerelementen filtern.

### <a name="create-a-query"></a>Erstellen einer Abfrage

1. Schreiben Sie in der Protokollsuche eine Abfrage, und wählen Sie **Speichern**. **Suche speichern** wird angezeigt. Informationen zum Erstellen von Abfragen finden Sie unter [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal mit einer Abfrage](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md).
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery.png)

2. Fügen Sie unter **Suche speichern** einen **Namen** und eine **Kategorie** hinzu, und wählen Sie dann **speichern**.   
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery1.png)

3. Wählen Sie zum Anzeigen der Abfrage **Favoriten**.    
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery3.png)

    ![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery4.png)

### <a name="use-a-saved-query"></a>Verwenden einer gespeicherten Abfrage

* Wählen Sie in der Protokollsuche **Favoriten**, um gespeicherte Abfragen anzuzeigen.  Wählen Sie eine Abfrage aus, um Abfrageergebnisse anzuzeigen.
![Startseite wählen](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery5.png)


## <a name="next-steps"></a>Nächste Schritte
[Benutzerdefiniertes Nachverfolgungsschema](logic-apps-track-integration-account-custom-tracking-schema.md "Learn about Custom Tracking Schema")   
[AS2-Nachverfolgungsschema](logic-apps-track-integration-account-as2-tracking-schemas.md "Learn about AS2 Tracking Schema")    
[X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md "Learn about X12 Tracking Schema")  
[Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack")

