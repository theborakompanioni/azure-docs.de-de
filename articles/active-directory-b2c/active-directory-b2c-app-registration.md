---
title: 'Azure Active Directory B2C: Registrierung einer Anwendung | Microsoft Docs'
description: Registrieren Ihrer Anwendung bei Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f69
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 4649bec5290e09b8a18e3e12719e63bf5e57c0b0
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017



---
# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrieren der Anwendung

> [!IMPORTANT]
> Anwendungen, die im Azure-Portal auf dem Blatt „Azure AD B2C“ erstellt wurden, müssen am selben Speicherort verwaltet werden. Mit PowerShell oder über ein anderes Portal bearbeitete B2C-Anwendungen werden nicht mehr unterstützt und können nicht mehr mit Azure AD B2C verwendet werden. Unten finden Sie [weitere Informationen](#faulted-apps).
>

## <a name="prerequisite"></a>Voraussetzung

Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md)beschriebenen Schritte. Nachdem Sie alle Schritte in diesem Artikel ausgeführt haben, ist das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren zum Blatt „B2C-Funktionen“

Wenn das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet ist, sehen Sie das Blatt, sobald Sie sich als globaler Administrator des B2C-Mandanten beim [Azure-Portal](https://portal.azure.com/) anmelden.

Sie können auf das Blatt auch zugreifen, indem Sie im [Azure-Portal](https://portal.azure.com/) im linken Navigationsbereich auf **Weitere Dienste** klicken und dann nach **Azure AD B2C** suchen.

> [!IMPORTANT]
> Sie müssen als globaler Administrator des B2C-Mandanten festgelegt sein, um auf das Blade mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff.  Mit dem Mandantenumschalter oben rechts im Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.
>
>

## <a name="register-a-web-application"></a>Registrieren einer Webanwendung

1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
1. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
1. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z. B. „Contoso B2C-App“ eingeben.
1. Stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**.
1. Geben Sie einen [gültigen Wert](#choosing-a-web-app/api-reply-url) für die **Antwort-URLs** ein. Dies sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie z. B. Folgendes ein: `https://localhost:44316/`.
1. Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.
1. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.
1. Falls Ihre Webanwendung auch eine durch Azure AD B2C geschützte Web-API aufruft, ist Folgendes ratsam:
    1. Erstellung eines **Anwendungsgeheimnisses**. Klicken Sie hierzu auf dem Blatt **Schlüssel** auf die Schaltfläche **Schlüssel generieren**.
    1. Klicken Sie auf **API-Zugriff** und dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

> [!NOTE]
> **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden müssen.
>

## <a name="register-a-web-api"></a>Registrieren einer Web-API

1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
1. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
1. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z.B. „Contoso B2C-API“ eingeben.
1. Stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**.
1. Geben Sie einen [gültigen Wert](#choosing-a-web-app/api-reply-url) für die **Antwort-URLs** ein. Dies sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie z. B. Folgendes ein: `https://localhost:44316/`.
1. Geben Sie einen **App-ID-URI** ein. Dies ist der Bezeichner, der für Ihre Web-API verwendet wird. Geben Sie z.B. „notes“ ein. Unten wird dann der vollständige ID-URI generiert.
1. Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.
1. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.
1. Klicken Sie auf **Published scopes** (Veröffentlichte Bereiche). Hier definieren Sie die Berechtigungen (Bereiche), die anderen Anwendungen gewährt werden können.
1. Fügen Sie nach Bedarf weitere Bereiche hinzu. Standardmäßig wird der Bereich „user_impersonation“ definiert. Auf diese Weise können andere Anwendungen im Namen des angemeldeten Benutzers auf diese API zugreifen. Der Bereich kann auch entfernt werden.
1. Klicken Sie auf **Speichern**.

## <a name="register-a-mobilenative-application"></a>Registrieren einer mobilen/systemeigenen Anwendung

1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
1. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
1. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z. B. „Contoso B2C-App“ eingeben.
1. Stellen Sie den Schalter **Nativen Client einschließen** auf **Ja**.
1. Geben Sie einen **Umleitungs-URI** mit einem benutzerdefinierten Schema ein. Beispielsweise „com.onmicrosoft.contoso.appname://redirect/path“. Stellen Sie sicher, dass Sie einen [gültigen Umleitungs-URI](#choosing-a-native-application-redirect-uri) auswählen und keine Sonderzeichen wie Unterstriche eingeben.
1. Klicken Sie auf **Speichern**, um Ihre Anwendung zu registrieren.
1. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.
1. Falls Ihre Webanwendung auch eine durch Azure AD B2C geschützte Web-API aufruft, ist Folgendes ratsam:
    1. Erstellung eines **Anwendungsgeheimnisses**. Klicken Sie hierzu auf dem Blatt **Schlüssel** auf die Schaltfläche **Schlüssel generieren**.
    1. Klicken Sie auf **API-Zugriff** und dann auf **Hinzufügen**, und wählen Sie Ihre Web-API und Bereiche (Berechtigungen) aus.

> [!NOTE]
> **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden müssen.
>

## <a name="limitations"></a>Einschränkungen

### <a name="choosing-a-web-appapi-reply-url"></a>Auswählen einer Antwort-URL für eine Web-App/API

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

### <a name="choosing-a-native-application-redirect-uri"></a>Auswählen eines Umleitungs-URI für eine native Anwendung

Bei der Auswahl eines Umleitungs-URIs für mobile/systemeigene Anwendungen sind zwei Aspekte zu berücksichtigen:

* **Eindeutigkeit**: Das Schema des Umleitungs-URIS muss für jede Anwendung eindeutig sein. In unserem Beispiel (com.onmicrosoft.contoso.appname://redirect/path) verwenden wir „com.onmicrosoft.contoso.appname“ als Schema. Es wird empfohlen, diesem Muster zu folgen. Wenn zwei Anwendungen dasselbe Schema verwenden, wird den Benutzer das Dialogfeld „App auswählen“ angezeigt. Wenn der Benutzer die falsche Auswahl trifft, schlägt die Anmeldung fehl.
* **Vollständigkeit**: Der Umleitungs-URI muss ein Schema und einen Pfad aufweisen. Der Pfad muss nach der Domäne mindestens ein Schrägstrich enthalten (Beispiel: „//contoso/“ funktioniert, aber „//contoso“ schlägt fehl).

Stellen Sie sicher, dass der Umleitungs-URI keine Sonderzeichen wie Unterstriche enthält.

### <a name="faulted-apps"></a>Fehlerhafte Apps

B2C-Anwendungen sollten NICHT wie folgt bearbeitet werden:

* In anderen Portalen für die Anwendungsverwaltung, z.B. im [klassischen Azure-Portal](https://manage.windowsazure.com/) und dem [App-Registrierungsportal](https://apps.dev.microsoft.com/).
* Mithilfe der Graph-API oder PowerShell

Wenn Sie die B2C-Anwendung wie oben beschrieben bearbeiten und sie dann noch einmal auf dem Azure AD B2C-Featureblatt im Azure-Portal bearbeiten möchten, wird sie eine fehlerhafte App, und Ihre Anwendung kann nicht mehr mit Azure AD B2C verwendet werden. Sie müssen die Anwendung löschen und neu erstellen.

Um die App zu löschen, rufen Sie das [App-Registrierungsportal](https://apps.dev.microsoft.com/) auf und löschen dort die Anwendung. Damit die Anwendung angezeigt wird, müssen Sie der Besitzer der Anwendung sein (und nicht nur Administrator des Mandanten).

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun über eine bei Azure AD B2C registrierte Anwendung verfügen, können Sie zum Einstieg eines der [Schnellstarttutorials](active-directory-b2c-overview.md#get-started) ausführen.
