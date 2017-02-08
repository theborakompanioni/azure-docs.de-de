---
title: Azure Active Directory B2C | Microsoft Docs
description: Erstellen von Webanwendungen mit der Azure Active Directory-Implementierung des OpenID Connect-Authentifizierungsprotokolls.
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9a4ccad94520bd0d811ba9dcfd6f7cc680c89a1f


---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Webanmeldungen mit OpenID Connect
OpenID Connect ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei Webanwendungen angemeldet werden können.  Mithilfe der Azure Active Directory (Azure AD) B2C-Implementierung von OpenID Connect können Sie die Registrierung, die Anmeldung und die sonstige Identitätsverwaltung Ihrer Webanwendungen nach Azure AD auslagern. In diesem Leitfaden wird dies sprachunabhängig erläutert. Im Leitfaden wird das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken beschrieben.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0-Protokoll für die *Autorisierung*, damit es als Protokoll für die *Authentifizierung* verwendet werden kann. Dadurch können Sie einmaliges Anmelden mithilfe von OAuth ausführen. Dabei wird das Konzept eines `id_token`eingeführt, eines Sicherheitstokens, mit dem der Client die Identität des Benutzers überprüfen und grundlegende Profilinformationen über den Benutzer erhalten kann.

Da OAuth 2.0 erweitert wird, können Apps zudem **Zugriffstoken** sicher abrufen. Mit Zugriffstoken können Sie auf die Ressourcen zugreifen, die von einem [Autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics) gesichert werden. OpenID Connect wird für Webanwendungen empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird. Wenn Sie in mobilen oder Desktopanwendungen mit Azure AD B2C eine Identitätsverwaltung einfügen möchten, verwenden Sie [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) anstelle von OpenID Connect.

Azure AD B2C erweitert das OpenID Connect-Standardprotokoll, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Dazu werden [**Richtlinienparameter**](active-directory-b2c-reference-policies.md) eingeführt, mit denen Sie OpenID Connect zum Hinzufügen von Benutzeroberflächen z.B. für die Registrierung, Anmeldung und Profilverwaltung zu Ihrer App verwenden können. Hier zeigen wir Ihnen, wie Sie mit OpenID Connect und Richtlinien diese Benutzeroberflächen in Webanwendungen implementieren. Wir zeigen Ihnen außerdem, wie Zugriffstoken für den Zugriff auf Web-APIs abgerufen werden.

Die HTTP-Beispielanforderungen unten verwenden unser B2C-Beispielverzeichnis **fabrikamb2c.onmicrosoft.com** sowie unsere Beispielanwendung **https://aadb2cplayground.azurewebsites.net** und Richtlinien. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene B2C-Mandanten, -Anwendungen und -Richtlinien erstellen](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Übermitteln von Authentifizierungsanforderungen
Wenn die Web-App Benutzer authentifizieren und eine Richtlinie ausführen muss, kann sie die Benutzer an den `/authorize` -Endpunkt weiterleiten. Dies ist der interaktive Teil des Datenflusses, bei dem der Benutzer je nach Richtlinie tatsächlich Aktionen durchführt.

In dieser Anforderung gibt der Client die Berechtigungen, die er vom Benutzer benötigt, im `scope`-Parameter und die anzuwendende Richtlinie im `p`-Parameter an. Unten sind drei Beispiele aufgeführt (mit Zeilenumbrüchen für bessere Lesbarkeit), die jeweils eine andere Richtlinie verwenden. Um ein Gefühl für die Funktionsweise der einzelnen Anforderung zu erhalten, fügen Sie sie in einem Browser ein und führen sie aus.

