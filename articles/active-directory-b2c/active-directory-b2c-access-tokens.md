---
title: 'Azure Active Directory B2C: Anfordern von Zugriffstokens | Microsoft-Dokumentation'
description: "In diesem Artikel wird erläutert, wie eine Clientanwendung eingerichtet und ein Zugriffstoken abgerufen wird."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: 1c75f17f-5ec5-493a-b906-f543b3b1ea66
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: parakhj
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 4b361a8e69f885d5b89ac9b2086e2731ee4d8b48
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Anfordern von Zugriffstokens

Ein Zugriffstoken (in den Antworten von Azure AD B2C als **access\_token** bezeichnet) ist eine Form von Sicherheitstoken, mit dem ein Client auf Ressourcen zugreifen kann, die mit einem [Autorisierungsserver](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics) wie etwa einer Web-API geschützt sind. Zugriffstokens werden als [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) dargestellt und enthalten Informationen zu dem gewünschten Ressourcenserver und den für den Server erteilten Berechtigungen. Beim Aufrufen des Ressourcenservers muss das Zugriffstoken in der HTTP-Anforderung vorhanden sein.

In diesem Artikel wird das Konfigurieren einer Clientanwendung und Web-API zum Abrufen eines **access\_token** erläutert.

> [!NOTE]
> **Web-API-Ketten (im Auftrag von) werden von Azure AD B2C nicht unterstützt.**
>
> Viele Architekturen umfassen eine Web-API, die eine andere Downstream-Web-API aufrufen muss, wobei beide durch Azure AD B2C gesichert sind. Dieses Szenario kommt häufig bei nativen Clients mit Web-API-Back-End vor, das wiederum einen Microsoft Online-Dienst aufruft, z.B. die Azure AD Graph-API.
>
> Dieses Szenario der verketteten Web-API kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0-JWT-Bearer unterstützt werden, auch bekannt als „Im Auftrag von“-Ablauf. Der „Im Auftrag von“-Ablauf ist in Azure AD B2C derzeit noch nicht implementiert.

## <a name="register-a-web-api-and-publish-permissions"></a>Registrieren einer Web-API und Veröffentlichen von Berechtigungen

Vor der Anforderung eines Zugriffstokens müssen Sie zuerst eine Web-API registrieren und Berechtigungen veröffentlichen (Bereiche), die für die Clientanwendung erteilt werden können.

### <a name="register-a-web-api"></a>Registrieren einer Web-API

1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
1. Klicken Sie oben auf dem Blatt auf **+Hinzufügen** .
1. Geben Sie einen **Namen** für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z.B. „Contoso API“ eingeben.
1. Stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**.
1. Geben Sie einen beliebigen Wert für die **Antwort-URLs** ein. Geben Sie z. B. Folgendes ein: `https://localhost:44316/`. Der Wert spielt keine Rolle, da eine API das Token nicht direkt von Azure AD B2C empfangen sollte.
1. Geben Sie einen **App-ID-URI** ein. Dies ist der Bezeichner, der für Ihre Web-API verwendet wird. Geben Sie z.B. „notes“ in das Feld ein. Der **App-ID-URI** wäre dann `https://{tenantName}.onmicrosoft.com/notes`.
1. Klicken Sie auf **Erstellen** , um Ihre Anwendung zu registrieren.
1. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID** zur späteren Verwendung in Ihrem Code.

### <a name="publishing-permissions"></a>Veröffentlichungsberechtigungen

Zu Berechtigungen analoge Bereiche sind erforderlich, wenn Ihre App eine API aufruft. Einige Beispiele für Bereiche sind „read“ oder „write“. Angenommen, Sie möchten, dass Ihre Web oder native App aus einer API „liest“. Ihre App würde Azure AD B2C aufrufen und ein Zugriffstoken anfordern, das Zugriff auf den Bereich „read“ gewährt. Damit Azure AD B2C ein solches Zugriffstoken ausgeben kann, muss der App die Berechtigung „read“ aus der bestimmten API gewährt werden. Zu diesem Zweck muss Ihre API zuerst den Bereich „read“ veröffentlichen.

1. Öffnen Sie im Blatt **Anwendungen** von Azure AD B2C die Web-API-Anwendung („Contoso-API“).
1. Klicken Sie auf **Published scopes** (Veröffentlichte Bereiche). Hier definieren Sie die Berechtigungen (Bereiche), die anderen Anwendungen gewährt werden können.
1. Fügen Sie **Bereichswerte** (z.B. „read“) nach Bedarf hinzu. Standardmäßig wird der Bereich „user_impersonation“ definiert. Wenn Sie möchten, können Sie dies ignorieren. Geben Sie eine Beschreibung des Bereichs in die Spalte **Bereichsname** ein.
1. Klicken Sie auf **Speichern**.

> [!IMPORTANT]
> Der **Bereichsname** ist die Beschreibung für den **Bereichswert**. Wenn Sie den Bereich verwenden, achten Sie darauf, den **Bereichswert** zu verwenden.

## <a name="grant-a-native-or-web-app-permissions-to-a-web-api"></a>Erteilen von nativen oder Web-App-Berechtigungen für eine Web-API

Sobald eine API konfiguriert ist, um Bereiche zu veröffentlichen, müssen der Clientanwendung Berechtigungen für diese Bereiche über das Azure Portal erteilt werden.

