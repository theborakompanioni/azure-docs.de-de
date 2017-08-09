---
title: "Nachverfolgen von B2B-Nachrichten in Operations Management Suite – Azure Logic Apps | Microsoft-Dokumentation"
description: "Nachverfolgen der B2B-Kommunikation für Ihr Integrationskonto und Logik-Apps in Operations Management Suite (OMS) mit Azure Log Analytics"
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
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 3ef7a4054be80547b0d91ad1f13777d915005f8b
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="track-b2b-communication-in-the-microsoft-operations-management-suite-oms"></a>Nachverfolgen der B2B-Kommunikation in Microsoft Operations Management Suite (OMS)

Nach dem Einrichten der B2B-Kommunikation zwischen zwei laufenden Geschäftsprozessen oder -anwendungen über Ihr Integrationskonto können diese Entitäten Nachrichten miteinander austauschen. Um zu überprüfen, ob diese Nachrichten richtig verarbeitet werden, können Sie AS2-, X12- und EDIFACT-Nachrichten mit [Azure Log Analytics](../log-analytics/log-analytics-overview.md) in der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) nachverfolgen. Beispielsweise können Sie diese webbasierten Nachverfolgungsfunktionen nutzen, um Nachrichten nachzuverfolgen:

* Anzahl und Status von Nachrichten
* Bestätigungsstatus
* Korrelieren von Nachrichten mit Bestätigungen
* Ausführliche Fehlerbeschreibung für Fehler
* Suchfunktionen

## <a name="requirements"></a>Anforderungen

* Eine Logik-App, für die die Diagnoseprotokollierung eingerichtet ist. Informieren Sie sich über das [Erstellen einer Logik-App](logic-apps-create-a-logic-app.md) und das [Einrichten der Protokollierung für diese Logik-App](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Ein Integrationskonto, für das die Überwachung und Protokollierung eingerichtet ist. Informieren Sie sich über das [Erstellen eines Integrationskontos](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) und das [Einrichten der Überwachung und Protokollierung für dieses Konto](../logic-apps/logic-apps-monitor-b2b-message.md).

* [Veröffentlichen Sie Diagnosedaten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) in OMS, falls Sie dies noch nicht getan haben.

> [!NOTE]
> Nachdem Sie die vorherigen Anforderungen erfüllt haben, sollten Sie über einen Arbeitsbereich in der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) verfügen. Sie sollten denselben OMS-Arbeitsbereich für die Nachverfolgung Ihrer B2B-Kommunikation in OMS nutzen. 
>  
> Falls Sie keinen OMS-Arbeitsbereich haben, können Sie sich unter [Erste Schritte mit einem Log Analytics-Arbeitsbereich](../log-analytics/log-analytics-get-started.md) über die Erstellung informieren.

## <a name="add-the-logic-apps-b2b-solution-to-the-operations-management-suite-oms"></a>Hinzufügen der Lösung „B2B-Logik-App-Verwaltung“ zur Operations Management Suite (OMS)

