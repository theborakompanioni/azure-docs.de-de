---
title: Integration des Azure Mobile Engagement Android SDKs
description: "Neueste Updates und Verfahren für das Android SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0282abbf44406cac89c13520bc2a4e375817ed1f


---
# <a name="how-to-integrate-gcm-with-mobile-engagement"></a>Integrieren von GCM mit Mobile Engagement
> [!IMPORTANT]
> Bevor Sie dieser Anleitung folgen, müssen Sie das unter „Integrieren von Mobile Engagement unter Android“ beschriebene Integrationsverfahren befolgen.
> 
> Dieses Dokument ist nur relevant, wenn Sie das Reach-Modul bereits integriert haben und Pushübertragungen an Google Play-Geräte planen. Lesen Sie zunächst das Dokument "So integrieren Sie Engagement Reach auf Android", um Informationen zum Integrieren von Reach-Kampagnen zu erhalten.
> 
> 

## <a name="introduction"></a>Einführung
Durch die Integration von GCM kann Ihre Anwendung Pushnachrichten empfangen.

GCM-Nutzlasten, die mithilfe von Push an das SDK übertragen werden, enthalten immer den `azme` -Schlüssel im Datenobjekt. Wenn Sie GCM in Ihrer Anwendung also zu einem anderen Zweck verwenden, können Sie Push-Vorgänge basierend auf diesem Schlüssel filtern.

> [!IMPORTANT]
> Über GCM können nur Geräte Pushnachrichten empfangen, die über Android 2.2 oder höher verfügen, auf denen Google Play installiert ist und die über eine Google-Hintergrundverbindung verfügen. Dieser Code kann jedoch ohne Sicherheitsbedenken auch auf Geräten integriert werden, die GCM nicht unterstützen (es werden nur „Intents“ verwendet).
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>Erstellen eines Google Cloud Messaging-Projekts mit API-Schlüssel
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>SDK-Integration
### <a name="managing-device-registrations"></a>Verwalten von Geräteregistrierungen
Jedes Gerät muss einen Registrierungsbefehl an die Google-Server senden, anderenfalls sind sie nicht erreichbar.

Ein Gerät die Registrierung zum Empfang von GCM-Benachrichtigungen auch aufheben (die Registrierung für das Gerät wird automatisch aufgehoben, wenn die Anwendung deinstalliert wird).

Wenn Sie das [Google Play SDK] nicht verwenden oder nicht bereits selbst eine Registrierungsanforderung gesendet haben, kann Engagement das Gerät automatisch für Sie registrieren.

Fügen Sie zur Aktivierung in der `AndroidManifest.xml`-Datei im Tag `<application/>` den folgenden Code ein:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Übermitteln der Registrierungs-ID an den Engagement-Pushdienst, um Benachrichtigungen zu erhalten
Um die Registrierungs-ID des Geräts an den Engagement-Pushdienst zu übermitteln und Benachrichtigungen zu empfangen, fügen Sie in der `AndroidManifest.xml`-Datei im Tag `<application/>` den folgenden Code ein (selbst, wenn Sie Geräteregistrierungen selbst verwalten):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Stellen Sie sicher, dass Sie die folgenden Berechtigungen in der Datei `AndroidManifest.xml` festgelegt haben (vor dem Tag `</application>`).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Gewähren des Zugriffs für Mobile Engagement auf den GCM-API-Schlüssel
Befolgen Sie [diesen Leitfaden](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) , um Mobile Engagement Zugriff auf den GCM-API-Schlüssel zu gewähren.

[Google Play SDK]:https://developers.google.com/cloud-messaging/android/start



<!--HONumber=Nov16_HO3-->


