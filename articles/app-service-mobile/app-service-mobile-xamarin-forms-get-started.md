---
title: Erste Schritte mit Mobile Apps unter Verwendung von Xamarin.Forms
description: "In diesem Einführungstutorial erfahren Sie, wie Sie Mobile Apps für die Xamarin.Forms-Entwicklung verwenden."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: ee12caaad4095cff6dae3282f747ae804f93db81
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="create-a-xamarinforms-app"></a>Erstellen einer Xamarin.Forms-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
Dieses Tutorial zeigt, wie Sie einer mobilen Xamarin.Forms-App mithilfe des Mobile Apps-Features von Azure App Service als Back-End einen cloudbasierten Back-End-Dienst hinzufügen. Sie erstellen sowohl ein neues Mobile Apps-Back-End als auch eine einfache Xamarin.Forms-Aufgabenlisten-App, die App-Daten in Azure speichert.

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin.Forms.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto besitzen, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Informationen finden Sie auf der Seite [Setup und Installation](https://msdn.microsoft.com/library/mt613162.aspx) für Visual Studio und Xamarin.

* Ein Mac, auf dem Xcode Version 7.0 oder höher und Xamarin Studio Community installiert ist. Informationen finden Sie unter [Setup und Installation](https://msdn.microsoft.com/library/mt613162.aspx) für Visual Studio und Xamarin sowie unter [Setup, Installation und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

## <a name="create-a-new-mobile-apps-back-end"></a>Erstellen eines neuen Mobile Apps-Back-Ends

Gehen Sie zum Erstellen eines neuen Mobile Apps-Back-Ends wie folgt vor:

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie nun ein Mobile Apps-Back-End eingerichtet, das Ihre mobilen Clientanwendungen verwenden können. Im nächsten Schritt laden Sie ein Serverprojekt für ein einfaches Aufgabenlisten-Back-End herunter und veröffentlichen es anschließend in Azure.

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts

Gehen Sie wie folgt vor, um das Serverprojekt für die Verwendung des Node.js- oder .NET-Back-Ends zu konfigurieren:

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Herunterladen und Ausführen der Xamarin.Forms-Projektmappe

Die Projektmappe kann auf zwei Arten heruntergeladen werden. Sie können die Projektmappe entweder auf einen Mac herunterladen und in Xamarin Studio öffnen oder sie auf einen Windows-Computer herunterladen und in Visual Studio öffnen. Verwenden Sie in letzterem Fall einen vernetzten Mac zum Erstellen der iOS-App. Weitere Informationen finden Sie auf der Seite [Setup und Installation](https://msdn.microsoft.com/library/mt613162.aspx) für Visual Studio und Xamarin.

Gehen Sie auf einem Mac oder Windows-Computer wie folgt vor:

1. Öffnen Sie das [Azure-Portal].

2. Wählen Sie auf dem Blatt **Einstellungen** für Ihre mobile App unter **Mobil** Folgendes aus: **Erste Schritte** > **Xamarin.Forms**. Wählen Sie unter **Schritt 3** die Option **Neue App erstellen** und anschließend **Herunterladen** aus.

   Dadurch wird ein Projekt heruntergeladen, das eine mit Ihrer mobilen App verbundene Clientanwendung enthält. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.

3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Xamarin Studio (Mac) oder Visual Studio (Windows).

   ![Extrahiertes Projekt in Xamarin Studio][9]

   ![Extrahiertes Projekt in Visual Studio][8]

## <a name="optional-run-the-ios-project"></a>(Optional) Ausführen des iOS-Projekts
In diesem Abschnitt führen Sie das Xamarin iOS-Projekt für iOS-Geräte aus. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Ausführen** die Option **Debuggen starten** aus, um das Projekt zu erstellen und die App im iPhone-Emulator zu starten.

#### <a name="in-visual-studio"></a>In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem iOS-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**.

4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im iPhone-Emulator zu starten.

   > [!NOTE]
   > Sollten beim Erstellen des Projekts Probleme auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die neueste Version der Xamarin-Supportpakete. Bei Schnellstartprojekten dauert die Aktualisierung auf die neuesten Versionen unter Umständen etwas länger.    
   >
   >

5. Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

    ![][10]

    Dadurch wird eine post-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

    > [!NOTE]
    > Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
    >
    >

## <a name="optional-run-the-android-project"></a>(Optional) Ausführen des Android-Projekts
In diesem Abschnitt führen Sie das Xamarin-Droid-Projekt für Android aus. Wenn Sie nicht mit Android-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Ausführen** die Option **Debuggen starten** aus, um das Projekt zu erstellen und die App im Android-Emulator zu starten.

#### <a name="in-visual-studio"></a>In Visual Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt (Droid), und wählen Sie **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem Android-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**.

4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im Android-Emulator zu starten.

   > [!NOTE]
   > Sollten beim Erstellen des Projekts Probleme auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die neueste Version der Xamarin-Supportpakete. Bei Schnellstartprojekten dauert die Aktualisierung auf die neuesten Versionen unter Umständen etwas länger.    
   >
   >

5. Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

    ![][11]
    
    Dadurch wird eine post-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.
    
    > [!NOTE]
    > Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
    >
    >

## <a name="optional-run-the-windows-project"></a>(Optional) Ausführen des Windows-Projekts

In diesem Abschnitt führen Sie das Xamarin-WinApp-Projekt für Windows-Geräte aus. Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-visual-studio"></a>In Visual Studio

1. Klicken Sie mit der rechten Maustaste auf eines der Windows-Projekte, und wählen Sie die Option **Als Startprojekt festlegen** aus.

2. Wählen Sie im Menü **Build** die Option **Konfigurations-Manager** aus.

3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** neben dem gewählten Windows-Projekt die Kontrollkästchen **Erstellen** und **Bereitstellen**.

4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im Windows-Emulator zu starten.

   > [!NOTE]
   > Sollten beim Erstellen des Projekts Probleme auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die neueste Version der Xamarin-Supportpakete. Bei Schnellstartprojekten dauert die Aktualisierung auf die neuesten Versionen unter Umständen etwas länger.    
   >
   >

5. Geben Sie in der App einen aussagekräftigen Text ein (beispielsweise *Xamarin kennenlernen*), und wählen Sie anschließend das Pluszeichen (**+**) aus.

    Dadurch wird eine POST-Anforderung an das neue, in Azure gehostete Mobile Apps-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom Mobile Apps-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.
    
    ![][12]
    
    > [!NOTE]
    > Den Code, der auf Ihr Mobile Apps-Back-End zugreift, finden Sie in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
    >
    >

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen von Authentifizierung zur App](app-service-mobile-xamarin-forms-get-started-users.md)  
  Enthält Informationen über die Authentifizierung von Benutzern der App mit einem Identitätsanbieter.

* [Hinzufügen von Pushbenachrichtigungen zur App](app-service-mobile-xamarin-forms-get-started-push.md)  
  Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile Apps-Back-End für die Verwendung von Azure Notification Hubs zum Senden der Pushbenachrichtigungen konfigurieren.

* [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Informieren Sie sich, wie Sie die Offlineunterstützung für Ihre App mit einem Mobile Apps-Back-End hinzufügen. Mit der Offlinesynchronisierung können Sie die Daten Ihrer mobilen App auch ohne Netzwerkverbindung anzeigen, hinzufügen oder ändern.

* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
  Erfahren Sie, wie Sie das SDK für verwaltete Clients in Ihrer Xamarin-App verwenden.

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/

