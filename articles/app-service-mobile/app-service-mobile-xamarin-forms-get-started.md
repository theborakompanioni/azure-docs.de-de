<properties
	pageTitle="Erste Schritte mit Mobile Apps unter Verwendung von Xamarin.Forms"
	description="Befolgen Sie dieses Lernprogramm für die ersten Schritte bei der Verwendung von Azure Mobile Apps für die Xamarin.Forms-Entwicklung."
	services="app-service\mobile"
	documentationCenter="xamarin"
	authors="adrianhall"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-xamarin"
	ms.devlang="dotnet"
	ms.topic="hero-article"
	ms.date="10/01/2016"
	ms.author="adrianha"/>

#Erstellen einer Xamarin.Forms-App

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

##Übersicht

Dieses Lernprogramm zeigt Ihnen, wie Sie einen cloudbasierten Back-End-Dienst mithilfe eines mobilen Azure-App-Back-Ends zu einer mobilen Xamarin.Forms-App hinzufügen. Sie erstellen sowohl ein neues mobiles App-Back-End und eine einfache Xamarin.Forms-_Todo list_-App, die App-Daten in Azure speichert.

Das Abschließen dieses Lernprogramms ist eine Voraussetzung für alle anderen Mobile Apps-Lernprogramme für Xamarin.Forms.

##Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Falls Sie kein Konto besitzen, können Sie sich für eine Azure-Testversion registrieren. So erhalten Sie bis zu 10 kostenlose mobile Apps, die Sie auch nach Ablauf der Testversion weiter nutzen können. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

* Visual Studio mit Xamarin. Anweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).

* Ein Mac, auf dem Xcode Version 7.0 oder höher und Xamarin Studio Community installiert ist. Informationen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) und [Setup, Installation und Überprüfungen für Mac-Benutzer](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).
 
>[AZURE.NOTE] Wenn Sie Azure App Service ausprobieren möchten, ehe Sie sich für ein Azure-Konto anmelden, können Sie unter [App Service testen](https://tryappservice.azure.com/?appServiceName=mobile) sofort kostenlos eine kurzlebige mobile Starter-App in App Service erstellen. Keine Kreditkarte erforderlich, keine Verpflichtungen.

## Erstellen eines neuen Azure Mobile App-Back-Ends

Führen Sie die folgenden Schritte aus, um ein neues mobiles App-Back-End zu erstellen.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]


Sie haben nun ein Azure-Back-End für mobile Apps bereitgestellt, das von Ihren mobilen Clientanwendungen verwendet werden kann. Als Nächstes laden Sie ein Serverprojekt für ein einfaches "Aufgabenlisten"-Back-End herunter und veröffentlichen es in Azure.

## Konfigurieren des Serverprojekts

Befolgen Sie die nachstehenden Anweisungen zum Konfigurieren des Serverprojekts für die Verwendung des Node.js- oder .NET-Back-Ends.

