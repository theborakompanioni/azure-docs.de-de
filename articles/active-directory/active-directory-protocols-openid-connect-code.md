<properties
	pageTitle="Übersicht über das Azure AD .NET-Protokoll | Microsoft Azure"
	description="In diesem Artikel wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mithilfe von Azure Active Directory und OpenID Connect verwenden."
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


# Autorisieren des Zugriffs auf Webanwendungen mit OpenID Connect und Azure Active Directory

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) ist eine einfache Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. OAuth 2.0 definiert Mechanismen zum Beschaffen und Verwenden von **Zugriffstoken** für den Zugriff auf geschützte Ressourcen, aber es werden keine Standardmethoden zum Bereitstellen von Identitätsinformationen definiert. OpenID Connect implementiert die Authentifizierung als Erweiterung des OAuth 2.0-Autorisierungsprozesses. Es werden Informationen zum Endbenutzer in Form eines `id_token`-Elements bereitgestellt, mit dem die Identität des Benutzers überprüft und grundlegende Profilinformationen zum Benutzer geliefert werden.

OpenID Connect wird für Webanwendungen empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird.

## Authentifizierungsfluss bei OpenID Connect

Der grundlegende Anmeldevorgang umfasst die folgenden Schritte – sie werden unten jeweils im Detail beschrieben.