Damit OMS für Ihre Logik-App B2B-Nachrichten nachverfolgt, müssen Sie die Lösung **B2B-Logik-App-Verwaltung** dem OMS-Portal hinzufügen. Informieren Sie sich über das [Hinzufügen von Lösungen zu OMS](../log-analytics/log-analytics-get-started.md).

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Weitere Dienste**. Suchen Sie nach „log analytics“, und wählen Sie wie hier gezeigt die Option **Log Analytics**:

   ![Suchen nach Log Analytics](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Wählen Sie unter **Log Analytics** Ihren OMS-Arbeitsbereich aus. 

   ![Auswählen Ihres OMS-Arbeitsbereichs](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Wählen Sie unter **Verwaltung** die Option **OMS-Portal**.

   ![Auswählen des OMS-Portals](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Wählen Sie nach dem Öffnen der OMS-Startseite die Option **Lösungskatalog**.    

   ![Auswählen des Lösungskatalogs](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Suchen Sie unter **Alle Lösungen** nach **B2B-Logik-App-Verwaltung**.     

   ![Auswählen von „B2B-Logik-App-Verwaltung“](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Wählen Sie unter **B2B-Logik-App-Verwaltung** die Option **Hinzufügen**.

   ![„Hinzufügen“ auswählen](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   Auf der OMS-Startseite wird jetzt die Kachel für **Logic Apps-B2B-Nachrichten** angezeigt. 
   Auf dieser Kachel wird die Nachrichtenanzahl aktualisiert, wenn Ihre B2B-Nachrichten verarbeitet werden.

   ![OMS-Startseite, Kachel „Logic Apps-B2B-Nachrichten“](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-the-operations-management-suite"></a>Nachverfolgen des Nachrichtenstatus und der dazugehörigen Details in der Operations Management Suite

1. Nachdem Ihre B2B-Nachrichten verarbeitet wurden, können Sie den Status und die Details für diese Nachrichten anzeigen. Wählen Sie auf der OMS-Startseite die Kachel **Logic Apps-B2B-Nachrichten**.

   ![Aktualisierte Nachrichtenanzahl](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Standardmäßig werden auf der Kachel **Logic Apps-B2B-Nachrichten** Daten basierend auf einem einzelnen Tag angezeigt. Wählen Sie oben auf der OMS-Seite das Steuerelement für die Bereichsauswahl aus, um für den Datenbereich ein anderes Intervall festzulegen:
   > 
   > ![Ändern des Datenbereichs](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. Nachdem das Dashboard mit dem Nachrichtenstatus angezeigt wird, können Sie weitere Details für einen bestimmten Nachrichtentyp anzeigen, für den Daten basierend auf einem einzelnen Tag angezeigt werden. Wählen Sie die Kachel für **AS2**, **X12** oder **EDIFACT** aus.

   ![Anzeigen des Nachrichtenstatus](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   Für die von Ihnen gewählte Kachel wird eine Liste mit Nachrichten angezeigt. 
   Weitere Informationen zu den Eigenschaften der einzelnen Nachrichtentypen finden Sie unter diesen Beschreibungen von Nachrichteneigenschaften:

   * [AS2-Nachrichteneigenschaften](#as2-message-properties)
   * [X12-Nachrichteneigenschaften](#x12-message-properties)
   * [EDIFACT-Nachrichteneigenschaften](#EDIFACT-message-properties)

   Eine AS2-Nachrichtenliste kann beispielsweise wie folgt aussehen:

   ![Anzeigen von AS2-Nachrichten](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. Wählen Sie diese Nachrichten aus, und wählen Sie anschließend die Option **Herunterladen**, um die Ein- und Ausgaben für bestimmte Nachrichten anzuzeigen oder zu exportieren. Speichern Sie die ZIP-Datei nach der entsprechenden Aufforderung auf Ihrem lokalen Computer, und extrahieren Sie diese Datei. 

   Der extrahierte Ordner enthält einen Ordner für jede ausgewählte Nachricht. 
   Wenn Sie Bestätigungen einrichten, enthält der Nachrichtenordner auch Dateien mit Bestätigungsdetails. 
   Jeder Nachrichtenordner enthält mindestens die folgenden Dateien: 
   
   * Für Menschen lesbare Dateien mit den Details zur Eingabe- und Ausgabenutzlast
   * Codierte Dateien mit den Ein- und Ausgaben

   Die Ordner- und Dateinamenformate für die einzelnen Nachrichtentypen finden Sie hier:

   * [AS2-Ordner- und -Dateinamenformate](#as2-folder-file-names)
   * [X12-Ordner- und -Dateinamenformate](#x12-folder-file-names)
   * [EDIFACT-Ordner- und -Dateinamenformate](#edifact-folder-file-names)

   ![Herunterladen von Nachrichtendateien](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. Wählen Sie auf der Seite **Protokollsuche** in der Nachrichtenliste eine Nachricht aus, um alle Aktionen anzuzeigen, die über die gleiche Ausführungs-ID verfügen.

   Sie können diese Aktionen nach Spalte sortieren oder nach bestimmten Ergebnissen suchen.

   ![Aktionen mit der gleichen Ausführungs-ID](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Wählen Sie **Favoriten**, um Ergebnisse mit vorab erstellten Abfragen zu durchsuchen.

   * Informieren Sie sich über das [Erstellen von Abfragen durch das Hinzufügen von Filtern](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Oder informieren Sie sich über das [Suchen von Daten mithilfe der Protokollsuche in Log Analytics](../log-analytics/log-analytics-log-searches.md).

   * Gehen Sie wie folgt vor, um die Abfrage im Suchfeld zu ändern: Aktualisieren Sie die Abfrage mit den Spalten und Werten, die Sie als Filter verwenden möchten.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Eigenschaftsbeschreibungen und Namensformate für AS2-, X12- und EDIFACT-Nachrichten

Für jeden Nachrichtentyp sind hier die Eigenschaftsbeschreibungen und Namensformate für heruntergeladene Nachrichtendateien aufgeführt.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>Eigenschaftsbeschreibungen für AS2-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen AS2-Nachrichten aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine AS2-Vereinbarung |
| Receiver | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine AS2-Vereinbarung |
| Logik-App | Die Logik-App, in der die AS2-Aktionen eingerichtet wurden |
| Status | AS2-Nachrichtenstatus <br>Erfolg = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Es wird keine MDN eingerichtet. <br>Erfolg = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Eine MDN wird eingerichtet und empfangen, oder eine MDN wird gesendet. <br>Fehler = Eine ungültige AS2-Nachricht wurde empfangen. Es wird keine MDN eingerichtet. <br>Ausstehend = Eine gültige AS2-Nachricht wurde empfangen oder gesendet. Eine MDN wird eingerichtet, und eine MDN wird erwartet. |
| Ack | MDN-Nachrichtenstatus <br>Akzeptiert = Positive MDN wurde empfangen oder gesendet. <br>Ausstehend = Es wird auf das Empfangen oder Senden einer MDN gewartet. <br>Abgelehnt = Eine negative MDN wurde empfangen oder gesendet. <br>Nicht erforderlich = Eine MDN wurde in der Vereinbarung nicht eingerichtet. |
| Richtung | Richtung der AS2-Nachricht |
| Korrelations-ID | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| Nachrichten-ID | AS2-Nachrichten-ID aus den AS2-Nachrichtenheadern |
| Zeitstempel | Der Zeitpunkt, zu dem die AS2-Aktion die Nachricht verarbeitet hat |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2-Namensformate für heruntergeladene Nachrichtendateien

Hier sind die Namensformate für die einzelnen heruntergeladenen AS2-Nachrichtenordner und -dateien aufgeführt.

| Ordner oder Datei | Namensformat |
| :------------- | :---------- |
| Nachrichtenordner | [Absender]\_[Empfänger]\_AS2\_[Korrelations-ID]\_[Nachrichten-ID]\_[Zeitstempel] |
| Eingabe, Ausgabe und, falls eingerichtet, Bestätigungsdateien | **Eingabenutzlast**: [Absender]\_[Empfänger]\_AS2\_[Korrelations-ID]\_input_payload.txt </p>**Ausgabenutzlast**: [Absender]\_[Empfänger]\_AS2\_[Korrelations-ID]\_Ausgabe\_payload.txt </p></p>**Eingaben**: [Absender]\_[Empfänger]\_AS2\_[Korrelations-ID]\_inputs.txt </p></p>**Ausgaben**: [Absender]\_[Empfänger]\_AS2\_[Korrelations-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>Beschreibungen der Eigenschaften von X12-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen X12-Nachrichten aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine X12-Vereinbarung |
| Receiver | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine X12-Vereinbarung |
| Logik-App | Die Logik-App, in der die X12-Aktionen eingerichtet wurden |
| Status | X12-Nachrichtenstatus <br>Erfolg = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Keine Funktionsbestätigung wird eingerichtet. <br>Erfolg = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet und empfangen, oder eine Funktionsbestätigung wird gesendet. <br>Fehler = Eine ungültige X12-Nachricht wurde empfangen oder gesendet. <br>Ausstehend = Eine gültige X12-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet, und eine Funktionsbestätigung wird erwartet. |
| Ack | Status der Funktionsbestätigung (997) <br>Akzeptiert = Positive Funktionsbestätigung empfangen oder gesendet. <br>Abgelehnt = Negative Funktionsbestätigung empfangen oder gesendet. <br>Ausstehend = Funktionsbestätigung wird erwartet, wurde aber noch nicht empfangen. <br>Ausstehend = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden <br>Nicht erforderlich = Funktionsbestätigung wurde nicht eingerichtet. |
| Richtung | Richtung der X12-Nachricht |
| Korrelations-ID | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| Msg type | EDI-X12-Nachrichtentyp |
| ICN | Austauschkontrollnummer für die X12-Nachricht |
| TSCN | Transaktionssatz-Kontrollnummer für die X12-Nachricht |
| Zeitstempel | Der Zeitpunkt, zu dem die X12-Aktion die Nachricht verarbeitet hat |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12-Namensformate für heruntergeladene Nachrichtendateien

Hier sind die Namensformate für die einzelnen heruntergeladenen X12-Nachrichtenordner und -dateien aufgeführt.

| Ordner oder Datei | Namensformat |
| :------------- | :---------- |
| Nachrichtenordner | [Absender]\_[Empfänger]\_X12\_[Austauschkontrollnummer]\_[Globale Kontrollnummer]\_[Transaktionssatz-Kontrollnummer]\_[Zeitstempel] |
| Eingabe, Ausgabe und, falls eingerichtet, Bestätigungsdateien | **Eingabenutzlast**: [Absender]\_[Empfänger]\_X12\_[Austauschkontrollnummer]\_input_payload.txt </p>**Ausgabenutzlast**: [Absender]\_[Empfänger]\_X12\_[Austauschkontrollnummer]\_output\_payload.txt </p></p>**Eingaben**: [Absender]\_[Empfänger]\_X12\_[Austauschkontrollnummer]\_inputs.txt </p></p>**Ausgaben**: [Absender]\_[Empfänger]\_X12\_[Austauschkontrollnummer]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>Eigenschaftsbeschreibungen für EDIFACT-Nachrichten

Hier sind die Eigenschaftsbeschreibungen für die einzelnen EDIFACT-Nachrichten aufgeführt.

| Eigenschaft | Beschreibung |
| --- | --- |
| Sender | Der unter **Empfangseinstellungen** angegebene Gastpartner oder der unter **Sendeeinstellungen** angegebene Hostpartner für eine EDIFACT-Vereinbarung |
| Receiver | Der unter **Empfangseinstellungen** angegebene Hostpartner oder der unter **Sendeeinstellungen** angegebene Gastpartner für eine EDIFACT-Vereinbarung |
| Logik-App | Die Logik-App, in der die EDIFACT-Aktionen eingerichtet wurden |
| Status | EDIFACT-Nachrichtenstatus <br>Erfolg = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Keine Funktionsbestätigung wird eingerichtet. <br>Erfolg = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet und empfangen, oder eine Funktionsbestätigung wird gesendet. <br>Fehler = Eine ungültige EDIFACT-Nachricht wurde empfangen oder gesendet. <br>Ausstehend = Eine gültige EDIFACT-Nachricht wurde empfangen oder gesendet. Eine Funktionsbestätigung wird eingerichtet, und eine Funktionsbestätigung wird erwartet. |
| Ack | Status der Funktionsbestätigung (997) <br>Akzeptiert = Positive Funktionsbestätigung empfangen oder gesendet. <br>Abgelehnt = Negative Funktionsbestätigung empfangen oder gesendet. <br>Ausstehend = Funktionsbestätigung wird erwartet, wurde aber noch nicht empfangen. <br>Ausstehend = Funktionsbestätigung wurde generiert, konnte aber nicht an Partner gesendet werden. <br>Nicht erforderlich = Funktionsbestätigung wurde nicht eingerichtet. |
| Richtung | Richtung der EDIFACT-Nachricht |
| Korrelations-ID | ID, die alle Trigger und Aktionen in einer Logik-App korreliert |
| Msg type | EDIFACT-Nachrichtentyp |
| ICN | Austauschkontrollnummer für die EDIFACT-Nachricht |
| TSCN | Transaktionssatz-Kontrollnummer für die EDIFACT-Nachricht |
| Zeitstempel | Der Zeitpunkt, zu dem die EDIFACT-Aktion die Nachricht verarbeitet hat |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT-Namensformate für heruntergeladene Nachrichtendateien

Hier sind die Namensformate für die einzelnen heruntergeladenen EDIFACT-Nachrichtenordner und -dateien aufgeführt.

| Ordner oder Datei | Namensformat |
| :------------- | :---------- |
| Nachrichtenordner | [Absender]\_[Empfänger]\_EDIFACT\_[Austauschkontrollnummer]\_[Globale Kontrollnummer]\_[Transaktionssatz-Kontrollnummer]\_[Zeitstempel] |
| Eingabe, Ausgabe und, falls eingerichtet, Bestätigungsdateien | **Eingabenutzlast**: [Absender]\_[Empfänger]\_EDIFACT\_[Austauschkontrollnummer]\_input_payload.txt </p>**Ausgabenutzlast**: [Absender]\_[Empfänger]\_EDIFACT\_[Austauschkontrollnummer]\_output\_payload.txt </p></p>**Eingaben**: [Absender]\_[Empfänger]\_EDIFACT\_[Austauschkontrollnummer]\_inputs.txt </p></p>**Ausgaben**: [Absender]\_[Empfänger]\_EDIFACT\_[Austauschkontrollnummer]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Nächste Schritte

* [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal mit einer Abfrage](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
