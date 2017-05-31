---
title: "Azure AD v2 – Android – Erste Schritte – Konfigurieren | Microsoft-Dokumentation"
description: Informationen, wie eine Android-App ein Zugriffstoken abrufen und die Microsoft Graph-API oder APIs aufrufen kann, die Zugriffstoken vom Azure Active Directory-v2-Endpunkt erfordern
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: b034bf99351c5b33c51d9a8401434160bddb15be
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen der Registrierungsinformationen der Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Client-ID Ihrem Projekt hinzufügen.

1.    Öffnen Sie `MainActivity` (unter `app` > `java` > *`{host}.{namespace}`*).
2.    Ersetzen Sie die Zeile, die mit `final static String CLIENT_ID` beginnt, durch:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Öffnen Sie: `app` > `manifests` > `AndroidManifest.xml`
4. Fügen Sie die folgende Aktivität dem Knoten `manifest\application` hinzu. Hierdurch wird `BrowserTabActivity` registriert, um dem Betriebssystem das Fortsetzen der Anwendung nach Abschluss der Authentifizierung zu ermöglichen:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

### <a name="what-is-next"></a>Wie geht es weiter?

[Testen und validieren](active-directory-mobileanddesktopapp-android-test.md)

