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
ms.date: 03/16/2017
ms.author: parakhj
ms.translationtype: Human Translation
ms.sourcegitcommit: 80be19618bd02895d953f80e5236d1a69d0811af
ms.openlocfilehash: 7a28c92e921354cd4a623af29098a40e1c8b56b0
ms.contentlocale: de-de
ms.lasthandoff: 06/07/2017


---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Anfordern von Zugriffstokens

Ein Zugriffstoken (als **access\_token** bezeichnet) ist eine Form von Sicherheitstoken, mit dem ein Client auf Ressourcen zugreifen kann, die mit einem [Autorisierungsserver](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics) wie etwa einer Web-API geschützt sind. Zugriffstokens werden als [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) dargestellt und enthalten Informationen zu dem gewünschten Ressourcenserver und den für den Server erteilten Berechtigungen. Beim Aufrufen des Ressourcenservers muss das Zugriffstoken in der HTTP-Anforderung vorhanden sein.

In diesem Artikel wird erläutert, wie eine Clientanwendung konfiguriert und eine Anforderung zum Abrufen eines **access\_token** von den Endpunkten `authorize` und `token` getätigt wird.

> [!NOTE]
> **Web-API-Ketten (im Auftrag von) werden von Azure AD B2C nicht unterstützt.**
>
> Viele Architekturen umfassen eine Web-API, von der eine andere Downstream-Web-API aufgerufen werden muss, wobei beide durch Azure AD B2C gesichert sind. Dieses Szenario kommt häufig bei nativen Clients mit Web-API-Back-End vor, das wiederum einen Microsoft Online-Dienst aufruft, z. B. die Azure AD Graph-API.
>
> Dieses Szenario der verketteten Web-API kann mithilfe der Berechtigung für Anmeldeinformationen über den OAuth 2.0-JWT-Bearer unterstützt werden, auch bekannt als „Im Auftrag von“-Ablauf. Der „Im Auftrag von“-Ablauf ist in Azure AD B2C derzeit noch nicht implementiert.

## <a name="prerequisite"></a>Voraussetzung

Vor der Anforderung eines Zugriffstokens müssen Sie zuerst eine Web-API registrieren und Berechtigungen veröffentlichen, die für die Clientanwendung erteilt werden können. Befolgen Sie die Schritte im Abschnitt [Registrieren einer Web-API](active-directory-b2c-app-registration.md#register-a-web-api), um zu beginnen.

## <a name="granting-permissions-to-a-web-api"></a>Erteilen von Berechtigungen für eine Web-API

Damit eine Clientanwendung bestimmte Berechtigungen für eine API beziehen kann, müssen der Clientanwendung diese Berechtigungen über das Azure-Portal erteilt werden. So erteilen Sie Berechtigungen für eine Clientanwendung:

1. Navigieren Sie zum Menü **Anwendungen** auf dem Blatt „B2C-Funktionen“.
1. Sofern nicht bereits geschehen, registrieren Sie eine Clientanwendung ([Web-App](active-directory-b2c-app-registration.md#register-a-web-application) oder [Nativer Client](active-directory-b2c-app-registration.md#register-a-mobilenative-application)).
1. Wählen Sie auf dem Blatt „Einstellungen“ Ihrer Anwendung die Option **API-Zugriff** aus.
1. Klicken Sie auf **Hinzufügen**.
1. Wählen Sie Ihre Web-API und die Bereiche (Berechtigungen), die Sie erteilen möchten.
1. Klicken Sie auf **OK**.

> [!NOTE]
> Azure AD B2C bittet die Benutzer Ihrer Clientanwendung nicht um deren Zustimmung. Stattdessen erfolgt die Zustimmung abhängig von den Berechtigungen, die zwischen den oben beschriebenen Anwendungen konfiguriert sind, ausschließlich über den Administrator. Wenn eine Berechtigung für eine Anwendung widerrufen wurde, wird diese Berechtigung allen Benutzern, die zuvor diese Berechtigung beziehen konnten, verwehrt.

## <a name="requesting-a-token"></a>Anfordern eines Tokens

Um ein Zugriffstoken für eine Ressourcenanwendung zu erhalten, muss die Clientanwendung die im **scope**-Parameter der Anforderung gewünschten Berechtigungen angeben. Um beispielsweise die Leseberechtigung für die Ressourcenanwendung mit dem App-ID-URI `https://contoso.onmicrosoft.com/notes` abzurufen, würde der Bereich `https://contoso.onmicrosoft.com/notes/read` lauten. Nachfolgend wird ein Beispiel für eine Autorisierungscodeanforderung an den Endpunkt `authorize` vorgestellt.

> [!NOTE]
> An diesem Punkt werden benutzerdefinierte Domänen und Zugriffstoken nicht unterstützt. Sie müssen Ihre Domäne yourtenantId.onmicrosoft.com in der Anforderungs-URL verwenden.

```
https://login.microsoftonline.com/<yourTenantId>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
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

Möglicherweise müssen Sie mehr Bereiche/Berechtigungen für eine Ressource anfordern, als für Ihre Clientanwendung erteilt wurden. In diesem Fall wird der Aufruf erfolgreich ausgeführt, wenn mindestens eine Berechtigung erteilt wird. Der „scp“-Anspruch des resultierenden **access\_token** wird nur bei den Berechtigungen aufgefüllt, die erfolgreich erteilt wurden.

> [!NOTE] 
> Die Anforderung von Berechtigungen für zwei verschiedene Webressourcen in derselben Anforderung wird nicht unterstützt. Derartige Anforderungen schlagen fehl.

### <a name="special-cases"></a>Spezialfälle

Der OpenID Connect-Standard gibt mehrere spezielle „scope“-Werte an. Die folgenden speziellen Bereiche stellen die Berechtigung für den Zugriff auf das Profil des Benutzers dar:

* **openid**: Hierdurch wird ein ID-Token angefordert.
* **offline\_access**: Hierdurch wird ein Aktualisierungstoken (mithilfe von Autorisierungscodeabläufen) angefordert.

Wenn der Parameter „response\_type“ in einer `authorize`-Anforderung „token“ enthält, muss der Parameter „scope“ mindestens eine Ressourcenberechtigung enthalten (außer „openid“ und „offline\_access“), die erteilt wird. Andernfalls wird die `authorize` Anforderung mit einem Fehler beendet.

## <a name="the-returned-token"></a>Das zurückgegebene Token

In einem erfolgreich angewendeten **access\_token** (entweder vom Endpunkt `authorize` oder `token`) sind folgende Ansprüche vorhanden:

| Name | Anspruch | Beschreibung |
| --- | --- | --- |
|Zielgruppe |`aud` |Die \*Anwendungs-ID\* der jeweiligen Ressource, auf die das Token Zugriff gewährt. |
|Umfang |`scp` |Die Berechtigungen, die der Ressource erteilt werden. Mehrere erteilte Berechtigungen werden durch ein Leerzeichen voneinander getrennt. |
|Autorisierte Partei |`azp` |Die \*Anwendungs-ID\* der Clientanwendung, die die Anforderung initiiert hat. |

Wenn Ihre API das **access\_token** erhält, muss diese das [Token überprüfen](active-directory-b2c-reference-tokens.md), um die Authentizität des Tokens und die Richtigkeit der Ansprüche nachzuweisen.

Wir sind stets offen für Feedback und Vorschläge. Wenn Sie Probleme mit diesem Thema oder Vorschläge zur Verbesserung dieses Inhalts haben, würden wir uns über Ihr Feedback unten auf der Seite freuen. Anforderungen neuer Features geben Sie bitte unter [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) ein.

