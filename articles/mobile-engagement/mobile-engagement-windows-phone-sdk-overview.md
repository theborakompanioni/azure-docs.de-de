---
title: "Azure Mobile Engagement – Übersicht über das Windows Phone Silverlight SDK | Microsoft-Dokumentation"
description: "Übersicht über das Windows Phone Silverlight SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 51858d6daca27a75e7f87af507600c7a193c874f
ms.openlocfilehash: c4e8ceee4104c3d3a6c3e6b79322ba1cf8463b22


---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Übersicht über das Windows Phone Silverlight SDK für Azure Mobile Engagement
Beginnen Sie hier, um Details zur Integration von Azure Mobile Engagement in einer Windows Phone Silverlight-App abzurufen. Wenn Sie es gleich ausprobieren möchten, sollten Sie unbedingt das [15-Minuten-Lernprogramm](mobile-engagement-windows-phone-get-started.md)bearbeiten.

Klicken Sie, um den [SDK-Inhalt](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integrationsverfahren
1. Hier starten: [Integrieren von Mobile Engagement in eine Windows Phone Silverlight-App](mobile-engagement-windows-phone-integrate-engagement.md)
2. Für Benachrichtigungen: [Integration von Reach (Benachrichtigungen) in einer Windows Phone Silverlight-App](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Tag-Plan-Implementierung: [Verwenden der erweiterten Mobile Engagement-Tagging-API in einer Windows Phone Silverlight-App](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Versionshinweise
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
Teil des NuGet-Pakets *MicrosoftAzure.MobileEngagement* **v3.4.1**

* Verbesserungen der Stabilität.

Eine frühere Version finden Sie unter [Vollständige Versionshinweise](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
Wenn Sie bereits eine ältere Version unseres SDK in Ihrer Anwendung integriert haben, müssen Sie die folgenden Punkte beim Aktualisieren des SDK beachten.

Möglicherweise müssen Sie mehrere Verfahren befolgen, wenn Sie mehrere Versionen des SDK übersprungen haben. Die vollständigen [Upgrade-Verfahren](mobile-engagement-windows-phone-upgrade-procedure.md)lesen. Wenn Sie beispielsweise von 0.10.1 zu 0.11.0 migrieren, müssen Sie zunächst den Vorgang "von 0.9.0 zu 0.10.1" ausführen und anschließend den Vorgang "0.10.1 zu 0.11.0".

### <a name="from-200-to-330"></a>Von 2.0.0 zu 3.3.0
#### <a name="test-logs"></a>Testprotokolle
Vom SDK produzierte Konsolenprotokolle können jetzt aktiviert/deaktiviert/gefiltert werden. Um diese anzupassen, aktualisieren Sie die Eigenschaft `EngagementAgent.Instance.TestLogEnabled` auf einen der aus der `EngagementTestLogLevel`-Enumeration verfügbaren Werte, beispielsweise:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Upgrade von älteren Versionen
Siehe [Upgrade-Verfahren](mobile-engagement-windows-phone-upgrade-procedure.md)




<!--HONumber=Feb17_HO2-->


