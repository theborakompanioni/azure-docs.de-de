<properties
	pageTitle="Demo-App zu Azure Mobile Engagement"
	description="Beschreibt, wo Sie die Azure Mobile Engagement-Demo-App herunterladen, wie Sie sie verwenden und ihre Vorteile nutzen können."
	services="mobile-engagement"
	documentationCenter="mobile" 
	authors="piyushjo" 
	manager="erikre" 
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/29/2016"
	ms.author="piyushjo" />

# Demo-App zu Azure Mobile Engagement

Wir haben eine Demo-App zu Azure Mobile Engagement für die **iOS**-, **Android**- und **Windows**-Plattformen veröffentlicht, mit deren Hilfe Sie nützliche Ressourcen finden und mehr über Azure Mobile Engagement erfahren können.

Die App bietet Ihnen Folgendes:

1. Nützliche Links zu spezifischen Azure Mobile Engagement-Ressourcen wie Videos, Dokumentation und Supportforum, wo Sie Anfragen zu Features usw. stellen können. 
2. Beispielbenachrichtigungen, die von Azure Mobile Engagement unterstützt werden, damit Sie Ideen für Ihre eigenen mobilen Anwendungen erhalten. 
3. Eine Referenzimplementierung, mit der Sie erlernen können, wie Mobile Engagement zu folgenden Zwecken in Ihrer eigenen App implementiert wird: 

	- Sammeln von Analysedaten 
	- Implementieren fortschrittlichen Benachrichtigungszenarien wie z.B. *Vollbildinterstitial* oder *Popup*
	- Implementieren von Umfragen
	- Implementieren automatischer Push-/Datenpushszenarien   

## App-Installation
Diese Apps sind in den jeweiligen App Stores verfügbar:

