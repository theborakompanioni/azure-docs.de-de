---
title: Erstellen einer Cordova-App in Azure Mobile App Service-Apps | Microsoft Docs
description: "Befolgen Sie dieses Tutorial für die ersten Schritte bei der Verwendung mobiler Azure-App-Back-Ends für die Apache Cordova-Entwicklung."
services: app-service\mobile
documentationcenter: javascript
author: adrianhall
manager: erikre
editor: 
tags: 
keywords: cordova,javascript,mobile,client
ms.assetid: 0b08fc12-0a80-42d3-9cc1-9b3f8d3e3a3f
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e8a51979c5444d1e1b454d8f434b421f9cc17c24


---
# <a name="create-an-apache-cordova-app"></a>Erstellen einer Apache Cordova-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
Dieses Tutorial zeigt Ihnen, wie Sie einen cloudbasierten Back-End-Dienst mithilfe eines mobilen Azure-App-Back-Ends zu einer mobilen Apache Cordova-App hinzufügen.  Sie erstellen sowohl ein neues Mobile App-Back-End als auch eine einfache Apache Cordova-App des Typs *Aufgabenliste* , die App-Daten in Azure speichert.

Das Absolvieren dieses Tutorials ist Voraussetzung für alle anderen Apache Cordova-Tutorials zur Verwendung des Features „Mobile Apps“ von Azure App Service.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Einen PC mit [Visual Studio Community 2015] oder höher.
* [Visual Studio-Tools für Apache Cordova].
* Ein [aktives Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

Sie können auch Visual Studio umgehen und direkt die Apache Cordova-Befehlszeile verwenden.  Dies ist hilfreich, wenn Sie das Tutorial auf einem Macintosh-Computer ausführen.  Das Kompilieren von Apache Cordova-Clientanwendungen über die Befehlszeile wird in diesem Tutorial nicht behandelt.

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen mobilen Azure-App-Back-Ends
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Ansehen eines Videos mit ähnlichen Schritten](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Herunterladen und Ausführen der Apache Cordova-App
[!INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie dieses Schnellstarttutorial abgeschlossen haben, fahren Sie mit einem der folgenden Tutorials fort:

* [Hinzufügen von Authentifizierung] zu Ihrer Apache Cordova-App
* [Hinzufügen von Pushbenachrichtigungen] zu Ihrer Apache Cordova-App

Erfahren Sie mehr über die Schlüsselkonzepte von Azure App Service.

* [Authentifizierung]
* [Pushbenachrichtigungen]

Erfahren Sie, wie Sie die SDKs nutzen,

* [Apache Cordova-SDK]
* [ASP.NET Server-SDK]
* [Node.js Server-SDK]

<!-- Images. -->

<!-- URLs -->
[Azure-Portal]: https://portal.azure.com/
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio-Tools für Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Hinzufügen von Authentifizierung]: app-service-mobile-cordova-get-started-users.md
[Hinzufügen von Pushbenachrichtigungen]: app-service-mobile-cordova-get-started-push.md
[Authentifizierung]: app-service-mobile-auth.md
[Pushbenachrichtigungen]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova-SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server-SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server-SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md



<!--HONumber=Nov16_HO2-->


