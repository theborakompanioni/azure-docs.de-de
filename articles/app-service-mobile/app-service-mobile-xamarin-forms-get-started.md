---
title: Erste Schritte mit Mobile Apps unter Verwendung von Xamarin.Forms
description: "Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Mobile Apps für die Xamarin.Forms-Entwicklung."
services: app-service\mobile
documentationcenter: xamarin
author: adrianhall
manager: erikre
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: adrianha
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6a48181c6c22e6042dd8ef39c12415979e384f8b


---
# <a name="create-a-xamarinforms-app"></a>Erstellen einer Xamarin.Forms-App
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Übersicht
Dieses Lernprogramm zeigt Ihnen, wie Sie einen cloudbasierten Back-End-Dienst mithilfe eines mobilen Azure-App-Back-Ends zu einer mobilen Xamarin.Forms-App hinzufügen. Sie erstellen sowohl ein neues mobiles App-Back-End als auch eine einfache Xamarin.Forms-App des Typs *Aufgabenliste*, die App-Daten in Azure speichert.

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin.Forms.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto besitzen, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/pricing/free-trial/).
* Visual Studio mit Xamarin. Anweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) . 
* Ein Mac, auf dem Xcode Version 7.0 oder höher und Xamarin Studio Community installiert ist. Informationen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) und [Setup, Installation und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).

> [!NOTE]
> Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://tryappservice.azure.com/?appServiceName=mobile)sofort kostenlos eine kurzlebige mobile Starter-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.
> 
> 

## <a name="create-a-new-azure-mobile-app-backend"></a>Erstellen eines neuen Azure Mobile App-Back-Ends
Führen Sie die folgenden Schritte aus, um ein neues mobiles App-Back-End zu erstellen.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Sie haben nun ein Azure-Back-End für mobile Apps bereitgestellt, das von Ihren mobilen Clientanwendungen verwendet werden kann. Als Nächstes laden Sie ein Serverprojekt für ein einfaches "Aufgabenlisten"-Back-End herunter und veröffentlichen es in Azure.

## <a name="configure-the-server-project"></a>Konfigurieren des Serverprojekts
Befolgen Sie die nachstehenden Anweisungen zum Konfigurieren des Serverprojekts für die Verwendung des Node.js- oder .NET-Back-Ends.

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-xamarinforms-solution"></a>Herunterladen und Ausführen der Xamarin.Forms-Lösung
Hier stehen Ihnen einige Optionen zur Verfügung. Sie können die Lösung auf einem Mac herunterladen und in Xamarin Studio öffnen, oder Sie können die Lösung auf einem Windows-Computer herunterladen und in Visual Studio öffnen. Verwenden Sie in letzterem Fall einen vernetzten Mac zum Erstellen der iOS-App. Falls Sie ausführlichere Informationen zu den Xamarin-Setup-Szenarien benötigen, lesen Sie [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

Fahren wir fort:

1. Öffnen Sie auf Ihrem Mac oder Ihrem Windows-Computer das [Azure-Portal] in einem Browserfenster.
2. Klicken Sie auf dem Blatt „Einstellungen“ für Ihre mobile App auf **Erste Schritte** (unter „Mobil“) > **Xamarin.Forms**. Klicken Sie unter Schritt 3 auf **Neue App erstellen** , falls noch nicht ausgewählt.  Klicken Sie dann auf die Schaltfläche **Herunterladen** .
   
   Dadurch wird ein Projekt heruntergeladen, das eine Clientanwendung enthält, die mit der mobilen App verbunden ist. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.
3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Xamarin Studio oder Visual Studio.
   
   ![][9]
   
   ![][8]

## <a name="optional-run-the-ios-project"></a>(Optional) Ausführen des iOS-Projekts
Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin iOS-Projekts für iOS-Geräte. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um das Projekt zu erstellen und die App im iPhone-Simulator zu starten.

#### <a name="in-visual-studio"></a>In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des iOS-Projekts.
4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im iPhone-Simulator zu starten.
   
   > [!NOTE]
   > Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.    
   > 
   > 

Geben Sie in der App einen sinnvollen Text ein, z.B. *Xamarin kennenlernen*, und klicken Sie dann auf die Schaltfläche **+**.

![][10]

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [!NOTE]
> Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
> 
> 

## <a name="optional-run-the-android-project"></a>(Optional) Ausführen des Android-Projekts
Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin Android-Projekts für Android. Wenn Sie nicht mit Android-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-xamarin-studio"></a>In Xamarin Studio
1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um das Projekt zu erstellen und die App im Android-Emulator zu starten.

#### <a name="in-visual-studio"></a>In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf das Android (Droid)-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Android-Projekts.
4. Drücken Sie **F5** , um das Projekt zu erstellen und die App in einem Android-Emulator zu starten.
   
   > [!NOTE]
   > Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.    
   > 
   > 

Geben Sie in der App einen sinnvollen Text ein, z.B. *Xamarin kennenlernen*, und klicken Sie dann auf die Schaltfläche **+**.

![][11]

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [!NOTE]
> Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
> 
> 

## <a name="optional-run-the-windows-project"></a>(Optional) Ausführen des Windows-Projekts
Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin WinApp-Projekts für Windows-Geräte. Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

#### <a name="in-visual-studio"></a>In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf eines der Windows-Projekte, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
3. Aktivieren Sie im Dialogfeld **Konfigurations-Manager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des ausgewählten Windows-Projekts.
4. Drücken Sie **F5** , um das Projekt zu erstellen und die App im Windows-Emulator zu starten.
   
   > [!NOTE]
   > Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.    
   > 
   > 

Geben Sie in der App einen sinnvollen Text ein, z.B. *Xamarin kennenlernen*, und klicken Sie dann auf die Schaltfläche **+**.

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

![][12]

> [!NOTE]
> Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Hinzufügen von Authentifizierung zur App](app-service-mobile-xamarin-forms-get-started-users.md)  
   Enthält Informationen über die Authentifizierung von Benutzern der App mit einem Identitätsanbieter.
* [Hinzufügen von Pushbenachrichtigungen zur App](app-service-mobile-xamarin-forms-get-started-push.md)  
   Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile App-Back-End für die Verwendung von Azure Notification Hubs zum Senden von Pushbenachrichtigungen konfigurieren.
* [Aktivieren der Offlinesynchronisierung für Ihre Windows-App](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
   Erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (also das Anzeigen, Hinzufügen oder Ändern von Daten), auch wenn keine Netzwerkverbindung besteht.
* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md)  
   Erfahren Sie, wie Sie das SDK für verwaltete Clients in Ihrer Xamarin-App verwenden. 

<!-- Anchors. -->
[Erste Schritte mit Mobile App-Back-Ends]:#getting-started
[Erstellen eines neuen mobilen App-Back-Ends]:#create-new-service
[Next Steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile App-SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-Portal]: https://portal.azure.com/




<!--HONumber=Nov16_HO2-->


