<properties
	pageTitle="Integration des Android SDK für Azure Mobile Engagement"
	description="Beschreibt die Integration von Azure Mobile Engagement SDK in Android-Apps"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="08/10/2016"
	ms.author="piyushjo;ricksal" />

# Integration des Android SDK für Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-sdk-overview.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
- [iOS](mobile-engagement-ios-sdk-overview.md)
- [Android](mobile-engagement-android-sdk-overview.md)

In diesem Dokument werden alle für das Android-SDK für Azure Mobile Engagement verfügbaren Integrations- und Konfigurationsoptionen beschrieben.

## Voraussetzungen

[AZURE.INCLUDE [Voraussetzungen](../../includes/mobile-engagement-android-prereqs.md)]

## Erweiterte Funktionen

### Berichterstellungsfunktionen

Sie können die folgenden Funktionen hinzufügen:

1. [Erweiterte Berichterstellungsoptionen](mobile-engagement-android-advanced-reporting.md)
2. [Erweiterte Berichterstellungsoptionen für Speicherorte](mobile-engagement-android-location-reporting.md)
3. [Erweiterte Konfigurationsoptionen](mobile-engagement-android-advanced-configuration.md)

### Benachrichtigungen:
[Integrieren von Reach (Benachrichtigungen) in Ihre Android-App](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [Integrieren von GCM mit Mobile Engagement](mobile-engagement-android-gcm-integrate.md)

2. Amazon Device Messaging (ADM): [Integrieren von ADM mit Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### Tag-Plan-Implementierung:
[Verwenden der erweiterten Kennzeichnungs-API von Mobile Engagement in Ihrer Android-App](mobile-engagement-android-use-engagement-api.md)

## Versionshinweise

### 4\.2.3 (08/10/2016)

 - Keine WLAN-Sperre mehr.
 - Behebung eines Deadlocks, der auftritt, wenn „getDeviceId“ vor „init“ aufgerufen wird (Fehler seit 4.2.0).

Alle Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-android-release-notes.md).

## Upgrade-Verfahren

Wenn Sie bereits eine ältere Version unseres SDKs in die Anwendung integriert haben, finden Sie Informationen dazu in [Upgrade-Verfahren](mobile-engagement-android-upgrade-procedure.md).

<!---HONumber=AcomDC_0810_2016-->