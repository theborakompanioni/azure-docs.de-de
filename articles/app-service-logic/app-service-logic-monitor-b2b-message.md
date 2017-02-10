---
title: "Überwachen von B2B-Nachrichten | Microsoft Docs"
description: "Überwachen des Integrationskontos"
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
ms.date: 10/21/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 63ba6e8fec8df1cdbc9928b011130f47c5facf89
ms.openlocfilehash: 8ce3585a557f6b2c9fa4a4c85142cdce9357a90b


---
# <a name="monitor-b2b-messages"></a>Überwachen von B2B-Nachrichten
Die B2B-Kommunikation umfasst den Nachrichtenaustausch zwischen zwei laufenden Geschäftsprozessen oder Anwendungen. Mit der Beziehung wird eine Vereinbarung zwischen Geschäftsprozessen definiert. Nachdem die Kommunikation eingerichtet wurde, wird eine Möglichkeit zur Überwachung benötigt, um sicherzustellen, dass die Kommunikation wie erwartet funktioniert.  Die Nachverfolgung von Nachrichten wurde für die B2B-Protokolle implementiert: AS2, X12 und EDIFACT.  Sie können Ihr Integrationskonto so konfigurieren, dass mehr Details und Debugfunktionen vorhanden sind.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
* Ein Integrationskonto. Sie können ein [Integrationskonto](app-service-logic-enterprise-integration-create-integration-account.md) erstellen.
* Eine Logik-App. Sie können eine [Logik-App](app-service-logic-create-a-logic-app.md) erstellen und die Protokollierung aktivieren. Entsprechende Schritte finden Sie [hier](app-service-logic-monitor-your-logic-apps.md).

## <a name="enable-logging-for-an-integration-account"></a>Aktivieren der Protokollierung für ein Integrationskonto
Sie können die Protokollierung für ein Integrationskonto entweder mit dem **Azure-Portal** oder mit dem **Monitor** aktivieren.

### <a name="enable-logging-with-azure-portal"></a>Aktivieren der Protokollierung über das Azure-Portal

1. Wählen Sie **Integrationskonto** und dann **Diagnoseprotokolle**. 
![Auswählen des Integrationskontos](./media/app-service-logic-monitor-integration-account/Pic5.png)  

2. Wählen Sie Ihr **Abonnement**, die **Ressourcengruppe** und das **Integrationskonto** unter „Ressourcentyp“ aus. Wählen Sie anschließend in der Dropdownliste „Ressource“ das **Integrationskonto** aus, um die Diagnose zu aktivieren.  Klicken Sie auf **Diagnose aktivieren**, um die Diagnose für das ausgewählte Integrationskonto zu aktivieren.               
![Auswählen des Integrationskontos](./media/app-service-logic-monitor-integration-account/Pic2.png) 

3. Wählen Sie den Status **EIN**.       
![Aktivieren der Diagnose](./media/app-service-logic-monitor-integration-account/Pic3.png) 

4. Wählen Sie **Send to Log Analytics** (An Log Analytics senden), und konfigurieren Sie Log Analytics für das Ausgeben von Daten.               
![Aktivieren der Diagnose](./media/app-service-logic-monitor-integration-account/Pic4.png)

### <a name="enable-logging-with-monitor"></a>Aktivieren der Protokollierung mit Monitor

1. Wählen Sie **Monitor**, und klicken Sie auf **Diagnoseprotokolle** .  
![Auswählen von Monitor](./media/app-service-logic-monitor-integration-account/Pic1.png)

2. Wählen Sie Ihr **Abonnement**, die **Ressourcengruppe** und das **Integrationskonto** unter „Ressourcentyp“ aus. Wählen Sie anschließend in der Dropdownliste „Ressource“ das **Integrationskonto** aus, um die Diagnose zu aktivieren.  Klicken Sie auf **Diagnose aktivieren**, um die Diagnose für das ausgewählte Integrationskonto zu aktivieren.               
![Auswählen des Integrationskontos](./media/app-service-logic-monitor-integration-account/Pic2.png)

3. Wählen Sie den Status **EIN**.       
![Aktivieren der Diagnose](./media/app-service-logic-monitor-integration-account/Pic3.png) 

4. Wählen Sie **Send to Log Analytics** (An Log Analytics senden), und konfigurieren Sie Log Analytics für das Ausgeben von Daten. ![Aktivieren der Diagnose](./media/app-service-logic-monitor-integration-account/Pic4.png)

## <a name="extending-your-solutions"></a>Erweitern Ihrer Lösungen
Zusätzlich zu **Log Analytics** können Sie Ihr Integrationskonto und [Logik-Apps](./app-service-logic-monitor-your-logic-apps.md) für einen Event Hub oder ein Speicherkonto konfigurieren.         
![Azure-Diagnoseeinstellungen](./media/app-service-logic-monitor-your-logic-apps/diagnostics.png)

Sie können diese Telemetrie aus dem Event Hub oder dem Speicher auch in anderen Diensten wie [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) und [Power BI](https://powerbi.com) verwenden, um eine Echtzeitüberwachung Ihrer Integrationsworkflows einzurichten.

## <a name="supported-tracking-schema"></a>Unterstützte Nachverfolgungsschemas
Wir unterstützen die folgenden Arten von Nachverfolgungsschemas.  Mit Ausnahme von „Benutzerdefiniert“ handelt es sich jeweils um feste Schemas.

* [Benutzerdefiniertes Nachverfolgungsschema](app-service-logic-track-integration-account-custom-tracking-shema.md)   
* [AS2-Nachverfolgungsschema](app-service-logic-track-integration-account-as2-tracking-shemas.md)   
* [X12-Nachverfolgungsschema](app-service-logic-track-integration-account-x12-tracking-shemas.md)  

## <a name="next-steps"></a>Nächste Schritte
[Nachverfolgen von B2B-Nachrichten im OMS-Portal](app-service-logic-track-b2b-messages-omsportal.md "Tracking B2B messages")   
[Weitere Informationen zum Enterprise Integration Pack](app-service-logic-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 




<!--HONumber=Nov16_HO3-->


