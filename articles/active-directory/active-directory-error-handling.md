<properties
	pageTitle="Fehlerbehandlung in OAuth 2.0 | Microsoft Azure"
	description="Dieser Artikel beschreibt häufig auftretende Fehler in OAuth 2.0 mit Azure Active Directory und die bewährten Methoden für den Umgang damit."
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

# Fehlerbehandlung in OAuth 2.0

[AZURE.INCLUDE [active-directory-protocols](../../includes/active-directory-protocols.md)]

In diesem Artikel lernen Sie einige bewährten Methoden für die Behandlung von Fehlern kennen, die beim Autorisieren Ihrer Anwendung in Azure Active Directory (Azure AD) häufig auftreten. Weitere Informationen zum Autorisierungsendpunkt sowie zum Tokenausstellungs-Endpunkt finden Sie unter [Authentication flow for an application in Azure AD](active-directory-protocols-oauth-code.md) (Autorisierungsfluss für eine Anwendung in Azure AD).

## Fehler beim Autorisierungsendpunkt

Zu Beginn der Autorisierung sendet die Clientanwendung eine Anforderung an den `/authorize`-Endpunkt von Azure AD mit einer Liste der Berechtigungen, die vom Benutzer erforderlich sind.

Autorisierungsendpunktfehler gehen vom `/authorize`-Endpunkt aus. Diese werden entweder als HTTP 200-Fehler auf einer Webseite zurückgegeben, oder mithilfe eines HTTP 302-Umleitungscodes, wenn eine Clientanwendung für die Fehlerbehandlung verfügbar ist.

Im Folgenden finden Sie ein Beispiel für eine HTTP 302-Fehlerantwort vom Azure AD-Autorisierungsendpunkt, wenn bei einer Anforderung der erforderliche `response_type`-Parameter nicht vorhanden ist.

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?error=invalid_request&error_description=AADSTS90014%3a+The+request+body+must+contain+the+following+parameter%3a+%27response_type%27.%0d%0aTrace+ID%3a+57f5cb47-2278-4802-a018-d05d9145daad%0d%0aCorrelation+ID%3a+570a9ed3-bf1d-40d1-81ae-63465cc25488%0d%0aTimestamp%3a+2013-12-31+05%3a51%3a35Z&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschreibung |
|-----------|-------------|
| Fehler | Ein in Abschnitt 5.2 definierter Fehlercodewert des [OAuth 2.0-Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749). In der folgenden Tabelle finden Sie die Fehlercodes, die von Azure AD ausgegeben werden. |
| error\_description | Detailliertere Beschreibung des Fehlers. Diese Meldung ist nicht für den Endbenutzer ausgelegt. |
| state | Der Statuswert ist ein zufällig generierter, nicht wiederverwendeter Wert, der in der Anforderung versendet und in der Antwort zurückgegeben wird, um webseitenübergreifende Anforderungsfälschungen (CSFR) zu vermeiden. |

### Fehlercodes beim Autorisierungsendpunktfehler

Die folgende Tabelle beschreibt die verschiedenen Fehlercodes, die im `error`-Parameter der Fehlerantwort zurückgegeben werden können.

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid\_request | Protokollfehler, z.B. ein fehlender erforderlicher Parameter. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird.|
| unauthorized\_client | Die Clientanwendung darf keinen Autorisierungscode anfordern. | Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| access\_denied | Der Ressourcenbesitzer hat die Zustimmung verweigert. | Die Clientanwendung kann den Benutzer benachrichtigen, dass sie ohne Zustimmung des Benutzers nicht fortfahren kann. |
| unsupported\_response\_type | Der Autorisierungsserver unterstützt den Antworttyp in der Anforderung nicht. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstest entdeckt wird.|
|server\_error | Der Server hat einen unerwarteten Fehler erkannt. | Wiederholen Sie die Anforderung. Diese Fehler werden durch temporäre Bedingungen verursacht. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund eines temporären Fehlers verzögert ist. |
| temporarily\_unavailable | Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. | Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |
| invalid\_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist.| Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |

## Fehler beim Tokenausstellungs-Endpunkt

Sobald die Anwendung einen Autorisierungscode vom `/authorize`-Endpunkt erhält, übergibt sie ihn an den `/token`-Endpunkt, um ein Zugriffstoken für die angegebene Ressource zu erhalten.

Fehler beim Tokenausstellungs-Endpunkt gehen vom `/token`-Endpunkt aus. Dies sind HTTP-Fehlercodes, da der Client den Tokenausstellungs-Endpunkt direkt aufruft. Zusätzlich zum HTTP-Statuscode gibt der Azure AD-Tokenausstellungs-Endpunkt auch ein JSON-Dokument mit Objekten zurück, die den Fehler beschreiben.

Ein Fehler bei einem ungültigen `client_id`-Parameter in der Anforderung könnte beispielsweise wie folgt aussehen:

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8

