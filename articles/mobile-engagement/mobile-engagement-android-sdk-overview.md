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
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2b89a13230439698854656f0bb5367f1ed35ce1c


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
### <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Keine WLAN-Sperre mehr.
* Behebung eines Deadlocks, der auftritt, wenn „getDeviceId“ vor „init“ aufgerufen wird (Fehler seit 4.2.0).

Alle Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
Wenn Sie bereits eine ältere Version unseres SDK in die Anwendung integriert haben, finden Sie Informationen dazu in [Upgrade-Verfahren](mobile-engagement-android-upgrade-procedure.md).




<!--HONumber=Nov16_HO3-->