![OpenId Connect-Authentifizierungsfluss](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## Senden der Anmeldeanforderung

Wenn die Webanwendung den Benutzer authentifizieren muss, muss sie ihn direkt an den `/authorize`-Endpunkt weiterleiten. Diese Anforderung ähnelt dem ersten Abschnitt des [OAuth 2.0-Autorisierungscodeflusses](active-directory-protocols-oauth-code.md), aber es gibt auch einige wichtige Unterschiede:

- Die Anforderung muss im `scope`-Parameter den Bereich `openid` enthalten.
- Der `response_type`-Parameter muss `id_token` enthalten.
- Die Anforderung muss den `nonce`-Parameter enthalten.

Ein Beispiel für eine Anforderung sieht also wie folgt aus:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| tenant | erforderlich | Mit dem `{tenant}`-Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Die zulässigen Werte sind Mandantenbezeichner, z.B. `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder `common` für mandantenunabhängige Token. |
| client\_id | erforderlich | Die Anwendungs-ID, die Ihrer App zugewiesen wird, wenn Sie sie bei Azure AD registrieren. Diese finden Sie im klassischen Azure-Portal. Klicken Sie auf **Active Directory**. Klicken Sie anschließend auf das Verzeichnis, auf die Anwendung und dann auf **Konfigurieren**. |
| response\_type | erforderlich | Muss das `id_token` für die OpenID Connect-Anmeldung enthalten. Es können auch andere Antworttypen enthalten sein, z. B. `code`. |
| Bereich | erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen. Für OpenID Connect muss der Bereich `openid` enthalten sein, der in der Berechtigung „Sie werden angemeldet“ in der Zustimmungsbenutzeroberfläche resultiert. Sie können in diese Anforderung auch andere Bereiche für das Anfordern der Zustimmung aufnehmen. |
| nonce | erforderlich | Ein Wert in der von der App erzeugten Anforderung, die im sich ergebenden `id_token`-Element als Anspruch enthalten ist. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge oder GUID, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| redirect\_uri | empfohlen | Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| response\_mode | empfohlen | Gibt die Methode an, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll. Unterstützt Werte sind `form_post` für *HTTP-Formularbereitstellung* oder `fragment` für *URL-Fragment*. Bei Webanwendungen empfiehlt sich die Verwendung von `response_mode=form_post` als sicherste Übertragung von Token in die Anwendung.  
| state | empfohlen | Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12). Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung | optional | Gibt den Typ der erforderlichen Benutzerinteraktion an. Zu diesem Zeitpunkt sind die einzigen gültigen Werte „login“, „none“ und „consent“. `prompt=login` zwingt den Benutzer, die Anmeldeinformationen in dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich. `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird. Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der Endpunkt einen Fehler zurück. `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| login\_hint | optional | Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, nachdem sie den Benutzernamen mithilfe des Anspruchs `preferred_username` aus einer vorherigen Anmeldung extrahiert haben. |


Zu diesem Zeitpunkt wird der Benutzer dazu aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen.

### Beispiel für eine Antwort

Eine Antwort nach der Benutzerauthentifizierung sieht beispielsweise wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| id\_token | Das von der App angeforderte `id_token`-Element. Sie können mit dem `id_token`-Element die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. |
| state | Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

### Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error\_description | Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |


## Überprüfen des ID-Tokens


Das Empfangen eines `id_token`-Elements allein reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur validieren und die Ansprüche im `id_token`-Element gemäß den Anforderungen der App überprüfen. Der Azure AD-Endpunkt verwendet JSON-Webtoken (JWTs) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Sie können `id_token` auch im Clientcode überprüfen. Es ist aber eine bewährte Methode, `id_token` an einen Back-End-Server zu senden und die Überprüfung dort durchzuführen. Nachdem Sie die Signatur des `id_token`-Elements validiert haben, müssen Sie noch einige Ansprüche überprüfen.

Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

- Sicherstellen, dass sich der Benutzer/die Organisation für die App angemeldet hat.
- Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und Berechtigungen verfügt.
- Sicherstellen, dass eine bestimmte Stärke der Authentifizierung aufgetreten ist, z.B. die mehrstufige Authentifizierung.

Nachdem Sie das `id_token`-Element vollständig überprüft haben, können Sie mit dem Benutzer eine Sitzung beginnen und die Ansprüche im `id_token`-Element zum Abrufen von Informationen über den Benutzer in der App verwenden. Diese Informationen kann für die Anzeige, für Datensätze, für die Autorisierung usw. verwendet werden. Weitere Informationen zu den Tokentypen und Ansprüchen finden Sie unter [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md).

## Senden einer Abmeldungsanforderung

Wenn Sie den Benutzer bei der App abmelden möchten, reicht es nicht, die Cookies der App zu löschen oder die Sitzung mit dem Benutzer auf andere Weise zu beenden. Sie müssen den Benutzer für die Abmeldung außerdem an den `end_session_endpoint` umleiten. Wenn Sie dies nicht tun, kann sich der Benutzer erneut für Ihre Anwendung authentifizieren, ohne die Anmeldeinformationen erneut einzugeben, da diese über eine gültige SSO-Sitzung beim Azure AD-Endpunkt verfügen.

Sie können den Benutzer einfach an den `end_session_endpoint` umleiten, der im OpenID Connect-Metadatendokument aufgeführt wird:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | ------------ |
| post\_logout\_redirect\_uri | empfohlen | Die URL, an die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll. Wenn keine Angabe erfolgt, wird dem Benutzer eine generische Meldung angezeigt. |

## Tokenbeschaffung

Viele Web-Apps müssen nicht nur den Benutzer anmelden, sondern auch mithilfe von OAuth im Namen dieses Benutzers auf einen Webdienst zugreifen. Bei diesem Szenario wird OpenID Connect für die Benutzerauthentifizierung kombiniert, und gleichzeitig wird ein Autorisierungscode (`authorization_code`) abgerufen, der mithilfe des OAuth-Autorisierungscodeflusses zum Abrufen von Zugriffstoken (`access_tokens`) verwendet werden kann.

## Abrufen von Zugriffstoken

Zum Abrufen von Zugriffstoken müssen Sie die oben aufgeführte Anmeldeanforderung abwandeln:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e		// Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 	  // Your registered Redirect Uri, url encoded
&response_mode=form_post						      // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F									 
&state=12345						 				 // Any value, provided by your app
&nonce=678910										 // Any value, provided by your app
```

Durch das Einschließen von Berechtigungsbereichen in die Anforderung und durch die Verwendung von `response_type=code+id_token` stellt der `authorize`-Endpunkt sicher, dass der Benutzer den im `scope`-Abfrageparameter angegebenen Berechtigungen zugestimmt hat. Er gibt für die App dann einen Autorisierungscode zum Austausch für ein Zugriffstoken zurück.

### Erfolgreiche Antwort

Eine erfolgreiche Antwort mit `response_mode=form_post` sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| id\_token | Das von der App angeforderte `id_token`-Element. Sie können mit dem `id_token`-Element die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. |
| Code | Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. Autorisierungscodes sind sehr kurzlebig und laufen in der Regel nach etwa zehn Minuten ab. |
| state | Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

### Fehlerantwort

Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| Fehler | Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error\_description | Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Nachdem Sie einen Autorisierungs`code` und einen `id_token` erhalten haben, können Sie den Benutzer anmelden und Zugriffstoken in seinem Namen abrufen. Zum Anmelden des Benutzers müssen Sie das `id_token`-Element genau wie oben beschrieben validieren. Zum Abrufen von Zugriffstoken können Sie die in unserer [OAuth-Protokolldokumentation beschriebenen Schritte](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token) ausführen

<!---HONumber=AcomDC_0601_2016-->