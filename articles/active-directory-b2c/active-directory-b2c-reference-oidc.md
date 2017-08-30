---
title: 'Webanmeldung mit OpenID Connect: Azure AD B2C | Microsoft-Dokumentation'
description: Erstellen von Webanwendungen mit der Azure Active Directory-Implementierung des OpenID Connect-Authentifizierungsprotokolls.
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 21d420c8-3c10-4319-b681-adf2e89e7ede
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: saeedakhter-msft
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: b0c33a47dd0cae79eab32ac578448fae8bf59be5
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Webanmeldungen mit OpenID Connect
OpenID Connect ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei Webanwendungen angemeldet werden können. Mithilfe der Azure Active Directory B2C (Azure AD B2C)-Implementierung von OpenID Connect können Sie die Registrierung, die Anmeldung und die sonstige Identitätsverwaltung Ihrer Webanwendungen nach Azure Active Directory (Azure AD) auslagern. In diesem Leitfaden wird dies sprachunabhängig erläutert. Er beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0-Protokoll für die *Autorisierung*, damit es als Protokoll für die *Authentifizierung* verwendet werden kann. Dadurch können Sie einmaliges Anmelden mithilfe von OAuth ausführen. Dabei wird das Konzept eines *ID-Token* eingeführt, eines Sicherheitstokens, mit dem der Client die Identität des Benutzers überprüfen und grundlegende Profilinformationen über den Benutzer erhalten kann.