{"error":"invalid_request","error_description":"AADSTS90011: Request is ambiguous, multiple application identifiers found. Application identifiers: '197451ec-ade4-40e4-b403-02105abd9049, 597451ec-ade4-40e4-b403-02105abd9049'.\r\nTrace ID: 4457d068-2a03-42b2-97f2-d55325289d86\r\nCorrelation ID: 6b3474d8-233e-463f-b0a3-86433d8ba889\r\nTimestamp: 2013-12-31 06:31:41Z","error_codes":[90011],"timestamp":"2013-12-31 06:31:41Z","trace_id":"4457d068-2a03-42b2-97f2-d55325289d86","correlation_id":"6b3474d8-233e-463f-b0a3-86433d8ba889"}
```
### HTTP-Statuscodes

Die folgende Tabelle enthält die HTTP-Statuscodes, die vom Tokenausstellungs-Endpunkt zurückgegeben werden. In einigen Fällen ist der Fehlercode ausreichend, um die Antwort zu beschreiben. Wenn Fehler auftreten, müssen Sie jedoch die zugehörigen JSON-Dokumente analysieren und den Fehlercode überprüfen.

| HTTP-Code | Beschreibung |
|-----------|-------------|
| 400 | Standard-HTTP-Code. Wird in den meisten Fällen verwendet und wird in der Regel aufgrund einer fehlerhaften Anforderung ausgegeben. Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| 401 | Fehler bei der Authentifizierung. Tritt beispielsweise auf, wenn die Anforderung den Parameter „client\_secret“ nicht enthält.|
| 403 | Fehler bei der Autorisierung. Der Benutzer verfügt beispielsweise nicht über die Berechtigung zum Zugriff auf die Ressource. |
| 500 | Ein interner Fehler ist beim Dienst aufgetreten. Wiederholen Sie die Anforderung. |

### JSON-Dokumentobjekte in der Fehlerantwort

| JSON-Objekt | Beschreibung |
|-------------|-------------|
| Fehler | Ein in Abschnitt 5.2 definierter Fehlercodewert des [OAuth 2.0-Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749). In der folgenden Tabelle finden Sie die Fehlercodes, die von Azure AD ausgegeben werden. |
| error\_description | Detailliertere Beschreibung des Fehlers. Diese Meldung ist nicht für den Endbenutzer ausgelegt. |
| timestamp | Das Datum und die Uhrzeit des Auftretens des Fehlers in koordinierter Weltzeit (UTC). |
| trace\_id | Eine ID, die die Fehlerablaufverfolgung identifiziert. |
| correlation\_id | 	Ein Wert, der vom Client generiert wird. Diese ID ist im JSON-Fehlerdokument enthalten, wenn die Anforderung an den Tokenausstellungs-Endpunkt diesen Wert im `client-request-id`-Header enthält. Diese ID kann von anderen Aufrufen in derselben Sitzung verwendet werden. |
| error\_codes | Enthält eine Liste der Fehlercodes, die verschiedenen Bedingungen des Diensts zugeordnet sind. Verwenden Sie diese Codes nicht in Ihrer Anwendungslogik. Sie können sie jedoch verwenden, um ein Problem zu diagnostizieren und anhand des Fehlercodes online nach Informationen zu suchen. |

### JSON-Dokument-Fehlercodewerte

| Fehlercode | Beschreibung | Clientaktion |
|------------|-------------|---------------|
| invalid\_request | Protokollfehler, z.B. ein fehlender erforderlicher Parameter. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| invalid\_grant | Die Autorisierungsgewährung (oder deren Parameter) oder das Aktualisierungstoken ist ungültig, abgelaufen oder gesperrt. | Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| unauthorized\_client | Der authentifizierte Client ist zur Verwendung dieses Autorisierungsgewährungstyps nicht autorisiert. | Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| invalid\_client | Clientauthentifizierung fehlgeschlagen. | Die Client-Anmeldeinformationen sind nicht gültig. Um das Problem zu beheben, aktualisiert der Anwendungsadministrator die Anmeldeinformationen. |
| unsupported\_grant\_type | Der Autorisierungsserver unterstützt den Autorisierungsgewährungstyp nicht. | Ändern Sie den Gewährungstyp in der Anforderung. Diese Art von Fehler sollte nur während der Entwicklung auftreten und bei den ersten Tests erkannt werden. |
| invalid\_resource | Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist. | Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| interaction\_required | Die Anforderung erfordert eine Benutzerinteraktion. Beispielsweise ist ein zusätzlicher Schritt zur Authentifizierung erforderlich. | Wiederholen Sie die Anforderung mit der gleichen Ressource. |
| temporarily\_unavailable | Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. | Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist.|

## Fehler von gesicherten Ressourcen

Dies sind die Fehler, die unter Umständen von einer gesicherten Ressource, z.B. einer Web-API, zurückgegeben werden, wenn die [RFC 6750](http://tools.ietf.org/html/rfc6750)-Spezifikation des OAuth 2.0-Autorisierungsframework implementiert wird.

Gesicherte Ressourcen, die RFC 6750 implementieren, geben HTTP-Statuscodes zurück. Wenn die Anforderung keine Authentifizierungsinformationen enthält oder wenn das Token fehlt, enthält die Antwort einen `WWW-Authenticate`-Header. Wenn eine Anforderung fehlschlägt, antwortet der Ressourcenserver mit einem HTTP-Statuscode und einem Fehlercode.

Im Folgenden finden Sie ein Beispiel für eine nicht erfolgreiche Antwort, wenn die Clientanforderung das Bearertoken nicht enthält:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

## Fehlerparameter

| Parameter | Beschreibung |
|-----------|-------------|
| authorization\_uri | Der URI (physische Endpunkt) des Autorisierungsservers. Dieser Wert wird auch als Suchschlüssel verwendet, um weitere Informationen über den Server aus einem Discovery-Endpunkt zu erhalten. <p><p> Der Client muss überprüfen, ob der Autorisierungsserver vertrauenswürdig ist. Wenn die Ressource von Azure AD geschützt wird, ist es ausreichend, zu prüfen, dass die URL mit https://login.windows.net beginnt oder einem anderen Hostnamen, den Azure AD unterstützt. Eine mandantenspezifische Ressource sollte immer einen mandantenspezifischen Autorisierungs-URI zurückgeben. |
| Fehler | Ein in Abschnitt 5.2 definierter Fehlercodewert des [OAuth 2.0-Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749).|
| error\_description | Detailliertere Beschreibung des Fehlers. Diese Meldung ist nicht für den Endbenutzer ausgelegt.|
| resource\_id | Gibt den eindeutigen Bezeichner der Ressource zurück. Die Clientanwendung kann diesen Bezeichner als Wert für den `resource`-Parameter verwenden, wenn sie ein Token für die Ressource anfordert. <p><p> Es ist sehr wichtig, dass die Clientanwendung diesen Wert überprüft, da andernfalls ein schädlicher Dienst möglicherweise einen Angriff mit einer **Erhöhung von Rechten** ausführt.<p><p> Die empfohlene Strategie, um Angriffen zu verhindern, besteht darin, zu überprüfen, ob die `resource_id` die Basis der Web-API-URL entspricht, auf die zugegriffen wird. Beispielsweise beim Zugriff auf https://service.contoso.com/data kann `resource_id` htttps://service.contoso.com/ sein. Die Clientanwendung muss eine `resource_id` ablehnen, die nicht mit der Basis-URL beginnt, sofern es kein zuverlässiges alternatives Verfahren zum Überprüfen der ID gibt. |

## Bearerschema-Fehlercodes

Die Spezifikation RFC 6750 definiert die folgenden Fehler für Ressourcen, die in der Antwort das WWW-Authenticate-Header und das Bearerschema verwenden.

| HTTP-Statuscode | Fehlercode | Beschreibung | Clientaktion |
|------------------|------------|-------------|---------------|
| 400 | invalid\_request | Die Anforderung ist nicht richtig formatiert. Möglicherweise fehlt ein Parameter, oder der gleiche Parameter wird zweimal verwendet. | Beheben Sie den Fehler, und wiederholen Sie die Anforderung. Diese Art von Fehler sollte nur während der Entwicklung auftreten und bei den ersten Tests erkannt werden. |
| 401 | invalid\_token | Das Zugriffstoken ist nicht vorhanden, ungültig oder wurde widerrufen. Der Wert des Parameters „error\_description“ enthält weitere Details. | Fordern Sie ein neues Token vom Autorisierungsserver an. Wenn das neue Token fehlschlägt, ist ein unerwarteter Fehler aufgetreten. Sendet eine Fehlermeldung an den Benutzer. Eine Wiederholung erfolgt nach einer beliebigen Zeitspanne. |
| 403 | insufficient\_scope | Das Zugriffstoken enthält nicht die erforderlichen Berechtigungen zum Identitätswechsel, um auf die Ressource zuzugreifen. | Senden Sie eine neue Autorisierungsanforderung an den Autorisierungsendpunkt. Wenn die Antwort den Bereichsparameter enthält, verwenden Sie den Bereichswert in der Anforderung für die Ressource. |
| 403 | insufficient\_access | Der Antragsteller des Tokens besitzt nicht die Berechtigungen, die für den Zugriff auf die Ressource erforderlich sind. | Der Benutzer wird aufgefordert, ein anderes Konto zu verwenden oder Berechtigungen für die angegebene Ressource anzufordern. |

<!---HONumber=AcomDC_0601_2016-->