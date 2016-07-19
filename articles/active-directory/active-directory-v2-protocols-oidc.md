
<properties
	pageTitle="Azure AD v2.0: OpenID Connect-Protokoll | Microsoft Azure"
	description="Erstellen von Webanwendungen mit der Azure AD v2.0-Implementierung des OpenID Connect-Authentifizierungsprotokolls."
	services="active-directory"
	documentationCenter=""
	authors="dstrockis"
	manager="msmbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/23/2016"
	ms.author="dastrock"/>

# v2.0-Protokolle – OpenID Connect
OpenID Connect ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei Webanwendungen angemeldet werden können. Die Implementierung von OpenID Connect im v2.0-Endpunkt ermöglicht es Ihnen, sich bei Ihren webbasierten Anwendungen anzumelden und über APIs darauf zuzugreifen. Diese sprachunabhängige Anleitung beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

> [AZURE.NOTE]
	Nicht alle Szenarios und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0- *Autorisierungs* protokoll zur Verwendung als *Authentifizierungs* protokoll, um einmaliges Anmelden mit OAuth zu ermöglichen. Dabei wird das Konzept eines `id_token` eingeführt, eines Sicherheitstokens, mit dem der Client die Identität des Benutzers überprüfen und grundlegende Profilinformationen über den Benutzer erhalten kann. Durch die Erweiterung von OAuth 2.0 können Apps **Zugriffstoken** sicher abrufen, die zum Zugriff auf Ressourcen verwendet werden können, die mithilfe eines [Autorisierungsservers](active-directory-v2-protocols.md#the-basics) geschützt werden. OpenID Connect wird für [Webanwendungen](active-directory-v2-flows.md#web-apps) empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird.

## Protokolldiagramm – Anmeldung
Der grundlegende Anmeldevorgang umfasst die folgenden Schritte – sie werden unten jeweils im Detail beschrieben.

![OpenId Connect-Verantwortlichkeitsbereiche](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## Senden der Anmeldeanforderung
Wenn die Web-App den Benutzer authentifizieren muss, kann sie ihn direkt an den `/authorize`-Endpunkt weiterleiten. Diese Anforderung ähnelt dem ersten Abschnitt des [OAuth 2.0-Autorisierungscodeflusses](active-directory-v2-protocols-oauth-code.md), aber es gibt auch einige wichtige Unterschiede:

- Die Anforderung muss im `scope`-Parameter den Bereich `openid` enthalten.
- Der `response_type`-Parameter muss `id_token` enthalten.
- Die Anforderung muss den `nonce`-Parameter enthalten.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [AZURE.TIP] Klicken Sie auf den Link unten, um diese Anforderung auszuführen. Nach der Anmeldung sollte der Browser mit einem `id_token` in der Adressleiste zu `https://localhost/myapp/` umgeleitet werden. Im Rahmen dieses Tutorials wird bei dieser Anforderung `response_mode=query` verwendet. Empfohlen wird die Verwendung von `response_mode=form_post`. <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parameter | | Beschreibung |
| ----------------------- | ------------------------------- | --------------- |
| tenant | erforderlich | Mit dem `{tenant}`-Wert im Pfad der Anforderung kann gesteuert werden, welche Benutzer sich bei der Anwendung anmelden können. Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner. Ausführlichere Informationen finden Sie in den [Grundlagen zu Protokollen](active-directory-v2-protocols.md#endpoints). |
| client\_id | erforderlich | Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com)) Ihrer Anwendung zugewiesen hat. |
| response\_type | erforderlich | Muss das `id_token` für die OpenID Connect-Anmeldung enthalten. Es können auch andere Antworttypen enthalten sein, z. B. `code`. |
| redirect\_uri | empfohlen | Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich | erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen. Für OpenID Connect muss der Bereich `openid` enthalten sein, der auf der Zustimmungsbenutzeroberfläche die Anmeldeberechtigung ergibt. Sie können in diese Anforderung auch andere Bereiche für das Anfordern der Zustimmung aufnehmen. |
| nonce | erforderlich | Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten sein wird. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| response\_mode | empfohlen | Gibt die Methode an, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll. Kann entweder "Abfrage", "form\_post" oder "Fragment" sein. Bei Webanwendungen empfiehlt sich die Verwendung von `response_mode=form_post`, um eine möglichst sichere Tokenübertragung an die Anwendung zu gewährleisten.  
| state | empfohlen | Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Normalerweise wird ein eindeutiger, nach dem Zufallsprinzip generierter Wert verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12). Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung | optional | Gibt den Typ der erforderlichen Benutzerinteraktion an. Zu diesem Zeitpunkt sind die einzigen gültigen Werte „login“, „none“ und „consent“. `prompt=login` zwingt den Benutzer, die Anmeldeinformationen in dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich. `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird. Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der v2.0-Endpunkt einen Fehler zurück. `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| login\_hint | optional | Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, nachdem sie den Benutzernamen mithilfe des Anspruchs `preferred_username` aus einer vorherigen Anmeldung extrahiert haben. |
| domain\_hint | optional | Kann `consumers` oder `organizations` sein. Wenn dieser Parameter vorhanden ist, wird der E-Mail-basierte Ermittlungsvorgang übersprungen, den der Benutzer auf der v2.0-Anmeldeseite durchläuft, was die Benutzerfreundlichkeit verbessert. Apps verwenden diesen Parameter häufig für die wiederholte Authentifizierung, indem sie den Anspruch `tid` aus dem ID-Token extrahieren. Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat. Verwenden Sie andernfalls `domain_hint=organizations`. |
Zu diesem Zeitpunkt wird der Benutzer dazu aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen. Der V2.0-Endpunkt stellt auch sicher, dass der Benutzer den Berechtigungen zugestimmt hat, die im `scope`-Abfrageparameter angegeben sind. Wenn der Benutzer keiner Berechtigung zugestimmt hat, wird er dazu aufgefordert, den erforderlichen Berechtigungen zuzustimmen. Nähere Einzelheiten zu [Berechtigungen, Zustimmung und mehrinstanzenfähigen Apps erhalten Sie hier](active-directory-v2-scopes.md).

