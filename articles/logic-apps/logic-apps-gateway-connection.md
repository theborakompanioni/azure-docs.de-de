---
title: "Zugreifen auf lokale Datenquellen für Azure Logic Apps | Microsoft-Dokumentation"
description: "Richten Sie das lokale Datengateway ein, damit Sie aus Logik-Apps auf lokale Datenquellen zugreifen können."
keywords: "Zugreifen auf Daten, lokal, Datenübertragung, Verschlüsselung, Datenquellen"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/5/2017
ms.author: LADocs; dimazaid; estfan
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 8446790af6af160c4b2d463191405faaed68bf0e
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="access-data-sources-on-premises-from-logic-apps-with-the-on-premises-data-gateway"></a>Zugreifen aus Logik-Apps auf lokale Datenquellen mit dem lokalen Datengateway

Um aus Ihren Logik-Apps auf lokale Datenquellen zuzugreifen, richten Sie ein lokales Datengateway ein, das von Logik-Apps mit unterstützten Connectors verwendet werden kann. Das Gateway fungiert als Brücke, die schnelle Datenübertragung und Verschlüsselung zwischen lokalen Datenquellen und Ihren Logik-Apps bereitstellt. Das Gateway überträgt Daten aus lokalen Quellen durch verschlüsselte Kanäle über Azure Service Bus. Der gesamte Datenverkehr stammt als sicherer ausgehender Datenverkehr vom Gateway-Agent. Erfahren Sie mehr darüber, [wie das Datengateway funktioniert](logic-apps-gateway-install.md#gateway-cloud-service). 

Das Gateway unterstützt Verbindungen mit diesen lokalen Datenquellen:

*   BizTalk Server
*   DB2  
*   Dateisystem
*   Informix
*   MQ
*   MySQL
*   Oracle-Datenbank
*   PostgreSQL
*   SAP-Anwendungsserver 
*   SAP-Nachrichtenserver
*   SharePoint nur für HTTP, nicht für HTTPS
*   SQL Server
*   Teradata

In diesen Schritten wird gezeigt, wie das lokale Datengateway einzurichten ist, damit es mit Ihren Logik-Apps funktioniert. Weitere Informationen zu unterstützten Connectors finden Sie unter [Connectors für Azure Logic Apps](../connectors/apis-list.md). 

Weitere Informationen zu Datengateways für andere Microsoft-Dienste finden Sie in den folgenden Artikeln:

*   [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/): [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)
*   [Microsoft Power BI | Lokales Datengateway](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services | Lokales Datengateway](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow | Lokales Datengateway](https://flow.microsoft.com/documentation/gateway-manage/)

## <a name="requirements"></a>Requirements (Anforderungen)

* Sie müssen [das Datengateway bereits auf einem lokalen Computer installiert](logic-apps-gateway-install.md) haben.

* Sie benötigen das Azure-Konto, für das die Geschäfts- oder Schul-E-Mail-Adresse verwendet wurde, mit der [das lokale Datengateway installiert wurde](logic-apps-gateway-install.md#requirements).

* Die Gatewayinstallation darf noch nicht von einer anderen Azure-Gatewayressource beansprucht sein. Sie können Ihre Gatewayinstallation nur einer Gateway-Ressource zuordnen. Beanspruchen tritt auf, wenn Sie die Gatewayressource so erstellen, dass die Installation für andere Ressourcen nicht verfügbar ist.

## <a name="set-up-the-data-gateway-connection"></a>Einrichten der Datengatewayverbindung

### <a name="1-install-the-on-premises-data-gateway"></a>1. Installieren des lokalen Datengateways

Führen Sie (sofern noch nicht geschehen) [die Schritte zum Installieren des lokalen Datengateways](logic-apps-gateway-install.md) aus. Bevor Sie mit den weiteren Schritten fortfahren, müssen Sie das Datengateway auf einem lokalen Computer installiert haben.

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a>2. Erstellen einer Azure-Ressource für das lokale Datengateway

Nachdem Sie das Gateway auf einem lokalen Computer installiert haben, müssen Sie Ihr Datengateway als Ressource in Azure erstellen. In diesem Schritt wird Ihre Gatewayressource auch Ihrem Azure-Abonnement zugeordnet.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com "Azure-Portal") an. Achten Sie darauf, dass Sie die Azure-Geschäfts- oder -Schul-E-Mail-Adresse verwenden, mit der Sie das Gateway installiert haben.

2. Wählen Sie im linken Menü in Azure den Befehl **Neu** > **Unternehmensintegration** > **Lokales Datengateway** aus:

   ![Nach „Lokales Datengateway“ suchen](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. Stellen Sie auf dem Blatt **Verbindungsgateway erstellen** die folgenden Details bereit, um Ihre Datengatewayressource zu erstellen:

    * **Name**: Geben Sie einen Namen für die Gatewayressource ein. 

    * **Abonnement**: Wählen Sie das Azure-Abonnement aus, das Sie Ihrer Gatewayressource zuordnen möchten. 
    Dieses Abonnement muss mit dem Abonnement für Ihre Logik-App identisch sein.
   
      Das standardmäßige Abonnement basiert auf dem Azure-Konto, das Sie zum Anmelden verwendet haben.

    * **Ressourcengruppe**: Erstellen Sie eine Ressourcengruppe, oder wählen Sie eine vorhandene Ressourcengruppe aus, um die Gatewayressource bereitzustellen. 
    Über Ressourcengruppen können Sie zugehörige Azure-Assets als eine Sammlung verwalten.

    * **Standort**: Azure schränkt diesen Standort auf die Region ein, die während der [Gatewayinstallation](logic-apps-gateway-install.md) für den Gatewayclouddienst ausgewählt wurde . 

      > [!NOTE]
      > Stellen Sie sicher, dass der Standort der Gatewayressource mit dem Standort des Gatewayclouddiensts übereinstimmt. Andernfalls kann es sein, dass Ihre Gatewayinstallation im nächsten Schritt nicht in der Liste der installierten Gateways zur Auswahl angezeigt wird.
      > 
      > Sie können unterschiedliche Regionen für Ihre Gatewayressource und Ihre Logik-App verwenden.

    * **Installationsname**: Wenn Ihre Gatewayinstallation noch nicht ausgewählt ist, wählen Sie das Gateway aus, das Sie zuvor installiert haben. 

    Um die Gatewayressource zu Ihrem Azure-Dashboard hinzuzufügen, wählen Sie **An Dashboard anheften** aus. 
    Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

    Beispiel:

    ![Angeben von Details zum Erstellen Ihres lokalen Datengateways](./media/logic-apps-gateway-connection/createblade.png)

    Um Ihr Datengateway jederzeit zu finden oder anzuzeigen, navigieren Sie über das Azure Hauptmenü auf der linken Seite zu  **Weitere Dienste** > **Unternehmensintegration** > **Lokale Datengateways**.

    ![Wechseln zu „Weitere Dienste“, „Unternehmensintegration“, „Lokale Datengateways“](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a>3. Verbinden Ihrer Logik-App mit dem lokalen Datengateway

Nachdem Sie Ihre Datengatewayressource erstellt und Ihr Azure-Abonnement mit dieser Ressource verknüpft haben, erstellen Sie eine Verbindung zwischen Ihrer Logik-App und dem Datengateway.

> [!NOTE]
> Der Standort für die Gatewayverbindung muss sich in derselben Region befinden wie Ihre Logik-App, Sie können aber ein Datengateway verwenden, das sich in einer anderen Region befindet.

1. Erstellen oder öffnen Sie Ihre Logik-App im Azure-Portal im Logik-App-Designer.

2. Fügen Sie einen Connector hinzu, der lokale Verbindungen unterstützt, z.B. SQL Server.

3. Gehen Sie entsprechend der gezeigten Reihenfolge vor. Aktivieren Sie **Über lokales Datengateway verbinden**, geben Sie einen eindeutigen Namen und die erforderlichen Informationen ein, und wählen Sie die Datengatewayressource aus, die Sie verwenden möchten. Wenn Sie fertig sind, wählen Sie **Erstellen** aus.

   > [!TIP]
   > Ein eindeutiger Verbindungsname vereinfacht später ein Erkennen dieser Verbindung, insbesondere wenn Sie mehrere Verbindungen erstellen. Sofern erforderlich, geben Sie auch die qualifizierte Domäne für Ihren Benutzernamen an. 

   ![Erstellen der Verbindung zwischen der Logik-App und dem Datengateway](./media/logic-apps-gateway-connection/blankconnection.png)

Herzlichen Glückwunsch, die Gatewayverbindung kann nun für Ihre Logik-App verwendet werden.

## <a name="edit-your-gateway-connection-settings"></a>Bearbeiten der Einstellungen für die Gatewayverbindung

Nach dem Sie eine Gatewayverbindung für Ihre Logik-App erstellt haben, kann es sein, dass Sie die Einstellungen für diese Verbindung später aktualisieren möchten.

1. So finden Sie die Gatewayverbindung:

   * Wählen Sie auf dem Blatt für die Logik-App unter **Entwicklungstools** die Option **API-Verbindungen** aus. 
   
     Im Bereich **API-Verbindungen** werden alle API-Verbindungen aufgeführt, die Ihrer Logik-App zugeordnet sind, darunter auch die Gatewayverbindungen.

     ![Navigieren zu Ihrer Logik-App, Auswählen von „API-Verbindungen“](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * Oder wechseln Sie über das Azure-Hauptmenü auf der linken Seite zu **Weitere Dienste** > **Web & Mobile Services** > **API Verbindungen**, um alle API-Verbindungen anzuzeigen, einschließlich der Gatewayverbindungen, die Ihrem Azure-Abonnement zugeordnet sind. 

   * Alternativ können Sie über das Azure-Hauptmenü auf der linken Seite zu **Alle Ressourcen** für alle API-Verbindungen wechseln, darunter die Gatewayverbindungen, die Ihrem Azure-Abonnement zugeordnet sind.

2. Wählen Sie die Gatewayverbindung aus, die Sie anzeigen oder bearbeiten möchten, und wählen Sie **API-Verbindung bearbeiten** aus.

   > [!TIP]
   > Sollten Ihre Aktualisierungen nicht wirksam werden, [beenden Sie den Gateway-Windows-Dienst, und starten Sie ihn neu](./logic-apps-gateway-install.md#restart-gateway).

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a>Wechseln oder Löschen der lokalen Datengatewayressource

Um eine andere Ressource zu erstellen, Ihr Gateway mit einer anderen Ressource zu verknüpfen oder die Gatewayressource zu entfernen, können Sie die Gatewayressource löschen, ohne dass sich dies auf die Gatewayinstallation auswirkt. 

1. Wechseln Sie über das Azure-Hauptmenü auf der linken Seite zu **Alle Ressourcen**. 
2. Suchen Sie nach Ihrem Datengatewayressource, und wählen Sie es aus.
3. Wählen Sie **Lokales Datengateway** aus, und wählen Sie auf der Symbolleiste für Ressourcen das Symbol **Löschen** aus.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Sichern Ihrer Logik-Apps](./logic-apps-securing-a-logic-app.md)
* [Allgemeine Beispiele und Szenarien für Logik-Apps](./logic-apps-examples-and-scenarios.md)