1. **Universelle Windows-Demo-App**

	- [Link zum Download aus dem Windows App Store](https://www.microsoft.com/de-DE/store/apps/azure-mobile-engagement/9nblggh4qmh2) 
	- Die App wurde als universelle Windows 10-App entwickelt, und der Quellcode ist auf [Github](https://github.com/Azure/azure-mobile-engagement-app-ios) verfügbar.

2. **iOS-Demo-App**

	- [Link zum Download vom Apple Store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090) 
	- Die App wurde in iOS Swift entwickelt, und der Quellcode ist auf [Github](https://github.com/Azure/azure-mobile-engagement-app-ios) verfügbar.

3. **Android-Demo-App**

	- [Link zum Download vom Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement)
	- Der Quellcode steht auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android) zur Verfügung.

![][1]

![][2] ![][3]


## Verwendung

Sie können diese Apps auf folgende Weise nutzen:

**1) Laden Sie die Apps über die oben angegebenen Links zu den App Stores auf Ihr Gerät herunter.**

>[AZURE.IMPORTANT] Sie benötigen weder ein Azure-Konto, noch müssen Sie die App mit einem Back-End verbinden. Die App funktioniert unabhängig.

- Sobald die App auf Ihrem Gerät verfügbar ist, können Sie über die Links im Menü auf der linken Seite alle nützlichen Ressourcen zu Azure Mobile Engagement finden. 
- Außerdem haben wir dieser Anwendung den [RSS-Feed unseres Diensts](https://aka.ms/azmerssfeed) hinzugefügt, sodass Sie immer über die neuesten Produktupdates informiert werden.
- Sie können auch die Beispielszenarien für Benachrichtigungen durchgehen, um zu erfahren, welche Art von Benachrichtigungen von Azure Mobile Engagement für die einzelnen Plattformen unterstützt werden. Diese Benachrichtigungen können lokal ausprobiert werden, d.h. Sie können auf die Schaltflächen auf dem Bildschirm klicken, um Benachrichtigungen anzuzeigen. Diese sehen genauso aus wie die Benachrichtigungen, die Sie über die Azure Mobile Engagement-Plattform senden. 

![][4]
    
![][5] ![][6]

**2) Laden Sie den Quellcode von den oben aufgeführten Github-Links herunter.**

- Nachdem Sie den Quellcode heruntergeladen haben, öffnen Sie ihn in der entsprechenden Entwicklungsumgebung, d.h. XCode für iOS, Android Studio für Android und Visual Studio für Windows. 
- Sie sollten als Nächstes unsere [grundlegenden Schritte zur SDK-Integration](mobile-engagement-windows-store-dotnet-get-started.md) befolgen, um diese App mit einer eigenen Mobile Engagement-Back-End-Instanz verbinden zu können. 
	- In der App muss eine Verbindungszeichenfolge konfiguriert werden.  
	- Außerdem müssen Sie die Plattform für Pushbenachrichtigungen für Ihre App konfigurieren. 
- Sie werden feststellen, dass diese App selbst mit Azure Mobile Engagement instrumentiert ist. Wenn Sie daher die App nach dem Verbinden mit dem Back-End öffnen, werden Ihnen auf der Registerkarte „Überwachen“ die Benutzersitzung, Aktivitäten, Ereignisse usw. angezeigt. 
- Zudem können Sie auch Benachrichtigungen von Ihrer eigenen Mobile Engagement-Instanz aus an diese App senden, statt lokale Benachrichtigungen zu verwenden. 
	- Hier können Sie Ihr Gerät über die Menüoption **Geräte-ID abrufen** in der App als Testgerät hinzufügen. Sie erhalten eine Geräte-ID, die Sie dann als Testgerät bei der Back-End-Instanz Ihrer Plattform registrieren können. 

	![][7]
	    
	![][8] ![][9]

## Hauptfunktionen der Demo-App

1. Wie bereits erwähnt, stehen Ihnen mit dieser App alle wichtigen Ressourcen für Azure Mobile Engagement bereit. Im Menü auf der linken Seite können Sie die Links durchgehen. 

2. Erleben Sie für jede Plattform Benachrichtigungen außerhalb der App. Diese Benachrichtigungen können als **reine Benachrichtigung** übermittelt werden, sodass beim Klicken auf die Benachrichtigung einfach ein nativer Bildschirm der Anwendung geöffnet wird (mithilfe von **Deep Linking**). Alternativ dazu können Benachrichtigungen als **Webankündigung** übermittelt werden, sodass Sie zusätzliche HTML-Inhalte aus dem Mobile Engagement-Back-End bereitstellen können, die beim Klicken auf die Benachrichtigung angezeigt werden.

	![][29]

	
	- Unter iOS müssen Sie die App schließen, um Pushbenachrichtigungen außerhalb der App oder Systempushbenachrichtigungen sehen zu können. Sehen Sie sich hier die Implementierung für das Hinzufügen **interaktiver Schaltflächen** an, in dieser Benachrichtigung außerhalb der App beispielsweise für *Feedback* und *Freigabe*, mit denen der Benutzer direkt von der Benachrichtigung aus eine Aktion ausführen kann. 
	    
	![][11] ![][14]
	
	
	- Auf Android werden die von Android unterstützten Optionen für das Hinzufügen eines mehrzeiligen Texts (**Großer Text**) oder eines Bilds in der Benachrichtigung (**Großes Bild**) angezeigt, zusammen mit den **interaktiven Schaltflächen**, wie sie auch in iOS unterstützt werden. 
	
	![][12] ![][15]
	
	
	- Unter Windows 10 können Sie anzeigen, wie die Benachrichtigungen auf dem PC aussehen. Diese Benachrichtigung wird auch im **Benachrichtigungs-Center** von Windows 10 angezeigt. Es gibt momentan keine Unterstützung für das Hinzufügen **interaktiver Schaltflächen** im Windows SDK. 
	
	![][10] ![][13]

3. Sehen Sie sich für jede Plattform In-App-Standardbenachrichtigungen an. Dabei handelt es sich um einen zweistufigen Vorgang, bei dem zuerst ein Fenster **Benachrichtigung** angezeigt wird. Nach einem Klick auf dieses Fenster wird eine **Ankündigung** als Vollbild angezeigt, wie unten dargestellt. Der Inhalt dieser Ankündigung stammt aus Ihrer Mobile Engagement-Back-End-Instanz. Das SDK enthält Vorlagen sowohl für Benachrichtigungen als auch für Ankündigungen, die wie in dieser Demo-App ganz einfach durch Hinzufügen eines Logos und einer Farbe angepasst werden können.

	![][16]
	
	![][17] **iOS** ![][18] **Android**

4. Sie können diese Standardeinstellung auch über das Feature **Kategorie** von Azure Mobile Engagement anpassen. In der Demo-App haben wir zwei gängige Möglichkeiten zum Verändern der Oberfläche für Benachrichtigungen gezeigt. Beachten Sie, dass das Feature „Kategorie“ im Windows SDK noch nicht unterstützt wird.

	**Vollbildinterstitial**
	
	![][30]

	![][21] ![][22]

	**Popupbenachrichtigung**

	![][31]
	
	![][19] ![][20]

5. Azure Mobile Engagement unterstützt auch einen speziellen Typ von In-App-Benachrichtigung namens **Umfragen**. Mit diesem Typ können Sie schnelle Umfragen an Ihre segmentierten App-Benutzer senden. Sie können, wie im Folgenden gezeigt, Fragen und Optionen für die einzelnen Fragen hinzufügen, die dem App-Benutzer dann als In-App-Benachrichtigungen angezeigt werden.

	![][32]

	![][26]
	    
	![][27] **iOS** ![][28] **Android**

5. Azure Mobile Engagement unterstützt auch das automatische Senden einer **Datenpush**-Benachrichtigung. Hierbei können Sie einige Daten von Ihrem Dienst senden, wie die JSON-Daten im folgenden Beispiel, die Sie in Ihrer App verarbeiten und anhand derer Sie Aktionen ausführen können. So ändern wir beispielsweise den Preis eines Elements selektiv mit der Datenpushbenachrichtigung.

	![][33]

	![][23]
	    
	![][24] **iOS** ![][25] **Android**

> [AZURE.NOTE] Beachten Sie, dass Sie für all diese Benachrichtigungen detaillierte Anweisungen anzeigen können, indem Sie in einem beliebigen Bildschirm einer Beispielbenachrichtigung auf *Hier klicken, um eine Anleitung zum Senden dieser Benachrichtigungen von der Mobile Engagement-Plattform zu erhalten* klicken.


[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png

<!---HONumber=AcomDC_0504_2016-->