Sobald der Benutzer authentifiziert ist und seine Zustimmung erteilt hat, gibt der v2.0-Endpunkt mithilfe der Methode im festgelegten `response_mode`-Parameter eine Antwort auf dem angegebenen `redirect_uri` an Ihre App zurück.

#### Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=form_post` sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| id\_token | Das ID-Token, das die Anwendung angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [Referenz für den v2.0-Endpunkttoken](active-directory-v2-tokens.md). |
| state | Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### Fehlerantwort
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

#### Fehlercodes beim Autorisierungsendpunktfehler

Die folgende Tabelle beschreibt die verschiedenen Fehlercodes, die im `error`-Parameter der Fehlerantwort zurückgegeben werden können:

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid\_request | Protokollfehler, z.B. ein fehlender erforderlicher Parameter. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird.|
| unauthorized\_client | Die Clientanwendung darf keinen Autorisierungscode anfordern. | Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| access\_denied | Der Ressourcenbesitzer hat die Zustimmung verweigert. | Die Clientanwendung kann den Benutzer benachrichtigen, dass sie ohne Zustimmung des Benutzers nicht fortfahren kann. |
| unsupported\_response\_type | Der Autorisierungsserver unterstützt den Antworttyp in der Anforderung nicht. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird.|
|server\_error | Der Server hat einen unerwarteten Fehler erkannt. | Wiederholen Sie die Anforderung. Diese Fehler werden durch temporäre Bedingungen verursacht. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund eines temporären Fehlers verzögert ist. |
| temporarily\_unavailable | Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. | Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |
| invalid\_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist.| Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |

## Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens allein reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur des ID-Tokens validieren und die Ansprüche im Token gemäß der App-Anforderungen überprüfen. Der v2.0-Endpunkt verwendet [JSON-Webtoken (JSTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Sie können `id_token` auch im Clientcode überprüfen. Es ist jedoch üblich, `id_token` an einen Back-End-Server zu senden und die Überprüfung dort auszuführen. Nachdem Sie die Signatur des ID-Tokens validiert haben, müssen Sie noch einige Ansprüche überprüfen. Weitere Informationen finden Sie in der [V2.0-Tokenreferenz](active-directory-v2-tokens.md). Dort finden Sie auch Einzelheiten zum [Überprüfen von Token](active-directory-v2-tokens.md#validating-tokens) und wichtige Informationen zum Signaturschlüsselrollover ([Überprüfen der Signatur](active-directory-v2-tokens.md#validating-tokens)). Wir empfehlen, zum Analysieren und Überprüfen von Token eine Bibliothek zu verwenden. Für die meisten Sprachen und Plattformen ist mindestens eine Bibliothek verfügbar.
<!--TODO: Improve the information on this-->

Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

- Sicherstellen, dass sich der Benutzer/die Organisation für die App angemeldet hat.
- Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und Berechtigungen verfügt.
- Sicherstellen, dass eine bestimmte Stärke der Authentifizierung aufgetreten ist, z.B. die mehrstufige Authentifizierung.

