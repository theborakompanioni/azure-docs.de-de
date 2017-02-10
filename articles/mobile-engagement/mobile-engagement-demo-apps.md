---
title: Demo-App zu Azure Mobile Engagement | Microsoft Docs
description: "Beschreibt, wo Sie die Azure Mobile Engagement-Demo-App herunterladen und wie Sie sie verwenden und ihre Vorteile nutzen können."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8381edb569e19a85c1259f7791b477cfa6e51ea3


---
# <a name="azure-mobile-engagement-demo-app"></a>Demo-App zu Azure Mobile Engagement
Wir haben eine Demo-App zu Azure Mobile Engagement für die Plattformen **iOS**, **Android** und **Windows** veröffentlicht, mit deren Hilfe Sie nützliche Ressourcen finden und mehr über Mobile Engagement erfahren können.

Die App bietet Ihnen Folgendes:

* Nützliche Links zu Mobile Engagement-Ressourcen, z.B. Videos, Dokumentation, Supportforum und die Möglichkeit zum Anfordern von Features.
* Beispielbenachrichtigungen, die von Mobile Engagement unterstützt werden, damit Sie Ideen für Ihre eigenen mobilen Anwendungen erhalten.
* Nutzung einer Referenzimplementierung, mit der Sie erlernen können, wie Mobile Engagement in Ihrer eigenen App implementiert wird. Sie können Folgendes lernen:
  
  * Sammeln von Analysedaten
  * Implementieren erweiterter Benachrichtigungsszenarien, z.B. *Vollbildinterstitial* oder *Popup*.
  * Implementieren von Umfragen
  * Implementieren von automatischen Push-/Datenpushszenarien   

## <a name="app-installation"></a>App-Installation
Diese App ist in den folgenden App Stores verfügbar:

* **Universelle Windows-Demo-App**:
  
  * Laden Sie die App aus dem [Windows App Store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2) herunter.
  * Die App wurde als universelle Windows 10-App entwickelt. Der Quellcode steht auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows)zur Verfügung.
* **iOS-Demo-App**:
  
  * Laden Sie die App aus dem [Apple Store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090)herunter.
  * Die App wurde mit iOS Swift entwickelt. Der Quellcode steht auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios)zur Verfügung.
* **Android-Demo-App**:
  
  * Laden Sie die App aus dem [Google Play Store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement)herunter.
  * Der Quellcode steht auf [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android)zur Verfügung.

![Universelle Windows-Demo-App][1]

![iOS-Demo-App][2]
![Android-Demo-App][3]

## <a name="usage"></a>Verwendung
Sie können diese App wie folgt nutzen:

**Laden Sie die App über die (oben angegebenen) App Store-Links auf Ihr Gerät herunter:**

> [!IMPORTANT]
> Sie benötigen kein Azure-Konto und müssen für die App auch keine Verbindung mit dem Back-End herstellen. Die App funktioniert eigenständig.
> 
> 

