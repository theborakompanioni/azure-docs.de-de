---
title: Lokale Datengatewayverbindung mit Logik-Apps | Microsoft-Dokumentation
description: Informationen zum Herstellen einer Verbindung mit dem lokalen Datengateway aus einer Logik-App heraus.
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
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: 0a16f22b6e3bb60091409c64b631afcba3d6db10


---
# <a name="connect-to-the-on-premises-data-gateway-for-logic-apps"></a>Herstellen einer Verbindung mit dem lokalen Datengateway für Logik-Apps
Mit unterstützten Logik-Apps-Connectors können Sie die Verbindung zum Zugriff auf lokale Daten über das lokale Datengateway konfigurieren.  Die folgenden Schritte führen Sie durch die Installation und Konfiguration des lokalen Datengateways für den Einsatz mit einer Logik-App.

## <a name="prerequisites"></a>Voraussetzungen
  * Um das lokale Datengateway Ihrem Konto (auf Azure Active Directory-Basis) zuzuordnen, müssen Sie eine E-Mail-Adresse Ihrer Schule bzw. Ihres Unternehmens in Azure angeben.
  * Bei Verwendung eines Microsoft-Kontos (z.B. @outlook.com, @live.com) können Sie mit Ihrem Azure-Konto mit [folgenden Schritten](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) eine Schul- bzw. Unternehmens-E-Mailadresse erstellen. 
  * Das lokale Datengateway muss [auf einem lokalen Computer installiert sein](logic-apps-gateway-install.md).
  * Das Gateway darf nicht bereits anderweitig als lokales Azure-Datengateway in Anspruch genommen werden ([der Anspruch erfolgt mit der Erstellung von Schritt 2 unten](#2-create-an-azure-on-premises-data-gateway-resource)) – eine Installation kann nur einer einzigen Gatewayressource zugeordnet werden.

## <a name="installing-and-configuring-the-connection"></a>Installieren und Konfigurieren der Verbindung
### <a name="1-install-the-on-premises-data-gateway"></a>1. Installieren des lokalen Datengateways
Informationen zum Installieren von lokalen Datengateways finden Sie [in diesem Artikel](logic-apps-gateway-install.md).  Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie mit den verbleibenden Schritten fortfahren können.

### <a name="2-create-an-azure-on-premises-data-gateway-resource"></a>2. Erstellen einer lokalen Azure-Datengatewayressource
Nach der Installation müssen Sie das lokale Datengateway Ihrem Azure-Abonnement zuordnen.

1. Melden Sie sich mit der gleichen Schul- bzw. Unternehmens-E-Mailadresse bei Azure an, die Sie während der Installation des Gateways verwendet haben.
2. Klicken Sie auf die Ressourcenschaltfläche **Neu** .
3. Suchen Sie das **Lokale Datengateway**
4. Geben Sie die Informationen ein, um das Gateway Ihrem Konto zuzuordnen – einschließlich der Auswahl des entsprechenden **Installationsnamens**
   
    ![Verbindung mit einem lokalen Datengateway][1]
5. Klicken Sie auf die Schaltfläche **Erstellen** , um die Ressource zu erstellen.

### <a name="3-create-a-logic-app-connection-in-the-designer"></a>3. Erstellen einer Logik-App-Verbindung im Designer
Da Ihr Azure-Abonnement jetzt einer Instanz des lokalen Datengateways zugeordnet ist, können Sie aus einer Logik-App heraus eine Verbindung zu dem Gateway erstellen.

1. Öffnen Sie eine Logik-App und wählen Sie einen Connector, der lokale Konnektivität unterstützt (zum Zeitpunkt der Erstellung dieses Dokuments SQL Server).
2. Aktivieren Sie das Kontrollkästchen für **Verbinden über lokales Datengateway**
   
    ![Gatewayerstellung mit Logik-App-Designer][2]
3. Wählen Sie das **Gateway** , mit dem eine Verbindung hergestellt werden soll, und geben Sie alle anderen erforderlichen Verbindungsinformationen an.
4. Klicken Sie auf **Erstellen** , um die Verbindung zu erstellen.

Die Verbindung sollte jetzt erfolgreich für die Verwendung in Ihrer Logik-App konfiguriert werden.  

## <a name="next-steps"></a>Nächste Schritte
* [Allgemeine Beispiele und Szenarien für Logik-Apps](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Unternehmensintegrationsfeatures](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/logic-apps-gateway-connection/createblade.png
[2]: ./media/logic-apps-gateway-connection/blankconnection.png
[3]: ./media/logic-apps-logic-gateway-connection/checkbox.png



<!--HONumber=Jan17_HO3-->


