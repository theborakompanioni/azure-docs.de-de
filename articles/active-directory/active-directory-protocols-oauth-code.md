<properties
	pageTitle="Übersicht über das Azure AD .NET-Protokoll | Microsoft Azure"
	description="Dieser Artikel beschreibt, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mithilfe von Azure Active Directory und OAuth 2.0 verwenden."
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="01/21/2016"
	ms.author="priyamo"/>


# Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

Azure Active Directory (Azure AD) verwendet OAuth 2.0, um den Zugriff auf Webanwendungen und Web-APIs in Ihrem Azure AD-Mandanten zu autorisieren. Diese sprachunabhängige Anleitung beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

Der OAuth 2.0-Autorisierungscodefluss wird in [Abschnitt 4.1 der OAuth 2.0-Spezifikation](https://tools.ietf.org/html/rfc6749#section-4.1) beschrieben. Er wird zum Ausführen der Authentifizierung und Autorisierung in den meisten App-Typen genutzt, einschließlich Web-Apps und nativ installierten Apps.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## OAuth 2.0-Autorisierungsfluss

Allgemein sieht der gesamte Autorisierungsfluss für eine Anwendung etwa wie folgt aus:

![OAuth-Autorisierungscodefluss](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## Anfordern eines Autorisierungscodes

Der Autorisierungscodefluss beginnt damit, dass der Client den Benutzer auf den `/authorize`-Endpunkt leitet. In dieser Anforderung gibt der Client die Berechtigungen an, die er vom Benutzer abrufen muss. Sie können die OAuth 2.0-Endpunkte von der Seite Ihrer Anwendung im klassischen Azure-Portal beziehen (unten über die Schaltfläche **Endpunkte anzeigen**).

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| tenant | erforderlich | Mit dem `{tenant}`-Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Die zulässigen Werte sind Mandantenbezeichner, z.B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder `common` für mandantenunabhängigen Token. |
| client\_id | erforderlich | Die Anwendungs-Id, die Ihrer App zugewiesen wird, wenn Sie sie mit Azure AD registrieren. Diese finden Sie im Azure-Verwaltungsportal. Klicken Sie auf **Active Directory**. Klicken Sie anschließend auf das Verzeichnis, danach auf die Anwendung und anschließend auf **Konfigurieren**. |
| response\_type | erforderlich | Muss `code` für den Autorisierungscodefluss enthalten. |
| redirect\_uri | empfohlen | Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. Für native und mobile Apps sollten Sie den Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden. |
| Bereich | erforderlich | Eine durch Leerzeichen getrennte Liste mit [Bereichen](active-directory-v2-scopes.md), denen der Benutzer zustimmen soll. |
| response\_mode | empfohlen | Gibt die Methode an, die zum Senden des resultierenden Tokens zurück an Ihre App verwendet werden soll. Kann `query` oder `form_post` sein. |
| state | empfohlen | Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, [um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12). Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung | optional | Gibt den Typ der erforderlichen Benutzerinteraktion an. Zu diesem Zeitpunkt sind die einzigen gültigen Werte „login“, „none“ und „consent“. `prompt=login` zwingt den Benutzer, die Anmeldeinformationen in dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich. `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird. Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der v2.0-Endpunkt einen Fehler zurück. `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| login\_hint | optional | Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, nachdem sie den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahiert haben. |
| domain\_hint | optional | Kann entweder `consumers` oder `organizations` sein. Wenn dieser Parameter vorhanden ist, wird der E-Mail-basierte Ermittlungsvorgang übersprungen, den der Benutzer auf der v2.0-Anmeldeseite durchläuft, was die Benutzerfreundlichkeit verbessert. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, indem sie `tid` aus einer vorherigen Anmeldung extrahieren. Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat. Verwenden Sie andernfalls `domain_hint=organizations`. |

> [AZURE.NOTE] Wenn der Benutzer Teil einer Organisation ist, kann ein Administrator der Organisation im Auftrag des Benutzers zustimmen oder ablehnen oder dem Benutzer die Zustimmung erlauben. Der Benutzer erhält die Option zur Zustimmung nur dann, wenn der Administrator dies zulässt.

Nun wird der Benutzer zur Eingabe der Anmeldeinformationen und zur Zustimmung zu den im Abfrageparameter `scope` angegebenen Berechtigungen aufgefordert. Nach der Authentifizierung und der Zustimmung durch den Benutzer versendet Azure AD eine Antwort an Ihre App unter der `redirect_uri`-Adresse in Ihrer Anforderung.

### Erfolgreiche Antwort

Eine erfolgreiche Antwort sieht wie folgt aus:

```
http://localhost:12345/?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&state=12345
&session_state=733ad279-b681-49c3-9215-951abf94d2c5
```

| Parameter | Beschreibung |
| -----------------------| --------------- |
| admin\_consent | Der Wert ist „true“, wenn ein Administrator einer Aufforderung zu einer Zustimmungsanforderung zugestimmt hat.|
| Code | Der Autorisierungscode, den die App angefordert hat. Die Anwendung kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. |
| session\_state | Ein eindeutiger Wert, der die aktuelle Benutzersitzung identifiziert. Dieser Wert ist eine GUID, sollte jedoch als opaker Wert behandelt werden, der ohne Prüfung übergeben wird. |
| state | Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte die Statuswerte in der Anforderung überprüfen.
Wenn ein Statusparameter in der Anforderung enthalten ist, wird der gleiche Wert in der Antwort angezeigt. Es hat sich bewährt, wenn die Anwendung überprüft, ob die Statuswerte in der Anforderung und in der Antwort identisch sind.

### Fehlerantwort

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese entsprechend behandeln kann.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

## Fordern Sie ein Zugriffstoken mithilfe des Autorisierungscodes an.

Wenn Sie einen Autorisierungscode erworben und die Berechtigung vom Benutzer erhalten haben, können Sie den Code für ein Zugriffstoken auf die gewünschte Ressource einlösen, indem Sie eine POST-Anforderung an den `/token`-Endpunkt senden:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: client_secret only required for web apps
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------------- |
| tenant | erforderlich | Mit dem `{tenant}`-Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Die zulässigen Werte sind Mandantenbezeichner, z.B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder `common` für mandantenunabhängigen Token. |
| client\_id | erforderlich | Die Anwendungs-Id, die Ihrer App zugewiesen wird, wenn Sie sie mit Azure AD registrieren. Diese finden Sie im klassischen Azure-Portal. Klicken Sie auf **Active Directory**. Klicken Sie anschließend auf das Verzeichnis, danach auf die Anwendung und anschließend auf **Konfigurieren**. |
| grant\_type | erforderlich | Muss der `authorization_code` für den Autorisierungscodefluss sein. |
| Code | erforderlich | Der `authorization_code`, den die Sie im vorherigen Abschnitt abgerufen haben |
| redirect\_uri | erforderlich | Der `redirect_uri`-Wert, den Sie zum Abrufen von `authorization_code` verwendet haben |
| client\_secret | erforderlich für Web-Apps | Der geheime App-Schlüssel, den Sie im App-Registrierungsportal für Ihre App erstellt haben. Er sollte nicht in einer systemeigenen App verwendet werden, da geheime Client-Schlüssel nicht zuverlässig auf Geräten gespeichert werden können. Er ist für Web-Apps und Web-APIs erforderlich, die die Möglichkeit haben, `client_secret` sicher auf dem Server zu speichern. |


### Erfolgreiche Antwort

Eine erfolgreiche Antwort sieht wie folgt aus:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| access\_token | Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung auf geschützten Ressourcen verwenden, wie z. B. eine Web-API. |
| token\_type | Gibt den Wert des Tokentyps an. Bearertoken ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth2.0 Authorization Framework: Bearer Token Usage (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) (OAuth2.0-Autorisierungsframework: Verwendung von Bearertoken (RFC 6750)). |
| expires\_in | Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Bereich | Die Identitätswechselberechtigungen, die der Clientanwendung gewährt wurden. Die Standardberechtigung ist `user_impersonation`. Der Besitzer der gesicherten Ressource kann zusätzliche Werte in Azure AD registrieren.|
| refresh\_token | Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Zugriffstoken zusätzliche Zugriffstoken zu erhalten. Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. |
| id\_token | Ein unsigniertes JSON-Webtoken (JWT). Die App kann die Segmente dieses Tokens mit einer base64-URL decodieren, um Informationen über den angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich jedoch nicht für Autorisierungs- und Sicherheitsgrenzen auf sie verlassen. |

### JWT-Tokenansprüche
Das JWT-Token im Wert des Parameters `id_token` kann in die folgenden Ansprüche decodiert werden:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Der Parameter `id_token` beinhaltet die folgenden Anspruchstypen. Weitere Informationen zu JSON-Webtoken finden Sie im [JWT IETF-Spezifikationsentwurf](http://go.microsoft.com/fwlink/?LinkId=392344). Weitere Informationen zu den Tokentypen und Ansprüchen finden Sie unter [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md).

| Anspruchstyp | Beschreibung |
|------------|-------------|
| aud | Zielgruppe des Tokens. Wenn das Token für eine Clientanwendung ausgestellt wird, ist die Zielgruppe die `client_id` des Clients.
| exp | Ablaufzeit. Die Uhrzeit, zu der das Token abläuft. Damit das Token gültig ist, darf das aktuelle Datum/die aktuelle Uhrzeit nicht größer sein als der Wert `exp`. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| family\_name | Nachname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| given\_name | Vorname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| iat | „Issued at time“ (Ausgestellt um). Der Zeitpunkt, zu dem das JWT ausgestellt wurde. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| iss | Identifiziert den Herausgeber des Tokens. |
| nbf | „Not before time“ (Nicht vor). Der Zeitpunkt, ab dem das Token gültig ist. Damit das Token gültig ist, darf das aktuelle Datum/die aktuelle Uhrzeit nicht kleiner sein als der Wert „Nbf“. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| oid | Objektbezeichner (ID) des Benutzerobjekts in Azure AD. |
| sub | Der Token-Antragstellerbezeichner. Dies ist ein dauerhafter und unveränderlicher Bezeichner für den Benutzer, den das Token beschreibt. Verwenden Sie diesen Wert in der Logik für das Zwischenspeichern. |
| tid | Mandantenbezeichner (ID) des Azure AD-Mandanten, der das Token ausgestellt hat. |
| unique\_name | Ein eindeutiger Bezeichner, der dem Benutzer angezeigt werden kann. Dies ist in der Regel ein Benutzerprinzipalname (UPN). |
| upn | Benutzerprinzipalname. |
| ver | Version. Die Version des JWT-Tokens, in der Regel 1.0. |

### Fehlerantwort

Eine Beispiel für eine Fehlerantwort sieht wie folgt aus:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error\_description | Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error\_codes | Eine Liste der spezifischen STS-Fehlercodes, die bei der Diagnose helfen können |
| timestamp | Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace\_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation\_id | Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann|

## Verwenden des Zugriffstokens für den Zugriff auf die Ressource

Nachdem Sie erfolgreich ein `access_token` abgerufen haben, können Sie das Token für Anforderungen an Web-APIs verwenden, indem Sie es in den `Authorization`-Header einbeziehen. Die Spezifikation [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) erklärt, wie Bearertoken in HTTP-Anforderungen für den Zugriff auf geschützte Ressourcen verwendet werden.

### Beispiel für eine Anforderung

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

## Aktualisieren der Zugriffstoken

Zugriffstoken sind kurzlebig und müssen nach Ablauf aktualisiert werden, damit Sie weiterhin auf Ressourcen zugreifen können. Übermitteln Sie zum Aktualisieren von `access_token` eine weitere `POST`-Anforderung an den `/token`-Endpunkt, dieses Mal unter Angabe des `refresh_token` statt des `code`:

Die Gültigkeitsdauer eines Aktualisierungstokens wird nicht bereitgestellt und variiert ausgehend von den Richtlinieneinstellungen und dem Zeitpunkt, zu dem die Autorisierungscodegewährung von Azure AD zurückgezogen wird. Ihre Anwendung sollte es auch verarbeiten können, wenn die Anforderung für ein neues Zugriffstoken fehlschlägt. In dem Fall kehrt sie zu dem Code zurück, der ein neues Zugriffstoken anfordert.

Ein Beispiel für eine Anforderung an den **mandantenspezifischen** Endpunkt (Sie können auch den **allgemeinen** Endpunkt verwenden), um ein neues Zugriffstoken mithilfe eines Aktualisierungstokens abzurufen, sieht folgendermaßen aus:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parameter | Beschreibung |
|-----------|-------------|
| access\_token | Das neue Zugriffstoken, das angefordert wurde.|
| expires\_in | Die verbleibende Gültigkeitsdauer des Tokens in Sekunden. Ein typischer Wert wäre etwa 3600 (eine Stunde). |
| expires\_on | Datum und Uhrzeit, wenn das Token abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. |
| refresh\_token | Ein neues OAuth 2.0-Aktualisierungstoken, das zum Anfordern neuer Zugriffstoken verwendet werden kann, wenn das Token in dieser Antwort abläuft. |
| resource | Gibt die gesicherte Ressource an, die das Zugriffstoken für den Zugriff verwenden kann. |
| Bereich | Die Identitätswechselberechtigungen, die der nativen Clientanwendung gewährt wurden. Die Standardberechtigung ist **user\_impersonation**. Der Besitzer der Zielressource kann alternative Werte in Azure AD registrieren. |
| token\_type | Der Tokentyp. Der einzige derzeit unterstützte Wert ist **bearer**. |

### Erfolgreiche Antwort

Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### Fehlerantwort

Eine Beispiel für eine Fehlerantwort sieht wie folgt aus:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

<!---HONumber=AcomDC_0601_2016-->