---
title: "Azure AD v2 – Android – Erste Schritte – Test | Microsoft-Dokumentation"
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
ms.openlocfilehash: 85f9b475a77af9865882f4193c602756e345cc02
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
## <a name="test-your-code"></a>Testen Ihres Codes

1. Stellen Sie Ihren Code Ihrem Gerät/Geräteemulator bereit.
2. Wenn Sie zum Testen bereit sind, verwenden Sie zum Anmelden ein Microsoft Azure Active Directory-Konto (Organisationskonto) oder ein Microsoft-Konto (live.com, outlook.com). 

![Beispielscreenshot](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Anmeldung](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Zustimmung
Bei der ersten Anmeldung bei Ihrer Anwendung wird dem Benutzer ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem er explizit seine Zustimmung geben muss: 

![Zustimmung](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Erwartete Ergebnisse
Angezeigt werden sollten die Ergebnisse eines Aufrufs des Microsoft Graph-API-Endpunkts „me“, der zum Abrufen des Benutzerprofils verwendet wird: https://graph.microsoft.com/v1.0/me. Eine Liste der gängigen Microsoft Graph-Endpunkte finden Sie in diesem [Artikel](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen
Die Graph-API erfordert den Bereich `user.read` zum Lesen des Benutzerprofils. Dieser Bereich wird jeder Anwendung, die über unser Registrierungsportal registriert wird, standardmäßig hinzugefügt. Einige andere Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern zusätzliche Bereiche. Beispielsweise ist für Graph `Calendars.Read` erforderlich, um Kalender von Benutzern aufzulisten. Um auf den Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie diese Informationen zur delegierten Anwendungsregistrierung und dann `Calendars.Read` dem Aufruf von `AcquireTokenAsync` hinzufügen. Wenn Sie die Anzahl der Bereiche weiter erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich erfordert (nicht empfohlen), können Sie `ClientId` als Bereich im `AcquireTokenAsync`-Aufruf verwenden.
<!--end-collapse-->