Weitere Informationen zu den Ansprüchen in einem ID-Token finden Sie in der [Tokenreferenz zum V2.0-Endpunkt](active-directory-v2-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie mit dem Benutzer eine Sitzung beginnen und die Ansprüche im ID-Token zum Abrufen von Informationen über den Benutzer in der App verwenden. Diese Informationen kann für die Anzeige, für Datensätze, für die Autorisierung usw. verwendet werden.

## Senden einer Abmeldungsanforderung

Der OpenIdConnect-`end_session_endpoint` wird derzeit vom V2.0-Endpunkt nicht unterstützt. Dies bedeutet, dass Ihre App keine Anforderung an den v2.0-Endpunkt zum Beenden der Sitzung eines Benutzers und zum Löschen von Cookies, die vom v2.0-Endpunkt festgelegt wurden, senden kann. Um einen Benutzer abzumelden, kann eine App ganz einfach die eigene Sitzung mit dem Benutzer beenden, und die Sitzung des Benutzers mit dem v2.0-Endpunkt unverändert lassen. Wenn der Benutzer das nächste Mal versucht, sich anzumelden, wird eine Seite mit dem Hinweis "Konto auswählen" angezeigt, auf der die aktiv angemeldeten Konten aufgeführt werden. Auf dieser Seite kann der Benutzer sich von jedem Konto abmelden, sodass die Sitzung mit dem v2.0-Endpunkt beendet wird.

<!--

When you wish to sign the user out of the app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->

## Protokolldiagramm – Tokenerfassung
Viele Web-Apps müssen nicht nur den Benutzer anmelden, sondern auch mithilfe von OAuth im Namen dieses Benutzers auf einen Webdienst zugreifen. Dieses Szenario kombiniert OpenID Connect für die Benutzerauthentifizierung und ruft gleichzeitig einen Autorisierungscode („authorization\_code) ab, der mithilfe des OAuth-Autorisierungscodeflusses Zugriffstoken („access\_token“) abrufen kann:

Der vollständige Ablauf für die Anmeldung mit OpenID Connect und die Erfassung Abruf von Token sieht etwa wie folgt aus. Die einzelnen Schritte werden unten im Detail beschrieben.

![OpenId Connect-Verantwortlichkeitsbereiche](../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## Abrufen von Zugriffstoken
Zum Abrufen von Zugriffstoken müssen Sie die oben aufgeführte Anmeldeanforderung leicht abwandeln:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e		// Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F 	  // Your registered Redirect Uri, url encoded
&response_mode=form_post						      // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes your app needs  
offline_access%20										 
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345						 				 // Any value, provided by your app
&nonce=678910										 // Any value, provided by your app
```

> [AZURE.TIP] Klicken Sie auf den Link unten, um diese Anforderung auszuführen. Nach der Anmeldung sollte der Browser mit einem `id_token` und einem `code` in der Adressleiste zu `https://localhost/myapp/` umgeleitet werden. Im Rahmen dieses Tutorials wird bei dieser Anforderung `response_mode=query` verwendet. Empfohlen wird die Verwendung von `response_mode=form_post`. <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

Durch das Einschließen von Berechtigungsbereichen in die Anforderung und durch die Verwendung von `response_type=code+id_token` stellt der v2.0-Endpunkt sicher, dass der Benutzer den im `scope`-Abfrageparameter angegebenen Berechtigungen zugestimmt hat, und gibt dann der App einen Autorisierungscode zum Austausch für ein Zugriffstoken zurück.

#### Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=form_post` sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschreibung |
| ----------------------- | ------------------------------- |
| id\_token | Das ID-Token, das die Anwendung angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [Referenz für den v2.0-Endpunkttoken](active-directory-v2-tokens.md). |
| Code | Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. Autorisierungscodes sind sehr kurzlebig und laufen in der Regel nach etwa zehn Minuten ab. |
| state | Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### Fehlerantwort
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

Eine Beschreibung der möglichen Fehlercodes und der jeweils empfohlenen Clientaktion finden Sie unter [Fehlercodes beim Autorisierungsendpunktfehler](#error-codes-for-authorization-endpoint-errors).

Nachdem Sie einen Autorisierungs`code` und einen `id_token` erhalten haben, können Sie den Benutzer anmelden und Zugriffstoken in seinem Namen abrufen. Zum Anmelden des Benutzers müssen Sie das `id_token` genau wie [oben beschrieben](#validating-the-id-token) validieren. Zum Abrufen von Zugriffstoken können Sie die in unserer [OAuth-Protokolldokumentation beschriebenen Schritte](active-directory-v2-protocols-oauth-code.md#request-an-access-token) ausführen

<!---HONumber=AcomDC_0629_2016-->