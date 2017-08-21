---
title: "Integration des Android SDK für Azure Mobile Engagement"
description: Beschreibt die Integration von Azure Mobile Engagement SDK in Android-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.translationtype: HT
ms.sourcegitcommit: 94d1d4c243bede354ae3deba7fbf5da0652567cb
ms.openlocfilehash: 35935e911f1f17989beb71978396c6d1b7d601d6
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Integration des Android SDK für Azure Mobile Engagement
> [!div class="op_single_selector"]
> * [Universal Windows](mobile-engagement-windows-store-sdk-overview.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
> * [iOS](mobile-engagement-ios-sdk-overview.md)
> * [Android](mobile-engagement-android-sdk-overview.md)
> 
> 

In diesem Dokument werden alle für das Android-SDK für Azure Mobile Engagement verfügbaren Integrations- und Konfigurationsoptionen beschrieben.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Erweiterte Funktionen
### <a name="reporting-features"></a>Berichterstellungsfunktionen
Sie können die folgenden Funktionen hinzufügen:

1. [Erweiterte Berichterstellungsoptionen](mobile-engagement-android-advanced-reporting.md)
2. [Erweiterte Berichterstellungsoptionen für Speicherorte](mobile-engagement-android-location-reporting.md)
3. [Erweiterte Konfigurationsoptionen](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Benachrichtigungen:
[Integrieren von Reach (Benachrichtigungen) in Ihre Android-App](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [Integrieren von GCM mit Mobile Engagement](mobile-engagement-android-gcm-integrate.md)
2. Amazon Device Messaging (ADM): [Integrieren von ADM mit Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Tag-Plan-Implementierung:
[Verwenden der erweiterten Kennzeichnungs-API von Mobile Engagement in Ihrer Android-App](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Versionshinweise

### <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Beheben eines Absturzes, der in seltenen Fällen beim Abrufen von `EngagementAgentUtils.isInDedicatedEngagementProcess` auftritt, worauf ebenfalls die `EngagementApplication`-Klasse zugreift.

### <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8-Unterstützung (frühere Versionen des SDK funktionieren nicht auf Android 8).
* Keine Abhängigkeit von der Support-Bibliothek mehr.
* Entfernen der `EngagementFragmentActivity`-Klasse.
* Wegen der [Einschränkungen von Ausführungen im Hintergrund](https://developer.android.com/preview/features/background.html) auf Android 8 können Protokolle im Hintergrund verzögert werden, bis der Benutzer mit dem Gerät interagiert. Dies wirkt sich auf die Statistiken der Pushkampagnen **Delivered** (Übermittelt) und **System notification displayed** (Systembenachrichtigung angezeigt) aus, die verzögert werden, wenn sich das Gerät im Ruhezustand befindet (die Benachrichtigung wird dennoch angezeigt und löst in Echtzeit ohne Probleme ein Klingeln und Vibrieren aus).
* Wegen der [Einschränkungen des Standorts im Hintergrund](https://developer.android.com/preview/features/background-location-limits.html) wird der Speicherort im Hintergrund auf Android 8 nicht in Echtzeit regelmäßig aktualisiert.

Alle Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, finden Sie Informationen dazu in [Upgrade-Verfahren](mobile-engagement-android-upgrade-procedure.md).


