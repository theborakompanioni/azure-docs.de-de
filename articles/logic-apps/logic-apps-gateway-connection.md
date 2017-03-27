---
title: "Zugriff auf lokale Daten – Azure Logic Apps | Microsoft-Dokumentation"
description: "Wie Ihre Logik-Apps durch Herstellen einer Verbindung mit einem lokalen Datengateway auf lokale Daten zugreifen können."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/05/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: ce0be184fe11a7e5873639d79961e98de730ec86
ms.lasthandoff: 03/10/2017


---
# <a name="connect-to-on-premises-data-from-logic-apps"></a>Herstellen einer Verbindung mit lokalen Daten für Logik-Apps

Sie können eine Verbindung mit einem lokalen Datengateway für unterstützte Azure Logic Apps-Connectors einrichten, um auf lokale Daten zuzugreifen. Die folgenden Schritte führen Sie durch die Installation und Einrichtung des lokalen Datengateways für den Einsatz mit Ihren Logik-Apps.
Das lokale Datengateway unterstützt die folgenden Datenquellenverbindungen:

*   BizTalk Server
*    DB2  
*   Dateisystem
*   Informix
*   MQ
*    Oracle-Datenbank 
*   SAP-Anwendungsserver 
*   SAP-Nachrichtenserver
*    SQL Server

Weitere Informationen zu diesen Verbindungen finden Sie unter [Connectors für Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="requirements"></a>Anforderungen

* Um das lokale Datengateway Ihrem Konto (auf Azure Active Directory-Basis) zuzuordnen, müssen Sie eine Geschäfts-, Schul- oder Uni-E-Mail-Adresse in Azure angeben.

* Bei Verwendung eines Microsoft-Kontos wie z.B. @outlook.com können Sie mit Ihrem Azure-Konto eine [Geschäfts-, Schul- oder Uni-E-Mail-Adresse erstellen](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal).

* Das lokale Datengateway muss bereits [auf einem lokalen Computer installiert sein](logic-apps-gateway-install.md).

* Sie können Ihre Installation nur einer Gateway-Ressource zuordnen. Ihr Gateway kann nicht von einem anderen lokalen Azure-Datengateway beansprucht werden. Das Beanspruchen erfolgt beim [Erstellen während Schritt 2 in diesem Thema](#2-create-an-azure-on-premises-data-gateway-resource).

## <a name="install-and-configure-the-connection"></a>Installieren und Konfigurieren der Verbindung

### <a name="1-install-the-on-premises-data-gateway"></a>1. Installieren des lokalen Datengateways

Führen Sie (sofern noch nicht geschehen) diese Schritte aus, um [das lokale Datengateway zu installieren](logic-apps-gateway-install.md). Wenn Sie mit den anderen Schritten fortfahren, muss das Datengateway auf einem lokalen Computer installiert sein.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Erstellen einer lokalen Azure-Datengatewayressource

Nach der Installation des Gateways müssen Sie es Ihrem Azure-Abonnement zuordnen.

1. Melden Sie sich mit der Geschäfts-, Schul- oder Uni-E-Mail-Adresse, die Sie bei der Installation des Gateways verwendet haben, bei Azure an.
2. Wählen Sie **Neu**.
3. Suchen Sie das **Lokale Datengateway**, und wählen sie es aus.
4. Geben Sie die Informationen ein, einschließlich der Auswahl des entsprechenden **Installationsnamens**, um das Gateway Ihrem Konto zuzuordnen.
   
    ![Verbindung mit einem lokalen Datengateway][1]

5. Klicken Sie auf **Erstellen**, um die Ressource zu erstellen.

### <a name="3-create-a-logic-app-connection-in-logic-app-designer"></a>3. Erstellen einer Logik-App-Verbindung im Logik-App-Designer

Da Ihr Azure-Abonnement jetzt einer Instanz des lokalen Datengateways zugeordnet ist, können Sie aus einer Logik-App heraus eine Verbindung mit dem Gateway erstellen.

1. Öffnen Sie eine Logik-App, und wählen Sie einen Connector aus, der lokale Konnektivität unterstützt, wie z.B. SQL Server.
2. Wählen Sie **Verbinden über lokales Datengateway**.
   
    ![Gatewayerstellung mit Logik-App-Designer][2]

3. Wählen Sie das **Gateway** aus, mit dem eine Verbindung hergestellt werden soll, und geben Sie alle anderen erforderlichen Verbindungsinformationen an.
4. Klicken Sie auf **Erstellen**, um die Verbindung zu erstellen.

Ihre Verbindung ist jetzt für die Verwendung durch Ihre Logik-App konfiguriert.

## <a name="next-steps"></a>Nächste Schritte

* [Allgemeine Beispiele und Szenarien für Logik-Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Unternehmensintegrationsfeatures](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-gateway-connection/createblade.png
[2]: ./media/logic-apps-gateway-connection/blankconnection.png
[3]: ./media/logic-apps-logic-gateway-connection/checkbox.png

