---
title: "Hinzufügen von Pushbenachrichtigungen zu Ihrer Android-App mit Mobile Apps | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die Verwendung von Mobile Apps zum Senden von Pushbenachrichtigungen an Ihre Android-App."
services: app-service\mobile
documentationcenter: android
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: ef43d1df2a64d85f256b28cd7db5a4f4356a7ec0
ms.openlocfilehash: 055f55966de1e5c1bb41a6e0bf1c0da1758a3ed0
ms.contentlocale: de-de
ms.lasthandoff: 12/13/2016

---
# <a name="add-push-notifications-to-your-android-app"></a>Hinzufügen von Pushbenachrichtigungen zu Ihrer Android-App
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Übersicht
In diesem Tutorial fügen Sie Pushbenachrichtigungen zum [Android-Schnellstartprojekt] hinzu, damit jedes Mal, wenn ein Datensatz eingefügt wird, eine Pushbenachrichtigung an das Gerät gesendet wird.

Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Erweiterungspaket für Pushbenachrichtigungen hinzufügen. Weitere Informationen finden Sie unter [Arbeiten mit dem .NET Back-End-Server SDK für Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:

* IDE in Abhängigkeit vom Projekt-Backend:

  * [Android Studio](https://developer.android.com/sdk/index.html), wenn diese App über ein Node.js-Backend verfügt
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) oder höher, wenn diese App über ein .NET-Back-End verfügt
* Android 2.3 oder höher, Google-Repository Revision 27 oder höher und Google Play Services 9.0.2 oder höher für Firebase Cloud Messaging
* Führen Sie den [Android-Schnellstartprojekt] durch.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Erstellen eines Projekts, das Firebase Cloud Messaging unterstützt
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurieren eines Notification Hubs
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Konfigurieren von Azure zum Senden von Pushbenachrichtigungen
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Aktivieren von Pushbenachrichtigungen für das Serverprojekt
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Hinzufügen von Pushbenachrichtigungen zur App
In diesem Abschnitt aktualisieren Sie Ihre Android-Client-App, um Pushbenachrichtigungen zu erhalten.

### <a name="verify-android-sdk-version"></a>Prüfen der Version des Android SDK
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Als Nächstes installieren Sie Google Play Services. Google Cloud Messaging verfügt über einige Mindestanforderungen an die API-Ebene für Entwicklung und Tests, denen die Eigenschaft **minSdkVersion** im Manifest entsprechen muss.

Wenn Sie auf einem älteren Gerät testen, lesen Sie unter [Set Up Google Play Services SDK] (Einrichten des Google Play Services SDK) die Informationen zum Festlegen des Werts, und legen Sie ihn entsprechend fest.

### <a name="add-google-play-services-to-the-project"></a>Hinzufügen von Google Play Services zum Projekt
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Code hinzufügen
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testen der App mit dem veröffentlichten mobilen Dienst
Sie können die App testen, indem Sie ein Android-Telefon direkt mit einem USB-Kabel verbinden oder ein virtuelles Gerät im Emulator verwenden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie dieses Tutorial abgeschlossen haben, können Sie mit den folgenden Tutorials fortfahren:

* [Hinzufügen der Authentifizierung zu Ihrer Android-App.](app-service-mobile-android-get-started-users.md)
  Erfahren Sie, wie Sie dem Aufgabenlisten-Schnellstartprojekt unter Android mithilfe eines unterstützten Identitätsanbieters eine Authentifizierung hinzufügen.
* [Aktivieren der Offlinesynchronisierung für Ihre Android-App.](app-service-mobile-android-get-started-offline-data.md)
  Informieren Sie sich, wie Sie die Offlineunterstützung mit einem Mobile Apps-Back-End zu Ihrer App hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (also das Anzeigen, Hinzufügen oder Ändern von Daten) auch dann, wenn keine Netzwerkverbindung besteht.

<!-- URLs -->
[Android-Schnellstartprojekt]: app-service-mobile-android-get-started.md

[Set Up Google Play Services SDK]:https://developers.google.com/android/guides/setup

