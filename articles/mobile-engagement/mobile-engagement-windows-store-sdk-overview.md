---
title: Windows Universal-SDK-Integration
description: Windows Universal-SDK-Integration für Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal

---
# Windows Universal-SDK-Integration für Azure Mobile Engagement
In diesem Dokument werden alle für das Windows Universal-SDK für Azure Mobile Engagement verfügbaren Integrations- und Konfigurationsoptionen beschrieben.

## Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen, müssen Sie zunächst unser [15-Minuten-Tutorial](mobile-engagement-windows-store-dotnet-get-started.md) abschließen.

## Erweiterte Funktionen
### Berichterstellungsfunktionen
Sie können die folgenden Funktionen hinzufügen:

1. [Erweiterte Berichterstellungsoptionen](mobile-engagement-windows-store-advanced-reporting.md)
2. [Erweiterte Konfigurationsoptionen](mobile-engagement-windows-store-advanced-configuration.md)

### Benachrichtigungen
[Windows Universal-Apps Reach SDK-Integration](mobile-engagement-windows-store-integrate-engagement-reach.md)

### Tag-Plan-Implementierung:
[Verwenden der Engagement-API auf der universellen Windows-Plattform](mobile-engagement-windows-store-use-engagement-api.md)

## Versionshinweise
### 3\.4.0 (19.04.2016)
* Reach-Overlayverbesserungen.
* „TestLogLevel“-API zum Aktivieren/Deaktivieren/Filtern von Konsolenprotokollen, die vom SDK ausgegeben wurden, wurde hinzugefügt.
* Behebung des Problems mit Inaktivitätsbenachrichtigungen, die sich darauf bezogen, dass die erste Aktivität beim App-Start nicht angezeigt wurde.

Frühere Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-windows-store-release-notes.md).

## Upgrade-Verfahren
Wenn Sie bereits eine ältere Version von Engagement in Ihre Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Wenn Sie mehrere Versionen des SDK übersprungen haben, müssen Sie möglicherweise mehrere Verfahren befolgen. Die vollständigen [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md) lesen. Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "0.10.1 zu 0.11.0".

### Von 3.3.0 bis 3.4.0
#### Testprotokolle
Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### Ressourcen
Das Reach-Overlay wurde verbessert. Es ist Teil der SDK-NuGet-Paket-Ressourcen.

Während der Aktualisierung auf die neue Version des SDK können Sie auswählen, ob Sie vorhandene Dateien aus dem Overlayordner von Ihren Ressourcen beibehalten möchten oder nicht:

* Wenn das vorherige Overlay bei Ihnen funktioniert, oder Sie die `WebView`-Elemente manuell integrieren, dann können Sie entscheiden, Ihre vorhandenen Dateien beizubehalten – es wird weiterhin funktionieren.
* Um auf das neue Overlay zu aktualisieren, ersetzen Sie einfach den gesamten `overlay`-Ordner aus Ihren Ressourcen durch den neuen aus dem SDK-Paket (UWP-Apps: Nach dem Upgrade erhalten Sie den neuen Overlayordner aus % USERPROFILE%\\.nuget\\packages\\MicrosoftAzure.MobileEngagement\\3.4.0\\content\\win81\\Resources).

> [!WARNING]
> Mithilfe des neuen Overlays werden alle an der vorherigen Version vorgenommenen Anpassungen überschrieben.
> 
> 

### Upgrade von älteren Versionen
Siehe [Upgrade-Verfahren](mobile-engagement-windows-store-upgrade-procedure.md)

<!---HONumber=AcomDC_0817_2016-->