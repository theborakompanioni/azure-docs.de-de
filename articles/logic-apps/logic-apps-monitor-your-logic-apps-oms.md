---
title: "Überwachen und Gewinnen von Erkenntnissen zu Logik-App-Ausführungen mit OMS – Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen Sie Ihre Logik-App-Ausführungen mit Log Analytics und Operations Management Suite (OMS), um Einblicke zu gewinnen und ausführlichere Debuggingdetails für die Problembehandlung und Diagnose zu erhalten."
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.translationtype: HT
ms.sourcegitcommit: 540180e7d6cd02dfa1f3cac8ccd343e965ded91b
ms.openlocfilehash: 0e9f0ef3c87b5c0da1cc4ad16d37178c8f5c9625
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---

# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Überwachen und Gewinnen von Erkenntnissen zu Logik-App-Ausführungen mit Operations Management Suite (OMS) und Log Analytics

Sie können beim Erstellen einer Logik-App gleichzeitig Log Analytics aktivieren, um die Überwachung durchzuführen und umfassendere Debuginformationen zu erhalten. Log Analytics ermöglicht die Diagnoseprotokollierung und Überwachung für Ihre Logik-App-Ausführungen über das OMS-Portal (Operations Management Suite). Wenn Sie OMS die Lösung für die Logik-App-Verwaltung hinzufügen, erhalten Sie den aggregierten Status für Ihre Logik-App-Ausführungen und spezifische Details wie Status, Ausführungszeit, Status der erneuten Übermittlung und Korrelations-IDs.

In diesem Thema wird veranschaulicht, wie Sie Log Analytics aktivieren oder die Lösung für die Logik-App-Verwaltung in OMS installieren, um Laufzeitereignisse und Daten für Ihre Logik-App-Ausführung anzuzeigen.

 > [!TIP]
 > Führen Sie diese Schritte zum [Aktivieren der Diagnoseprotokollierung und Senden von Logik-App-Laufzeitdaten an OMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics) aus, um Ihre vorhandenen Logik-Apps zu überwachen.

## <a name="requirements"></a>Anforderungen

