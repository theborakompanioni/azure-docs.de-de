---
title: Integration von Windows Phone Silverlight Engagement-SDK
description: Integrieren von Azure Mobile Engagement in Windows Phone Silverlight-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 67f69a5a92c922bc7357c1e4bcc88f55e16d1255


---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Integration von Windows Phone Silverlight Engagement-SDK
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Dieses Verfahren beschreibt die einfachste Möglichkeit zum Aktivieren der Azure Mobile Engagement-Funktionen zur Analyse und Überwachung in Ihrer Windows Phone Silverlight-Anwendung.

Die folgenden Schritte sind ausreichend, um den Bericht von Protokollen zu aktivieren, die zur Berechnung aller Statistiken zu Benutzern, Sitzungen, Aktivitäten, Abstürzen und technischen Informationen notwendig sind. Der Bericht mit Protokollen, die zum Berechnen weiterer Statistiken wie Ereignissen, Fehlern und Aufträgen erforderlich sind, muss manuell über die Engagement-API ausgeführt werden, da diese Statistiken anwendungsabhängig sind. (Informationen hierzu finden Sie weiter unten unter [Verwenden der erweiterten Mobile Engagement-Tagging-API in Ihrer Windows Phone Silverlight-App](mobile-engagement-windows-phone-use-engagement-api.md)).

