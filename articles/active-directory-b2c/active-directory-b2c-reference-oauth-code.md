---
title: Azure Active Directory B2C | Microsoft Docs
description: Erstellen von Webanwendungen mit der Azure Active Directory-Implementierung des OpenID Connect-Authentifizierungsprotokolls.
services: active-directory-b2c
documentationcenter: ''
author: dstrockis
manager: mbaldwin
editor: ''

ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/22/2016
ms.author: dastrock

---
# <a name="azure-active-directory-b2c:-oauth-2.0-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0-Autorisierungscodefluss
Der OAuth 2.0-Autorisierungcodefluss kann in Apps verwendet werden, die auf einem Gerät installiert sind, um Zugriff auf geschützte Ressourcen wie Web-APIs zu gewähren. Mithilfe der Azure Active Directory (Azure AD) B2C-Implementierung von OAuth 2.0 können Sie Ihren Desktop- und mobilen Apps Aufgaben für Registrierung, Anmeldung und Identitätsverwaltung hinzufügen. Dieser Leitfaden ist sprachunabhängig. Er beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

<!-- TODO: Need link to libraries -->

Der OAuth 2.0-Autorisierungscodefluss wird in [Abschnitt 4.1 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben. Sie können ihn zum Ausführen der Authentifizierung und Autorisierung in der Mehrzahl der App-Typen nutzen, einschließlich [Web-Apps](active-directory-b2c-apps.md#web-apps) und [nativ installierter Apps](active-directory-b2c-apps.md#mobile-and-native-apps). Durch den Codefluss können Apps **Zugriffstoken** sicher abrufen, die für den Zugriff auf Ressourcen verwendet werden können, für deren Schutz ein [Autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics) genutzt wird.

Dieser Leitfaden konzentriert sich auf einen speziellen Aspekt des OAuth 2.0-Autorisierungscodeflusses – die**öffentlichen Clients**. Ein öffentlicher Client ist jede Clientanwendung, der nicht bei der sicheren Verwaltung der Integrität von geheimen Kennwörtern vertraut werden kann. Dazu gehören mobile Apps, Desktop-Apps und nahezu jede Anwendung, die auf einem Gerät ausgeführt wird und Zugriffstoken abrufen muss. Wenn Sie in einer Web-App mit Azure AD B2C eine Identitätsverwaltung einfügen möchten, verwenden Sie [OpenID Connect](active-directory-b2c-reference-oidc.md) anstelle von OAuth 2.0.

Azure AD B2C erweitert den OAuth 2.0-Standardfluss, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Dazu werden [**Richtlinienparameter**](active-directory-b2c-reference-policies.md) eingeführt, mit denen Sie OAuth 2.0 zum Hinzufügen von Benutzeroberflächen für die Registrierung, Anmeldung und Profilverwaltung zu Ihrer App verwenden können. Hier zeigen wir, wie Sie mit OAuth 2.0 und Richtlinien diese Benutzeroberflächen in nativen Anwendungen implementieren. Wir zeigen außerdem, wie Zugriffstoken für den Zugriff auf Web-APIs abgerufen werden.

Die HTTP-Beispielanforderungen unten verwenden unser B2C-Beispielverzeichnis **fabrikamb2c.onmicrosoft.com**sowie unsere Beispielanwendung und die Richtlinien. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene B2C-Verzeichnisse, -Anwendungen und -Richtlinien erstellen](#use-your-own-b2c-directory).

## <a name="1.-get-an-authorization-code"></a>1. Abrufen von Autorisierungscodes
Der Autorisierungscodefluss beginnt damit, dass der Client den Benutzer auf den `/authorize`-Endpunkt leitet. Dies ist der interaktive Teil des Datenflusses, bei dem der Benutzer tatsächlich Aktionen durchführt. In dieser Anforderung gibt der Client die Berechtigungen, die er vom Benutzer benötigt, im `scope`-Parameter und die anzuwendende Richtlinie im `p`-Parameter an. Unten sind drei Beispiele aufgeführt (mit Zeilenumbrüchen für bessere Lesbarkeit), die jeweils eine andere Richtlinie verwenden.

#### <a name="use-a-sign-in-policy"></a>Verwenden einer Anmelderichtlinie
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

#### <a name="use-a-sign-up-policy"></a>Verwenden einer Registrierungsrichtlinie
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

#### <a name="use-an-edit-profile-policy"></a>Verwenden einer Profilbearbeitungsrichtlinie
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
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com) Ihrer App zugewiesen hat. |
| response_type |Erforderlich |Der Antworttyp, der `code` für den Autorisierungscodefluss enthalten muss. |
| redirect_uri |Erforderlich |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein **Zugriffstoken** erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit der gleichen Client-ID verwendet werden kann.  Der `offline_access`-Bereich gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt.  Außerdem können Sie den Bereich `openid` verwenden, um ein **ID-Token** von Azure AD B2C anzufordern. |
| response_mode |Empfohlen |Die Methode, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll. Dies kann entweder „query“, „form_post“ oder „fragment“ sein. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern. Der Status wird auch zum Codieren von Informationen über den Status des Benutzers in der App vor der Authentifizierungsanforderung verwendet, z. B. für Informationen zu der Seite, die der Benutzer besucht hat, oder zur ausgeführten Richtlinie. |
| p |Erforderlich |Die Richtlinie, die ausgeführt wird. Dies ist der Name einer Richtlinie, die in Ihrem B2C-Verzeichnis erstellt wird. Der Wert für den Richtliniennamen muss mit „b2c\_1\_“ beginnen. Weitere Informationen zu Richtlinien finden Sie unter [Erweiterbares Richtlinienframework](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Zu diesem Zeitpunkt ist der einzige gültige Wert 'Anmeldung', der den Benutzer zur Eingabe von Anmeldeinformationen für diese Anforderung zwingt. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie abzuschließen. Dafür muss der Benutzer z. B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen – je nachdem, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD mithilfe der im `response_mode`-Parameter angegebenen Methode eine Antwort über den angegebenen `redirect_uri` an Ihre App zurück. Die Antwort ist in den oben aufgeführten Fällen, unabhängig von der ausgeführten Richtlinie, immer gleich.

Eine erfolgreiche Antwort mit `response_mode=query` sieht wie folgt aus:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameter | Beschreibung |
| --- | --- |
| Code |Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für eine Zielressource verwenden. Autorisierungscodes sind sehr kurzlebig. In der Regel laufen sie nach etwa 10 Minuten ab. |
| state |Die vollständige Beschreibung finden Sie in der vorherigen Tabelle. Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die App sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| state |Die vollständige Beschreibung finden Sie in der ersten Tabelle in diesem Abschnitt. Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die App sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

## <a name="2.-get-a-token"></a>2. Abrufen von Token
Nachdem Sie einen Autorisierungscode erhalten haben, können Sie den `code` für ein Token für die gewünschte Ressource einlösen, indem Sie eine `POST`-Anforderung an den `/token`-Endpunkt senden. In Azure AD B2C ist die einzige Ressource, für die Sie ein Token anfordern können, die Back-End-Web-API Ihrer App. Für das Anfordern eines Tokens für sich selbst wird die Client-ID Ihrer App als Bereich verwendet:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des Autorisierungscodes verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der *Abfragezeichenfolge*hinzufügen, nicht im POST-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für den Autorisierungscodefluss `authorization_code` lauten muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein **Zugriffstoken** erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit der gleichen Client-ID verwendet werden kann.  Der `offline_access`-Bereich gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt.  Außerdem können Sie den Bereich `openid` verwenden, um ein **ID-Token** von Azure AD B2C anzufordern. |
| Code |Erforderlich |Der Autorisierungscode, den Sie im ersten Abschnitt des Vorgangs erhalten haben. |
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
| access_token |Das signierte JSON-Webtoken (JWT), das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gilt. Dieser kann für das Zwischenspeichern von Token zur späteren Verwendung verwendet werden. |
| expires_in |Gibt an, wie lange das Token gültig ist (in Sekunden). |
| refresh_token |Ein OAuth 2.0-Aktualisierungstoken. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |

Fehlerantworten sehen wie folgt aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

## <a name="3.-use-the-token"></a>3. Verwenden des Tokens
Nachdem Sie ein `access_token` erhalten haben, können Sie das Token für Anforderungen an die Back-End-Web-APIs verwenden, indem Sie es in den `Authorization`-Header einfügen:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4.-refresh-the-token"></a>4. Aktualisieren des Tokens
Zugriffs- und ID-Token sind kurzlebig. Nach ihrem Ablauf müssen Sie sie aktualisieren, um weiterhin auf Ressourcen zugreifen zu können. Dazu übermitteln Sie eine weitere `POST`-Anforderung an den `/token`-Endpunkt. Geben Sie dieses Mal `refresh_token` anstelle von `code` an:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des ursprünglichen Aktualisierungstokens verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der *Abfragezeichenfolge*hinzufügen, nicht im POST-Text. |
| client_id |Empfohlen |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für diesen Abschnitt des Autorisierungscodeflusses `refresh_token` lauten muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Mit der Verwendung der Client-ID als Bereich wird angegeben, dass für die App ein **Zugriffstoken** erforderlich ist, das für Ihren eigenen Dienst oder die Web-API mit der gleichen Client-ID verwendet werden kann.  Der `offline_access`-Bereich gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt.  Außerdem können Sie den Bereich `openid` verwenden, um ein **ID-Token** von Azure AD B2C anzufordern. |
| redirect_uri |Optional |Der Umleitungs-URI der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| refresh_token |Erforderlich |Das ursprüngliche Aktualisierungstoken, das Sie im zweiten Abschnitt des Vorgangs erhalten haben. |

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
| access_token |Das signierte JSON-Webtoken (JWT), das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gilt. Dieser kann für das Zwischenspeichern von Token zur späteren Verwendung verwendet werden. |
| expires_in |Gibt an, wie lange das Token gültig ist (in Sekunden). |
| refresh_token |Ein OAuth 2.0-Aktualisierungstoken. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |

Fehlerantworten sehen wie folgt aus:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

## <a name="use-your-own-b2c-directory"></a>Verwenden eines eigenen B2C-Verzeichnisses
Wenn Sie diese Anforderungen selbst ausprobieren möchten, müssen Sie zunächst diese drei Schritte ausführen und dann die Beispielwerte durch Ihre eigenen ersetzen:

* [Erstellen eines B2C-Verzeichnisses](active-directory-b2c-get-started.md) : Verwenden Sie dann den Namen Ihres Verzeichnisses in den Anforderungen.
* [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zum Abrufen einer Anwendungs-ID und eines Umleitungs-URI. Sie können Ihrer App einen **systemeigenen Client** hinzufügen.
* [Erstellen der Richtlinien](active-directory-b2c-reference-policies.md) zum Abrufen der Richtliniennamen.

<!--HONumber=Oct16_HO2-->