Da OAuth 2.0 erweitert wird, können Apps zudem *Zugriffstoken* sicher abrufen. Mit Zugriffstoken können Sie auf die Ressourcen zugreifen, die von einem [Autorisierungsserver](active-directory-b2c-reference-protocols.md#the-basics) gesichert werden. OpenID Connect wird für Webanwendungen empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird. Wenn Sie in mobilen oder Desktopanwendungen mit Azure AD B2C eine Identitätsverwaltung einfügen möchten, verwenden Sie [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) anstelle von OpenID Connect.

Azure AD B2C erweitert das OpenID Connect-Standardprotokoll, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Dazu werden [Richtlinienparameter](active-directory-b2c-reference-policies.md) eingeführt, mit denen Sie OpenID Connect zum Hinzufügen von Benutzeroberflächen z.B. für die Registrierung, die Anmeldung und die Profilverwaltung zu Ihrer App verwenden können. Hier zeigen wir Ihnen, wie Sie mit OpenID Connect und Richtlinien diese Benutzeroberflächen in ihren Webanwendungen implementieren. Wir zeigen Ihnen außerdem, wie Zugriffstoken für den Zugriff auf Web-APIs abgerufen werden.

Die HTTP-Beispielanforderungen unten verwenden unser B2C-Beispielverzeichnis fabrikamb2c.onmicrosoft.com sowie unsere Beispielanwendung https://aadb2cplayground.azurewebsites.net und Richtlinien. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene B2C-Mandanten, -Anwendungen und -Richtlinien erstellen](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Übermitteln von Authentifizierungsanforderungen
Wenn die Web-App Benutzer authentifizieren und eine Richtlinie ausführen muss, kann sie die Benutzer an den `/authorize` -Endpunkt weiterleiten. Dies ist der interaktive Teil des Datenflusses, bei dem der Benutzer je nach Richtlinie Aktionen durchführt.

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
| response_type |Erforderlich |Der Antworttyp, der einen ID-Token für OpenID Connect enthalten muss. Wenn Ihre Web-App auch Token für den Aufruf einer Web-API benötigt, können Sie wie hier `code+id_token` verwenden. |
| redirect_uri |Empfohlen |Der `redirect_uri`-Parameter der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der `redirect_uri`-Parameter übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für die Anmeldung des Benutzers und das Abrufen von Daten über den Benutzer in Form von ID-Token an (dies wird weiter unten in diesem Artikel erläutert). Der `offline_access` -Bereich ist für Web-Apps optional. Er gibt an, dass Ihre App ein *Aktualisierungstoken* für den dauerhaften Zugriff auf Ressourcen benötigt. |
| response_mode |Empfohlen |Die Methode, die zum Senden des resultierenden Autorisierungscods zurück an Ihre App verwendet werden soll. Es kann sich um `query`, `form_post`, oder `fragment` handeln.  Der `form_post`-Antwortmodus wird empfohlen, für die optimale Sicherheit. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der ebenfalls in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern. Der Status wird auch zum Codieren von Informationen über den Status des Benutzers in der App vor der Authentifizierungsanforderung verwendet, z. B. für Informationen zu der Seite, die der Benutzer besucht hat. |
| nonce |Erforderlich |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten sein wird. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| p |Erforderlich |Die Richtlinie, die ausgeführt wird. Dies ist der Name einer Richtlinie, die in Ihrem B2C-Mandanten erstellt wird. Der Wert für den Richtliniennamen muss mit `b2c\_1\_` beginnen. Weitere Informationen zu Richtlinien finden Sie unter [Erweiterbares Richtlinienframework](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Zu diesem Zeitpunkt ist der einzige gültige Wert `login`, der den Benutzer zur Eingabe von Anmeldeinformationen für diese Anforderung zwingt. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie abzuschließen. Dafür muss der Benutzer z.B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen – je nachdem, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD, mithilfe der im `response_mode`-Parameter angegebenen Methode eine Antwort über den angegebenen `redirect_uri` an Ihre App zurück. Die Antwort ist dieselbe für jeden der vorangegangenen Fälle, unabhängig von der Richtlinie, die ausgeführt wird.

Eine erfolgreiche Antwort mit `response_mode=fragment` sieht wie folgt aus:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| id_token |Das ID-Token, das die App angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |
| Code |Der Autorisierungscode, den die App angefordert hat, sofern `response_type=code+id_token` verwendet wurde. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für eine Zielressource verwenden. Autorisierungscodes sind von sehr kurzer Lebensdauer. In der Regel laufen sie nach etwa 10 Minuten ab. |
| state |Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |

Fehlerantworten können auch an den `redirect_uri`-Parameter gesendet werden, damit die App diese angemessen behandeln kann:

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
| state |Die vollständige Beschreibung finden Sie in der ersten Tabelle in diesem Abschnitt. Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |

## <a name="validate-the-id-token"></a>Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen außerdem die Signatur des ID-Tokens validieren und die Ansprüche im Token, gemäß den Anforderungen Ihrer App überprüfen. Azure AD B2C verwendet [JSON-Webtoken (JWT)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Viele Open Source-Bibliotheken sind für die Überprüfung von JWTs für unterschiedliche Sprachen verfügbar. Wir empfehlen die Verwendung einer dieser Optionen, anstatt eine eigene Validierungslogik zu implementieren. Diese Informationen sind hilfreich, um die ordnungsgemäße Verwendung dieser Bibliotheken zu erlernen.

Azure AD B2C verfügt über einen OpenID Connect-Metadatenendpunkt, mit dem die App zur Laufzeit Informationen über Azure AD B2C abrufen kann. Diese Informationen umfassen Endpunkte, Tokeninhalte und Token-Signaturschlüssel. Es gibt ein JSON-Metadatendokument für jede Richtlinie in Ihrem B2C-Mandanten. Das Metadatendokument für die `b2c_1_sign_in`-Richtlinie in `fabrikamb2c.onmicrosoft.com` befindet sich z.B. unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Eine der Eigenschaften dieses Konfigurationsdokuments ist der `jwks_uri`, dessen Wert für diese Richtlinie wie folgt lautet:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Es gibt zwei Möglichkeiten, zu ermitteln, welche Richtlinie zum Signieren eines ID-Tokens verwendet wurde (und wo die Metadaten abgerufen werden können). Zunächst einmal ist der Richtlinienname im `acr`-Anspruch im ID-Token enthalten. Informationen zum Analysieren von Ansprüchen nach einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit besteht darin, die Richtlinie beim Übermitteln der Anforderung im Wert des Parameters `state` zu verschlüsseln und später zu entschlüsseln, um die verwendete Richtlinie zu bestimmen. Beide Methoden sind gültig.

Nachdem Sie das Metadatendokument aus dem OpenID Connect-Metadatenendpunkt erhalten haben, können Sie die öffentlichen RSA 256-Schlüssel (die sich an diesem Endpunkt befinden) zum Überprüfen der Signatur des ID-Tokens verwenden. Es gibt möglicherweise zu einem bestimmten Zeitpunkt mehrere Schlüssel an diesem Endpunkt. Sie werden jeweils durch einen `kid`-Anspruch identifiziert. Der Header des ID-Tokens enthält außerdem einen `kid`-Anspruch, der anzeigt, welcher Schlüssel zum Signieren des ID-Tokens verwendet wurde. Weitere Informationen finden Sie unter [Azure AD B2C token reference (Verweis auf den Azure AD B2C-Token)](active-directory-b2c-reference-tokens.md) (insbesondere im Abschnitt [Überprüfen von Token](active-directory-b2c-reference-tokens.md#token-validation)).
<!--TODO: Improve the information on this-->

Nachdem Sie die Signatur des ID-Tokens validiert haben, müssen Sie einige Ansprüche überprüfen, z.B.: Beispiel:

* Sie sollten den `nonce`-Anspruch überprüfen, um Token-Replay-Angriffe zu verhindern. Dessen Wert sollte dem in der Anmeldeanforderung angegebenen Wert entsprechen.
* Sie sollten den `aud`-Anspruch überprüfen, um sicherzustellen, dass das ID-Token für Ihre App ausgestellt wurde. Der Wert sollte der Anwendungs-ID der App entsprechen.
* Sie sollten die Ansprüche `iat` und `exp` überprüfen, um sicherzustellen, dass das ID-Token noch nicht abgelaufen ist.

Es gibt auch einige weitere Überprüfungen, die Sie durchführen sollten. Diese werden ausführlich in der [OpenID Connect Core Spec (Core-Spezifikation des OpenID Connect) ](http://openid.net/specs/openid-connect-core-1_0.html) beschrieben.  Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer/die Organisation für die App registriert hat.
* Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und die richtigen Berechtigungen verfügt.
* Sicherstellen, dass eine bestimmte Authentifizierungsmethode verwendet wird, z. B. Azure Multi-Factor Authentication.

Weitere Informationen zu Ansprüchen in einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie eine Sitzung mit dem Benutzer beginnen. Verwenden Sie die Ansprüche im ID-Token, um Informationen über den Benutzer in Ihrer App zu erhalten. Die Verwendung dieser Information umfasst die Anzeige, Datensätze und Autorisierung.

## <a name="get-a-token"></a>Abrufen von Token
Wenn Ihre Web-App nur Richtlinien ausführen soll, können Sie die nächsten Abschnitte überspringen. Diese Abschnitte gelten nur für Web-Apps, die authentifizierte Aufrufe an eine Web-API durchführen müssen und die auch von Azure AD B2C geschützt werden.

Sie können den erhaltenen Autorisierungscode (mit `response_type=code+id_token`) für ein Token für die gewünschte Ressource einlösen, indem Sie eine `POST`-Anforderung an den `/token`-Endpunkt senden. Derzeit ist die einzige Ressource, für die Sie ein Token anfordern können, die Back-End-Web-API Ihrer App. Für das Anfordern eines Tokens für sich selbst wird die Client-ID der App als Bereich verwendet:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des Autorisierungscodes verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der Abfragezeichenfolge hinzufügen, nicht im `POST`-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für den Autorisierungscodefluss `authorization_code` lauten muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für das Anmelden des Benutzers und das Abrufen von Daten über den Benutzer in Form von id_token-Parametern an. Damit können Sie Token an die Back-End-Web-API ihrer App übermitteln, die durch dieselbe Anwendungs-ID wie der Client dargestellt wird. Der `offline_access`-Bereich gibt an, dass Ihre App ein Aktualisierungstoken für den dauerhaften Zugriff auf Ressourcen benötigt. |
| Code |Erforderlich |Der Autorisierungscode, den Sie im ersten Abschnitt des Vorgangs erhalten haben. |
| redirect_uri |Erforderlich |Der `redirect_uri`-Parameter der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| client_secret |Erforderlich |Der geheime Schlüssel der Anwendung, den Sie im [Azure-Portal](https://portal.azure.com/)generiert haben. Dieser geheime Schlüssel der Anwendung ist ein wichtiges Sicherheitsartefakt. Sie sollten ihn sicher auf dem Server speichern. Sie sollten auch darauf achten, diesen geheimen Clientschlüssel in regelmäßigen Abständen zu wechseln. |

Eine erfolgreiche Token-Antwort sieht wie folgt aus:

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
| token_type |Der Wert des Tokentyps. Der einzige Typ, der von Azure AD unterstützt wird, ist `Bearer`. |
| access_token |Das signierte JWT-Token, das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gültig ist. Diese können verwendet werden, für die Zwischenspeicherung von Token für die spätere Verwendung. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Beachten Sie, dass Sie den `offline_access`-Bereich sowohl für die Autorisierung als auch in den Tokenanforderungen verwenden müssen, um ein Aktualisierungstoken zu erhalten. |

Fehlerantworten sehen aus wie folgt:

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
Nachdem Sie ein Zugriffstoken erhalten haben, können Sie das Token für Anforderungen an die Back-End-Web-APIs verwenden, indem Sie es in den -`Authorization`Header einfügen:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Aktualisieren des Tokens
Zugriffs- und ID-Token sind kurzlebig. Nach ihrem Ablauf müssen Sie sie aktualisieren, um weiterhin auf Ressourcen zugreifen zu können. Dazu übermitteln Sie eine weitere `POST`-Anforderung an den `/token`-Endpunkt. Geben Sie diesmal den `refresh_token`-Parameter anstelle von `code` an:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abrufen des ursprünglichen Aktualisierungstokens verwendet wurde. Sie können in dieser Anforderung keine andere Richtlinie verwenden. Sie müssen diesen Parameter in der Abfragezeichenfolge hinzufügen, nicht im POST-Text. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| grant_type |Erforderlich |Der Berechtigungstyp, der für diesen Abschnitt des Autorisierungscodeflusses als Autorisierungscode dienen muss. |
| Bereich |Empfohlen |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für das Anmelden des Benutzers und das Abrufen von Daten über den Benutzer in Form von ID-Token an. Damit können Sie Token an die Back-End-Web-API ihrer App übermitteln, die durch dieselbe Anwendungs-ID wie der Client dargestellt wird. Der `offline_access`-Bereich gibt an, dass Ihre App ein Aktualisierungstoken für den dauerhaften Zugriff auf Ressourcen benötigt. |
| redirect_uri |Empfohlen |Der `redirect_uri`-Parameter der Anwendung, bei der Sie den Autorisierungscode erhalten haben. |
| refresh_token |Erforderlich |Das ursprüngliche Aktualisierungstoken, das Sie im zweiten Abschnitt des Vorgangs erhalten haben. Beachten Sie, dass Sie den `offline_access`-Bereich sowohl für die Autorisierung als auch in den Tokenanforderungen verwenden müssen, um ein Aktualisierungstoken zu erhalten. |
| client_secret |Erforderlich |Der geheime Schlüssel der Anwendung, den Sie im [Azure-Portal](https://portal.azure.com/)generiert haben. Dieser geheime Schlüssel der Anwendung ist ein wichtiges Sicherheitsartefakt. Sie sollten ihn sicher auf dem Server speichern. Sie sollten auch darauf achten, diesen geheimen Clientschlüssel in regelmäßigen Abständen zu wechseln. |

Eine erfolgreiche Token-Antwort sieht wie folgt aus:

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
| token_type |Der Wert des Tokentyps. Der einzige Typ, der von Azure AD unterstützt wird, ist `Bearer`. |
| access_token |Das signierte JWT-Token, das Sie angefordert haben. |
| Bereich |Die Bereiche, für die das Token gilt. Dieser kann für das Zwischenspeichern von Token zur späteren Verwendung verwendet werden. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Token zusätzliche Token zu erhalten.  Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. Weitere Details finden Sie in der [B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |

Fehlerantworten sehen aus wie folgt:

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

Sie können den Benutzer einfach an den `end_session`-Endpunkt umleiten, der im gleichen OpenID Connect-Metadatendokument aufgeführt wird, das weiter oben im Abschnitt „Überprüfen des ID-Tokens“ beschrieben wird:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die Sie zum Abmelden des Benutzers von der Anwendung verwenden möchten. |
| post_logout_redirect_uri |Empfohlen |Die URL, an die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll. Wenn dies nicht angegeben wird, gibt Azure AD B2C eine generische Nachricht an den Benutzer aus. |

> [!NOTE]
> Beim Weiterleiten des Benutzers an den `end_session`-Endpunkt, wird zwar ein Teil des SSO-Zustands des Benutzers bei Azure AD B2C gelöscht, der Benutzer wird aber nicht von seiner Sitzung beim Identitätsanbieter (IDP) eines sozialen Netzwerks abgemeldet. Wenn der Benutzer bei einer nachfolgenden Anmeldung denselben Identitätsanbieter auswählt, wird der Benutzer ohne erneute Eingabe seiner Anmeldeinformationen wieder authentifiziert. Wenn ein Benutzer sich von der B2C-Anwendung abmelden möchte, bedeutet dies nicht unbedingt, dass er sich auch vollständig von seinem Facebook-Konto abmelden möchte. Bei lokalen Konten wird die Sitzung des Benutzers jedoch ordnungsgemäß beendet.
> 
> 

## <a name="use-your-own-b2c-tenant"></a>Verwenden eines eigenen B2C-Mandanten
Wenn Sie diese Anforderungen selbst ausprobieren möchten, müssen Sie zunächst diese drei Schritte ausführen und dann die vorher beschriebenen Beispielwerte durch Ihre eigenen ersetzen:

1. [Erstellen eines B2C-Mandanten](active-directory-b2c-get-started.md)und Verwenden des Namens Ihres Mandanten in den Anforderungen.
2. [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zum Abrufen einer Anwendungs-ID. Binden Sie eine Web-App/Web-API in Ihre App ein. Erstellen Sie optional ein Anwendungsgeheimnis.
3. [Erstellen der Richtlinien](active-directory-b2c-reference-policies.md) zum Abrufen der Richtliniennamen.