## <a name="supported-versions"></a>Unterstützte Versionen
Das Mobile Engagement-SDK für Windows Silverlight kann nur in Anwendungen für folgende Betriebssysteme eingebettet werden:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Informationen zu Windows Phone 8.1 (nicht Silverlight) finden Sie im [Windows Universal-Integrationsverfahren](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-the-mobile-engagement-silverlight-sdk"></a>Installieren Sie das Mobile Engagement Silverlight-SDK
Das Mobile Engagement-SDK für Windows Silverlight steht als NuGet-Paket unter dem Namen *MicrosoftAzure.MobileEngagement*zur Verfügung. Sie können es vom Nuget-Paket-Manager für Visual Studio installieren. 

## <a name="add-the-capabilities"></a>Hinzufügen von Funktionen
Das Engagement-SDK benötigt einige Funktionen des Windows Phone Silverlight-SDK, damit es einwandfrei funktioniert.

Öffnen Sie die Datei `WMAppManifest.xml` und stellen Sie sicher, dass die folgenden Funktionen im Bereich `Capabilities` deklariert werden:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-the-engagement-sdk"></a>Initialisieren des Engagement-SDK
### <a name="engagement-configuration"></a>Engagement-Konfiguration
Die Engagement-Konfiguration erfolgt zentral in der Datei `Resources\EngagementConfiguration.xml` Ihres Projekts.

Bearbeiten Sie diese Datei, um Folgendes anzugeben:

* Die Verbindungszeichenfolge der Anwendung zwischen den Tags `<connectionString>` and `<\connectionString>`.

Wenn Sie sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung des Engagement-Agent aufrufen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird im klassischen Azure-Portal angezeigt.

### <a name="engagement-initialization"></a>Engagement-Initialisierung
Wenn Sie ein neues Projekt erstellen, wird eine Datei `App.xaml.cs` generiert. Diese Klasse erbt von `Application` und enthält viele wichtige Methoden. Sie wird auch zum Initialisieren des Engagement-SDK verwendet.

Ändern Sie `App.xaml.cs`:

* Fügen Sie Ihre Anweisungen `using` hinzu:
  
      using Microsoft.Azure.Engagement;
* Fügen Sie `EngagementAgent.Instance.Init` in die Methode `Application_Launching` ein:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* Fügen Sie `EngagementAgent.Instance.OnActivated` in die Methode `Application_Activated` ein:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> Wir raten dringend davon ab, die Engagement-Initialisierung an einer anderen Stelle der Anwendung hinzuzufügen. Bedenken Sie jedoch, dass die Methode `EngagementAgent.Instance.Init` in einem dedizierten Thread und nicht im UI-Thread ausgeführt wird.
> 
> 

## <a name="basic-reporting"></a>Grundlegende Berichterstellung
### <a name="recommended-method-overload-your-phoneapplicationpage-classes"></a>Empfohlene Methode: Überladen der `PhoneApplicationPage`-Klassen
Um den Bericht über alle Protokolle zu aktivieren, die zum Berechnen von Benutzern, Sitzungen, Aktivitäten, Abstürzen und technischen Statistiken durch Engagement erforderlich sind, können Sie einfach alle Ihre `PhoneApplicationPage`-Unterklassen von den `EngagementPage`-Klassen erben lassen.

Hier ist ein Beispiel für eine Seite Ihrer Anwendung. Das Gleiche gilt für alle Seiten der Anwendung.

#### <a name="c-source-file"></a>C#-Quelldatei
Ändern Sie die Datei Ihrer Seite `.xaml.cs` :

* Fügen Sie Ihre Anweisungen `using` hinzu:
  
      using Microsoft.Azure.Engagement;
* Ersetzen Sie `PhoneApplicationPage` durch `EngagementPage`:

**Ohne Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Mit Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Wenn Ihre Seite von der Methode `OnNavigatedTo` erbt, stellen Sie den Aufruf von `base.OnNavigatedTo(e)` sicher. Andernfalls wird die Aktivität nicht berichtet. Tatsächlich ruft `EngagementPage` `StartActivity` von innerhalb der Methode `OnNavigatedTo` ab.
> 
> 

#### <a name="xaml-file"></a>XAML-Datei
Ändern Sie die Datei Ihrer Seite `.xaml` :

* Fügen Sie Ihren Namespace-Deklarationen Folgendes hinzu:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Ersetzen Sie `phone:PhoneApplicationPage` durch `engagement:EngagementPage`:

**Ohne Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Mit Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-the-default-behavior"></a>Außerkraftsetzen des Standardverhaltens
Standardmäßig wird der Klassenname der Seite als der Name der Aktivität gemeldet, ohne Zusatz. Wenn die Klasse das Suffix „Page“ verwendet, löscht Engagement auch dieses.

Wenn Sie das Standardverhalten für den Namen außer Kraft setzen möchten, fügen Sie dem Code einfach Folgendes hinzu:

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Wenn Sie einige zusätzliche Informationen mit Ihrer Aktivität zusammen melden möchten, können Sie dies Ihrem Code hinzufügen:

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

Diese Methoden werden von innerhalb der Methode `OnNavigatedTo` Ihrer Seite aufgerufen.

### <a name="alternate-method-call-startactivity-manually"></a>Alternative Methode: Rufen Sie `StartActivity()` manuell auf
Wenn Sie Ihre `PhoneApplicationPage`-Klassen nicht überladen möchten oder können, können Sie stattdessen Ihre Aktivitäten durch direktes Aufrufen der `EngagementAgent`-Methoden starten.

Es empfiehlt sich, `StartActivity` innerhalb Ihrer `OnNavigatedTo`-Methode von „PhoneApplicationPage“ aufzurufen.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Stellen Sie sicher, dass Ihre Sitzung ordnungsgemäß beendet wird.
> 
> Das SDK ruft die Methode `EndActivity` automatisch auf, wenn die Anwendung geschlossen wird. Daher wird **dringend** empfohlen, bei jeder Änderung der Benutzeraktivität die Methode `StartActivity` und **niemals** die Methode `EndActivity` aufzurufen. Diese Methode sendet die Nachricht an den Engagement-Server, dass der aktuelle Benutzer die Anwendung verlassen hat, und dies wirkt sich auf alle Anwendungsprotokolle aus.
> 
> 

## <a name="advanced-reporting"></a>Erweiterte Berichterstellung
Möglicherweise möchten Sie anwendungsspezifische Ereignisse, Fehler und Aufträge melden; verwenden Sie dazu die anderen Methoden in der Klasse `EngagementAgent` . Mit der Engagement-API können alle erweiterten Funktionen von Engagement verwendet werden.

Weitere Informationen finden Sie unter [Verwenden der erweiterten Mobile Engagement-Tagging-API in Ihrer Windows Phone Silverlight-App](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Erweiterte Konfiguration
### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturzberichtsfunktion
Sie können die automatische Absturzberichtsfunktion von Engagement deaktivieren. Wenn dann eine unbehandelte Ausnahme auftritt, handelt Engagement nicht.

> [!WARNING]
> Wenn Sie diese Funktion deaktivieren möchten, bedenken Sie, dass Engagement Ihnen bei Auftreten eines nicht behandelten Absturzes der App weder eine Meldung zum Absturz sendet **NOCH** die Sitzung und Aufträge schließt.
> 
> 

Zum Deaktivieren der automatischen Berichterstellung bei Abstürzen passen Sie Ihre Konfiguration abhängig davon an, wie Sie sie deklariert haben:

#### <a name="from-engagementconfigurationxml-file"></a>In der Datei `EngagementConfiguration.xml`
Setzen Sie die Berichterstattung bei Abstürzen zwischen den Tags `<reportCrash>` und `</reportCrash>` auf `false`.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Im Objekt `EngagementConfiguration` während der Laufzeit
Setzen Sie Absturzbericht auf „false“ mithilfe des Objekts „EngagementConfiguration“.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-Modus
Standardmäßig meldet der Engagement-Dienst Protokolle in Echtzeit. Wenn Ihre Anwendung sehr häufig Protokolle meldet, ist es besser, die Protokolle zu puffern und sie dann gleichzeitig in regelmäßigen Zeitintervallen zu melden (dies wird als „Burst-Modus“ bezeichnet).

Rufen Sie dazu die folgende Methode auf:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**. Immer wenn Sie die Protokollierung in Echtzeit erneut aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0 auf.

Der Burst-Modus verlängert leicht die Akkulaufzeit, wirkt sich jedoch auf den Engagement-Monitor aus: Die Dauer von allen Sitzungen und Aufträgen wird auf den Burst-Schwellenwert gerundet (folglich sind eventuell Sitzungen und Aufträge, die kürzer als der Burst-Schwellenwert sind, möglicherweise nicht sichtbar). Es wird empfohlen, einen Burst-Schwellenwert von höchstens 30000 (30 s) zu verwenden. Sie müssen beachten, dass gespeicherte Protokolle auf 300 Elemente beschränkt sind. Wenn der Sendevorgang zu lange dauert, können einige Protokolle verloren gehen.

> [!WARNING]
> Der Burstschwellenwert kann nicht auf einen Zeitraum von weniger als einer Sekunde konfiguriert werden. Wenn Sie dies versuchen, zeigt das SDK eine Ablaufverfolgung mit einem Fehler an und setzt den Wert automatisch auf den Standardwert von 0 Sekunden zurück. Dadurch wird ausgelöst, dass das SDK die Protokolle in Echtzeit meldet.
> 
> 




<!--HONumber=Nov16_HO3-->


