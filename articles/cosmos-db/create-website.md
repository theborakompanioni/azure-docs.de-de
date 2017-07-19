---
title: "Bereitstellen einer Web-App mit einer Vorlage – Azure Cosmos DB | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie ein Azure Cosmos DB-Konto, eine Azure App Service-Web-App und eine Beispielwebanwendung mithilfe einer Vorlage des Azure Resource Managers bereitstellen.
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 633b88761de4d2c99cfd196cfac8e664fc83c546
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Bereitstellen von Azure Cosmos DB und Azure App Service-Web-Apps mithilfe einer Vorlage des Azure Resource Managers
In diesem Lernprogramm erfahren Sie, wie Sie mithilfe einer Azure Resource Manager-Vorlage [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), eine [Azure App Service-Web-App](http://go.microsoft.com/fwlink/?LinkId=529714) und eine Beispielwebanwendung bereitstellen und integrieren.

Mit Azure Resource Manager-Vorlagen können Sie die Bereitstellung und Konfiguration von Azure-Ressourcen problemlos automatisieren.  Dieses Tutorial zeigt, wie Sie eine Webanwendung bereitstellen und Azure Cosmos DB-Kontoverbindungsinformationen automatisch konfigurieren.

Nach Abschluss dieses Tutorials können Sie die folgenden Fragen beantworten:  

* Wie kann ich mithilfe einer Vorlage des Azure Resource Managers ein Azure Cosmos DB-Konto und eine Web-App in Azure App Service bereitstellen und integrieren?
* Wie kann ich mithilfe einer Vorlage des Azure Resource Managers ein Azure Cosmos DB-Konto, eine Web-App in App Service-Web-Apps und eine WebDeploy-Anwendung bereitstellen und integrieren?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Voraussetzungen
> [!TIP]
> Zwar wird für dieses Tutorial keine Erfahrung im Umgang mit Azure Resource Manager-Vorlagen oder JSON vorausgesetzt, wenn Sie jedoch die hierin verwendeten Vorlagen oder Bereitstellungsoptionen ändern möchten, sind Kenntnisse in jedem dieser Bereiche erforderlich.
> 
> 

Vor dem Ausführen der Anweisungen zu diesem Lernprogramm, müssen Sie sicherstellen, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Azure ist eine abonnementbasierte Plattform.  Weitere Informationen zum Erwerb eines Abonnements finden Sie unter [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/), [Spezielle Angebote](https://azure.microsoft.com/pricing/member-offers/) oder [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>Schritt 1: Herunterladen der Vorlagendateien
Als Erstes laden wir die Vorlagendateien herunter, die in diesem Tutorial verwendet werden.

1. Laden Sie die Vorlage für das [Beispiel zum Erstellen eines Azure Cosmos DB-Kontos, zum Erstellen von Web-Apps und zum Bereitstellen einer Demoanwendung](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) in einen lokalen Ordner (beispielsweise „C:\Azure Cosmos DBTemplates“) herunter. Diese Vorlage stellt ein Azure Cosmos DB-Konto, eine App Service-Web-App und eine Webanwendung bereit.  Außerdem wird die Webanwendung automatisch so konfiguriert, dass sie eine Verbindung mit dem Azure Cosmos DB-Konto herstellt.
2. Laden Sie die Vorlage für das [Beispiel zum Erstellen eines Azure Cosmos DB-Kontos und von Web-Apps](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) in einen lokalen Ordner (beispielsweise „C:\Azure Cosmos DBTemplates“) herunter. Mit dieser Vorlage werden ein Azure Cosmos DB-Konto und eine App Service-Web-App bereitgestellt; außerdem werden die Anwendungseinstellungen der Site so geändert, dass Azure Cosmos DB-Verbindungsinformationen problemlos angegeben werden können, eine Webanwendung ist jedoch nicht enthalten.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>Schritt 2: Bereitstellen des Beispiels mit Azure Cosmos DB-Konto, App Service-Web-App und Demoanwendung
Jetzt stellen wir unsere erste Vorlage bereit.

> [!TIP]
> Die Vorlage überprüft nicht, ob die unten eingegebenen Namen der Web-App und der Azure Cosmos DB-Kontoname (a) gültig und (b) verfügbar sind.  Sie sollten unbedingt die Verfügbarkeit der gewünschten Namen überprüfen, bevor Sie die Bereitstellung ausführen.
> 
> 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an, klicken Sie auf „Neu“, und suchen Sie nach „Vorlagenbereitstellung“.
    ![Screenshot der Benutzeroberfläche für die Vorlagenbereitstellung](./media/create-website/TemplateDeployment1.png)
2. Wählen Sie das Vorlagenbereitstellungselement aus, und klicken Sie anschließend auf **Erstellen**.
    ![Screenshot der Benutzeroberfläche für die Vorlagenbereitstellung](./media/create-website/TemplateDeployment2.png)
3. Klicken Sie auf **Vorlage bearbeiten**, fügen Sie den Inhalt der Vorlagendatei „DocDBWebsiteTodo.json“ ein, und klicken Sie auf **Speichern**.
   ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment3.png)
4. Klicken Sie auf **Parameter bearbeiten**, geben Sie Werte für die obligatorischen Parameter an, und klicken Sie auf **OK**.  Die Parameter lauten wie folgt:
   
   1. SITENAME: Gibt den Namen der App Service-Web-App an und wird verwendet, um die URL zu erstellen, die Sie für den Zugriff auf die Web-App verwenden (wenn Sie z.B. „mydemodocdbwebapp“ angeben, dann lautet die URL, mit der Sie auf die Web-App zugreifen, „mydemodocdbwebapp.azurewebsites.net“).
   2. HOSTINGPLANNAME: Gibt den Namen des zu erstellenden App Service-Hostingplans an.
   3. LOCATION: Gibt den Azure-Speicherort an, wo die Azure Cosmos DB- und Web-App-Ressourcen erstellt werden sollen.
   4. DATABASEACCOUNTNAME: Gibt den Namen des zu erstellenden Azure Cosmos DB-Kontos an.   
      
      ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment4.png)
