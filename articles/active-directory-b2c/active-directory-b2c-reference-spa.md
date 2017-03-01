---
title: Azure Active Directory B2C | Microsoft Docs
description: So erstellen Sie einseitige Apps direkt mithilfe eines impliziten Flusses.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: a45cc74c-a37e-453f-b08b-af75855e0792
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: parakhj
translationtype: Human Translation
ms.sourcegitcommit: f2eb1f67c5687ac9bd8b94f6480617e6dc87ac9f
ms.openlocfilehash: ace35adc1cd2745ea11cb60429a1e7892d1f66d3


---
# <a name="azure-active-directory-b2c-sign-in-for-single-page-apps-using-oauth-20-implicit-flow"></a>Azure Active Directory B2C: Anmelden für Einzelseiten-Apps mithilfe des impliziten OAuth 2.0-Flusses

> [!NOTE]
> Dieses Feature befindet sich noch in der Vorschauphase.
> 

Viele moderne Apps besitzen ein Single-Page-App-Front-End, das in erster Linie in JavaScript geschrieben ist. Häufig wird dazu ein Framework verwendet, z.B. AngularJS, Ember.js oder Durandal.js. Bei einseitigen Apps und anderen JavaScript-Apps, die hauptsächlich im Browser ausgeführt werden, gibt es in Bezug auf die Authentifizierung einige interessante Herausforderungen:

* Die Sicherheitsmerkmale dieser Apps unterscheiden sich grundlegend von herkömmlichen serverbasierten Webanwendungen.
* Zahlreiche Autorisierungsserver und Identitätsanbieter unterstützen keine CORS-Anforderungen.
* Umleitungen auf ganzseitige Browserseiten stören die Benutzererfahrung erheblich.

Zum Unterstützen dieser Anwendungen verwendet Azure AD B2C den impliziten OAuth 2.0-Fluss.  Der implizite OAuth 2.0-Fluss zum Gewähren einer Autorisierung wird in [Abschnitt 4.2 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749) beschrieben.  Bei diesem Ablauf empfängt die App Token direkt vom Azure AD-Autorisierungsendpunkt, ohne dass eine Kommunikation zwischen Servern stattfindet. Die gesamte Authentifizierungslogik und Sitzungsverarbeitung erfolgt ohne zusätzliche Seitenumleitungen vollständig im JavaScript-Client.

Azure AD B2C erweitert den impliziten OAuth 2.0-Standardfluss, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Dazu werden [**Richtlinienparameter**](active-directory-b2c-reference-policies.md) eingeführt, mit denen Sie OAuth 2.0 zum Hinzufügen von Benutzeroberflächen für die Registrierung, Anmeldung und Profilverwaltung zu Ihrer App verwenden können. Hier zeigen wir Ihnen, wie Sie den impliziten Fluss und Azure AD verwenden, um diese verschiedenen Oberflächen in Ihren einseitigen Anwendungen zu implementieren.  Nützliche Hinweise für den Einstieg finden Sie auch in unseren Beispielen für [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) und [.NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi).

