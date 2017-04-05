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
translationtype: Human Translation
ms.sourcegitcommit: 9553c9ed02fa198d210fcb64f4657f84ef3df801
ms.openlocfilehash: 5d15ad7a4e75410390891b5e11f72c6a649ebf53
ms.lasthandoff: 03/23/2017


---


# <a name="azure-ad-b2c-requesting-access-tokens"></a>Azure AD B2C: Anfordern von Zugriffstokens


Ein Zugriffstoken (als **access\_token** bezeichnet) ist eine Form von Sicherheitstoken, mit dem ein Client auf Ressourcen zugreifen kann, die mit einem [Autorisierungsserver](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics) wie etwa einer Web-API geschützt sind. Zugriffstokens werden als [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) dargestellt und enthalten Informationen zu dem gewünschten Ressourcenserver und den für den Server erteilten Berechtigungen. Beim Aufrufen des Ressourcenservers muss das Zugriffstoken in der HTTP-Anforderung vorhanden sein.

In diesem Artikel wird erläutert, wie eine Clientanwendung konfiguriert und eine Anforderung zum Abrufen eines **access\_token** von den Endpunkten `authorize` und `token` getätigt wird.

## <a name="prerequisite"></a>Voraussetzung

Vor der Anforderung eines Zugriffstokens müssen Sie zuerst eine Web-API registrieren und Berechtigungen veröffentlichen, die für die Clientanwendung erteilt werden können. Befolgen Sie die Schritte im Abschnitt [Registrieren einer Web-API](active-directory-b2c-app-registration.md), um zu beginnen.

## <a name="granting-permissions-to-a-web-api"></a>Erteilen von Berechtigungen für eine Web-API

Damit eine Clientanwendung bestimmte Berechtigungen für eine API beziehen kann, müssen der Clientanwendung diese Berechtigungen über das Azure-Portal erteilt werden. So erteilen Sie Berechtigungen für eine Clientanwendung:

1. Navigieren Sie zum Menü **Anwendungen** auf dem Blatt „B2C-Funktionen“.
2. Klicken Sie auf die Clientanwendung ([registrieren Sie eine Anwendung](active-directory-b2c-app-registration.md), wenn Sie noch keine besitzen).
3. Wählen Sie **API-Zugriff**.
4. Klicken Sie auf **Hinzufügen**.
5. Wählen Sie Ihre Web-API und die Bereiche (Berechtigungen), die Sie erteilen möchten.
6. Klicken Sie auf **OK**.

> [!NOTE]
> Azure AD B2C bittet die Benutzer Ihrer Clientanwendung nicht um deren Zustimmung. Stattdessen erfolgt die Zustimmung abhängig von den Berechtigungen, die zwischen den oben beschriebenen Anwendungen konfiguriert sind, ausschließlich über den Administrator. Wenn eine Berechtigung für eine Anwendung widerrufen wurde, wird diese Berechtigung allen Benutzern, die zuvor diese Berechtigung beziehen konnten, verwehrt.

## <a name="requesting-a-token"></a>Anfordern eines Tokens

Um einen Zugriffstoken für eine Ressourcenanwendung zu erhalten, muss die Clientanwendung die im **scope**-Parameter der Anforderung gewünschten Berechtigungen angeben. Um beispielsweise die Leseberechtigung für die Ressourcenanwendung mit dem App-ID-URI `https://contoso.onmicrosoft.com/notes` abzurufen, würde der Bereich `https://contoso.onmicrosoft.com/notes/read` lauten. Nachfolgend wird ein Beispiel für eine Autorisierungscodeanforderung an den Endpunkt `authorize` vorgestellt.

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
