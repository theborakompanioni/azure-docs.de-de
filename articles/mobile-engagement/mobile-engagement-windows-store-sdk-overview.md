---
title: "Azure Mobile Engagement – Versionshinweise für die Windows Universal SDK-Integration | Microsoft-Dokumentation"
description: "Windows Universal-SDK-Integration für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 51858d6daca27a75e7f87af507600c7a193c874f
ms.openlocfilehash: d616ad58156a19e89b3e106639a38df67cbd0abb


---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows Universal-SDK-Integration für Azure Mobile Engagement
In diesem Dokument werden alle für das Windows Universal-SDK für Azure Mobile Engagement verfügbaren Integrations- und Konfigurationsoptionen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, müssen Sie zunächst unser [15-Minuten-Tutorial](mobile-engagement-windows-store-dotnet-get-started.md)abschließen.

## <a name="advanced-features"></a>Erweiterte Funktionen
### <a name="reporting-features"></a>Berichterstellungsfunktionen
Sie können die folgenden Funktionen hinzufügen:

1. [Erweiterte Berichterstellungsoptionen](mobile-engagement-windows-store-advanced-reporting.md)
2. [Erweiterte Konfigurationsoptionen](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Benachrichtigungen
[Windows Universal-Apps Reach SDK-Integration](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Tag-Plan-Implementierung:
[Verwenden der Engagement-API auf der universellen Windows-Plattform](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Versionshinweise
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Verbesserungen der Stabilität.

Frühere Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
Wenn Sie bereits eine ältere Version von Engagement in Ihre Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Wenn Sie mehrere Versionen des SDK übersprungen haben, müssen Sie möglicherweise mehrere Verfahren befolgen. Die vollständigen [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)lesen. Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "0.10.1 zu 0.11.0".

### <a name="from-330-to-340"></a>Von 3.3.0 bis 3.4.0
#### <a name="test-logs"></a>Testprotokolle
Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Ressourcen
Das Reach-Overlay wurde verbessert. Es ist Teil der SDK-NuGet-Paket-Ressourcen.

Während der Aktualisierung auf die neue Version des SDK können Sie auswählen, ob Sie vorhandene Dateien aus dem Overlayordner von Ihren Ressourcen beibehalten möchten oder nicht:

* Wenn das vorherige Overlay bei Ihnen funktioniert, oder Sie die `WebView` -Elemente manuell integrieren, dann können Sie entscheiden, Ihre vorhandenen Dateien beizubehalten – es wird weiterhin funktionieren.
* Um auf das neue Overlay zu aktualisieren, ersetzen Sie einfach den gesamten `overlay`-Ordner aus Ihren Ressourcen durch den neuen aus dem SDK-Paket (UWP-Apps: Nach dem Upgrade erhalten Sie den neuen Overlayordner aus %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Mithilfe des neuen Overlays werden alle an der vorherigen Version vorgenommenen Anpassungen überschrieben.
> 
> 

### <a name="upgrade-from-older-versions"></a>Upgrade von älteren Versionen
Siehe [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)




<!--HONumber=Feb17_HO2-->


