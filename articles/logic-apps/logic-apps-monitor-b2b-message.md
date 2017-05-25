---
title: "Überwachen von Nachrichten in B2B-Transaktionen: Azure Logic Apps | Microsoft-Dokumentation"
description: "Überwachen und Nachverfolgen von Nachrichten während der B2B-Kommunikation zwischen Prozessen und Apps mithilfe von Logik-Apps in Ihrem Integrationskonto."
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
ms.date: 01/27/2017
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5913c81088724ef946ae147f4f3154fa6aefd22e
ms.openlocfilehash: dc760b4c08d0e1afff3bc1276f6ed2367d67629e
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017

---

# <a name="start-or-enable-logging-of-as2-x12-and-edifact-messages-to-monitor-success-errors-and-message-properties"></a>Starten oder Aktivieren der Protokollierung von AS2-, X12- und EDIFACT-Nachrichten für die Überwachung von Erfolgs-, Fehler- und Nachrichteneigenschaften

Die B2B-Kommunikation umfasst den Nachrichtenaustausch zwischen zwei laufenden Geschäftsprozessen oder Anwendungen. Mit der Beziehung wird eine Vereinbarung zwischen Geschäftsprozessen definiert. Nachdem die Kommunikation hergestellt wurde, können Sie eine Nachrichtenüberwachung einrichten, um zu prüfen, ob die Kommunikation wie erwartet funktioniert. Um mehr Details und Debugfunktionen zu erhalten, können Sie für Ihr Integrationskonto die Diagnose einrichten.

Die Nachverfolgung von Nachrichten ist für diese B2B-Protokolle verfügbar: AS2, X12 und EDIFACT. 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein Integrationskonto. Sie können ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md) erstellen.
* Eine Logik-App. Sie können eine [Logik-App](logic-apps-create-a-logic-app.md) erstellen und die [Protokollierung aktivieren](logic-apps-monitor-your-logic-apps.md).

## <a name="enable-logging-for-an-integration-account"></a>Aktivieren der Protokollierung für ein Integrationskonto

Sie können die Protokollierung für ein Integrationskonto entweder im **Azure-Portal** oder mit **Monitor** aktivieren.

### <a name="enable-logging-with-azure-portal"></a>Aktivieren der Protokollierung über das Azure-Portal

1. Wählen Sie Ihr Integrationskonto und dann **Diagnoseprotokolle** aus.

    ![Auswählen Ihres Integrationskontos](media/logic-apps-monitor-b2b-message/pic5.png)

2. Wählen Sie Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. Wählen Sie **Integrationskonten** in **Ressourcentyp** aus. Wählen Sie in **Ressource** Ihr Integrationskonto aus. Klicken Sie auf **Diagnose aktivieren**, um die Diagnose für das ausgewählte Integrationskonto zu aktivieren.

    ![Einrichten der Diagnose für Ihr Integrationskonto](media/logic-apps-monitor-b2b-message/pic2.png)

3. Wählen Sie den Status **EIN** aus.

    ![Aktivieren des Diagnosestatus](media/logic-apps-monitor-b2b-message/pic3.png)

4. Wählen Sie **Send to Log Analytics** (An Log Analytics senden), und konfigurieren Sie Log Analytics für das Ausgeben von Daten.

    ![Senden von Diagnosedaten an ein Protokoll](media/logic-apps-monitor-b2b-message/pic4.png)

### <a name="enable-logging-with-monitor"></a>Aktivieren der Protokollierung mit Monitor

0. Wählen Sie **Monitor** > **Diagnoseprotokolle** aus.

    ![Monitor > Diagnoseprotokolle auswählen](media/logic-apps-monitor-b2b-message/pic1.png)

0. Wählen Sie Ihr **Abonnement** und Ihre **Ressourcengruppe** aus. Wählen Sie **Integrationskonten** in **Ressourcentyp** aus. Wählen Sie in **Ressource** Ihr Integrationskonto aus. Klicken Sie auf **Diagnose aktivieren**, um die Diagnose für das ausgewählte Integrationskonto zu aktivieren.

    ![Einrichten der Diagnose für Ihr Integrationskonto](media/logic-apps-monitor-b2b-message/pic2.png)

0. Wählen Sie den Status **EIN** aus.

    ![Aktivieren des Diagnosestatus](media/logic-apps-monitor-b2b-message/pic3.png) 

0. Wählen Sie **Send to Log Analytics** (An Log Analytics senden), und konfigurieren Sie **Log Analytics** für das Ausgeben von Daten.

    ![Senden von Diagnosedaten an ein Protokoll](media/logic-apps-monitor-b2b-message/pic4.png)

## <a name="extend-your-solutions"></a>Erweitern Ihrer Lösungen

Zusätzlich zu **Log Analytics** können Sie Ihr Integrationskonto und [Logik-Apps](./logic-apps-monitor-your-logic-apps.md) für einen Event Hub oder ein Speicherkonto konfigurieren.

![Azure-Diagnoseeinstellungen](./media/logic-apps-monitor-your-logic-apps/diagnostics.png)

Sie können diese Telemetrie aus dem Event Hub oder dem Speicher auch in anderen Diensten wie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) und [Power BI](https://powerbi.com) verwenden, um eine Echtzeitüberwachung Ihrer Integrationsworkflows einzurichten.

## <a name="supported-tracking-schema"></a>Unterstützte Nachverfolgungsschemas

Wir unterstützen diese Nachverfolgung von Schematypen, die mit Ausnahme des Typs „Benutzerdefiniert“ alle feste Schemas haben.

* [Benutzerdefiniertes Nachverfolgungsschema](logic-apps-track-integration-account-custom-tracking-schema.md)
* [AS2-Nachverfolgungsschema](logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-Nachverfolgungsschema](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Nächste Schritte

[Nachverfolgen von B2B-Nachrichten im OMS-Portal](logic-apps-track-b2b-messages-omsportal.md "Nachverfolgen von B2B-Nachrichten im OMS-Portal")

[Weitere Informationen zum Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")


