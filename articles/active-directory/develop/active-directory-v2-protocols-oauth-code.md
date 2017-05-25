

---
title: "Azure AD v2.0 – OAuth-Autorisierungscodefluss | Microsoft-Dokumentation"
description: "Erstellen von Webanwendungen mit der Azure AD-Implementierung des OAuth 2.0-Authentifizierungsprotokolls."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: ae1d7d86-7098-468c-aa32-20df0a10ee3d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: e9da636d5dbe95ea7d0bd8cf186af30247ca8bd8
ms.contentlocale: de-de
ms.lasthandoff: 01/18/2017


---
# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0-Protokolle – OAuth 2.0-Autorisierungscodefluss
Der OAuth 2.0-Autorisierungcodefluss kann in Apps verwendet werden, die auf einem Gerät installiert sind, um Zugriff auf geschützte Ressourcen wie Web-APIs zu gewähren.  Die Implementierung von OAuth 2.0 im App-Modell v2.0 ermöglicht es Ihnen, sich bei mobilen Apps und Desktop-Apps anzumelden und über APIs darauf zuzugreifen.  Diese sprachunabhängige Anleitung beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt.  Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.
> 
> 

Der OAuth 2.0-Autorisierungcodefluss wird in [Abschnitt 4.1 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749)beschrieben.  Er wird zum Ausführen der Authentifizierung und Autorisierung in den meisten App-Typen genutzt, einschließlich [Web-Apps](active-directory-v2-flows.md#web-apps) und [nativ installierten Apps](active-directory-v2-flows.md#mobile-and-native-apps).  Durch den Codefluss können Apps Zugriffstoken sicher abrufen, die zum Zugriff auf Ressourcen verwendet werden können, die mithilfe des v2.0-Endpunkts geschützt werden.  

## <a name="protocol-diagram"></a>Protokolldiagramm
Allgemein sieht der gesamte Authentifizierungsablauf für eine systemeigene oder mobile Anwendung etwa wie folgt aus:

![OAuth-Autorisierungscodefluss](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Anfordern eines Autorisierungscodes
Der Autorisierungscodefluss beginnt damit, dass der Client den Benutzer auf den `/authorize` -Endpunkt leitet.  In dieser Anforderung gibt der Client die Berechtigungen an, die er vom Benutzer abrufen muss:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [!TIP]
> Klicken Sie auf den Link unten, um diese Anforderung auszuführen. Nach der Anmeldung sollte der Browser mit einem `code` in der Adressleiste zu `https://localhost/myapp/` umgeleitet werden.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parameter |  | Beschreibung |
| --- | --- | --- |
| Mandant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner.  Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihrer Anwendung zugewiesen hat. |
| response_type |erforderlich |Muss `code` für den Autorisierungscodefluss enthalten. |
| redirect_uri |empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können.  Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss.  Für native und mobile Apps sollten Sie den Standardwert `https://login.microsoftonline.com/common/oauth2/nativeclient` verwenden. |
| Bereich |erforderlich |Eine durch Leerzeichen getrennte Liste mit [Bereichen](active-directory-v2-scopes.md) , denen der Benutzer zustimmen soll. |
| response_mode |empfohlen |Gibt die Methode an, die zum Senden des resultierenden Tokens zurück an Ihre App verwendet werden soll.  Kann `query` oder `form_post` sein. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12).  Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung |optional |Gibt den Typ der erforderlichen Benutzerinteraktion an.  Zu diesem Zeitpunkt sind die einzigen gültigen Werte „login“, „none“ und „consent“.  `prompt=login` zwingt den Benutzer, die Anmeldeinformationen bei dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich.  `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird.  Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der v2.0-Endpunkt einen Fehler zurück.  `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| login_hint |optional |Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist.  Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, nachdem sie den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahiert haben. |
| domain_hint |optional |Kann `consumers` oder `organizations` sein.  Wenn dieser Parameter vorhanden ist, wird der E-Mail-basierte Ermittlungsvorgang übersprungen, den der Benutzer auf der v2.0-Anmeldeseite durchläuft, was die Benutzerfreundlichkeit verbessert.  Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, indem sie `tid` aus einer vorherigen Anmeldung extrahieren.  Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat.  Verwenden Sie andernfalls `domain_hint=organizations`. |

Zu diesem Zeitpunkt wird der Benutzer dazu aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen.  Der V2.0-Endpunkt stellt auch sicher, dass der Benutzer den Berechtigungen zugestimmt hat, die im `scope` -Abfrageparameter angegeben sind.  Wenn der Benutzer keiner Berechtigung zugestimmt hat, wird er dazu aufgefordert, den erforderlichen Berechtigungen zuzustimmen.  Nähere Einzelheiten zu [Berechtigungen, Zustimmung und mehrinstanzenfähigen Apps erhalten Sie hier](active-directory-v2-scopes.md).

Sobald der Benutzer authentifiziert ist und seine Zustimmung erteilt hat, gibt der v2.0-Endpunkt mithilfe der Methode im festgelegten `response_mode`-Parameter eine Antwort auf dem angegebenen `redirect_uri` an Ihre App zurück.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=query` sieht wie folgt aus:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| Code |Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden.  Autorisierungscodes sind sehr kurzlebig und laufen in der Regel nach etwa zehn Minuten ab. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Fehlercodes beim Autorisierungsendpunktfehler
Die folgende Tabelle beschreibt die verschiedenen Fehlercodes, die im `error` -Parameter der Fehlerantwort zurückgegeben werden können:

| Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- |
| invalid_request |Protokollfehler, z.B. ein fehlender erforderlicher Parameter. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird. |
| unauthorized_client |Die Clientanwendung darf keinen Autorisierungscode anfordern. |Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| access_denied |Der Ressourcenbesitzer hat die Zustimmung verweigert. |Die Clientanwendung kann den Benutzer benachrichtigen, dass sie ohne Zustimmung des Benutzers nicht fortfahren kann. |
| unsupported_response_type |Der Autorisierungsserver unterstützt den Antworttyp in der Anforderung nicht. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird. |
| server_error |Der Server hat einen unerwarteten Fehler erkannt. |Wiederholen Sie die Anforderung. Diese Fehler werden durch temporäre Bedingungen verursacht. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund eines temporären Fehlers verzögert ist. |
| temporarily_unavailable |Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. |Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |
| invalid_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist. |Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |

## <a name="request-an-access-token"></a>Anfordern eines Zugriffstokens
Nun, da Sie einen Autorisierungscode erworben und die Berechtigung vom Benutzer erhalten haben, können Sie den `code` für ein `access_token` auf die gewünschte Ressource einlösen, indem Sie eine `POST`-Anforderung an den `/token`-Endpunkt senden:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [!TIP]
> Führen Sie diese Anforderung in Postman aus. (Vergessen Sie dabei nicht, den `code` zu ersetzen.) [![Ausführen in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beschreibung |
| --- | --- | --- |
| Mandant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner.  Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihrer Anwendung zugewiesen hat. |
| grant_type |erforderlich |Muss der `authorization_code` für den Autorisierungscodefluss sein. |
| Bereich |erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen.  Die in diesem Abschnitt angeforderten Bereiche müssen den Bereichen entsprechen oder eine Teilmenge der Bereiche sein, die im ersten Abschnitt angefordert wurden.  Wenn die in dieser Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, gibt der v2.0-Endpunkt ein Token für die im ersten Bereich angegebene Ressource zurück.  Eine ausführlichere Erläuterung von Bereichen finden Sie in [Berechtigungen, Zustimmung und Bereiche](active-directory-v2-scopes.md). |
| Code |erforderlich |Der Autorisierungscode, den Sie im ersten Abschnitt des Vorgangs erhalten haben. |
| redirect_uri |erforderlich |Derselbe Wert für den Umleitungs-URI, der zum Abrufen des Autorisierungscodes verwendet wurde |
| client_secret |erforderlich für Web-Apps |Der geheime App-Schlüssel, den Sie im App-Registrierungsportal für Ihre App erstellt haben.  Er sollte nicht in einer systemeigenen App verwendet werden, da geheime Client-Schlüssel nicht zuverlässig auf Geräten gespeichert werden können.  Er ist erforderlich für Web-Apps und Web-APIs, die die Möglichkeit haben, den geheimen Client-Schlüssel sicher auf dem Server zu speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung bei geschützten Ressourcen verwenden, wie z.B. eine Web-API. |
| token_type |Gibt den Wert des Tokentyps an. Der Bearertyp ist der einzige Typ, den Azure AD unterstützt. |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Bereich |Die Bereiche, für die das Zugriffstoken gültig ist. |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um zusätzliche Zugriffstoken nach Ablauf der aktuellen Zugriffstoken zu erhalten.  Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten.  Weitere Details finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md). |
| id_token |Ein unsigniertes JSON-Webtoken (JWT). Die App kann die Segmente dieses Tokens mit einer base64-URL decodieren, um Informationen über den angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich in Bezug auf Autorisierungs- und Sicherheitsgrenzen aber nicht darauf verlassen.  Weitere Informationen zu ID-Token finden Sie in der [Tokenreferenz zum v2.0-Endpunkt](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fehlerantwort
Fehlerantworten sehen wie folgt aus:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error_codes |Eine Liste der spezifischen STS-Fehlercodes, die bei der Diagnose helfen können |
| timestamp |Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann |

#### <a name="error-codes-for-token-endpoint-errors"></a>Fehlercodes für Token-Endpunktfehler
| Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- |
| invalid_request |Protokollfehler, z.B. ein fehlender erforderlicher Parameter. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| invalid_grant |Der Autorisierungscode ist ungültig oder abgelaufen. |Versuchen Sie, eine neue Anforderung an den `/authorize`-Endpunkt zu senden. |
| unauthorized_client |Der authentifizierte Client ist zur Verwendung dieses Autorisierungsgewährungstyps nicht autorisiert. |Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| invalid_client |Clientauthentifizierung fehlgeschlagen. |Die Client-Anmeldeinformationen sind nicht gültig. Um das Problem zu beheben, aktualisiert der Anwendungsadministrator die Anmeldeinformationen. |
| unsupported_grant_type |Der Autorisierungsserver unterstützt den Autorisierungsgewährungstyp nicht. |Ändern Sie den Gewährungstyp in der Anforderung. Diese Art von Fehler sollte nur während der Entwicklung auftreten und bei den ersten Tests erkannt werden. |
| invalid_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist. |Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| interaction_required |Die Anforderung erfordert eine Benutzerinteraktion. Beispielsweise ist ein zusätzlicher Schritt zur Authentifizierung erforderlich. |Wiederholen Sie die Anforderung mit der gleichen Ressource. |
| temporarily_unavailable |Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. |Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |

## <a name="use-the-access-token"></a>Verwenden des Zugriffstokens
Nachdem Sie erfolgreich ein `access_token` abgerufen haben, können Sie das Token für Anforderungen an Web-APIs verwenden, indem Sie es in den `Authorization`-Header einschließen:

> [!TIP]
> Führen Sie diese Anforderung in Postman aus. (Ersetzen Sie zuerst den `Authorization`-Header.) [![Ausführen in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Aktualisieren des Zugriffstokens
Zugriffstoken sind kurzlebig. Daher müssen Sie sie nach Ablauf aktualisieren, um weiterhin auf Ressourcen zuzugreifen.  Dazu übermitteln Sie eine weitere `POST`-Anforderung an den `/token`-Endpunkt, dieses Mal unter Angabe des `refresh_token` statt des `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh      // NOTE: Only required for web apps
```

> [!TIP]
> Führen Sie diese Anforderung in Postman aus. (Vergessen Sie dabei nicht, den `refresh_token` zu ersetzen.) [![Ausführen in Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)
> 
> 

| Parameter |  | Beschreibung |
| --- | --- | --- |
| Mandant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner.  Weitere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) Ihrer Anwendung zugewiesen hat. |
| grant_type |erforderlich |Muss der `refresh_token` für diesen Abschnitt des Autorisierungscodeflusses sein. |
| Bereich |erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen.  Die in diesem Abschnitt angeforderten Bereiche müssen den Bereichen entsprechen oder eine Teilmenge der Bereiche sein, die im ursprünglichen Autorisierungscode-Abschnitt angefordert wurden.  Wenn die in dieser Anforderung angegebenen Bereiche mehrere Ressourcenserver umfassen, gibt der v2.0-Endpunkt ein Token für die im ersten Bereich angegebene Ressource zurück.  Eine ausführlichere Erläuterung von Bereichen finden Sie in [Berechtigungen, Zustimmung und Bereiche](active-directory-v2-scopes.md). |
| refresh_token |erforderlich |Das Aktualisierungstoken, das Sie im zweiten Abschnitt des Vorgangs erhalten haben. |
| redirect_uri |erforderlich |Derselbe Wert für den Umleitungs-URI, der zum Abrufen des Autorisierungscodes verwendet wurde |
| client_secret |erforderlich für Web-Apps |Der geheime App-Schlüssel, den Sie im App-Registrierungsportal für Ihre App erstellt haben.  Er sollte nicht in einer nativen App verwendet werden, da geheime Clientschlüssel nicht zuverlässig auf Geräten gespeichert werden können.  Er ist erforderlich für Web-Apps und Web-APIs, die die Möglichkeit haben, den geheimen Client-Schlüssel sicher auf dem Server zu speichern. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung bei geschützten Ressourcen verwenden, wie z.B. eine Web-API. |
| token_type |Gibt den Wert des Tokentyps an. Der Bearertyp ist der einzige Typ, den Azure AD unterstützt. |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Bereich |Die Bereiche, für die das Zugriffstoken gültig ist. |
| refresh_token |Ein neues Aktualisierungstoken von OAuth 2.0. Ersetzen Sie das alte Aktualisierungstoken durch das neu erworbene, um sicherzustellen, dass Ihre Aktualisierungstoken so lange wie möglich gültig bleiben. |
| id_token |Ein unsigniertes JSON-Webtoken (JWT). Die App kann die Segmente dieses Tokens mit einer base64-URL decodieren, um Informationen über den angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich in Bezug auf Autorisierungs- und Sicherheitsgrenzen aber nicht darauf verlassen.  Weitere Informationen zu ID-Token finden Sie in der [Tokenreferenz zum v2.0-Endpunkt](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Fehlerantwort
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error_codes |Eine Liste der spezifischen STS-Fehlercodes, die bei der Diagnose helfen können |
| timestamp |Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann |

Eine Beschreibung der Fehlercodes und der empfohlenen Clientaktion finden Sie unter [Fehlercodes für Token-Endpunktfehler](#error-codes-for-token-endpoint-errors).


