---
title: "Erste Schritte mit Azure Mobile App Service-Apps für Xamarin.iOS-Apps | Microsoft Docs"
description: "Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Mobile Apps für die Xamarin.iOS-Entwicklung."
services: app-service\mobile
documentationcenter: xamarin
author: adrianhall
manager: dwrede
editor: 
ms.assetid: 14428794-52ad-4b51-956c-deb296cafa34
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: cf8f9a06c4d720dade11ea67fc1befe07a56cf85


---
# <a name="create-a-xamarinios-app"></a>Erstellen einer Xamarin.iOS-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
In diesem Lernprogramm erfahren Sie, wie Sie mithilfe eines mobilen Azure-App-Back-Ends einen cloudbasierten Back-End-Dienst einer mobilen Xamarin.iOS-App hinzufügen können.  Sie erstellen sowohl ein neues mobiles App-Back-End als auch eine einfache Xamarin-iOS-App des Typs *Aufgabenliste*, die App-Daten in Azure speichert.

Das Absolvieren dieses Lernprogramms ist Voraussetzung für alle anderen Xamarin.iOS-Lernprogramme zur Verwendung des Features "Mobile Apps" von Azure App Service.

## <a name="prerequisites"></a>Voraussetzungen
Zum Durchführen dieses Tutorials benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto haben, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio mit Xamarin. Anweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) .
* Ein Mac, auf dem Xcode Version 7.0 oder höher und Xamarin Studio Community installiert ist. Informationen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) und [Setup, Installation und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, besuchen Sie [Azure App Service-App erstellen](https://tryappservice.azure.com/?appServiceName=mobile). Sie können direkt eine kurzzeitige mobile Start-App in App Service erstellen, und zwar ohne erforderliche Kreditkarte oder weitere Verpflichtungen.
> 
> 

## <a name="create-an-azure-mobile-app-backend"></a>Erstellen eines Azure Mobile App-Back-Ends
Führen Sie die folgenden Schritte aus, um ein Mobile App-Back-End zu erstellen.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts
Sie haben nun ein Azure-Back-End für mobile Apps bereitgestellt, das von Ihren mobilen Clientanwendungen verwendet werden kann. Als Nächstes laden Sie ein Serverprojekt für ein einfaches Back-End für „Aufgabenlisten“ herunter und veröffentlichen es in Azure.

Befolgen Sie die folgenden Schritte zum Konfigurieren des Serverprojekts für die Verwendung des Node.js- oder .NET-Back-Ends.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinios-app"></a>Herunterladen und Ausführen der Xamarin.iOS-App
1. Öffnen Sie das [Azure-Portal] in einem Browserfenster.
2. Klicken Sie auf dem Blatt „Einstellungen“ für Ihre mobile App auf **Erste Schritte** > **Xamarin.iOS**. Klicken Sie unter Schritt 3 auf **Neue App erstellen** , falls noch nicht ausgewählt.  Klicken Sie dann auf die Schaltfläche **Herunterladen** .
   
      Eine Clientanwendung, die sich mit Ihrem mobilen Back-End-verbindet, wird heruntergeladen. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.
3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Xamarin Studio (oder Visual Studio).
   
    ![][9]
   
    ![][8]
4. Drücken Sie F5, um das Projekt zu erstellen und die App im iPhone-Simulator zu starten.
5. Geben Sie in der App einen sinnvollen Text ein, z.B. *Xamarin kennenlernen*, und klicken Sie dann auf die Schaltfläche **+**.
   
    ![][10]
   
    Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [!NOTE]
> Sie können den Code überprüfen, der auf das mobile App-Back-End zum Abfragen und Einfügen von Daten zugreift. Sie finden ihn in der C#-Datei "QSTodoService.cs C#".
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen der Offlinesynchronisierung zu Ihrer App](app-service-mobile-xamarin-ios-get-started-offline-data.md)
* [Hinzufügen der Authentifizierung zu Ihrer App ](app-service-mobile-xamarin-ios-get-started-users.md)
* [Hinzufügen von Pushbenachrichtigungen zur Xamarin.Android-App](app-service-mobile-xamarin-ios-get-started-push.md)
* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)

<!-- Anchors. -->
[Erste Schritte mit Mobile App-Back-Ends]:#getting-started
[Erstellen eines neuen mobilen App-Back-Ends]:#create-new-service
[Next Steps]:#next-steps

<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-ios-get-started/xamarin-ios-quickstart.png
[8]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png
[9]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png
[10]: ./media/app-service-mobile-xamarin-ios-get-started/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/



<!--HONumber=Nov16_HO2-->