* Nachdem die App auf Ihrem Gerät verfügbar ist, können Sie über die Links im Menü auf der linken Seite auf die nützlichen Ressourcen zu Mobile Engagement zugreifen.
* Wir haben dieser Anwendung den [RSS-Feed des Diensts](https://aka.ms/azmerssfeed) hinzugefügt, damit Sie in Bezug auf die aktuellen Produktupdates immer auf dem neuesten Stand sind.
* Sie können auch die Beispielszenarien für Benachrichtigungen durchgehen, um zu erfahren, welche Art von Benachrichtigungen von Mobile Engagement für die einzelnen Plattformen unterstützt werden. Diese Benachrichtigungen können lokal ausprobiert werden. Dies bedeutet, dass Sie auf dem Bildschirm auf die Schaltflächen klicken können, um Benachrichtigungen anzuzeigen. Dies entspricht dem Senden der Benachrichtigungen über die Mobile Engagement-Plattform.

![App-Menü für Windows][4]

![App-Menü für iOS][5]
![App-Menü für Android][6]

**Laden Sie den Quellcode über die (oben angegebenen) GitHub-Links herunter:**

* Nachdem Sie den Quellcode heruntergeladen haben, öffnen Sie ihn in der entsprechenden Entwicklungsumgebung, also XCode für iOS, Android Studio für Android und Visual Studio für Windows.
* Als Nächstes sollten Sie unsere [grundlegenden Schritte zur SDK-Integration](mobile-engagement-windows-store-dotnet-get-started.md) befolgen, um diese App mit einer eigenen Mobile Engagement-Back-End-Instanz verbinden zu können.
  * In der App muss eine Verbindungszeichenfolge konfiguriert werden.
  * Außerdem müssen Sie die Plattform für Pushbenachrichtigungen für Ihre App konfigurieren.
* Sie werden merken, dass die eigentliche App mit Mobile Engagement instrumentiert wurde. Wenn Sie die App nach dem Herstellen der Verbindung mit dem Back-End öffnen, werden die Benutzersitzung, Aktivitäten, Ereignisse usw. auf der Registerkarte **Überwachen** angezeigt.
* Zudem können Sie auch Benachrichtigungen von Ihrer eigenen Mobile Engagement-Instanz aus an diese App senden, anstatt lokale Benachrichtigungen zu verwenden.
  
  * Hier können Sie Ihr Gerät als Testgerät hinzufügen, indem Sie in der App das Menüelement **Get the Device ID** (Geräte-ID abrufen) verwenden. Sie erhalten eine Geräte-ID, die Sie dann bei der Back-End-Instanz der Plattform als Testgerät registrieren.
    
    ![Geräte-ID unter Windows][7]
    
    ![Geräte-ID unter iOS][8]
    ![Geräte-ID unter Android][9]

## <a name="key-features-of-the-demo-app"></a>Hauptfunktionen der Demo-App
* Wie bereits erwähnt, stehen Ihnen mit dieser App alle wichtigen Ressourcen für Mobile Engagement zur Verfügung. Im Menü auf der linken Seite können Sie die Links durchgehen.
* Sie können für jede Plattform Benachrichtigungen außerhalb der App erhalten. Diese Benachrichtigungen können als **reine Benachrichtigung** übermittelt werden, sodass beim Klicken auf die Benachrichtigung einfach ein nativer Bildschirm der Anwendung geöffnet wird (mithilfe von **Deep Linking**). Alternativ dazu können Benachrichtigungen als **Webankündigung** übermittelt werden, sodass Sie zusätzliche HTML-Inhalte aus dem Mobile Engagement-Back-End bereitstellen können, die beim Klicken auf die Benachrichtigung angezeigt werden.
  
    ![Out-of-App-Benachrichtigungen][29]
* Unter iOS müssen Sie die App schließen, um Pushbenachrichtigungen außerhalb der App oder Systempushbenachrichtigungen sehen zu können. Sehen Sie sich hier die Implementierung für das Hinzufügen von **Aktionsschaltflächen** an – in dieser Benachrichtigung außerhalb der App beispielsweise für *Feedback* und *Freigabe* –, mit denen der Benutzer direkt von der Benachrichtigung aus eine Aktion durchführen kann.
  
    ![Out-of-App-Benachrichtigungen unter iOS][11] ![Anzeige von Out-of-App-Benachrichtigungen unter iOS][14]
* Unter Android werden die Optionen zum Hinzufügen von mehrzeiligem Text (**Big Text**) oder eines Benachrichtigungsbilds (**Big Picture**) zur Benachrichtigung sowie **Aktionsschaltflächen** (wie auch unter iOS) unterstützt.
  
    ![Out-of-App-Benachrichtigungen unter Android][12] ![Anzeige von Out-of-App-Benachrichtigungen unter Android][15]
* Unter Windows 10 können Sie anzeigen, wie die Benachrichtigungen auf dem PC aussehen. Diese Benachrichtigung wird auch im **Benachrichtigungs-Center**von Windows 10 angezeigt. Es ist momentan keine Unterstützung für das Hinzufügen von **Aktionsschaltflächen** im Windows SDK vorhanden.
  
    ![Out-of-App-Benachrichtigungen unter Windows][10] ![Out-of-App-Anzeige unter Windows][13]
* Sie können sich für jede Plattform In-App-Standardbenachrichtigungen ansehen. Dieser Vorgang besteht aus zwei Schritten, bei dem zuerst das Fenster **Benachrichtigung** angezeigt wird. Wenn Sie darauf klicken, wird eine **Ankündigung** als Vollbild geöffnet. Dies ist im folgenden Screenshot dargestellt. Der Inhalt dieser Ankündigung stammt aus Ihrer Mobile Engagement-Back-End-Instanz. Das SDK enthält Vorlagen für Benachrichtigungen und Ankündigungen. Diese können Sie wie in dieser Demo-App leicht anpassen, indem Sie ein Logo und Farben hinzufügen.  
  
    ![In-App-Benachrichtigungen unter Windows][16]
  
    ![In-App-Benachrichtigungen unter iOS][17]  ![In-App-Benachrichtigungen unter Android][18]
  
    **iOS**, **Android**
* Sie können diese Standardeinstellung auch über das Feature **Kategorie** von Mobile Engagement anpassen. In der Demo-App haben wir zwei gängige Möglichkeiten zum Verändern der Oberfläche für Benachrichtigungen gezeigt. Beachten Sie, dass das Feature „Kategorie“ im Windows SDK noch nicht unterstützt wird.
  
    **Vollbildinterstitial:**
  
    ![In-App-Benachrichtigung – Kategorie „Interstitial“][30]
  
    ![Kategorie „Interstitial“ unter iOS][21]     ![Kategorie „Interstitial“ unter Android][22]
  
    **Popupbenachrichtigung:**
  
    ![In-App-Benachrichtigung – Kategorie „Popup“][31]
  
    ![Popupbenachrichtigung unter iOS][19]    ![Popupbenachrichtigung unter Android][20]

**iOS**, **Android**

* Mobile Engagement unterstützt auch einen speziellen Typ von In-App-Benachrichtigung mit der Bezeichnung **Umfragen**. Dies ermöglicht Ihnen das Senden von schnellen Umfragen an Ihre segmentierten App-Benutzer. Sie können jeder Frage wie im folgenden Screenshot dargestellt Fragen und Optionen hinzufügen. Diese werden dem App-Benutzer dann als In-App-Benachrichtigung angezeigt.   
  
    ![Umfragebenachrichtigungen][32]
  
    ![Umfrage unter Windows][26]
  
    ![Umfrage unter iOS][27]   ![Umfrage unter Android][28]

**iOS**, **Android**

* Mobile Engagement unterstützt auch das Senden von automatischen Benachrichtigungen vom Typ **Datenpush**. Mit diesen Benachrichtigungen können Sie Daten aus Ihrem Dienst senden (z.B. die JSON-Daten aus dem folgenden Beispiel), die Sie dann in Ihrer App verarbeiten und für die Sie Aktionen durchführen können. Ein Beispiel hierfür ist das selektive Ändern des Preises eines Elements mit Datenpushbenachrichtigungen.
  
    ![Datenpushbenachrichtigung][33]
  
    ![Datenpushbenachrichtigung unter Windows][23]
  
    ![Datenpushbenachrichtigung unter iOS][24]  ![Datenpushbenachrichtigung unter Android][25]

**iOS**, **Android**

> [!NOTE]
> Sie können für all diese Benachrichtigungen detaillierte Anweisungen anzeigen, indem Sie in einem beliebigen Bildschirm einer Beispielbenachrichtigung auf **Hier klicken, um eine Anleitung zum Senden dieser Benachrichtigungen von der Mobile Engagement-Plattform zu erhalten** klicken.
> 
> 

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



<!--HONumber=Nov16_HO3-->


