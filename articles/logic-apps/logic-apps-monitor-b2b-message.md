---
title: "Überwachen von B2B-Transaktionen und Einrichten der Protokollierung: Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen Sie AS2-, X12 und EDIFACT-Nachrichten, und starten Sie die Diagnoseprotokollierung für Ihr Integrationskonto."
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
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.translationtype: HT
ms.sourcegitcommit: bde1bc7e140f9eb7bb864c1c0a1387b9da5d4d22
ms.openlocfilehash: 59aa8fc907d68485b7d78ae7466e2d2298d7d7d6
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---

# <a name="monitor-and-set-up-diagnostics-logging-for-b2b-communication-in-integration-accounts"></a>Überwachen und Einrichten der Diagnoseprotokollierung für die B2B-Kommunikation in Integrationskonten

Nachdem Sie über Ihr Integrationskonto die B2B-Kommunikation zwischen zwei laufenden Geschäftsprozessen oder -anwendungen eingerichtet haben, können diese Entitäten untereinander Nachrichten austauschen. Um zu prüfen, ob die Kommunikation erwartungsgemäß funktioniert, können Sie über den Dienst [Azure Log Analytics](../log-analytics/log-analytics-overview.md) die Überwachung für AS2-, X12- und EDIFACT-Nachrichten sowie die Diagnoseprotokollierung für Ihr Integrationskonto einrichten. Dieser Dienst aus der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) überwacht sowohl Ihre Cloudumgebung als auch Ihre lokale Umgebung und unterstützt Sie nicht nur dabei, deren Verfügbarkeit und Leistung zu gewährleisten, sondern erfasst auch Laufzeitdetails und Ereignisse für ein ausführlicheres Debugging. Darüber hinaus können Sie diese [Diagnosedaten mit anderen Diensten verwenden](#extend-diagnostic-data) (beispielsweise mit Azure Storage und Azure Event Hubs).

## <a name="requirements"></a>Anforderungen

* Eine Logik-App, für die die Diagnoseprotokollierung eingerichtet ist. Informationen zum Einrichten der Protokollierung für diese Logik-App finden Sie [hier](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

  > [!NOTE]
  > Wenn diese Anforderung erfüllt ist, verfügen Sie über einen Arbeitsbereich in der [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Es empfiehlt sich, beim Einrichten der Protokollierung für Ihr Integrationskonto den gleichen OMS-Arbeitsbereich zu verwenden. Falls Sie über keinen OMS-Arbeitsbereich verfügen, erfahren Sie [hier](../log-analytics/log-analytics-get-started.md), wie Sie einen OMS-Arbeitsbereich erstellen.

* Ein mit Ihrer Logik-App verknüpftes Integrationskonto. Informationen zum Erstellen eines Integrationskontos mit Logik-App-Verknüpfung finden Sie [hier](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md).

## <a name="turn-on-diagnostics-logging-for-your-integration-account"></a>Aktivieren der Diagnoseprotokollierung für Ihr Integrationskonto

Die Protokollierung kann entweder direkt über Ihr Integrationskonto oder [über den Azure Monitor-Dienst](#azure-monitor-service) aktiviert werden. Azure Monitor bietet eine grundlegende Überwachung mit Daten der Infrastrukturebene. Weitere Informationen zu Azure Monitor finden Sie [hier](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md).

### <a name="turn-on-diagnostics-logging-directly-from-your-integration-account"></a>Aktivieren der Diagnoseprotokollierung direkt über das Integrationskonto

1. Suchen Sie im [Azure-Portal](https://portal.azure.com) nach Ihrem Integrationskonto, und wählen Sie es aus. Wählen Sie unter **Überwachung** die Option **Diagnoseprotokolle** aus, wie hier zu sehen:

   ![Auswählen des eigenen Integrationskontos und der Option „Diagnoseprotokolle“](media/logic-apps-monitor-b2b-message/integration-account-diagnostics.png)

2. Nach dem Auswählen des eigenen Integrationskontos werden die folgenden Werte automatisch ausgewählt. Sind die Werte korrekt, wählen Sie **Diagnose aktivieren** aus. Wählen Sie andernfalls die gewünschten Werte aus:

   1. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie mit Ihrem Integrationskonto verwenden möchten.
   2. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, die Sie mit Ihrem Integrationskonto verwenden möchten.
   3. Wählen Sie unter **Ressourcentyp** die Option **Integrationskonten** aus. 
   4. Wählen Sie unter **Ressource** Ihr Integrationskonto aus. 
   5. Klicken Sie auf **Diagnose aktivieren**.

   ![Einrichten der Diagnose für Ihr Integrationskonto](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Wählen Sie unter **Diagnoseeinstellungen** > **Status** die Option **Ein** aus.

   ![Aktivieren der Azure-Diagnose](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Wählen Sie nun den OMS-Arbeitsbereich und die für die Protokollierung zu verwendenden Daten aus, wie hier zu sehen:

   1. Wählen Sie **An Log Analytics senden** aus. 
   2. Wählen Sie unter **Log Analytics** die Option **Konfigurieren** aus. 
   3. Wählen Sie unter **OMS-Arbeitsbereiche** den OMS-Arbeitsbereich aus, den Sie für die Protokollierung verwenden möchten.
   4. Wählen Sie unter **Protokoll** die Kategorie **IntegrationAccountTrackingEvents** aus.
   5. Wählen Sie **Speichern** aus.

   ![Einrichten von Log Analytics zum Senden von Diagnosedaten an ein Protokoll](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nun können Sie das [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) einrichten.

<a name="azure-monitor-service"></a>

### <a name="turn-on-diagnostics-logging-through-azure-monitor"></a>Aktivieren der Diagnoseprotokollierung über Azure Monitor

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) im Azure-Hauptmenü **Überwachen** > **Diagnoseprotokolle** aus. Wählen Sie als Nächstes Ihr Integrationskonto aus, wie hier zu sehen:

   ![Auswählen von „Überwachen“ > „Diagnoseprotokolle“ > eigenes Integrationskonto](media/logic-apps-monitor-b2b-message/monitor-service-diagnostics-logs.png)

2. Nach dem Auswählen des eigenen Integrationskontos werden die folgenden Werte automatisch ausgewählt. Sind die Werte korrekt, wählen Sie **Diagnose aktivieren** aus. Wählen Sie andernfalls die gewünschten Werte aus:

   1. Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie mit Ihrem Integrationskonto verwenden möchten.
   2. Wählen Sie unter **Ressourcengruppe** die Ressourcengruppe aus, die Sie mit Ihrem Integrationskonto verwenden möchten.
   3. Wählen Sie unter **Ressourcentyp** die Option **Integrationskonten** aus.
   4. Wählen Sie unter **Ressource** Ihr Integrationskonto aus.
   5. Klicken Sie auf **Diagnose aktivieren**.

   ![Einrichten der Diagnose für Ihr Integrationskonto](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account.png)

3. Wählen Sie unter **Diagnoseeinstellungen** die Option **Ein** aus.

   ![Aktivieren der Azure-Diagnose](media/logic-apps-monitor-b2b-message/turn-on-diagnostics-integration-account-2.png)

4. Wählen Sie anschließend den OMS-Arbeitsbereich und die Ereigniskategorie für die Protokollierung aus:

   1. Wählen Sie **An Log Analytics senden** aus. 
   2. Wählen Sie unter **Log Analytics** die Option **Konfigurieren** aus. 
   3. Wählen Sie unter **OMS-Arbeitsbereiche** den OMS-Arbeitsbereich aus, den Sie für die Protokollierung verwenden möchten.
   4. Wählen Sie unter **Protokoll** die Kategorie **IntegrationAccountTrackingEvents** aus.
   5. Wenn Sie fertig sind, wählen Sie **Speichern** aus.

   ![Einrichten von Log Analytics zum Senden von Diagnosedaten an ein Protokoll](media/logic-apps-monitor-b2b-message/send-diagnostics-data-log-analytics-workspace.png)

5. Nun können Sie das [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) einrichten.

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Erweitern der Nutzung von Diagnosedaten mit anderen Diensten

In Kombination mit Azure Log Analytics können Sie die Nutzung der Diagnosedaten Ihrer Logik-App mit anderen Azure-Diensten erweitern. Dies ermöglicht beispielsweise Folgendes: 

* [Archivieren von Azure-Diagnoseprotokollen in Azure Storage](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Streamen von Azure-Diagnoseprotokollen an Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Durch die Verwendung von Telemetriedaten und Analysen anderer Dienste (beispielsweise [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) und [Power BI](../log-analytics/log-analytics-powerbi.md)) können Sie dann eine Überwachung in Echtzeit implementieren. Beispiel:

* [Streamen von Daten von Event Hubs zu Stream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Analysieren von Streamingdaten mit Stream Analytics und Erstellen eines Dashboards für die Echtzeitanalyse in Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Erstellen Sie je nach den Optionen, die Sie einrichten möchten, zunächst [ein Azure-Speicherkonto](../storage/storage-create-storage-account.md) oder [einen Azure Event Hub](../event-hubs/event-hubs-create.md). Wählen Sie anschließend die gewünschten Sendeoptionen für Diagnosedaten aus:

![Senden von Daten an das Azure-Speicherkonto oder einen Event Hub](./media/logic-apps-monitor-b2b-message/storage-account-event-hubs.png)

> [!NOTE]
> Werte für die Beibehaltungsdauer gelten nur, wenn Sie sich für die Verwendung eines Speicherkontos entscheiden.

## <a name="supported-tracking-schemas"></a>Unterstützte Nachverfolgungsschemas

Azure unterstützt die folgenden Nachverfolgungsschematypen. Mit Ausnahme des Typs „Benutzerdefiniert“ besitzen alle ein festes Schema.

* [AS2-Nachverfolgungsschema](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschema](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Benutzerdefinierte Nachverfolgungsschemas](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

* [Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal](../logic-apps/logic-apps-track-b2b-messages-omsportal.md "Nachverfolgen von B2B-Nachrichten im Operations Management Suite-Portal")
* [Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")


