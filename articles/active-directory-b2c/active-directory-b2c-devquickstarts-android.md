---
title: 'Azure Active Directory B2C: Aufrufen von Web-APIs aus einer Android-Anwendung | Microsoft Docs'
description: "In diesem Artikel wird gezeigt, wie Sie eine Android-App für Aufgabenlisten erstellen, die eine Node.js-Web-API mithilfe von OAuth 2.0-Bearertoken aufruft. Sowohl die Android-App als auch die Web-API verwenden Azure Active Directory B2C zum Verwalten von Benutzeridentitäten und zum Authentifizieren von Benutzern."
services: active-directory-b2c
documentationcenter: android
author: xerners
manager: mbaldwin
editor: 
ms.assetid: d00947c3-dcaa-4cb3-8c2e-d94e0746d8b2
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/31/2017
ms.author: brandwe
translationtype: Human Translation
ms.sourcegitcommit: 3f89a0e06ea4596d38f7f7b5e52cea54c5e30780
ms.openlocfilehash: bb94ab8d794d1e836df57e1bc42df42825f2668d


---
# <a name="azure-ad-b2c-call-a-web-api-from-an-android-application"></a>Azure AD B2C: Aufrufen von Web-APIs aus einer Android-Anwendung
> [!WARNING]
> Wir haben ein aktualisiertes Android-Codebeispiel veröffentlicht, das [hier](https://github.com/Azure-Samples/active-directory-b2c-android-native-nodejs-webapi) zu finden ist.  In diesem Beispiel wird eine Drittanbieterbibliothek verwendet, deren Kompatibilität in grundlegenden Szenarien mit Azure AD B2C getestet wurde.  Diese Bibliothek nutzt eingebettete Webansichten anstelle des Systembrowsers.  Google hat [angekündigt](https://developers.googleblog.com/2016/08/modernizing-oauth-interactions-in-native-apps.html), dass die Google-Kontoanmeldung mit eingebetteten Webansichten ab dem 20. April 2017 nicht mehr unterstützt wird. Zu diesem Zeitpunkt müssen diejenigen, die Google-Konten unterstützen möchten, ihre Bibliotheken aktualisieren.  

>Microsoft stellt keine Fehlerbehebungen für diese Drittanbieterbibliotheken bereit und hat diese Bibliotheken keinerlei Prüfungen unterzogen. Probleme und Funktionsanforderungen müssen direkt an das Open Source-Projekt der Bibliothek weitergeleitet werden. Weitere Informationen hierzu finden Sie in [diesem Artikel](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>




<!--HONumber=Feb17_HO1-->