[AZURE.INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

##Herunterladen und Ausführen der Xamarin.Forms-Lösung

Hier stehen Ihnen einige Optionen zur Verfügung. Sie können die Lösung auf einem Mac herunterladen und in Xamarin Studio öffnen, oder Sie können die Lösung auf einem Windows-Computer herunterladen und in Visual Studio öffnen. Verwenden Sie in letzterem Fall einen vernetzten Mac zum Erstellen der iOS-App. Falls Sie ausführlichere Informationen zu den Xamarin-Setup-Szenarien benötigen, lesen Sie [Setup und Installation](https://msdn.microsoft.com/library/mt613162.aspx).

Fahren wir fort:

 1. Öffnen Sie auf Ihrem Mac oder Ihrem Windows-Computer das [Azure-Portal] in einem Browserfenster.
 2. Klicken Sie auf dem Blatt „Einstellungen“ für Ihre mobile App auf **Erste Schritte** (unter „Mobil“) > **Xamarin.Forms**. Klicken Sie unter Schritt 3 auf **Neue App erstellen**, falls noch nicht ausgewählt. Klicken Sie dann auf die Schaltfläche **Herunterladen**.

    Dadurch wird ein Projekt heruntergeladen, das eine Clientanwendung enthält, die mit der mobilen App verbunden ist. Speichern Sie die komprimierte Projektdatei auf dem lokalen Computer und merken Sie sich, wo Sie sie gespeichert haben.

 3. Extrahieren Sie das heruntergeladene Projekt, und öffnen Sie es dann in Xamarin Studio oder Visual Studio.

	![][9]

	![][8]


##(Optional) Ausführen des iOS-Projekts

Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin iOS-Projekts für iOS-Geräte. Wenn Sie nicht mit iOS-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####In Xamarin Studio

1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um das Projekt zu erstellen und die App im iPhone-Simulator zu starten.

####In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf das iOS-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
3. Aktivieren Sie im Dialogfeld **Konfigurationsmanager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des iOS-Projekts.
4. Drücken Sie **F5**, um das Projekt zu erstellen und die App im iPhone-Simulator zu starten.

	>[AZURE.NOTE] Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.

Geben Sie in der App sinnvollen Text ein, z. B. _Xamarin kennenlernen_, und klicken Sie dann auf die Schaltfläche **+**.

![][10]

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

>[AZURE.NOTE]
Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.

##(Optional) Ausführen des Android-Projekts

Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin Android-Projekts für Android. Wenn Sie nicht mit Android-Geräten arbeiten, können Sie diesen Abschnitt überspringen.

####In Xamarin Studio

1. Klicken Sie mit der rechten Maustaste auf das Android-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Klicken Sie im Menü **Ausführen** auf **Debuggen starten**, um das Projekt zu erstellen und die App im Android-Emulator zu starten.

####In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf das Android (Droid)-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
4. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
5. Aktivieren Sie im Dialogfeld **Konfigurationsmanager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des Android-Projekts.
6. Drücken Sie **F5**, um das Projekt zu erstellen und die App in einem Android-Emulator zu starten.

	>[AZURE.NOTE] Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.


Geben Sie in der App sinnvollen Text ein, z. B. _Xamarin kennenlernen_, und klicken Sie dann auf die Schaltfläche **+**.

![][11]

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

> [AZURE.NOTE]
Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.


##(Optional) Ausführen des Windows-Projekts


Dieser Abschnitt bezieht sich auf das Ausführen des Xamarin WinApp-Projekts für Windows-Geräte. Wenn Sie nicht mit Windows-Geräten arbeiten, können Sie diesen Abschnitt überspringen.


####In Visual Studio
1. Klicken Sie mit der rechten Maustaste auf eines der Windows-Projekte, und klicken Sie dann auf **Als Startprojekt festlegen**.
4. Klicken Sie im Menü **Build** auf **Konfigurations-Manager**.
5. Aktivieren Sie im Dialogfeld **Konfigurationsmanager** die Kontrollkästchen **Erstellen** und **Bereitstellen** des ausgewählten Windows-Projekts.
6. Drücken Sie **F5**, um das Projekt zu erstellen und die App im Windows-Emulator zu starten.

	>[AZURE.NOTE] Falls Probleme bei der Erstellung auftreten, führen Sie den NuGet-Paket-Manager aus, und aktualisieren Sie auf die aktuelle Version der Xamarin-Supportpakete. Manchmal werden die Schnellstartprojekte erst mit einer gewissen Verzögerung auf den neuesten Stand gebracht.


Geben Sie in der App sinnvollen Text ein, z. B. _Xamarin kennenlernen_, und klicken Sie dann auf die Schaltfläche **+**.

Dadurch wird eine POST-Anforderung an das neue in Azure gehostete mobile App-Back-End gesendet. Daten von der Anforderung werden in die TodoItem-Tabelle eingefügt. In der Tabelle gespeicherte Einträge werden vom mobilen App-Back-End zurückgegeben, und die Daten werden in der Liste angezeigt.

![][12]

> [AZURE.NOTE]
Sie finden den Code, der auf Ihr mobiles App-Back-End zugreift, in der C#-Datei „TodoItemManager.cs“ des Portable Class Library-Projekts Ihrer Projektmappe.

##Nächste Schritte

* [Hinzufügen von Authentifizierung zur App](app-service-mobile-xamarin-forms-get-started-users.md) Hier erhalten Sie Informationen über die Authentifizierung von Benutzern der App mit einem Identitätsanbieter.

* [Hinzufügen von Pushbenachrichtigungen zur App](app-service-mobile-xamarin-forms-get-started-push.md) Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile App-Back-End für die Verwendung von Azure Notification Hubs zum Senden von Pushbenachrichtigungen konfigurieren.

* [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-xamarin-forms-get-started-offline-data.md) Erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist.

* [Verwenden des verwalteten Clients für Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md) Erfahren Sie, wie Sie das SDK für verwaltete Clients in Ihrer Xamarin-App verwenden.


<!-- Anchors. -->
[Getting started with mobile app backends]: #getting-started
[Create a new mobile app backend]: #create-new-service
[Next Steps]: #next-steps


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

<!---HONumber=AcomDC_1005_2016-->