---
title: IT Service Management Connector in OMS | Microsoft-Dokumentation
description: "Verwenden Sie den IT Service Management Connector zum zentralen Überwachen und Verwalten von ITSM-Arbeitselementen in OMS und zum schnellen Beheben von Problemen."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Zentrales Verwalten von ITSM-Arbeitselementen mit dem IT Service Management Connector (Vorschau)

![Symbol für den IT Service Management Connector](./media/log-analytics-itsmc/itsmc-symbol.png)

Sie können den IT Service Management Connector (ITSMC) in OMS Log Analytics zum zentralen Überwachen und Verwalten von Arbeitselementen Ihrer ITSM-Produkte/-Dienste verwenden.

Der IT Service Management Connector wird in Ihre vorhandenen Produkte und Dienste für die IT-Dienstverwaltung (IT Service Management, ITSM) in OMS Log Analytics integriert.  Die Lösung weist eine bidirektionale Integration in ITSM-Produkte und -Dienste auf und bietet so den OMS-Benutzern eine Option zum Erstellen von Incidents, Warnungen oder Ereignissen in der ITSM-Lösung. Der Connector importiert zudem Daten, z.B. Incidents und Änderungsanforderungen, aus der ITSM-Lösung in OMS Log Analytics.

Mit dem IT Service Management Connector können Sie folgende Aktionen ausführen:

  - Zentrales Überwachen und Verwalten von Arbeitselementen für ITSM-Produkte und -Dienste in Ihrem Unternehmen.
  - Erstellen von ITSM Arbeitselementen (z.B. Warnungen, Ereignisse, Incidents) in ITSM über OMS-Warnungen und die Protokollsuche.
  - Lesen von Incidents und Änderungsanforderungen aus der ITSM-Lösung und Korrelieren dieser Daten mit relevanten Protokolldaten im Log Analytics-Arbeitsbereich.
  - Suchen und Auflösen von unerwarteten und ungewöhnlichen Ereignissen, noch bevor die Endbenutzer anrufen und sie dem Helpdesk melden.
  - Importieren der Daten von Arbeitselementen in Log Analytics und Erstellen von Key Performance Indicator-Berichten (KPI).  Mit diesen Berichten können Sie mehrere wichtige Elemente wie die Schadsoftwarebewertung identifizieren, bewerten und bearbeiten.
  - Anzeigen geordneter Dashboards mit detaillierteren Informationen zu Incidents, Änderungsanforderungen und betroffenen Systemen.
  - Schnelleres Behandeln von Problemen durch die Korrelation mit anderen Lösungen im Log Analytics-Arbeitsbereich.


## <a name="prerequisites"></a>Voraussetzungen

Zum Importieren von ITSM-Arbeitselementen in OMS Log Analytics erfordert die Lösung eine Verbindung zwischen dem IT Service Management Connector in OMS und dem ITSM-Produkt/-Dienst, aus dem die Arbeitselemente importiert werden.


## <a name="configuration"></a>Konfiguration

Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Verfahrens Ihrem OMS-Arbeitsbereich die IT Service Management Connector-Lösung hinzu.

Kachel für den IT Service Management Connector im Lösungskatalog:

