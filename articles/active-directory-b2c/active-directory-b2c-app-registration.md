---
title: 'Azure Active Directory B2C: Registrierung einer Anwendung | Microsoft Docs'
description: Registrieren Ihrer Anwendung bei Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: PatAltimore
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 3d4fe2fa10d848c8b29e4d22d284c0d378f07ae0
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrieren der Anwendung

In dieser Schnellstartanleitung erfahren Sie, wie Sie in wenigen Minuten eine Anwendung in einem Microsoft Azure Active Directory B2C-Mandanten (Azure AD) registrieren. Wenn Sie fertig sind, ist Ihre Anwendung für die Verwendung im Azure B2C-Mandanten registriert.

## <a name="prerequisites"></a>Voraussetzungen

Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)beschriebenen Schritte.

Anwendungen, die im Azure-Portal auf dem Blatt „Azure AD B2C“ erstellt wurden, müssen am selben Speicherort verwaltet werden. Mit PowerShell oder über ein anderes Portal bearbeitete B2C-Anwendungen werden nicht mehr unterstützt und können nicht mehr mit Azure AD B2C verwendet werden. Ausführliche Informationen finden Sie im Abschnitt [Fehlerhafte Apps](#faulted-apps). 

## <a name="navigate-to-b2c-settings"></a>Aufrufen der B2C-Einstellungen

Melden Sie sich im [Azure-Portal](https://portal.azure.com/) als globaler Administrator des B2C-Mandanten an. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../../includes/active-directory-b2c-switch-b2c-tenant.md)]

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](../../includes/active-directory-b2c-portal-navigate-b2c-service.md)]

Wählen Sie basierend auf dem zu registrierenden Anwendungstyp die nächsten Schritte:

