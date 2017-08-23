---
title: "Abfragen von B2B-Nachrichten in der Operations Management Suite – Azure Logic Apps | Microsoft-Dokumentation"
description: Verfolgen Sie AS2-, X12- und EDIFACT-Nachrichten in der Operations Management Suite mithilfe von Abfragen nach.
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
ms.openlocfilehash: 2748d3d3daf7c13dca05f663a4a088598e1b3605
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="query-for-as2-x12-and-edifact-messages-in-the-microsoft-operations-management-suite-oms"></a>Abfragen von AS2-, X12- und EDIFACT-Nachrichten in der Microsoft Operations Management Suite (OMS)

Sie können Abfragen erstellen, die Aktionen auf der Grundlage bestimmter Kriterien filtern, um die AS2-, X12- oder EDIFACT-Nachrichten zu finden, die Sie in der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) mithilfe von [Azure Log Analytics](../log-analytics/log-analytics-overview.md) nachverfolgen. So können Sie beispielsweise anhand einer bestimmten Austauschkontrollnummer nach Nachrichten suchen.

## <a name="requirements"></a>Anforderungen

* Eine Logik-App, für die die Diagnoseprotokollierung eingerichtet ist. Informationen zum Erstellen einer Logik-App finden Sie [hier](../logic-apps/logic-apps-create-a-logic-app.md). Informationen zum Einrichten der Protokollierung für die Logik-App stehen [hier](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics) zur Verfügung.

* Ein Integrationskonto, für das die Überwachung und Protokollierung eingerichtet ist. Informationen zum Erstellen eines Integrationskontos finden Sie [hier](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md). Informationen zum Einrichten der Überwachung und Protokollierung für das Konto stehen [hier](../logic-apps/logic-apps-monitor-b2b-message.md) zur Verfügung.

* [Veröffentlichen Sie Diagnosedaten in Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md), und [richten Sie die Nachrichtennachverfolgung in der OMS ein](../logic-apps/logic-apps-track-b2b-messages-omsportal.md), sofern noch nicht geschehen.

> [!NOTE]
> Wenn diese beiden Anforderungen erfüllt sind, verfügen Sie über einen Arbeitsbereich in der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Es empfiehlt sich, für die Nachverfolgung der B2B-Kommunikation in der OMS den gleichen OMS-Arbeitsbereich zu verwenden. 
>  
> Falls Sie über keinen OMS-Arbeitsbereich verfügen, erfahren Sie [hier](../log-analytics/log-analytics-get-started.md), wie Sie einen OMS-Arbeitsbereich erstellen.

## <a name="create-message-queries-with-filters-in-the-operations-management-suite-portal"></a>Erstellen von Nachrichtenabfragen mit Filtern über das Operations Management Suite-Portal

In diesem Beispiel erfahren Sie, wie Sie anhand der Austauschkontrollnummer nach Nachrichten suchen.

> [!TIP] 
> Wenn Ihnen der Name Ihres OMS-Arbeitsbereichs bekannt ist, navigieren Sie zur Startseite Ihres Arbeitsbereichs (`https://{your-workspace-name}.portal.mms.microsoft.com`), und fahren Sie mit Schritt 4 fort. Beginnen Sie andernfalls mit Schritt 1.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Weitere Dienste** aus. Suchen Sie nach „Log Analytics“, und wählen Sie die Option **Log Analytics** aus, wie hier zu sehen:

   ![Suchen nach Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Suchen Sie unter **Log Analytics** nach Ihrem OMS-Arbeitsbereich, und wählen Sie ihn aus.

   ![Auswählen Ihres OMS-Arbeitsbereichs](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Wählen Sie unter **Verwaltung** die Option **OMS-Portal** aus.

   ![Auswählen des OMS-Portals](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Wählen Sie auf Ihrer OMS-Startseite die Option **Protokollsuche** aus.

   ![Auswählen von „Protokollsuche“ auf Ihrer OMS-Startseite](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   Oder

   ![Auswählen von „Protokollsuche“ im OMS-Menü](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Geben Sie im Suchfeld ein Feld an, nach dem Sie suchen möchten, und drücken Sie die **EINGABETASTE**. Wenn Sie mit der Eingabe beginnen, werden in OMS mögliche Übereinstimmungen und verfügbare Vorgänge angezeigt. Weitere Informationen zur Datensuche in Log Analytics finden Sie [hier](../log-analytics/log-analytics-log-searches.md).

   In diesem Beispiel wird nach Ereignissen mit **Type=AzureDiagnostics** gesucht.

   ![Beginnen mit der Eingabe einer Abfragezeichenfolge](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. Wählen Sie auf der linken Leiste den gewünschten Zeitrahmen aus. Wenn Sie der Abfrage einen Filter hinzufügen möchten, wählen Sie **+Hinzufügen** aus.

   ![Hinzufügen eines Filters zur Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. Geben Sie unter **Filter hinzufügen** den Namen des gewünschten Filters ein. Wählen Sie den Filter aus, und wählen Sie **+Hinzufügen** aus.

   Zur Ermittlung der Austauschkontrollnummer wird in diesem Beispiel nach dem Wort „interchange“ (Austausch) gesucht und **event_record_messageProperties_interchangeControlNumber_s** als Filter ausgewählt.

   ![Auswählen des Filters](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. Wählen Sie auf der linken Leiste den gewünschten Filterwert und anschließend **Übernehmen** aus.

   In diesem Beispiel wird die Austauschkontrollnummer für die gewünschten Nachrichten ausgewählt.

   ![Auswählen des Filterwerts](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Kehren Sie zur Abfrageerstellung zurück. Ihre Abfrage wurde mit dem ausgewählten Filterereignis und -wert aktualisiert. Die vorherigen Ergebnisse werden nun ebenfalls gefiltert.

    ![Rückkehr zur Abfrage mit gefilterten Ergebnissen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Speichern der Abfrage zur späteren Verwendung

1. Wählen Sie auf der Seite **Protokollsuche** für Ihre Abfrage die Option **Speichern** aus. Benennen Sie die Abfrage, wählen Sie eine Kategorie aus, und wählen Sie anschließend **Speichern** aus.

   ![Benennen der Abfrage und Auswählen einer Kategorie](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Wählen Sie zum Anzeigen Ihrer Abfrage die Option **Favoriten** aus.

   ![Auswählen von „Favoriten“](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Wählen Sie unter **Gespeicherte Suchvorgänge** Ihre Abfrage aus, um die entsprechenden Ergebnisse anzuzeigen. Sie können die Abfrage bearbeiten, um andere Ergebnisse zu erhalten.

   ![Auswählen Ihrer Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-the-operations-management-suite-portal"></a>Suchen und Ausführen gespeicherter Abfragen im Operations Management Suite-Portal

1. Öffnen Sie die Startseite Ihres OMS-Arbeitsbereichs (`https://{your-workspace-name}.portal.mms.microsoft.com`), und wählen Sie **Protokollsuche** aus.

   ![Auswählen von „Protokollsuche“ auf Ihrer OMS-Startseite](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   Oder

   ![Auswählen von „Protokollsuche“ im OMS-Menü](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Wählen Sie auf der Startseite der **Protokollsuche** die Option **Favoriten** aus.

   ![Auswählen von „Favoriten“](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Wählen Sie unter **Gespeicherte Suchvorgänge** Ihre Abfrage aus, um die entsprechenden Ergebnisse anzuzeigen. Sie können die Abfrage bearbeiten, um andere Ergebnisse zu erhalten.

   ![Auswählen Ihrer Abfrage](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Nächste Schritte

* [AS2-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)