#### <a name="use-a-sign-in-policy"></a>Verwenden einer Anmelderichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden einer Registrierungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden einer Profilbearbeitungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| response_type |Erforderlich |Der Antworttyp, der `id_token` für OpenID Connect enthalten muss. Wenn Ihre Web-App auch Token für den Aufruf einer Web-API benötigt, können Sie wie hier `code+id_token`verwenden. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für die Anmeldung des Benutzers und das Abrufen von Daten über den Benutzer in Form von **ID-Token** an (dies wird weiter unten in diesem Artikel erläutert). Der `offline_access` -Bereich ist für Web-Apps optional. Er gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt. |
| response_mode |Empfohlen |Die Methode, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll. Dies kann entweder „query“, „form_post“ oder „fragment“ sein.  „form_post“ wird empfohlen, da diese Methode die größte Sicherheit bietet. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern. Der Status wird auch zum Codieren von Informationen über den Status des Benutzers in der App vor der Authentifizierungsanforderung verwendet, z. B. für Informationen zu der Seite, die der Benutzer besucht hat. |
| nonce |Erforderlich |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten ist. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| p |Erforderlich |Die Richtlinie, die ausgeführt wird. Dies ist der Name einer Richtlinie, die in Ihrem B2C-Mandanten erstellt wird. Der Wert für den Richtliniennamen muss mit „b2c\_1\_“ beginnen. Weitere Informationen zu Richtlinien finden Sie unter [Erweiterbares Richtlinienframework](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Zu diesem Zeitpunkt ist der einzige gültige Wert 'Anmeldung', der den Benutzer zur Eingabe von Anmeldeinformationen für diese Anforderung zwingt. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie abzuschließen. Dafür muss der Benutzer z. B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen – je nachdem, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD mithilfe der im `response_mode`-Parameter angegebenen Methode eine Antwort über den angegebenen `redirect_uri` an Ihre App zurück. Die Antwort ist in den oben aufgeführten Fällen, unabhängig von der ausgeführten Richtlinie, immer gleich.

Eine erfolgreiche Antwort mit `response_mode=fragment` sieht wie folgt aus:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| id_token |Das ID-Token, das die Anwendung angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |
| Code |Der Autorisierungscode, den die App angefordert hat, sofern `response_type=code+id_token` verwendet wurde. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für eine Zielressource verwenden. Autorisierungscodes sind sehr kurzlebig. In der Regel laufen sie nach etwa 10 Minuten ab. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| state |Die vollständige Beschreibung finden Sie in der vorherigen Tabelle. Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

## <a name="validate-the-idtoken"></a>Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens allein reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur des ID-Tokens validieren und die Ansprüche im Token gemäß den App-Anforderungen überprüfen. Azure AD B2C verwendet [JSON-Webtoken (JWT)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Viele Open Source-Bibliotheken sind für die Überprüfung von JWTs für unterschiedliche Sprachen verfügbar. Wir empfehlen die Verwendung einer dieser Optionen, anstatt eine eigene Validierungslogik zu implementieren. Diese Informationen sind hilfreich, um die ordnungsgemäße Verwendung dieser Bibliotheken zu erlernen.

Azure AD B2C verfügt über einen OpenID Connect-Metadatenendpunkt, mit dem die App zur Laufzeit Informationen über Azure AD B2C abrufen kann. Diese Informationen umfassen Endpunkte, Tokeninhalte und Token-Signaturschlüssel. Es gibt ein JSON-Metadatendokument für jede Richtlinie in Ihrem B2C-Mandanten. Das Metadatendokument für die `b2c_1_sign_in`-Richtlinie in `fabrikamb2c.onmicrosoft.com` befindet sich z.B. unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Eine der Eigenschaften dieses Konfigurationsdokuments ist der `jwks_uri`, dessen Wert für diese Richtlinie wie folgt lautet:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Es gibt zwei Möglichkeiten, zu ermitteln, welche Richtlinie zum Signieren eines ID-Tokens verwendet wurde (und wo die Metadaten abgerufen werden können). Zunächst einmal ist der Richtliniennamen im `acr`-Anspruch im ID-Token enthalten. Informationen zum Analysieren von Ansprüchen nach einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit besteht darin, die Richtlinie beim Übermitteln der Anforderung im Wert des Parameters `state` zu verschlüsseln und später zu entschlüsseln, um die verwendete Richtlinie zu bestimmen. Beide Methoden sind gleichwertig.

Nachdem Sie das Metadatendokument aus dem OpenID Connect-Metadatenendpunkt erhalten haben, können Sie die öffentlichen RSA 256-Schlüssel (die sich an diesem Endpunkt befinden) zum Überprüfen der Signatur des ID-Tokens verwenden. Es gibt möglicherweise zu einem bestimmten Zeitpunkt mehrere Schlüssel an diesem Endpunkt. Sie werden jeweils durch eine `kid` identifiziert. Der Header des ID-Tokens enthält außerdem einen `kid`-Anspruch, der anzeigt, welcher Schlüssel zum Signieren des ID-Tokens verwendet wurde. Weitere Informationen finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Dort finden Sie auch Einzelheiten zum [Überprüfen von Token](active-directory-b2c-reference-tokens.md#token-validation).
<!--TODO: Improve the information on this-->

Nachdem Sie die Signatur des ID-Tokens validiert haben, müssen Sie einige Ansprüche überprüfen, z.B.:

* Sie sollten den `nonce`-Anspruch überprüfen, um Token-Replay-Angriffe zu verhindern. Dessen Wert sollte dem in der Anmeldeanforderung angegebenen Wert entsprechen.
* Sie sollten den `aud`-Anspruch überprüfen, um sicherzustellen, dass das ID-Token für Ihre App ausgestellt wurde. Der Wert sollte der Anwendungs-ID der App entsprechen.
* Sie sollten die Ansprüche `iat` und `exp` überprüfen, um sicherzustellen, dass das ID-Token noch nicht abgelaufen ist.

Es gibt auch noch einige andere Überprüfungen, die Sie durchführen sollten. Diese sind in der [OpenID Connect Core-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html) ausführlich beschrieben.  Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer/die Organisation für die App registriert hat.
* Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und die richtigen Berechtigungen verfügt.
* Sicherstellen, dass eine bestimmte Authentifizierungsmethode verwendet wird, z. B. Azure Multi-Factor Authentication.

Weitere Informationen zu Ansprüchen in einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie mit dem Benutzer eine Sitzung beginnen und die Ansprüche im ID-Token zum Abrufen von Informationen über den Benutzer in der App verwenden. Diese Informationen können für die Anzeige, für Aufzeichnungen, für die Autorisierung usw. verwendet werden.

## <a name="get-a-token"></a>Abrufen von Token
Wenn Ihre Web-App lediglich Richtlinien ausführen muss, können Sie die nächsten Abschnitte überspringen. Diese Abschnitte gelten nur für Web-Apps, die authentifizierte Aufrufe an eine Web-API durchführen müssen und die auch von Azure AD B2C geschützt werden.

Sie können den erhaltenen Autorisierungscode (mit `response_type=code+id_token`) für ein Token für die gewünschte Ressource einlösen, indem Sie eine `POST`-Anforderung an den `/token`-Endpunkt senden. Derzeit ist die einzige Ressource, für die Sie ein Token anfordern können, die Back-End-Web-API Ihrer App. Für das Anfordern eines Tokens für sich selbst wird die Client-ID der App als Bereich verwendet:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des Autorisierungscodes verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der **Abfragezeichenfolge**hinzufügen, nicht im POST-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für den Autorisierungscodefluss `authorization_code` lauten muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für das Anmelden des Benutzers und das Abrufen von Daten über den Benutzer in Form von **ID-Token** an. Damit können Sie Token an die Back-End-Web-API der App übermitteln, die durch dieselbe Anwendungs-ID wie der Client dargestellt wird. Der `offline_access`-Bereich gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt. |
| Code |Erforderlich |Der Autorisierungscode, den Sie im ersten Abschnitt des Vorgangs erhalten haben. |
| redirect_uri |Erforderlich |Der Umleitungs-URI der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| client_secret |Erforderlich |Der geheime Schlüssel der Anwendung, den Sie im [Azure-Portal](https://portal.azure.com/)generiert haben. Dieser geheime Schlüssel der Anwendung ist ein wichtiges Sicherheitsartefakt. Sie sollten ihn sicher auf dem Server speichern. Sie sollten auch darauf achten, diesen geheimen Clientschlüssel in regelmäßigen Abständen zu rotieren. |

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
| Bereich |Die Bereiche, für die das Token gilt. Dieser kann für das Zwischenspeichern von Token zur späteren Verwendung verwendet werden. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| refresh_token |Ein OAuth 2.0-Aktualisierungstoken. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten.  Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Beachten Sie, dass Sie den `offline_access`-Bereich sowohl für die Autorisierung als auch in den Tokenanforderungen verwenden müssen, um ein Aktualisierungstoken zu erhalten. |

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

## <a name="use-the-token"></a>Verwenden des Tokens
Nachdem Sie ein `access_token` erhalten haben, können Sie das Token für Anforderungen an die Back-End-Web-APIs verwenden, indem Sie es in den `Authorization`-Header einfügen:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Aktualisieren des Tokens
Die ID-Token sind kurzlebig. Nach ihrem Ablauf müssen Sie sie aktualisieren, um weiterhin auf Ressourcen zugreifen zu können. Dazu übermitteln Sie eine weitere `POST`-Anforderung an den `/token`-Endpunkt. Geben Sie dieses Mal `refresh_token` anstelle von `code` an:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des ursprünglichen Aktualisierungstokens verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der **Abfragezeichenfolge**hinzufügen, nicht im POST-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für diesen Abschnitt des Autorisierungscodeflusses `refresh_token` lauten muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für das Anmelden des Benutzers und das Abrufen von Daten über den Benutzer in Form von **ID-Token** an. Damit können Sie Token an die Back-End-Web-API der App übermitteln, die durch dieselbe Anwendungs-ID wie der Client dargestellt wird. Der `offline_access`-Bereich gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| refresh_token |Erforderlich |Das ursprüngliche Aktualisierungstoken, das Sie im zweiten Abschnitt des Vorgangs erhalten haben. Beachten Sie, dass Sie den `offline_access`-Bereich sowohl für die Autorisierung als auch in den Tokenanforderungen verwenden müssen, um ein Aktualisierungstoken zu erhalten. |
| client_secret |Erforderlich |Der geheime Schlüssel der Anwendung, den Sie im [Azure-Portal](https://portal.azure.com/)generiert haben. Dieser geheime Schlüssel der Anwendung ist ein wichtiges Sicherheitsartefakt. Sie sollten ihn sicher auf dem Server speichern. Sie sollten auch darauf achten, diesen geheimen Clientschlüssel in regelmäßigen Abständen zu rotieren. |

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
| Bereich |Die Bereiche, für die das Token gilt. Dieser kann für das Zwischenspeichern von Token zur späteren Verwendung verwendet werden. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| refresh_token |Ein OAuth 2.0-Aktualisierungstoken. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten.  Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |

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

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldeanforderung
Wenn Sie den Benutzer bei der App abmelden möchten, reicht es nicht, die Cookies der App zu löschen oder die Sitzung mit dem Benutzer auf andere Weise zu beenden. Sie müssen den Benutzer außerdem für die Abmeldung zu Azure AD umleiten. Wenn Sie dies versäumen, kann sich der Benutzer möglicherweise erneut bei Ihrer App authentifizieren, ohne die Anmeldeinformationen erneut eingeben zu müssen. Die Ursache ist, das er über eine gültige SSO-Sitzung bei Azure AD verfügt.

Sie können den Benutzer einfach an den `end_session_endpoint` umleiten, der im gleichen OpenID Connect-Metadatendokument aufgeführt wird, das weiter oben im Abschnitt „Überprüfen des ID-Tokens“ beschrieben wird:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die Sie zum Abmelden des Benutzers von der Anwendung verwenden möchten. |
| post_logout_redirect_uri |Empfohlen |Die URL, an die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll. Wenn sie nicht angegeben ist, wird dem Benutzer eine generische Meldung von Azure AD B2C angezeigt. |

> [!NOTE]
> Beim Weiterleiten des Benutzers an den `end_session_endpoint` wird zwar ein Teil des SSO-Zustands des Benutzers bei Azure AD B2C gelöscht, der Benutzer wird aber nicht von seiner Sitzung beim Identitätsanbieter (IdP) eines sozialen Netzwerks abgemeldet. Wenn der Benutzer bei einer nachfolgenden Anmeldung denselben Identitätsanbieter auswählt, wird der Benutzer ohne erneute Eingabe seiner Anmeldeinformationen wieder authentifiziert. Wenn ein Benutzer sich von der B2C-Anwendung abmelden möchte, bedeutet dies nicht unbedingt, dass er sich auch vollständig von seinem Facebook-Konto abmelden möchte. Bei lokalen Konten wird die Sitzung des Benutzers jedoch ordnungsgemäß beendet.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Verwenden eines eigenen B2C-Mandanten
Wenn Sie diese Anforderungen selbst ausprobieren möchten, müssen Sie zunächst diese drei Schritte ausführen und dann die Beispielwerte durch Ihre eigenen ersetzen:

* [Erstellen eines B2C-Mandanten](active-directory-b2c-get-started.md)und Verwenden des Namens Ihres Mandanten in den Anforderungen.
* [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zum Abrufen einer Anwendungs-ID und eines Umleitungs-URI. Sie sollten eine **Web-App/Web-API** in die App einfügen und optional auch einen **geheimen Schlüssel für die Anwendung** erstellen.
* [Erstellen der Richtlinien](active-directory-b2c-reference-policies.md) zum Abrufen der Richtliniennamen.




<!--HONumber=Dec16_HO4-->


