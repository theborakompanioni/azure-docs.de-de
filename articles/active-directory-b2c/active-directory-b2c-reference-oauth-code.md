---
title: 'Azure Active Directory B2C: Autorisierungscodeflow | Microsoft-Dokumentation'
description: In diesem Artikel erfahren Sie, wie Web-Apps mit der Azure Active Directory-Implementierung des OpenID Connect-Authentifizierungsprotokolls erstellt werden.
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c371aaab-813a-4317-97df-b62e2f53d865
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 50ac9a0d95a581e696817364e94134abc089bfdf
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0-Autorisierungscodefluss
Durch Gewähren des OAuth 2.0-Autorisierungcodes in Apps, die auf einem Gerät installiert sind, können Sie auf geschützte Ressourcen wie Web-APIs zugreifen. Mithilfe der Azure Active Directory B2C(Azure AD B2C)-Implementierung von OAuth 2.0 können Sie Ihre mobilen und Desktop-Apps Aufgaben zur Registrierung, Anmeldung und Identitätsverwaltung hinzufügen. Dieser Artikel ist sprachunabhängig. In dem Artikel wird beschrieben, wie HTTP-Nachrichten ohne Verwendung von Open Source-Bibliotheken gesendet und empfangen werden.

<!-- TODO: Need link to libraries -->