Sie müssen über einen OMS-Arbeitsbereich verfügen, bevor Sie beginnen können. Informieren Sie sich, wie Sie [einen OMS-Arbeitsbereich erstellen](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Aktivieren der Diagnoseprotokollierung beim Erstellen von Logik-Apps

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine Logik-App. Wählen Sie **Neu** > **Unternehmensintegration** > **Logik-App** > **Erstellen**.

   ![Erstellen einer Logik-App](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. Führen Sie auf der Seite **Logik-App erstellen** wie gezeigt die folgenden Schritte aus:

   1. Geben Sie einen Namen für Ihre Logik-App an, und wählen Sie Ihr Azure-Abonnement aus. 
   2. Erstellen Sie eine Azure-Ressourcengruppe, oder wählen Sie sie aus.
   3. Legen Sie **Log Analytics** auf **Ein** fest. 
   Wählen Sie den OMS-Arbeitsbereich aus, für den Sie Daten für Ihre Logik-App-Ausführungen senden möchten. 
   4. Wählen Sie **An Dashboard anheften** > **Erstellen**, wenn alles bereit ist.

      ![Erstellen einer Logik-App](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Nach Abschluss dieses Schritts erstellt Azure Ihre Logik-App, die jetzt Ihrem OMS-Arbeitsbereich zugeordnet ist. 
      Mit diesem Schritt wird auch die Lösung für die Logik-App-Verwaltung automatisch in Ihrem OMS-Arbeitsbereich installiert.

3. [Fahren Sie mit diesen Schritten fort](#view-logic-app-runs-oms), um Ihre Logik-App-Ausführungen in OMS anzuzeigen.

## <a name="install-the-logic-apps-management-solution-in-oms"></a>Installieren der Lösung für die Logik-App-Verwaltung in OMS

Überspringen Sie diesen Schritt, falls Sie Log Analytics bereits beim Erstellen Ihrer Logik-App aktiviert haben. Sie haben die Lösung für die Logik-App-Verwaltung bereits in OMS installiert.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **Weitere Dienste** aus. Verwenden Sie für die Suche den Filter „log analytics“, und wählen Sie dann wie gezeigt **Log Analytics**:

   ![Auswählen von „Log Analytics“](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Wählen Sie unter **Log Analytics** Ihren OMS-Arbeitsbereich aus. 

   ![Auswählen Ihres OMS-Arbeitsbereichs](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Wählen Sie unter **Verwaltung** die Option **OMS-Portal**.

   ![Auswählen von „OMS-Portal“](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. Wenn auf Ihrer OMS-Startseite das Upgradebanner angezeigt wird, wählen Sie das Banner aus, um erst ein Upgrade für Ihren OMS-Arbeitsbereich durchzuführen. Wählen Sie dann **Lösungskatalog** aus.

   ![Auswählen von „Lösungskatalog“](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. Wählen Sie unter **Alle Lösungen** die Kachel für die Lösung **Logic Apps Management** (Logik-App-Verwaltung) aus.

   ![Auswählen von „Logic Apps Management“ (Logik-App-Verwaltung)](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. Wählen Sie **Hinzufügen**, um die Lösung in Ihrem OMS-Arbeitsbereich zu installieren.

   ![Auswählen von „Hinzufügen“ für „Logic Apps Management“ (Logik-App-Verwaltung)](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>Anzeigen Ihrer Logik-App-Ausführungen in Ihrem OMS-Arbeitsbereich

1. Navigieren Sie zur Übersichtsseite für Ihren OMS-Arbeitsbereich, um Anzahl und Status für Ihre Logik-App-Ausführungen anzuzeigen. Überprüfen Sie die Details auf der Kachel **Logic Apps Management** (Logik-App-Verwaltung).

   ![Übersichtskachel mit Anzahl und Status der Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Wenn anstelle der Kachel „Logik-App-Verwaltung“ dieses Upgradebanner angezeigt wird, wählen Sie das Banner aus, um zuerst ein Upgrade für Ihren OMS-Arbeitsbereich durchzuführen.
  
   > ![Durchführen eines Upgrades für den OMS-Arbeitsbereich](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. Wählen Sie die Kachel **Logic Apps Management** (Logik-App-Verwaltung), um eine Zusammenfassung mit weiteren Details zu Ihren Logik-App-Ausführungen anzuzeigen.

   Hier sind Ihre Logik-App-Ausführungen nach Name oder Ausführungsstatus gruppiert.

   ![Statuszusammenfassung für Ihre Logik-App-Ausführungen](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. Wählen Sie die Zeile für eine Logik-App oder einen Status aus, um alle Ausführungen für eine bestimmte Logik-App bzw. einen Status anzuzeigen.

   Hier ist ein Beispiel angegeben, bei dem alle Ausführungen für eine bestimmte Logik-App aufgeführt werden:

   ![Anzeigen von Ausführungen für eine Logik-App oder einen Status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > In der Spalte **Erneute Übermittlung** wird „Ja“ für die Ausführungen angezeigt, die sich aus einer erneut übermittelten Ausführung ergeben haben.

4. Zum Filtern dieser Ergebnisse können Sie sowohl die clientseitige als auch die serverseitige Filterung durchführen.

   * Clientseitiger Filter: Wählen Sie für jede Spalte die gewünschten Filter aus. 
   Hier einige Beispiele:

     ![Beispiel für Spaltenfilter](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Serverseitiger Filter: Verwenden Sie das Steuerelement für die Bereichsauswahl oben auf der Seite, um ein bestimmtes Zeitfenster auszuwählen oder die Anzahl von angezeigten Ausführungen zu begrenzen. 
   Standardmäßig werden nur jeweils 1.000 Datensätze angezeigt. 
   
     ![Ändern des Zeitfensters](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. Wählen Sie eine Zeile aus, um für eine bestimmte Ausführung alle Aktionen und die dazugehörigen Details anzuzeigen. Die Seite „Protokollsuche“ wird geöffnet. 

   * Wählen Sie **Tabelle**, um diese Informationen in einer Tabelle anzuzeigen.
   * Sie können die Abfragezeichenfolge in der Suchleiste bearbeiten, um die Abfrage zu ändern. 
   Wählen Sie **Erweiterte Analyse**, falls Sie mehr Optionen benötigen.

     ![Anzeigen von Aktionen und Details für eine Logik-App-Ausführung](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     Hier auf der Azure Log Analytics-Seite können Sie Abfragen aktualisieren und die Ergebnisse der Tabelle anzeigen. 
     Für diese Abfrage wird die [Kusto-Abfragesprache](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html) verwendet. Sie können diese Sprache bearbeiten, wenn Sie unterschiedliche Ergebnisse anzeigen möchten. 

     ![Azure Log Analytics – Abfrageansicht](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen von B2B-Nachrichten](../logic-apps/logic-apps-monitor-b2b-message.md)

