<properties
   pageTitle="Lokale Datengatewayverbindung mit Logik-Apps | Microsoft Azure"
   description="Informationen zum Herstellen einer Verbindung mit dem lokalen Datengateway aus einer Logik-App heraus."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/05/2016"
   ms.author="jehollan"/>

# Herstellen einer Verbindung mit dem lokalen Datengateway für Logik-Apps

Mit unterstützten Logik-Apps-Connectors können Sie die Verbindung zum Zugriff auf lokale Daten über das lokale Datengateway konfigurieren. Die folgenden Schritte führen Sie durch die Installation und Konfiguration des lokalen Datengateways für den Einsatz mit einer Logik-App.

## Voraussetzungen

* Um das lokale Datengateway Ihrem Konto (auf Azure Active Directory-Basis) zuzuordnen, müssen Sie eine E-Mail-Adresse Ihrer Schule bzw. Ihres Unternehmens in Azure angeben.
    * Bei Verwendung eines Microsoft-Kontos (z.B. @outlook.com, @live.com) können Sie mit Ihrem Azure-Konto mit [folgenden Schritten](../virtual-machines/virtual-machines-windows-create-aad-work-id.md#locate-your-default-directory-in-the-azure-classic-portal) eine Schul- bzw. Unternehmens-E-Mailadresse erstellen.

> [AZURE.WARNING] Derzeit gilt eine Einschränkung, dass die lokale Gateway-Installation nur abgeschlossen wird, wenn ein mit Power BI registriertes Konto verwendet wird. Registrieren Sie in der Zwischenzeit ein Konto mit „Power BI Free“, um die Installation erfolgreich abzuschließen.

* Das lokale Datengateway muss [auf einem lokalen Computer installiert sein](app-service-logic-gateway-install.md).
* Das Gateway darf nicht bereits anderweitig als lokales Azure-Datengateway in Anspruch genommen werden ([der Anspruch erfolgt mit der Erstellung von Schritt 2 unten](#2-create-an-azure-on-premises-data-gateway-resource)) – eine Installation kann nur einer einzigen Gatewayressource zugeordnet werden.

## Installieren und Konfigurieren der Verbindung

### 1\. Installieren des lokalen Datengateways

Informationen zum Installieren von lokalen Datengateways finden Sie [in diesem Artikel](app-service-logic-gateway-install.md). Das Gateway muss auf einem lokalen Computer installiert werden, bevor Sie mit den verbleibenden Schritten fortfahren können.

### 2\. Erstellen einer lokalen Azure-Datengatewayressource

Nach der Installation müssen Sie das lokale Datengateway Ihrem Azure-Abonnement zuordnen.

1. Melden Sie sich mit der gleichen Schul- bzw. Unternehmens-E-Mailadresse bei Azure an, die Sie während der Installation des Gateways verwendet haben.
1. Klicken Sie auf die Ressourcenschaltfläche **Neu**.
1. Suchen Sie das **Lokale Datengateway** und wählen Sie es aus.
1. Geben Sie die Informationen ein, um das Gateway Ihrem Konto zuzuordnen – einschließlich der Auswahl des entsprechenden **Installationsnamens**.

    ![Verbindung mit einem lokalen Datengateway][1]
1. Klicken Sie auf die Schaltfläche **Erstellen**, um die Ressource zu erstellen.

### 3\. Erstellen einer Logik-App-Verbindung im Designer

Da Ihr Azure-Abonnement jetzt einer Instanz des lokalen Datengateways zugeordnet ist, können Sie aus einer Logik-App heraus eine Verbindung zu dem Gateway erstellen.

1. Öffnen Sie eine Logik-App und wählen Sie einen Connector, der lokale Konnektivität unterstützt (zum Zeitpunkt der Erstellung dieses Dokuments SQL Server).
1. Aktivieren Sie das Kontrollkästchen für **Verbinden über lokales Datengateway**.

    ![Gatewayerstellung mit Logik-App-Designer][2]
1. Wählen Sie das **Gateway**, mit dem eine Verbindung hergestellt werden soll, und geben Sie alle anderen erforderlichen Verbindungsinformationen an.
1. Klicken Sie auf **Erstellen**, um die Verbindung zu erstellen.

Die Verbindung sollte jetzt erfolgreich für die Verwendung in Ihrer Logik-App konfiguriert werden.

## Nächste Schritte
- [Allgemeine Beispiele und Szenarien für Logik-Apps](app-service-logic-examples-and-scenarios.md)
- [Unternehmensintegrationsfeatures](app-service-logic-enterprise-integration-overview.md)

<!-- Image references -->
[1]: ./media/app-service-logic-gateway-connection/createblade.PNG
[2]: ./media/app-service-logic-gateway-connection/blankconnection.PNG
[3]: ./media/app-service-logic-gateway-connection/checkbox.PNG

<!---HONumber=AcomDC_0803_2016-->