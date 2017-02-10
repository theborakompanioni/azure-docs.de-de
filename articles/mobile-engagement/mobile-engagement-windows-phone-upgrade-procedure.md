---
title: "Windows Phone Silverlight SDK – Upgradeverfahren"
description: "Windows Phone Silverlight SDK für Azure Mobile Engagement – Upgradeverfahren"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 06b2a8b5e12d33c3ade469491b2694dd4a342cf1


---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK – Upgradeverfahren
Wenn Sie bereits eine ältere Version unseres SDK in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Möglicherweise müssen Sie mehrere Verfahren befolgen, wenn Sie mehrere Versionen des SDK übersprungen haben. Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "0.10.1 zu 0.11.0".

## <a name="from-200-to-330"></a>Von 2.0.0 zu 3.3.0
### <a name="test-logs"></a>Testprotokolle
Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-to-200"></a>Von 1.1.1 zu 2.0.0
Im Folgenden wird beschrieben, wie Sie die Migration einer SDK-Integration vom Capptain-Dienst, der von Capptain-SAS angeboten wird, in eine durch Azure Mobile Engagement unterstützte App durchführen. 

> [!IMPORTANT]
> Capptain und Mobile Engagement sind nicht dieselben Dienste, und die unten beschriebene Vorgehensweise hebt nur hervor, wie die Migration der Clientapp durchzuführen ist. Bei der Migration des SDK in die App werden Ihre Daten NICHT von den Capptain-Servern zu den Mobile Engagement-Servern migriert
> 
> 

Wenn Sie von einer früheren Version migrieren, ziehen Sie zunächst die Capptain-Website zurate, um zu 1.1.1 zu migrieren und wenden Sie dann das folgende Verfahren an

### <a name="nuget-package"></a>NuGet-Paket
Ersetzen Sie **Capptain.WindowsPhone** durch das NuGet-Paket **MicrosoftAzure.MobileEngagement**.

### <a name="applying-mobile-engagement"></a>Anwenden von Mobile Engagement
Im SDK wird der Begriff `Engagement`verwendet. Sie müssen Ihr Projekt entsprechend dieser Änderung aktualisieren.

Sie müssen Ihr aktuelles Capptain-NuGet-Paket deinstallieren. Bedenken Sie, dass dadurch alle Änderungen, die Sie im Capptain-Ressourcenordner vorgenommen haben, entfernt werden. Wenn Sie diese Dateien beibehalten möchten, erstellen Sie eine Kopie davon.

Danach installieren Sie das neue Microsoft Azure Engagement-NuGet-Paket für Ihr Projekt. Sie finden es direkt auf [NuGet](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Durch diese Aktion werden alle Ressourcendateien, die von Engagement verwendet werden, ersetzt und die neue Engagement-DLL Ihren Projektverweisen hinzugefügt.

Sie müssen Ihre Projektverweise bereinigen, indem Sie die Capptain-DLL-Verweise löschen. Wenn Sie dies nicht tun, kommt es zu Konflikten mit der Capptain-Version und Fehlern.

Wenn Sie Capptain-Ressourcen angepasst haben, kopieren Sie den Inhalt der alten Dateien, und fügen Sie ihn in die neuen Engagement-Dateien ein. Bitte beachten Sie, dass XAML- und CS-Dateien aktualisiert werden müssen.

Nach Abschluss dieser Schritte müssen Sie nur die alten Capptain-Verweise durch die neuen Engagement-Verweise ersetzen.

1. Alle Capptain-Namespaces müssen aktualisiert werden.
   
    Vor der Migration:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Nach der Migration:
   
        using Microsoft.Azure.Engagement;
2. Alle Capptain-Klassen, die "Capptain" enthalten, sollten "Engagement" enthalten.
   
    Vor der Migration:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Nach der Migration:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. Für XAML-Dateien ändern sich ebenfalls Capptain-Namespace und Attribute.
   
    Vor der Migration:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Nach der Migration:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Beachten Sie, dass andere Ressourcen wie Capptain-Bilder auch für die Verwendung von „Engagement“ umbenannt wurden.

### <a name="application-id--sdk-key"></a>Anwendungs-ID/SDK-Schlüssel
Engagement verwendet eine Verbindungszeichenfolge. Sie müssen bei Mobile Engagement keine Anwendungs-ID und keinen SDK-Schlüssel angeben, sie müssen nur eine Verbindungszeichenfolge angeben. Sie können diese in der EngagementConfiguration-Datei einrichten.

Die Konfiguration von Engagement kann in der `Resources\EngagementConfiguration.xml` -Datei des Projekts festgelegt werden.

Bearbeiten Sie diese Datei, um Folgendes anzugeben:

* Die Verbindungszeichenfolge der Anwendung zwischen den Tags `<connectionString>` and `<\connectionString>`.

Wenn Sie sie stattdessen zur Laufzeit angeben möchten, können Sie die folgende Methode vor der Initialisierung des Engagement-Agent aufrufen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

Die Verbindungszeichenfolge für die Anwendung wird im klassischen Azure-Portal angezeigt.

### <a name="items-name-change"></a>Namensänderung von Elementen
Alle Elemente namens *capptain* wurden in *engagement* umbenannt. Ebenso wurden alle *Capptain*-Elemente in *Engagement* umbenannt.

Beispiele für häufig verwendete Capptain-Elemente:

* CapptainConfiguration heißt nun EngagementConfigurationCapptainConfiguration trägt nun den Namen EngagementConfiguration
* CapptainAgent trägt nun den Namen EngagementAgent
* CapptainReach trägt nun den Namen EngagementReach
* CapptainHttpConfig trägt nun den Namen EngagementHttpConfig
* GetCapptainPageName trägt nun den Namen GetEngagementPageName

Beachten Sie, dass sich die Umbenennung auch auf überschriebene Methoden auswirkt.




<!--HONumber=Nov16_HO3-->