Der OAuth 2.0-Autorisierungscodefluss wird in [Abschnitt 4.1 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben. Sie können ihn zum Ausführen der Authentifizierung und Autorisierung bei den meisten App-Typen nutzen, einschließlich [Web-Apps](active-directory-b2c-apps.md#web-apps) und [nativ installierten Apps](active-directory-b2c-apps.md#mobile-and-native-apps). Über den OAuth 2.0-Autorisierungscodeflow können Sie sicher *Zugriffstokens* für Ihre Apps abrufen, die zum Zugriff auf mit einem [Autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics) geschützten Ressourcen verwendet werden können.

Dieser Artikel behandelt den OAuth 2.0-Autorisierungscodeflow von **öffentlichen Clients**. Ein öffentlicher Client ist jede Clientanwendung, der nicht bei der sicheren Verwaltung der Integrität von geheimen Kennwörtern vertraut werden kann. Dazu gehören mobile Apps, Desktop-Apps und im Wesentlichen jede Anwendung, die auf einem Gerät ausgeführt wird und Zugriffstokens abrufen muss. 

> [!NOTE]
> Wenn Sie in einer Web-App mit Azure AD B2C eine Identitätsverwaltung hinzufügen möchten, verwenden Sie [OpenID Connect](active-directory-b2c-reference-oidc.md) anstelle von OAuth 2.0.

Azure AD B2C erweitert den OAuth 2.0-Standardfluss, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Dabei wird der [Richtlinienparameter](active-directory-b2c-reference-policies.md) eingeführt. Durch integrierte Richtlinien können Sie mit OAuth 2.0 Funktionen für die Benutzeroberfläche zu Ihrer App hinzufügen, z.B. Registrierungs-, Anmelde- und Profilverwaltungsfunktionen. In diesem Artikel zeigen wir, wie Sie mit OAuth 2.0 und Richtlinien diese Benutzeroberflächenfunktionen in Ihre nativen Anwendungen implementieren. Wir zeigen Ihnen außerdem, wie Zugriffstokens für den Zugriff auf Web-APIs abgerufen werden.

In den HTTP-Beispielanforderungen in diesem Artikel wird das Azure AD B2C-Beispielverzeichnis **fabrikamb2c.onmicrosoft.com** verwendet. Zudem kommen unsere Beispielanwendung und -richtlinien zum Einsatz. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene Azure AD B2C-Verzeichnisse, -Anwendungen und -Richtlinien abrufen](#use-your-own-azure-ad-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. Abrufen von Autorisierungscodes
Der Autorisierungscodefluss beginnt damit, dass der Client den Benutzer auf den `/authorize` -Endpunkt leitet. Dies ist der interaktive Teil des Flows, bei dem der Benutzer tatsächlich Aktionen durchführt. In dieser Anforderung gibt der Client im Parameter `scope` die Berechtigungen an, die er vom Benutzer abrufen muss. Er gibt im Parameter `p` die auszuführende Richtlinie an. In den folgenden drei Beispielen (mit Zeilenumbrüchen für bessere Lesbarkeit) wird jeweils eine andere Richtlinie verwendet.

### <a name="use-a-sign-in-policy"></a>Verwenden einer Anmelderichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-policy"></a>Verwenden einer Registrierungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-policy"></a>Verwenden einer Profilbearbeitungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die Ihrer App im [Azure-Portal](https://portal.azure.com) zugewiesen wird. |
| response_type |Erforderlich |Der Antworttyp, der `code` für den Autorisierungscodefluss enthalten muss. |
| redirect_uri |Erforderlich |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure Active Directory (Azure AD) an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein Zugriffstoken erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit derselben Client-ID verwendet werden kann.  Der Bereich `offline_access` gibt an, dass Ihre App ein Aktualisierungstoken für den langfristigen Zugriff auf Ressourcen benötigt. Außerdem können Sie den Bereich `openid` verwenden, um ein ID-Token von Azure AD B2C anzufordern. |
| response_mode |Empfohlen |Die Methode, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet wird. Es kann sich um `query`, `form_post` oder `fragment` handeln. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen zu verwendenden Inhalt handeln. Normalerweise wird ein zufällig generierter eindeutiger Wert verwendet, um eine websiteübergreifende Anforderungsfälschung zu verhindern. Der Status wird außerdem verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist. (z.B. die Seite, die der Benutzer besucht hat, oder die ausgeführte Richtlinie). |
| p |Erforderlich |Die Richtlinie, die ausgeführt wird. Dies ist der Name einer Richtlinie, die in Ihrem Azure AD B2C-Verzeichnis erstellt wird. Der Wert für den Richtliniennamen muss mit **b2c\_1\_** beginnen. Weitere Informationen zu Richtlinien finden Sie unter [Integrierte Azure AD B2C-Richtlinien](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Gegenwärtig ist der einzige gültige Wert `login`, der den Benutzer zur Eingabe seiner Anmeldeinformationen für diese Anforderung zwingt. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie durchzuführen. Dafür muss der Benutzer z.B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen. Die Benutzeraktionen hängen davon ab, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD an Ihre App unter dem für `redirect_uri` verwendeten Wert eine Antwort zurück. Hierzu wird die im Parameter `response_mode` angegebene Methode verwendet. Die Antwort ist in den oben aufgeführten Benutzeraktionsszenarien immer gleich, unabhängig von der ausgeführten Richtlinie.

Eine erfolgreiche Antwort mit `response_mode=query` sieht wie folgt aus:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschreibung |
| --- | --- |
| Code |Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für eine Zielressource verwenden. Autorisierungscodes sind von sehr kurzer Lebensdauer. In der Regel laufen sie nach etwa 10 Minuten ab. |
| state |Die vollständige Beschreibung finden Sie im vorangehenden Abschnitt. Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die App sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |

Fehlerantworten können auch an den Umleitungs-URI gesendet werden, damit die App diese entsprechend behandeln kann:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren. Sie können mit der Zeichenfolge auch auf Fehler reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| state |Die vollständige Beschreibung finden Sie in der obigen Tabelle. Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die App sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |

## <a name="2-get-a-token"></a>2. Abrufen von Token
Nachdem Sie einen Autorisierungscode abgerufen haben, können Sie den `code` für ein Token für die gewünschte Ressource einlösen, indem Sie eine POST-Anforderung an den `/token`-Endpunkt senden. In Azure AD B2C ist die einzige Ressource, für die Sie ein Token anfordern können, die Back-End-Web-API Ihrer App. Für das Anfordern eines Tokens für sich selbst wird die Client-ID Ihrer App als Bereich verwendet:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des Autorisierungscodes verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der *Abfragezeichenfolge*hinzufügen, nicht im POST-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die Ihrer App im [Azure-Portal](https://portal.azure.com) zugewiesen wird. |
| grant_type |Erforderlich |Der Gewährungstyp. Beim Autorisierungscodefluss muss der Gewährungstyp `authorization_code` sein. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein Zugriffstoken erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit derselben Client-ID verwendet werden kann.  Der Bereich `offline_access` gibt an, dass Ihre App ein Aktualisierungstoken für den langfristigen Zugriff auf Ressourcen benötigt.  Außerdem können Sie den Bereich `openid` verwenden, um ein ID-Token von Azure AD B2C anzufordern. |
| Code |Erforderlich |Der Autorisierungscode, den Sie im ersten Abschnitt des Flows abgerufen haben. |
| redirect_uri |Erforderlich |Der Umleitungs-URI der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |

Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| --- | --- |
| not_before |Der Zeitpunkt in Epochenzeit, ab dem das Token gültig ist. |
| token_type |Der Wert des Tokentyps. Bearertoken ist der einzige Typ, den Azure AD unterstützt. |
| access_token |Das signierte JWT-Token (JSON Web Token), das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gültig ist. Sie können in den Bereichen auch Token für die spätere Verwendung zwischenspeichern. |
| expires_in |Gibt an, wie lange das Token gültig ist (in Sekunden). |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten. Aktualisierungstoken sind langlebig. Sie können diese verwenden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Informationen finden Sie unter [Azure AD B2C-Tokenverweise](active-directory-b2c-reference-tokens.md). |

Fehlerantworten sehen wie folgt aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren. Sie können mit der Zeichenfolge auch auf Fehler reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

## <a name="3-use-the-token"></a>3. Verwenden des Tokens
Nachdem Sie ein Zugriffstoken abgerufen haben, können Sie das Token für Anforderungen an die Back-End-Web-APIs verwenden, indem Sie es in den `Authorization`-Header einfügen:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Aktualisieren des Tokens
Zugriffs- und ID-Tokens sind kurzlebig. Nach ihrem Ablauf müssen Sie sie aktualisieren, um weiterhin auf Ressourcen zugreifen zu können. Übermitteln Sie zu diesem Zweck eine andere POST-Anforderung an den `/token`-Endpunkt. Geben Sie dieses Mal `refresh_token` anstelle von `code` an:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des ursprünglichen Aktualisierungstokens verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der *Abfragezeichenfolge*hinzufügen, nicht im POST-Text. |
| client_id |Empfohlen |Die Anwendungs-ID, die Ihrer App im [Azure-Portal](https://portal.azure.com) zugewiesen wird. |
| grant_type |Erforderlich |Der Gewährungstyp. Bei diesem Abschnitt des Autorisierungscodeflows muss der Gewährungstyp `refresh_token` sein. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein Zugriffstoken erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit derselben Client-ID verwendet werden kann.  Der Bereich `offline_access` gibt an, dass Ihre App ein Aktualisierungstoken für den langfristigen Zugriff auf Ressourcen benötigt.  Außerdem können Sie den Bereich `openid` verwenden, um ein ID-Token von Azure AD B2C anzufordern. |
| redirect_uri |Optional |Der Umleitungs-URI der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| refresh_token |Erforderlich |Das ursprüngliche Aktualisierungstoken, das Sie im zweiten Abschnitt des Flows abgerufen haben. |

Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameter | Beschreibung |
| --- | --- |
| not_before |Der Zeitpunkt in Epochenzeit, ab dem das Token gültig ist. |
| token_type |Der Wert des Tokentyps. Bearertoken ist der einzige Typ, den Azure AD unterstützt. |
| access_token |Das signierte JWT-Token, das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gültig ist. Sie können in den Bereichen auch Tokens für die spätere Verwendung zwischenspeichern. |
| expires_in |Gibt an, wie lange das Token gültig ist (in Sekunden). |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Informationen finden Sie unter [Azure AD B2C-Tokenverweise](active-directory-b2c-reference-tokens.md). |

Fehlerantworten sehen wie folgt aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren. Sie können mit der Zeichenfolge auch auf Fehler reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

## <a name="use-your-own-azure-ad-b2c-directory"></a>Verwenden Sie Ihr eigenes Azure AD B2C-Verzeichnis.
Führen Sie die folgenden Schritte durch, um diese Anforderungen selbst zu testen. Ersetzen Sie die in diesem Artikel verwendeten Beispielwerte durch Ihre eigenen Werte.

1. [Erstellen Sie ein Azure AD B2C-Verzeichnis](active-directory-b2c-get-started.md). Verwenden Sie den Namen Ihres Verzeichnisses in den Anforderungen.
2. [Erstellen Sie eine Anwendung](active-directory-b2c-app-registration.md) zum Abrufen einer Anwendungs-ID und eines Umleitungs-URI. Sie können Ihrer App einen nativen Client hinzufügen.
3. [Erstellen der Richtlinien](active-directory-b2c-reference-policies.md) zum Abrufen der Richtliniennamen.