1. Navigieren Sie zum Menü **Anwendungen** auf dem Blatt „B2C-Funktionen“.
1. Sofern nicht bereits geschehen, registrieren Sie eine Clientanwendung ([Web-App](active-directory-b2c-app-registration.md#register-a-web-app) oder [Nativer Client](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)). Wenn Sie dieses Handbuch als Ausgangspunkt nutzen, müssen Sie eine Clientanwendung registrieren.
1. Klicken Sie auf **API-Zugriff**.
1. Klicken Sie auf **Hinzufügen**.
1. Wählen Sie Ihre Web-API und die Bereiche (Berechtigungen), die Sie erteilen möchten.
1. Klicken Sie auf **OK**.

> [!NOTE]
> Azure AD B2C bittet die Benutzer Ihrer Clientanwendung nicht um deren Zustimmung. Stattdessen erfolgt die Zustimmung abhängig von den Berechtigungen, die zwischen den oben beschriebenen Anwendungen konfiguriert sind, ausschließlich über den Administrator. Wenn eine Berechtigung für eine Anwendung widerrufen wurde, wird diese Berechtigung allen Benutzern, die zuvor diese Berechtigung beziehen konnten, verwehrt.

## <a name="requesting-a-token"></a>Anfordern eines Tokens

Bei der Anforderung eines Zugriffstokens muss die Clientanwendung die im **scope**-Parameter der Anforderung gewünschten Berechtigungen angeben. Um z.B. den **Bereichswert** „read“ für die API anzugeben, die den **App-ID-URI** von `https://contoso.onmicrosoft.com/notes` hat, wäre der Bereich `https://contoso.onmicrosoft.com/notes/read`. Nachfolgend wird ein Beispiel für eine Autorisierungscodeanforderung an den Endpunkt `/authorize` vorgestellt.

> [!NOTE]
> Derzeit werden benutzerdefinierte Domänen und Zugriffstoken nicht unterstützt. Sie müssen Ihre Domäne tenantName.onmicrosoft.com in der Anforderungs-URL verwenden.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

Um mehrere Berechtigungen in derselben Anforderung zu beziehen, können Sie mehrere durch Leerzeichen getrennte Einträge in einem einzelnen **scope**-Parameter hinzufügen. Beispiel:

URL dekodiert:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

URL codiert:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

Möglicherweise müssen Sie mehr Bereiche (Berechtigungen) für eine Ressource anfordern, als für Ihre Clientanwendung erteilt wurden. In diesem Fall wird der Aufruf erfolgreich ausgeführt, wenn mindestens eine Berechtigung erteilt wird. Der „scp“-Anspruch des resultierenden **access\_token** wird nur bei den Berechtigungen aufgefüllt, die erfolgreich erteilt wurden.

> [!NOTE] 
> Die Anforderung von Berechtigungen für zwei verschiedene Webressourcen in derselben Anforderung wird nicht unterstützt. Derartige Anforderungen schlagen fehl.

### <a name="special-cases"></a>Spezialfälle

Der OpenID Connect-Standard gibt mehrere spezielle „scope“-Werte an. Die folgenden speziellen Bereiche stellen die Berechtigung für den Zugriff auf das Profil des Benutzers dar:

* **openid**: Hierdurch wird ein ID-Token angefordert.
* **offline\_access**: Hierdurch wird ein Aktualisierungstoken (mithilfe von [Autorisierungscodeabläufen](active-directory-b2c-reference-oauth-code.md)) angefordert.

Wenn der `response_type`-Parameter in einer `/authorize`-Anforderung `token` enthält, muss der `scope` -Parameter mindestens einen Ressourcenbereich (außer `openid` und `offline_access`) enthalten, für den Berechtigungen gewährt werden. Andernfalls wird die `/authorize` Anforderung mit einem Fehler beendet.

## <a name="the-returned-token"></a>Das zurückgegebene Token

In einem erfolgreich angewendeten **access\_token** (entweder vom Endpunkt `/authorize` oder `/token`) sind folgende Ansprüche vorhanden:

| Name | Anspruch | Beschreibung |
| --- | --- | --- |
|Zielgruppe |`aud` |Die **Anwendungs-ID** der jeweiligen Ressource, auf die das Token Zugriff gewährt. |
|Umfang |`scp` |Die Berechtigungen, die der Ressource erteilt werden. Mehrere erteilte Berechtigungen werden durch ein Leerzeichen voneinander getrennt. |
|Autorisierte Partei |`azp` |Die **Anwendungs-ID** der Clientanwendung, die die Anforderung initiiert hat. |

Wenn Ihre API das **access\_token** erhält, muss diese das [Token überprüfen](active-directory-b2c-reference-tokens.md), um die Authentizität des Tokens und die Richtigkeit der Ansprüche nachzuweisen.

Wir sind stets offen für Feedback und Vorschläge. Wenn Sie Probleme mit diesem Thema haben, posten Sie mit demTag [„azure-ad-b2c“](https://stackoverflow.com/questions/tagged/azure-ad-b2c) einen Beitrag auf Stack Overflow. Anforderungen neuer Features geben Sie bitte unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) ein.

## <a name="next-steps"></a>Nächste Schritte

* Erstellen einer Web-API mit [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Erstellen einer Web-API mit [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)

