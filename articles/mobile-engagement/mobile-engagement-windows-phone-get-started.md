---
title: "Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-Apps"
description: "Erfahren Sie, wie Sie Azure Mobile Engagement mit Analysen und Pushbenachrichtigungen für Windows Phone Silverlight-Apps verwenden."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: d2334a59d83c90bdd02c4fa29261d36aad292892
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Erste Schritte mit Azure Mobile Engagement für Windows Phone Silverlight-Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Sie mit Azure Mobile Engagement Ihre App-Nutzung nachvollziehen und Pushbenachrichtigungen an nach Segmenten eingeteilte Benutzer einer Windows Phone Silverlight-Anwendung senden können.
In diesem Tutorial wird ein einfaches Übertragungsszenario mit Mobile Engagement dargestellt. Sie erstellen eine leere Windows Phone Silverlight-App, die grundlegende Daten erfasst und Pushbenachrichtigungen über den Microsoft-Pushbenachrichtigungsdienst (MPNS) empfängt.

> [!NOTE]
> Der Azure Mobile Engagement-Dienst wird im März 2018 eingestellt und ist zurzeit nur für Bestandskunden verfügbar. Weitere Informationen finden Sie im Artikel zu [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

> [!NOTE]
> Projekte mit Windows Phone 8.1 und älteren Versionen werden in Visual Studio 2017 nicht unterstützt.  Weitere Informationen finden Sie unter [Visual Studio 2017 – Zielplattformen und Kompatibilität](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Informationen zu Windows Phone 8.1 (nicht Silverlight) finden Sie im [Windows Universal-Lernprogramm](mobile-engagement-windows-store-dotnet-get-started.md).
> 
> 

Für dieses Lernprogramm ist Folgendes erforderlich:

* Visual Studio 2013
* [MicrosoftAzure.MobileEngagement] 

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).
> 
> 

## <a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre Windows Phone-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Lernprogramm wird eine "einfache Integration" dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung. Die vollständige Integrationsdokumentation finden Sie im Artikel zur [Integration des Mobile Engagement Windows Phone SDK](mobile-engagement-windows-phone-sdk-overview.md).

Wir erstellen eine einfache App mit Visual Studio, um die Integration zu veranschaulichen.

### <a name="create-a-new-windows-phone-silverlight-project"></a>Erstellen eines neuen Windows Phone Silverlight-Projekts
In den folgenden Schritten wird die Verwendung von Visual Studio 2015 angenommen, obwohl die Schritte in früheren Versionen von Visual Studio ähnlich sind. 

1. Starten Sie Visual Studio, und wählen Sie auf dem **Startbildschirm** die Option **Neues Projekt** aus.
2. Wählen Sie im Popupfenster **Windows 8** -> **Windows Phone** -> **Leere App (Windows Phone Silverlight)** aus. Geben Sie den **Namen** der App und den **Lösungsnamen** ein, und klicken Sie dann auf **OK**.
   
    ![][1]
3. Sie können entweder **Windows Phone 8.0** oder **Windows Phone 8.1** als Ziel auswählen.

Sie haben nun eine neue Windows Phone Silverlight-App erstellt, in die das Azure Mobile Engagement-SDK integriert wird.

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
1. Installieren Sie das NuGet-Paket [MicrosoftAzure.MobileEngagement] im Projekt.
2. Öffnen Sie `WMAppManifest.xml` (im Ordner "Eigenschaften"), und stellen Sie sicher, dass folgende Deklarationen im Tag `<Capabilities />` vorliegen (fügen Sie sie hinzu, falls dies nicht der Fall ist):
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. Nun fügen Sie die zuvor für Ihre Mobile Engagement-App kopierte Verbindungszeichenfolge in die Datei `Resources\EngagementConfiguration.xml` zwischen den Tags `<connectionString>` und `</connectionString>` ein:
   
    ![][3]
4. In der Datei `App.xaml.cs`:
   
    a. Fügen Sie die Anweisung `using` hinzu:
   
            using Microsoft.Azure.Engagement;
   
    b. Initialisieren Sie das SDK in der Methode `Application_Launching`:
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    c. Fügen Sie in `Application_Activated` Folgendes ein:
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <a id="monitor"></a>Aktivieren der Überwachung in Echtzeit
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) an das Mobile Engagement-Back-End schicken.

1. Fügen Sie in "MainPage.Xaml.cs" die `using`-Anweisung hinzu:
   
        using Microsoft.Azure.Engagement;
2. Ersetzen Sie die Basisklasse **MainPage**, die sich vor **PhoneApplicationPage** befindet, durch **EngagementPage**.
   
        class MainPage : EngagementPage 
3. In der Datei `MainPage.xml`:
   
    a. Fügen Sie Ihre Namespace-Deklarationen hinzu:
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    b. Ersetzen Sie `phone:PhoneApplicationPage` im XML-Tagnamen durch `engagement:EngagementPage`.

## <a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern interagieren und diese mit Push-Benachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen erreichen. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre App für den Empfang ein.

### <a name="enable-your-app-to-receive-mpns-push-notifications"></a>Aktivieren des Empfangs von MPNS-Pushbenachrichtigungen in Ihrer App
Fügen Sie Ihrer Datei `WMAppManifest.xml` unter "Capabilities" neue Funktionen hinzu:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-the-reach-sdk"></a>Initialisieren des REACH-SDK
1. Rufen Sie `EngagementReach.Instance.Init();` in `App.xaml.cs` gleich nach der Agent-Initialisierung in der Funktion **Application_Launching** auf:
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. Rufen Sie `EngagementReach.Instance.OnActivated(e);` in `App.xaml.cs` gleich nach der Agent-Initialisierung in der Funktion **Application_Activated** auf:
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Sie sind fertig, und wir werden jetzt überprüfen, ob Sie diese einfache Integration ordnungsgemäß durchgeführt haben.

## <a id="send"></a>Versenden von Benachrichtigungen an die App
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Jetzt sollte auf Ihrem Gerät eine Benachrichtigung als In-App-Benachrichtigung angezeigt werden, wenn die App geöffnet ist, und andernfalls als Popupbenachrichtigung, wie unten dargestellt: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png