Die HTTP-Beispielanforderungen unten verwenden unser B2C-Beispielverzeichnis **fabrikamb2c.onmicrosoft.com** sowie unsere Beispielanwendung und -richtlinien. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene B2C-Verzeichnisse, -Anwendungen und -Richtlinien erstellen](#use-your-own-b2c-tenant).

## <a name="protocol-diagram"></a>Protokolldiagramm
Der vollständige implizite Anmeldevorgang sieht in etwa wie folgt aus. Die einzelnen Schritte werden unten im Detail beschrieben.

![OpenId Connect-Verantwortlichkeitsbereiche](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Übermitteln von Authentifizierungsanforderungen
Wenn die Web-App Benutzer authentifizieren und eine Richtlinie ausführen muss, leitet sie die Benutzer an den `/authorize`-Endpunkt weiter. Dies ist der interaktive Teil des Flusses, bei dem der Benutzer je nach Richtlinie tatsächlich Aktionen durchführt und ein `id_token` vom Azure AD-Endpunkt erhält.

In dieser Anforderung gibt der Client die Berechtigungen, die er vom Benutzer benötigt, im `scope`-Parameter und die anzuwendende Richtlinie im `p`-Parameter an. Unten sind drei Beispiele aufgeführt (mit Zeilenumbrüchen für bessere Lesbarkeit), die jeweils eine andere Richtlinie verwenden. Um ein Gefühl für die Funktionsweise der einzelnen Anforderung zu erhalten, fügen Sie sie in einem Browser ein und führen sie aus.

#### <a name="use-a-sign-in-policy"></a>Verwenden einer Anmelderichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Verwenden einer Registrierungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Verwenden einer Profilbearbeitungsrichtlinie
```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| response_type |erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten.  Kann auch den Antworttyp `token` enthalten. Mithilfe von `token` kann Ihre App ein Zugriffstoken direkt vom Autorisierungsendpunkt abrufen, ohne dass eine zweite Anforderung an den Autorisierungsendpunkt erforderlich ist.  Wenn Sie den Antworttyp `token` verwenden, muss der `scope`-Parameter einen Bereich enthalten, der angibt, für welche Ressource das Token ausgestellt wird. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| response_mode |empfohlen |Gibt die Methode an, die zum Senden des resultierenden Tokens zurück an Ihre App verwendet werden soll.  Muss für den impliziten Fluss `fragment` sein. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für die Anmeldung des Benutzers und das Abrufen von Daten über den Benutzer in Form von **ID-Token** an (dies wird weiter unten in diesem Artikel erläutert). Der `offline_access` -Bereich ist für Web-Apps optional. Er gibt an, dass Ihre App ein **Aktualisierungstoken** für den dauerhaften Zugriff auf Ressourcen benötigt. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern. Der Status wird auch zum Codieren von Informationen über den Status des Benutzers in der App vor der Authentifizierungsanforderung verwendet, z. B. für Informationen zu der Seite, die der Benutzer besucht hat. |
| nonce |Erforderlich |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten ist. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| p |Erforderlich |Die Richtlinie, die ausgeführt wird. Dies ist der Name einer Richtlinie, die in Ihrem B2C-Mandanten erstellt wird. Der Wert für den Richtliniennamen muss mit „b2c\_1\_“ beginnen. Weitere Informationen zu Richtlinien finden Sie unter [Erweiterbares Richtlinienframework](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Zu diesem Zeitpunkt ist der einzige gültige Wert 'Anmeldung', der den Benutzer zur Eingabe von Anmeldeinformationen für diese Anforderung zwingt. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie abzuschließen. Dafür muss der Benutzer z. B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen – je nachdem, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD mithilfe der im `response_mode`-Parameter angegebenen Methode eine Antwort über den angegebenen `redirect_uri` an Ihre App zurück. Die Antwort ist in den oben aufgeführten Fällen, unabhängig von der ausgeführten Richtlinie, immer gleich.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mithilfe von `response_mode=fragment` und `response_type=id_token+token` sieht wie folgt aus, wobei die Zeilenumbrüche der Lesbarkeit dienen:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das von der App angeforderte Zugriffstoken.  Das Zugriffstoken sollte nicht decodiert oder anderweitig untersucht werden, es kann als nicht transparente Zeichenfolge behandelt werden. |
| token_type |Der Wert des Tokentyps. Bearertoken ist der einzige Typ, den Azure AD unterstützt. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| Bereich |Die Bereiche, für die das Token gilt und die zum Zwischenspeichern von Token zur späteren Verwendung verwendet werden können. |
| id_token |Das ID-Token, das die Anwendung angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

#### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| Fehler |Eine Zeichenfolge mit einem Fehlercode, die zum Klassifizieren der möglicherweise auftretenden Fehlertypen verwendet wird. Der Fehlercode kann zur Fehlerbehandlung verwendet werden. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| state |Die vollständige Beschreibung finden Sie in der vorherigen Tabelle. Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind.|

## <a name="validate-the-idtoken"></a>Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens allein reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur des ID-Tokens validieren und die Ansprüche im Token gemäß den App-Anforderungen überprüfen. Azure AD B2C verwendet [JSON-Webtoken (JWT)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Viele Open Source-Bibliotheken sind für die Überprüfung von JWTs für unterschiedliche Sprachen verfügbar. Erwägen Sie die Verwendung dieser Optionen, anstatt eine eigene Überprüfungslogik zu implementieren. Diese Informationen sind hilfreich, um die ordnungsgemäße Verwendung dieser Bibliotheken zu erlernen.

Azure AD B2C verfügt über einen OpenID Connect-Metadatenendpunkt, mit dem die App zur Laufzeit Informationen über Azure AD B2C abrufen kann. Diese Informationen umfassen Endpunkte, Tokeninhalte und Token-Signaturschlüssel. Es gibt ein JSON-Metadatendokument für jede Richtlinie in Ihrem B2C-Mandanten. Das Metadatendokument für die `b2c_1_sign_in`-Richtlinie in `fabrikamb2c.onmicrosoft.com` befindet sich z.B. unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Eine der Eigenschaften dieses Konfigurationsdokuments ist der `jwks_uri`, dessen Wert für diese Richtlinie wie folgt lautet:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Es gibt zwei Möglichkeiten, zu ermitteln, welche Richtlinie zum Signieren eines ID-Tokens verwendet wurde (und wo die Metadaten abgerufen werden können). Zunächst einmal ist der Richtliniennamen im `acr`-Anspruch im ID-Token enthalten. Informationen zum Analysieren von Ansprüchen nach einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit besteht darin, die Richtlinie beim Übermitteln der Anforderung im Wert des Parameters `state` zu verschlüsseln und später zu entschlüsseln, um die verwendete Richtlinie zu bestimmen. Beide Methoden sind gleichwertig.

Nachdem Sie das Metadatendokument aus dem OpenID Connect-Metadatenendpunkt erhalten haben, können Sie die öffentlichen RSA 256-Schlüssel (die sich an diesem Endpunkt befinden) zum Überprüfen der Signatur des ID-Tokens verwenden. Es gibt möglicherweise zu einem bestimmten Zeitpunkt mehrere Schlüssel an diesem Endpunkt. Sie werden jeweils durch eine `kid` identifiziert. Der Header des ID-Tokens enthält außerdem einen `kid`-Anspruch, der anzeigt, welcher Schlüssel zum Signieren des ID-Tokens verwendet wurde. Weitere Informationen finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md). Dort finden Sie auch Einzelheiten zum [Überprüfen von Token](active-directory-b2c-reference-tokens.md#token-validation).
<!--TODO: Improve the information on this-->

Nach der Überprüfung der Signatur des ID-Tokens erfordern auch einige Ansprüche eine Überprüfung, wie z.B. folgende:

* Überprüfen Sie den `nonce`-Anspruch, um Tokenwiederholungsangriffe zu verhindern. Dessen Wert sollte dem in der Anmeldeanforderung angegebenen Wert entsprechen.
* Überprüfen Sie den `aud`-Anspruch, um sicherzustellen, dass das ID-Token für Ihre App ausgestellt wurde. Der Wert sollte der Anwendungs-ID der App entsprechen.
* Überprüfen Sie die Ansprüche `iat` und `exp`, um sicherzustellen, dass das ID-Token nicht abgelaufen ist.

Es gibt auch noch einige andere Überprüfungen, die Sie durchführen sollten. Diese sind in der [OpenID Connect Core-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html) ausführlich beschrieben.  Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer/die Organisation für die App registriert hat.
* Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und die richtigen Berechtigungen verfügt.
* Sicherstellen, dass eine bestimmte Authentifizierungsmethode verwendet wird, z. B. Azure Multi-Factor Authentication.

Weitere Informationen zu Ansprüchen in einem ID-Token finden Sie in der [Azure AD B2C-Tokenreferenz](active-directory-b2c-reference-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie mit dem Benutzer eine Sitzung beginnen und die Ansprüche im ID-Token zum Abrufen von Informationen über den Benutzer in der App verwenden. Diese Informationen können für die Anzeige, für Aufzeichnungen, für die Autorisierung usw. verwendet werden.

## <a name="get-access-tokens"></a>Abrufen von Zugriffstoken
Wenn Ihre Web-App lediglich Richtlinien ausführen muss, können Sie die nächsten Abschnitte überspringen. Diese Abschnitte gelten nur für Web-Apps, die authentifizierte Aufrufe an eine Web-API durchführen müssen und die auch von Azure AD B2C geschützt werden.

Nachdem Sie den Benutzer bei der einseitigen App angemeldet haben, können Sie Zugriffstoken zum Aufrufen der durch Azure AD gesicherten Web-APIs abrufen.  Auch wenn Sie mithilfe des Antworttyps `token` bereits ein Token erhalten haben, können Sie diese Methode zum Abrufen von Token für zusätzliche Ressourcen verwenden, ohne den Benutzer zur erneuten Anmeldung umzuleiten.

Im normalen Web-App-Fluss senden Sie dazu eine Anforderung an den `/token`-Endpunkt.  Der Endpunkt unterstützt jedoch keine CORS-Anforderungen, daher kommen AJAX-Aufrufe zum Abrufen und Aktualisieren von Token nicht infrage.  Stattdessen können Sie den impliziten Fluss in einem ausgeblendeten IFrame verwenden, um neue Token für andere Web-APIs zu erhalten:

```
// Line breaks for legibility only

https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
&p=b2c_1_sign_in
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Azure-Portal](https://portal.azure.com/) Ihrer App zugewiesen hat. |
| response_type |erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten.  Kann auch den Antworttyp `token` enthalten. Mithilfe von `token` kann Ihre App ein Zugriffstoken direkt vom Autorisierungsendpunkt abrufen, ohne dass eine zweite Anforderung an den Autorisierungsendpunkt erforderlich ist.  Wenn Sie den Antworttyp `token` verwenden, muss der `scope`-Parameter einen Bereich enthalten, der angibt, für welche Ressource das Token ausgestellt wird. |
| redirect_uri |empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können.  Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen.  Beziehen Sie zum Abrufen von Token alle Bereiche ein, die für die entsprechende Ressource erforderlich sind. |
| response_mode |empfohlen |Gibt die Methode an, die zum Zurücksenden des resultierenden Tokens an Ihre App verwendet wird.  Kann `query`, `form_post` oder `fragment` sein. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der in der Tokenantwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um websiteübergreifende Anforderungsfälschungsangriffe zu verhindern.  Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| nonce |erforderlich |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Token als Anspruch enthalten ist.  Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern.  Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die den Ursprung der Anforderung identifiziert. |
| Eingabeaufforderung |erforderlich |Verwenden Sie `prompt=none` zum Aktualisieren und Abrufen von Token in einem ausgeblendeten IFrame, um sicherzustellen, dass das IFrame auf der Anmeldeseite nicht hängenbleibt, sondern direkt zurückgegeben wird. |
| login_hint |erforderlich |Beziehen Sie zum Aktualisieren und Abrufen von Token in einem ausgeblendete IFrame den Benutzernamen des Benutzers in diesem Hinweis mit ein, damit zwischen verschiedenen Sitzungen, die der Benutzer möglicherweise ausführt, unterschieden werden kann. Sie können den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahieren. |
| domain_hint |erforderlich |Kann `consumers` oder `organizations` sein.  Schließen Sie zum Aktualisieren und Abrufen von Token in einem ausgeblendete IFrame „domain_hint“ in die Anforderung mit ein.  Extrahieren Sie den Anspruch `tid` aus dem ID-Token einer früheren Anmeldung, um zu bestimmen, welcher Wert verwendet werden soll.  Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat.  Verwenden Sie andernfalls `domain_hint=organizations`. |

Durch Festlegen des Parameters `prompt=none` ist diese Anforderung entweder erfolgreich, oder sie führt direkt zu einem Fehler und kehrt zu Ihrer Anwendung zurück.  Eine erfolgreiche Antwort wird an Ihre App an den angegebenen Umleitungs-URI (`redirect_uri`) gesendet. Dabei wird die im Parameter `response_mode` angegebene Methode verwendet.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=fragment` sieht wie folgt aus:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| access_token |Das von der App angeforderte Token |
| token_type |Ist immer `Bearer`. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Bereich |Die Bereiche, für die das Zugriffstoken gültig ist. |

#### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:  Im Fall von `prompt=none` wird folgender Fehler erwartet:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Wenn Sie diesen Fehler in der IFrame-Anforderung erhalten, muss sich der Benutzer erneut anmelden, um ein neues Token abzurufen.  Diesen Fall können Sie so behandeln, wie es für Ihre Anwendung am sinnvollsten ist.

## <a name="refreshing-tokens"></a>Aktualisieren von Token
Sowohl `id_token` als auch `access_token` laufen nach kurzer Zeit ab. Ihre App muss diese Token daher in regelmäßigen Abständen aktualisieren.  Zum Aktualisieren beider Tokentypen führen Sie die oben erwähnte verborgene IFrame-Anforderung unter Verwendung des Parameters `prompt=none` aus, um das Verhalten von Azure AD zu steuern.  Um ein neues `id_token` zu erhalten, verwenden Sie unbedingt `response_type=id_token` und `scope=openid` sowie einen `nonce`-Parameter.

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldeanforderung
Wenn Sie einen Benutzer von der App abmelden möchten, leiten Sie den Benutzer zum Abmelden an Azure AD weiter. Wenn Sie dies versäumen, kann sich der Benutzer möglicherweise erneut bei Ihrer App authentifizieren, ohne die Anmeldeinformationen erneut eingeben zu müssen. Die Ursache ist, das er über eine gültige SSO-Sitzung bei Azure AD verfügt.

Sie können den Benutzer einfach an den `end_session_endpoint` umleiten, der im gleichen OpenID Connect-Metadatendokument aufgeführt wird, das weiter oben im Abschnitt „Überprüfen des ID-Tokens“ beschrieben wird:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abmelden des Benutzers von der Anwendung verwenden werden soll. |
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

## <a name="samples"></a>Beispiele

* [Erstellen einer einseitigen App mithilfe von Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Erstellen einer einseitigen App mithilfe von .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)




<!--HONumber=Feb17_HO2-->