* [Registrieren einer Webanwendung](#register-a-web-app)
* [Registrieren einer Web-API](#register-a-web-api)
* [Registrieren einer mobilen oder nativen Anwendung](#register-a-mobile-or-native-app)
 
## <a name="register-a-web-app"></a>Registrieren einer Web-App

[!INCLUDE [active-directory-b2c-register-web-app](../../includes/active-directory-b2c-register-web-app.md)]

Wenn Ihre Webanwendung eine durch Azure AD B2C geschützte Web-API aufruft, gehen Sie wie folgt vor:
   1. Erstellen Sie ein Anwendungsgeheimnis. Klicken Sie hierzu auf dem Blatt **Schlüssel** auf die Schaltfläche **Schlüssel generieren**. Notieren Sie sich den Wert für **App-Schlüssel**. Sie können den Wert als Anwendungsgeheimnis im Code Ihrer Anwendung verwenden.
   2. Klicken Sie auf **API-Zugriff** und dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

> [!NOTE]
> **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden müssen.
> 

[Zu **Nächste Schritte** springen](#next-steps)

## <a name="register-a-web-api"></a>Registrieren einer Web-API

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

Klicken Sie auf **Veröffentlichte Bereiche**, um bei Bedarf weitere Bereiche hinzuzufügen. Standardmäßig wird der Bereich „user_impersonation“ definiert. Mithilfe des Bereichs „user_impersonation“ können andere Anwendungen im Namen des angemeldeten Benutzers auf diese API zugreifen. Der Bereich „user_impersonation“ kann auch entfernt werden.

[Zu **Nächste Schritte** springen](#next-steps)

## <a name="register-a-mobile-or-native-app"></a>Registrieren einer mobilen oder nativen Anwendung

[!INCLUDE [active-directory-b2c-register-mobile-native-app](../../includes/active-directory-b2c-register-mobile-native-app.md)]

[Zu **Nächste Schritte** springen](#next-steps)

## <a name="limitations"></a>Einschränkungen

### <a name="choosing-a-web-app-or-api-reply-url"></a>Auswählen einer Antwort-URL für eine Web-App oder -API

Aktuell sind in Azure AD B2C registrierte Apps auf eine begrenzte Anzahl von Antwort-URL-Werten beschränkt. Die Antwort-URL für Web-Apps und -Dienste muss mit dem Schema `https` beginnen, und alle Antwort-URL-Werte müssen dieselbe DNS-Domäne verwenden. Es ist beispielsweise nicht möglich, eine Web-App zu registrieren, die eine der folgenden Antwort-URLs aufweist:

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Das Registrierungssystem vergleicht den gesamten DNS-Namen der vorhandenen Antwort-URL mit dem DNS-Namen der von Ihnen hinzugefügten Antwort-URL. Die Anforderung zum Hinzufügen des DNS-Namens schlägt fehl, wenn eine der folgenden Bedingungen erfüllt ist:

* Der gesamte DNS-Name der neuen Antwort-URL entspricht nicht dem DNS-Namen der vorhandenen Antwort-URL.
* Der gesamte DNS-Name der neuen Antwort-URL ist keine Unterdomäne der vorhandenen Antwort-URL.

Beispiel: Die App besitzt die folgende Antwort-URL:

`https://login.contoso.com`

Eine Hinzufügung ist wie folgt möglich:

`https://login.contoso.com/new`

In diesem Fall stimmt der DNS-Name exakt überein. Alternativ haben Sie folgende Möglichkeit:

`https://new.login.contoso.com`

In diesem Fall verweisen Sie auf eine DNS-Unterdomäne von login.contoso.com. Wenn Sie eine App mit den Antwort-URLs „login-east.contoso.com“ und „login-west.contoso.com“ benötigen, müssen Sie die folgenden Antwort-URLs in der angegebenen Reihenfolge hinzufügen:

`https://contoso.com`

`https://login-east.contoso.com`

`https://login-west.contoso.com`

Die beiden letztgenannten URLs können hinzugefügt werden, da es sich hierbei um Unterdomänen der ersten Antwort-URL „contoso.com“ handelt.

### <a name="choosing-a-native-app-redirect-uri"></a>Auswählen eines Umleitungs-URI für eine native App

Bei der Auswahl eines Umleitungs-URIs für mobile/systemeigene Anwendungen sind zwei Aspekte zu berücksichtigen:

* **Eindeutigkeit**: Das Schema des Umleitungs-URIS muss für jede Anwendung eindeutig sein. In unserem Beispiel (com.onmicrosoft.contoso.appname://redirect/path) verwenden wir „com.onmicrosoft.contoso.appname“ als Schema. Es wird empfohlen, diesem Muster zu folgen. Wenn zwei Anwendungen dasselbe Schema verwenden, wird den Benutzer das Dialogfeld „App auswählen“ angezeigt. Wenn der Benutzer die falsche Auswahl trifft, schlägt die Anmeldung fehl.
* **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss nach der Domäne mindestens einen Schrägstrich enthalten. (Beispiel: „//contoso/“ funktioniert, aber „//contoso“ schlägt fehl.)

Stellen Sie sicher, dass der Umleitungs-URI keine Sonderzeichen wie Unterstriche enthält.

### <a name="faulted-apps"></a>Fehlerhafte Apps

B2C-Anwendungen sollten NICHT wie folgt bearbeitet werden:

* In anderen Portalen für die Anwendungsverwaltung, z.B. im [klassischen Azure-Portal](https://manage.windowsazure.com/) und dem [App-Registrierungsportal](https://apps.dev.microsoft.com/).
* Mithilfe der Graph-API oder PowerShell

Wenn Sie die B2C-Anwendung wie oben beschrieben bearbeiten und sie dann noch einmal auf dem Azure AD B2C-Featureblatt im Azure-Portal bearbeiten möchten, wird sie eine fehlerhafte App, und Ihre Anwendung kann nicht mehr mit Azure AD B2C verwendet werden. Sie müssen die Anwendung löschen und neu erstellen.

Um die App zu löschen, rufen Sie das [App-Registrierungsportal](https://apps.dev.microsoft.com/) auf und löschen dort die Anwendung. Damit die Anwendung angezeigt wird, müssen Sie der Besitzer der Anwendung sein (und nicht nur Administrator des Mandanten).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über eine bei Azure AD B2C registrierte Anwendung verfügen, können Sie zum Einstieg eines der [Schnellstarttutorials](active-directory-b2c-overview.md#get-started) ausführen.

> [!div class="nextstepaction"]
> [Erstellen einer ASP.NET-Web-App mit Registrierung, Anmeldung und Kennwortzurücksetzung](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