5. Wählen Sie eine vorhandene Ressourcengruppe aus, oder geben Sie einen Namen an, um eine neue Ressourcengruppe zu erstellen, und wählen Sie einen Speicherort für die Ressourcengruppe.

    ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment5.png)
6. Klicken Sie auf **Prüfen Sie die rechtlichen Bedingungen.** > **Kaufen** und anschließend auf **Erstellen**, um mit der Bereitstellung zu beginnen.  Wählen Sie **An Dashboard anheften** , damit die resultierende Bereitstellung auf Ihrer Azure-Portal-Startseite problemlos sichtbar ist.
   ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment6.png)
7. Wenn die Bereitstellung abgeschlossen ist, wird das Blatt der Ressourcengruppe geöffnet.
   ![Screenshot des Blatts „Ressourcengruppe“](./media/create-website/TemplateDeployment7.png)  
8. Um die Anwendung zu verwenden, navigieren Sie zur Web-App-URL (im obigen Beispiel: http://mydemodocdbwebapp.azurewebsites.net).  Die folgende Webanwendung wird angezeigt:
   
   ![Todo-Beispielanwendung](./media/create-website/image2.png)
9. Fahren Sie fort, erstellen Sie eine Reihe von Aufgaben in der Web-App, und kehren Sie dann zum Blatt der Ressourcengruppe im Azure-Portal zurück. Klicken Sie auf die Azure Cosmos DB-Kontoressource in der Liste „Ressourcen“ und dann auf **Abfrage-Explorer**.
    ![Screenshot der Zusammenfassung mit der hervorgehobenen Web-App](./media/create-website/TemplateDeployment8.png)  
10. Führen Sie die Standardabfrage „SELECT * FROM c“ aus, und überprüfen Sie die Ergebnisse.  Beachten Sie, dass die Abfrage die JSON-Darstellung der Aufgaben abgerufen hat, die Sie in Schritt 7 oben erstellt haben.  Sie können gerne mit Abfragen experimentieren. Führen Sie beispielsweise „SELECT * FROM c WHERE c.isComplete = true“ aus, um alle Aufgaben zurückzugeben, die als abgeschlossen gekennzeichnet wurden.
    
    ![Screenshot der Blätter „Abfrage-Explorer“ und „Ergebnisse“ mit Anzeige der Abfrageergebnisse](./media/create-website/image5.png)
11. Erkunden Sie die Oberfläche des Azure Cosmos DB-Portals, oder ändern Sie die Todo-Beispielanwendung.  Wenn Sie fertig sind, stellen wir eine andere Vorlage bereit.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>Schritt 3: Bereitstellen des Dokument-Kontos und Web-App-Beispiels
Jetzt stellen wir unsere zweite Vorlage bereit.  Diese Vorlage zeigt Ihnen sehr anschaulich, wie Sie Azure Cosmos DB-Verbindungsinformationen, z. B. Kontoendpunkt und Hauptschlüssel, mit Anwendungseinstellungen oder als benutzerdefinierte Verbindungszeichenfolge in eine Web-App einfügen können. Sie möchten vielleicht eine eigene Webanwendung mit einem Azure Cosmos DB-Konto bereitstellen, und die Verbindungsinformationen sollen während der Bereitstellung automatisch aufgefüllt werden.

> [!TIP]
> Die Vorlage überprüft nicht, ob die unten eingegebenen Namen der Web-App und der Azure Cosmos DB-Kontoname (a) gültig und (b) verfügbar sind.  Sie sollten unbedingt die Verfügbarkeit der gewünschten Namen überprüfen, bevor Sie die Bereitstellung ausführen.
> 
> 

1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf „Neu“, und suchen Sie nach „Vorlagenbereitstellung“.
    ![Screenshot der Benutzeroberfläche für die Vorlagenbereitstellung](./media/create-website/TemplateDeployment1.png)
2. Wählen Sie das Vorlagenbereitstellungselement aus, und klicken Sie anschließend auf **Erstellen**.
    ![Screenshot der Benutzeroberfläche für die Vorlagenbereitstellung](./media/create-website/TemplateDeployment2.png)
3. Klicken Sie auf **Vorlage bearbeiten**, fügen Sie den Inhalt der Vorlagendatei „DocDBWebSite.json“ ein, und klicken Sie auf **Speichern**.
   ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment3.png)