![Connector-Kachel](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Nach dem erfolgreichen Hinzufügen wird der IT Service Management Connector unter **OMS** > **Einstellungen** > **Verbundene Quellen** angezeigt.

![ITSMC verbunden](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Standardmäßig aktualisiert der IT Service Management Connector die Daten der Verbindung einmal alle 24 Stunden. Um die Daten Ihrer Verbindung bei Änderungen oder Vorlagenupdates, die Sie vornehmen, sofort zu aktualisieren, klicken Sie auf die Aktualisierungsschaltfläche, die neben der Verbindung angezeigt wird.

 ![ITSMC-Aktualisierung](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Management Packs
Diese Lösung erfordert keine Management Packs.

## <a name="connected-sources"></a>Verbundene Quellen

Die folgenden ITSM-Produkte/-Dienste werden vom IT Service Management-Connector unterstützt:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>Verwenden der Lösung

Nachdem Sie den OMS IT Service Management Connector mit Ihrem ITSM-Dienst verbunden haben, erfassen die Log Analytics-Dienste Daten aus verbundenen ITSM-Produkten/-Diensten.

> [!NOTE]
> - Daten, die von der IT Service Management Connector-Lösung importiert werden, werden in Log Analytics als Ereignisse mit dem Namen **ServiceDesk_CL** angezeigt.
- Ereignisse enthalten ein Feld mit dem Namen **ServiceDeskWorkItemType_s**. Der Wert kann ein Incident oder eine Änderungsanforderung sein, je nach den Arbeitselementdaten im **ServiceDesk_CL**-Ereignis.

## <a name="input-data"></a>Eingabedaten
Arbeitselemente, die aus den ITSM-Produkten/-Diensten importiert werden.

Die folgenden Informationen sind Beispiele für Daten, die vom IT Service Management Connector gesammelt werden:

> [!NOTE]
> Abhängig vom in Log Analytics importierten Arbeitselementtyp enthält **ServiceDesk_CL** die folgenden Felder:

**Arbeitselement:** **Incidents**  
ServiceDeskWorkItemType_s="Incident"

**Felder**

- ServiceDeskConnectionName
- Service Desk-ID
- Zustand
- Dringlichkeit
- Auswirkung
- Priority
- Eskalation
- Erstellt von
- Gelöst von
- Geschlossen von
- Quelle
- Zugewiesen zu
- Kategorie
- Titel
- Beschreibung
- Erstellt am
- Geschlossen am
- Gelöst am
- Datum der letzten Änderung
- Computer


**Arbeitselement:** **Änderungsanforderungen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Felder**
- ServiceDeskConnectionName
- Service Desk-ID
- Erstellt von
- Geschlossen von
- Quelle
- Zugewiesen zu
- Titel
- Typ
- Kategorie
- Zustand
- Eskalation
- Konfliktstatus
- Dringlichkeit
- Priority
- Risiko
- Auswirkung
- Zugewiesen zu
- Erstellt am
- Geschlossen am
- Datum der letzten Änderung
- Anforderungsdatum
- Geplantes Startdatum
- Geplantes Enddatum
- Startdatum der Arbeit
- Enddatum der Arbeit
- Beschreibung
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>Ausgabedaten für einen ServiceNow-Incident

| OMS-Feld | ITSM-Feld |
|:--- |:--- |
| ServiceDeskId_s| NUMBER |
| IncidentState_s | Zustand |
| Urgency_s |Dringlichkeit |
| Impact_s |Auswirkung|
| Priority_s | Priority |
| CreatedBy_s | Geöffnet von |
| ResolvedBy_s | Gelöst von|
| ClosedBy_s  | Geschlossen von |
| Source_s| Kontakttyp |
| AssignedTo_s | Zugewiesen zu  |
| Category_s | Kategorie |
| Title_s|  Kurzbeschreibung |
| Description_s|  Hinweise |
| CreatedDate_t|  Geöffnet |
| ClosedDate_t| closed|
| ResolvedDate_t|Gelöst|
| Computer  | Konfigurationselement |

## <a name="output-data-for-a-servicenow-change-request"></a>Ausgabedaten für eine ServiceNow-Änderungsanforderung

| OMS-Feld | ITSM-Feld |
|:--- |:--- |
| ServiceDeskId_s| NUMBER |
| CreatedBy_s | Angefordert von |
| ClosedBy_s | Geschlossen von |
| AssignedTo_s | Zugewiesen zu  |
| Title_s|  Kurzbeschreibung |
| Type_s|  Typ |
| Category_s|  Kategorie |
| CRState_s|  Zustand|
| Urgency_s|  Dringlichkeit |
| Priority_s| Priority|
| Risk_s| Risiko|
| Impact_s| Auswirkung|
| RequestedDate_t  | Datum der Anforderung |
| ClosedDate_t | Geschlossen am |
| PlannedStartDate_t  |     Geplantes Startdatum |
| PlannedEndDate_t  |   Geplantes Enddatum |
| WorkStartDate_t  | Tatsächliches Startdatum |
| WorkEndDate_t | Tatsächliches Enddatum|
| Description_s | Beschreibung |
| Computer  | Konfigurationselement |

**Log Analytics-Beispielbildschirm für ITSM-Daten:**

![Log Analytics-Bildschirm](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>IT Service Management Connector – Integration in andere OMS-Lösungen

Der IT Service Management Connector unterstützt derzeit die Integration in die Service Map-Lösung.

Service Map ermittelt automatisch die Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen. Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich. Weitere Informationen: [Service Map](../operations-management-suite/operations-management-suite-service-map.md).

Mit dieser Integration können Sie die Service Desk-Elemente anzeigen, die in den ITSM-Lösungen erstellt werden, wie im folgenden Beispiel gezeigt:

![Integrierte Lösung ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Erstellen von ITSM-Arbeitselementen für OMS-Warnungen

Für die OMS-Warnungen können Sie zugehörige Arbeitselemente in den verbundenen ITSM-Quellen erstellen.  Führen Sie dazu das folgende Verfahren aus:

1. Führen Sie im Fenster **Protokollsuche** eine Protokollsuchabfrage zum Anzeigen von Daten aus. Abfrageergebnisse sind die Quelle für Arbeitselemente.
2. Klicken Sie in **Protokollsuche** auf **Warnung**, um die Seite **Warnungsregel hinzufügen** zu öffnen.

    ![Log Analytics-Bildschirm](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Geben Sie im Fenster **Warnungsregel hinzufügen** die erforderlichen Details für **Name**, **Schweregrad**, **Suchabfrage** und **Warnungskriterien** (Zeitfenster/metrische Maßeinheit) an.
4. Wählen Sie **Ja** für **ITSM-Aktionen** aus.
5. Wählen Sie Ihre ITSM-Verbindung aus der Liste **Verbindung auswählen** aus.
6. Geben Sie die Details nach Bedarf an.
7. Um ein gesondertes Arbeitselement für jeden Protokolleintrag dieser Warnung zu erstellen, aktivieren Sie das Kontrollkästchen **Einzelne Arbeitselemente für jeden Protokolleintrag erstellen**.

    Oder

    Lassen Sie dieses Kontrollkästchen deaktiviert, um nur ein Arbeitselement für eine beliebige Anzahl von Protokolleinträgen zu dieser Warnung zu erstellen.

7. Klicken Sie auf **Speichern**.

Die OMS-Warnung wird unter **Warnungen** erstellt. Die Arbeitselemente der entsprechenden ITSM-Verbindung werden erstellt, wenn die Bedingung der angegebenen Warnung erfüllt ist.

## <a name="create-itsm-work-items-from-oms-logs"></a>Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen

Sie können mithilfe der OMS-Protokollsuche Arbeitselemente in den verbundenen ITSM-Quellen erstellen. Führen Sie dazu das folgende Verfahren aus:

1. Suchen Sie in **Protokollsuche** nach den erforderlichen Daten, wählen Sie die Details aus, und klicken Sie auf **Arbeitselement erstellen**.

    Das Fenster **ITSM-Arbeitselement erstellen** wird angezeigt:

    ![Log Analytics-Bildschirm](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Fügen Sie die folgenden Details hinzu:

  - **Arbeitselementtitel**: Titel für das Arbeitselement.
  - **Arbeitselementbeschreibung**: Beschreibung für das neue Arbeitselement.
  - **Betroffener Computer**: Name des Computers, auf dem diese Protokolldaten gefunden wurden.
  - **Verbindung auswählen**: ITSM-Verbindung, in der dieses Arbeitselement erstellt werden soll.
  - **Arbeitselement**: Typ des Arbeitselements.

3. Um eine vorhandene Arbeitselementvorlage für einen Incident zu verwenden, klicken Sie unter **Arbeitselement basierend auf Vorlage generieren** auf **Ja**, und klicken Sie dann auf **Erstellen**.

    Oder

    Klicken Sie auf **Nein**, wenn Sie Ihre benutzerdefinierten Werte bereitstellen möchten.

4. Geben Sie die entsprechenden Werte in die Textfelder **Kontakttyp**, **Auswirkung**, **Dringlichkeit**, **Kategorie** und **Unterkategorie** ein, und klicken Sie dann auf **Erstellen**.

Das Arbeitselement wird in ITSM erstellt, und Sie können es auch in OMS anzeigen.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Problembehandlung bei ITSM-Verbindungen in OMS
1.  Wenn für die Verbindung auf der Benutzeroberfläche der verbundenen Quelle ein Fehler auftritt und die Fehlermeldung **Fehler beim Speichern der Verbindung** angezeigt wird, gehen Sie folgendermaßen vor:
 - Stellen Sie für ServiceNow-, Cherwell- und Provance-Verbindungen sicher, dass Sie Benutzername/Kennwort und Client-ID/geheimer Clientschlüssel für jede der Verbindungen richtig eingegeben haben. Wenn der Fehler weiterhin auftritt, überprüfen Sie, ob Sie über ausreichende Berechtigungen für das entsprechende ITSM-Produkt verfügen, um die Verbindung herzustellen.
 - Stellen Sie für Service Manager sicher, dass die Web-App erfolgreich bereitgestellt und die Hybridverbindung erstellt wird. Um zu überprüfen, ob die Verbindung mit dem lokalen Service Manager-Computer erfolgreich hergestellt wird, besuchen Sie die Web-App-URL, wie in der Dokumentation zum Herstellen der [Hybridverbindung](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) erläutert.

2.  Wenn Daten von ServiceNow nicht in OMS synchronisiert werden, stellen Sie sicher, dass sich die ServiceNow-Instanz nicht im Energiesparmodus befindet. Dies kann mitunter in ServiceNow Dev-Instanzen im Leerlauf vorkommen. Melden Sie das Problem andernfalls.
3.  Wenn Warnungen von OMS ausgelöst werden, aber Arbeitselemente nicht im ITSM-Produkt erstellt werden oder Konfigurationselemente nicht erstellt/nicht mit Arbeitselementen verknüpft werden oder bei allen generischen Informationen, gehen Sie folgendermaßen vor:
 -  Die IT Service Management Connector-Lösung im OMS-Portal kann verwendet werden, um eine Zusammenfassung der Verbindungen/Arbeitselemente/Computer usw. zu erhalten. Klicken Sie auf die Fehlermeldung auf dem Statusblatt, navigieren Sie zur **Protokollsuche**, und zeigen Sie die Verbindung mit dem Fehler über die Details in der Fehlermeldung an.
 - Sie können die Fehler/zugehörigen Informationen mithilfe von *Type=ServiceDeskLog_CL* direkt auf der Seite **Protokollsuche** anzeigen.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Problembehandlung bei der Service Manager-Web-App-Bereitstellung
1.  Stellen Sie bei Problemen mit der Web-App-Bereitstellung sicher, dass Sie für das angegebene Abonnement über ausreichende Berechtigungen zum Erstellen/Bereitstellen von Ressourcen verfügen.
2.  Wenn die Fehlermeldung **Objektverweis ist nicht auf eine Instanz eines Objekts festgelegt** angezeigt wird, während das [Skript](log-analytics-itsmc-service-manager-script.md) ausgeführt wird, stellen Sie sicher, dass Sie im Abschnitt **Benutzerkonfiguration** gültige Werte eingegeben haben.
3.  Wenn Sie den Service Bus Relay-Namespace nicht erstellen, stellen Sie sicher, dass der erforderliche Ressourcenanbieter im Abonnement registriert ist. Wenn er nicht registriert ist, erstellen Sie ihn manuell über das Azure-Portal. Sie können ihn auch beim [Erstellen der Hybridverbindung](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) über das Azure-Portal erstellen.


## <a name="contact-us"></a>Kontakt

Kontaktieren Sie uns bei Fragen oder Feedback zum IT Service Management Connector über [omsitsmfeedback@microsoft.com](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen von ITSM-Produkten/-Diensten zum IT Service Management Connector](log-analytics-itsmc-connections.md).

