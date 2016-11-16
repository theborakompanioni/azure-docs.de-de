---
title: "Erste Schritte mit Azure Mobile Apps für Xamarin.Android-Apps"
description: "Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Mobile Apps für die Xamarin Android-Entwicklung."
services: app-service\mobile
documentationcenter: xamarin
author: adrianhall
manager: erikre
editor: 
ms.assetid: 81649dd3-544f-40ff-b9b7-60c66d683e60
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e59a87ec6017061078d5cb5df93207c7f93ec00b


---
# <a name="create-a-xamarinandroid-app"></a>Erstellen einer Xamarin.Android-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
In diesem Tutorial erfahren Sie, wie Sie einer Xamarin.Android-App einen cloudbasierten Back-End-Dienst hinzufügen. Weitere Informationen finden Sie unter [Was sind Mobile Apps?](app-service-mobile-value-prop.md)

Screenshot der fertigen App:

![][0]

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin Android-Apps.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto haben, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio mit Xamarin. Anweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .

> [!NOTE]
> Wenn Sie mobile Apps ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [App Service testen](https://tryappservice.azure.com/?appServiceName=mobile).  Sie können sofort eine kurzlebige Starter-Logik-App in Mobile App erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="create-an-azure-mobile-app-backend"></a>Erstellen eines Azure Mobile App-Back-Ends
Führen Sie die folgenden Schritte aus, um ein Mobile App-Back-End zu erstellen.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben nun ein Azure-Back-End für mobile Apps bereitgestellt, das von Ihren mobilen Clientanwendungen verwendet werden kann. Als Nächstes laden Sie ein Serverprojekt für ein einfaches Back-End für „Aufgabenlisten“ herunter und veröffentlichen es in Azure.

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinandroid-app"></a>Herunterladen und Ausführen der Xamarin.Android-App
1. Klicken Sie unter **Herunterladen und Xamarin.Android-Projekt ausführen** auf die Schaltfläche **Herunterladen**.
   
      Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.
2. Drücken Sie die **F5** -Taste, um das Projekt zu erstellen und die App zu starten.
3. Geben Sie in der App einen sinnvollen Text wie z.B. *Tutorial fertigstellen* ein, und klicken Sie dann auf die Schaltfläche **Hinzufügen**.
   
    ![][10]
   
    Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden von dem mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.
   
   > [!NOTE]
   > Sie können den Code überprüfen, der zum Abfragen und Einfügen von Daten auf das mobile App-Back-End zugreift. Der Code befindet sich in der C#-Datei "ToDoActivity.cs".
   > 
   > 

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen der Offlinesynchronisierung zu Ihrer App](app-service-mobile-xamarin-android-get-started-offline-data.md)
* [Hinzufügen der Authentifizierung zu Ihrer App ](app-service-mobile-xamarin-android-get-started-users.md)
* [Hinzufügen von Pushbenachrichtigungen zu Ihrer Xamarin.Android-App](app-service-mobile-xamarin-android-get-started-push.md)
* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Images. -->
[0]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/app-service-mobile-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[8]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/app-service-mobile-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/app-service-mobile-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Azure-Portal]: https://azure.portal.com/
[Visual Studio]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!--HONumber=Nov16_HO2-->