4. Klicken Sie auf **Parameter bearbeiten**, geben Sie Werte für die obligatorischen Parameter an, und klicken Sie auf **OK**.  Die Parameter lauten wie folgt:
   
   1. SITENAME: Gibt den Namen der App Service-Web-App an und wird verwendet, um die URL zu erstellen, die Sie für den Zugriff auf die Web-App verwenden (wenn Sie z.B. „mydemodocdbwebapp“ angeben, dann lautet die URL, mit der Sie auf die Web-App zugreifen, „mydemodocdbwebapp.azurewebsites.net“).
   2. HOSTINGPLANNAME: Gibt den Namen des zu erstellenden App Service-Hostingplans an.
   3. LOCATION: Gibt den Azure-Speicherort an, wo die Azure Cosmos DB- und Web-App-Ressourcen erstellt werden sollen.
   4. DATABASEACCOUNTNAME: Gibt den Namen des zu erstellenden Azure Cosmos DB-Kontos an.   
      
      ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment4.png)
5. Wählen Sie eine vorhandene Ressourcengruppe aus, oder geben Sie einen Namen an, um eine neue Ressourcengruppe zu erstellen, und wählen Sie einen Speicherort für die Ressourcengruppe.

    ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment5.png)
6. Klicken Sie auf **Prüfen Sie die rechtlichen Bedingungen.** > **Kaufen** und anschließend auf **Erstellen**, um mit der Bereitstellung zu beginnen.  Wählen Sie **An Dashboard anheften** , damit die resultierende Bereitstellung auf Ihrer Azure-Portal-Startseite problemlos sichtbar ist.
   ![Screenshot der Benutzeroberfläche der Vorlagenbereitstellung](./media/create-website/TemplateDeployment6.png)
7. Wenn die Bereitstellung abgeschlossen ist, wird das Blatt der Ressourcengruppe geöffnet.
   ![Screenshot des Blatts „Ressourcengruppe“](./media/create-website/TemplateDeployment7.png)  
8. Klicken Sie in der Liste „Ressourcen“ auf die Web-App-Ressource und anschließend auf **Anwendungseinstellungen**.
    ![Screenshot der Ressourcengruppe](./media/create-website/TemplateDeployment9.png)  
9. Beachten Sie, dass Anwendungseinstellungen für den Azure Cosmos DB-Endpunkt und jeden der Azure Cosmos DB-Hauptschlüssel vorhanden sind.

    ![Screenshot von Anwendungseinstellungen](./media/create-website/TemplateDeployment10.png)  
10. Sie können das Azure-Portal gerne weiter erkunden oder eines der Azure Cosmos DB-[Beispiele](http://go.microsoft.com/fwlink/?LinkID=402386) durcharbeiten, um Ihre eigene Azure Cosmos DB-Anwendung zu erstellen.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Nächste Schritte
Glückwunsch! Sie haben Azure Cosmos DB, eine App Service-Web-App und eine Beispielwebanwendung mithilfe von Vorlagen des Azure Resource Managers bereitgestellt.

* Um weitere Informationen zu Azure Cosmos DB zu erhalten, klicken Sie [hier](http://azure.com/docdb).
* Weitere Informationen zu Azure App Service Web-Apps erhalten Sie, indem Sie [hier](http://go.microsoft.com/fwlink/?LinkId=325362)klicken.
* Weitere Informationen zu Vorlagen des Azure-Ressourcen-Managers erhalten Sie, indem Sie [hier](https://msdn.microsoft.com/library/azure/dn790549.aspx)klicken.

## <a name="whats-changed"></a>Änderungen
* Hinweise zu den Änderungen von Websites zum App Service finden Sie unter: [Azure App Service und vorhandene Azure-Dienste](http://go.microsoft.com/fwlink/?LinkId=529714)
* Hinweise zu den Veränderungen des neuen Portals gegenüber dem alten finden Sie unter [Referenz zur Navigation im klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715)

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](http://go.microsoft.com/fwlink/?LinkId=523751)sofort kostenlos eine kurzlebige Starter-Web-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 


