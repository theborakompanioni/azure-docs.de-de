---
title: 'Azure Active Directory B2C: Einseitige Apps mithilfe des impliziten Flusses | Microsoft-Dokumentation'
description: "In diesem Artikel wird erläutert, wie einseitige Apps direkt mithilfe des impliziten OAuth 2.0-Flusses mit Azure Active Directory B2C erstellt werden."
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: 44ff168599e9078506e1afdd0f1dc4657ef0964d
ms.contentlocale: de-de
ms.lasthandoff: 06/24/2017


---
# <a name="azure-ad-b2c-single-page-app-sign-in-by-using-oauth-20-implicit-flow"></a>Azure AD B2C: Anmeldung über einseitige Apps mithilfe des impliziten OAuth 2.0-Flusses

> [!NOTE]
> Dieses Feature befindet sich in der Vorschauphase.
> 

Viele moderne Apps besitzen ein Single-Page-App-Front-End, das in erster Linie in JavaScript geschrieben ist. Häufig wird die App mithilfe eines Frameworks wie AngularJS, Ember.js oder Durandal geschrieben. Bei einseitigen Apps und anderen JavaScript-Apps, die hauptsächlich in einem Browser ausgeführt werden, gibt es in Bezug auf die Authentifizierung einige zusätzliche Herausforderungen:

* Die Sicherheitsmerkmale dieser Apps unterscheiden sich grundlegend von herkömmlichen serverbasierten Webanwendungen.
* Zahlreiche Autorisierungsserver und Identitätsanbieter unterstützen keine CORS-Anforderungen (CORS = Cross-Origin Resource Sharing).
* Umleitungen auf ganzseitige Browserseiten können die Benutzererfahrung erheblich stören.

Zum Unterstützen dieser Anwendungen verwendet Azure Active Directory B2C (Azure AD B2C) den impliziten OAuth 2.0-Fluss. Der implizite OAuth 2.0-Fluss zum Gewähren einer Autorisierung wird in [Abschnitt 4.2 der OAuth 2.0-Spezifikation](http://tools.ietf.org/html/rfc6749) beschrieben. Beim impliziten Ablauf empfängt die App Token direkt vom Azure AD-Autorisierungsendpunkt (Azure Active Directory), ohne dass eine Kommunikation zwischen Servern stattfindet. Die gesamte Authentifizierungslogik und Sitzungsverarbeitung erfolgt ohne zusätzliche Seitenumleitungen vollständig im JavaScript-Client.

Azure AD B2C erweitert den impliziten OAuth 2.0-Standardfluss, sodass mehr als nur eine einfache Authentifizierung und Autorisierung erfolgt. Azure AD B2C führt den [Richtlinienparameter](active-directory-b2c-reference-policies.md) ein. Mit dem Richtlinienparameter können Sie OAuth 2.0 zum Hinzufügen von Benutzeroberflächen für die Registrierung, Anmeldung und Profilverwaltung zu Ihrer App verwenden. In diesem Artikel wird Ihnen gezeigt, wie Sie den impliziten Fluss und Azure AD für die Implementierung dieser verschiedenen Oberflächen in Ihren einseitigen Anwendungen verwenden. Nützliche Hinweise für den Einstieg finden Sie auch in unseren Beispielen für [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi) und [Microsoft .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi).

In den HTTP-Beispielanforderungen in diesem Artikel wird das Azure AD B2C-Beispielverzeichnis **fabrikamb2c.onmicrosoft.com** verwendet. Zudem werden eine eigene Beispielanwendung und -richtlinien verwendet. Sie können selbst Anforderungen mit diesen Werten testen oder eigene Werte verwenden.
Erfahren Sie, wie Sie [eigene Azure AD B2C-Verzeichnisse, -Anwendungen und -Richtlinien abrufen](#use-your-own-b2c-tenant).


## <a name="protocol-diagram"></a>Protokolldiagramm

Die implizite Anmeldevorgang sieht in etwa wie die folgende Abbildung aus. Die einzelnen Schritte werden später im Artikel detailliert beschrieben.

![OpenID Connect-Verantwortlichkeitsbereiche](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Übermitteln von Authentifizierungsanforderungen
Wenn die Web-App Benutzer authentifizieren und eine Richtlinie ausführen muss, leitet sie die Benutzer an den `/authorize`-Endpunkt weiter. Dies ist der interaktive Teil des Datenflusses, bei dem der Benutzer je nach Richtlinie Aktionen durchführt. Der Benutzer ruft ein ID-Token vom Azure AD-Endpunkt ab.

In dieser Anforderung gibt der Client im Parameter `scope` die Berechtigungen an, die er vom Benutzer abrufen muss. Er gibt im Parameter `p` die auszuführende Richtlinie an. In den folgenden drei Beispielen (mit Zeilenumbrüchen für bessere Lesbarkeit) wird jeweils eine andere Richtlinie verwendet. Um ein Gefühl für die Funktionsweise der einzelnen Anforderung zu erhalten, fügen Sie sie in einem Browser ein und führen sie aus.

### <a name="use-a-sign-in-policy"></a>Verwenden einer Anmelderichtlinie
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

### <a name="use-a-sign-up-policy"></a>Verwenden einer Registrierungsrichtlinie
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

### <a name="use-an-edit-profile-policy"></a>Verwenden einer Profilbearbeitungsrichtlinie
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

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die Ihrer App im [Azure-Portal](https://portal.azure.com) zugewiesen wird. |
| response_type |Erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten. Kann auch den Antworttyp `token` enthalten. Mithilfe von `token` kann Ihre App ein Zugriffstoken direkt vom Autorisierungsendpunkt abrufen, ohne dass eine zweite Anforderung an den Autorisierungsendpunkt erforderlich ist.  Wenn Sie den Antworttyp `token` verwenden, muss der Parameter `scope` einen Bereich enthalten, in dem angegeben wird, für welche Ressource das Token ausgegeben wird. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, den Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| response_mode |Empfohlen |Gibt die Methode an, die zum Zurücksenden des resultierenden Tokens an Ihre App verwendet wird.  Verwenden Sie `fragment` für implizite Flüsse. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Ein einzelner Bereichswert gibt Azure AD an, dass beide Berechtigungen angefordert werden. Der `openid`-Bereich gibt eine Berechtigung für das Anmelden des Benutzers und das Abrufen von Daten über den Benutzer in Form von ID-Token an. (Dies wird später im Artikel erläutert.) Der `offline_access` -Bereich ist für Web-Apps optional. Er gibt an, dass Ihre App ein Aktualisierungstoken für den dauerhaften Zugriff auf Ressourcen benötigt. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der ebenfalls in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen zu verwendenden Inhalt handeln. Normalerweise wird ein zufällig generierter eindeutiger Wert verwendet, um eine websiteübergreifende Anforderungsfälschung zu verhindern. Der Status wird auch zum Codieren von Informationen über den Status des Benutzers in der App vor der Authentifizierungsanforderung verwendet, z.B. für Informationen zu der Seite, die der Benutzer besucht hat. |
| nonce |Erforderlich |Ein (von der App generierter) Wert in der Anforderung, der im resultierenden ID-Token als Anspruch enthalten ist. Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| p |Erforderlich |Die auszuführende Richtlinie. Dies ist der Name einer Richtlinie, die in Ihrem Azure AD B2C-Mandanten erstellt wird. Der Wert für den Richtliniennamen muss mit **b2c\_1\_** beginnen. Weitere Informationen finden Sie unter [Integrierte Azure AD B2C-Richtlinien](active-directory-b2c-reference-policies.md). |
| Eingabeaufforderung |Optional |Der Typ der erforderlichen Benutzerinteraktion. Derzeit ist `login` der einzige gültige Wert. Dadurch wird der Benutzer dazu gezwungen, die Anmeldeinformationen bei dieser Anforderung einzugeben. Einmaliges Anmelden wird nicht verwendet. |

An diesem Punkt wird der Benutzer aufgefordert, den Workflow der Richtlinie abzuschließen. Dafür muss der Benutzer z.B. den Benutzernamen und das Kennwort angeben, sich mit der Identität eines sozialen Netzwerks anmelden, sich für das Verzeichnis registrieren oder andere Schritte ausführen. Benutzeraktionen hängen davon ab, wie die Richtlinie definiert ist.

Nachdem der Benutzer die Richtlinie abgeschlossen hat, gibt Azure AD in Ihrer App unter dem für `redirect_uri` verwendeten Wert eine Antwort zurück. Hierzu wird die im Parameter `response_mode` angegebene Methode verwendet. Die Antwort ist in den oben aufgeführten Benutzeraktionsszenarios, unabhängig von der ausgeführten Richtlinie, immer gleich.

### <a name="successful-response"></a>Erfolgreiche Antwort
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
| access_token |Das von der App angeforderte Zugriffstoken.  Das Zugriffstoken sollte nicht decodiert oder anderweitig untersucht werden. Es kann als nicht transparente Zeichenfolge behandelt werden. |
| token_type |Der Wert des Tokentyps. Bearertoken ist der einzige Typ, den Azure AD unterstützt. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| Bereich |Die Bereiche, für die das Token gültig ist. Sie können in den Bereichen auch Token für die spätere Verwendung zwischenspeichern. |
| id_token |Das ID-Token, das die App angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie im [Azure AD B2C-Tokenverweis](active-directory-b2c-reference-tokens.md). |
| state |Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den Umleitungs-URI gesendet werden, damit die App diese angemessen behandeln kann:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Zeichenfolge mit einem Fehlercode, die zum Klassifizieren der möglicherweise auftretenden Fehlertypen verwendet wird. Sie können auch den Fehlercode für die Fehlerbehandlung verwenden. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| state |Die vollständige Beschreibung finden Sie in der obigen Tabelle. Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind.|

## <a name="validate-the-id-token"></a>Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur des ID-Tokens überprüfen und die Ansprüche im Token gemäß den Anforderungen Ihrer App überprüfen. Azure AD B2C verwendet [JSON-Webtoken (JWT)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Für die Überprüfung von JWTs sind je nach gewünschter Sprache viele Open Source-Bibliotheken verfügbar. Erwägen Sie die Verwendung verfügbarer Open-Source-Bibliotheken, anstatt eine eigene Überprüfungslogik zu implementieren. Die Informationen in diesem Handbuch sind hilfreich für die ordnungsgemäße Verwendung dieser Bibliotheken.

Azure AD B2C verfügt über einen OpenID Connect-Metadatenendpunkt. Eine App kann mit diesem Endpunkt Informationen über Azure AD B2C zur Laufzeit abrufen. Diese Informationen umfassen Endpunkte, Tokeninhalte und Token-Signaturschlüssel. Es gibt ein JSON-Metadatendokument für jede Richtlinie in Ihrem Azure AD B2C-Mandanten. Das Metadatendokument für die Richtlinie b2c_1_sign_in im Mandanten fabrikamb2c.onmicrosoft.com befindet sich beispielsweise unter:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

`jwks_uri` ist eine der Eigenschaften dieses Konfigurationsdokuments. Der Wert für die gleiche Richtlinie würde wie folgt lauten:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Sie haben zwei Optionen zum Ermitteln, welche Richtlinie zum Signieren eines ID-Tokens verwendet wurde (und wo die Metadaten abgerufen werden können). Zunächst einmal ist der Richtlinienname im `acr`-Anspruch in `id_token` enthalten. Informationen zum Analysieren von Ansprüchen nach einem ID-Token finden Sie im [Azure AD B2C-Tokenverweis](active-directory-b2c-reference-tokens.md). Eine andere Möglichkeit besteht darin, die Richtlinie beim Ausgeben der Anforderung im Wert des Parameters `state` zu verschlüsseln. Anschließend kann der Parameter `state` entschlüsselt werden, um zu bestimmen, welche Richtlinie verwendet wurde. Beide Methoden sind gültig.

Nachdem Sie das Metadatendokument aus dem OpenID Connect-Metadatenendpunkt erhalten haben, können Sie die öffentlichen RSA-256-Schlüssel (die sich an diesem Endpunkt befinden) zum Überprüfen der Signatur des ID-Tokens verwenden. Es gibt möglicherweise zu einem bestimmten Zeitpunkt mehrere Schlüssel an diesem Endpunkt. Sie werden jeweils durch eine `kid` identifiziert. Der Header der `id_token` enthält auch einen `kid`-Anspruch. Er gibt an, mit welchem dieser Schlüssel das ID-Token signiert wurde. Weitere Informationen finden Sie im [Azure AD B2C-Tokenverweis](active-directory-b2c-reference-tokens.md#token-validation). Dort finden Sie auch Einzelheiten zum [Überprüfen von Token](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Nach der Überprüfung der Signatur des ID-Tokens müssen auch einige Ansprüche überprüft werden. Beispiel:

* Überprüfen Sie den `nonce`-Anspruch, um Tokenwiederholungsangriffe zu verhindern. Dessen Wert sollte dem in der Anmeldeanforderung angegebenen Wert entsprechen.
* Überprüfen Sie den `aud`-Anspruch, um sicherzustellen, dass das ID-Token für Ihre App ausgegeben wurde. Der Wert sollte der Anwendungs-ID der App entsprechen.
* Überprüfen Sie die Ansprüche `iat` und `exp`, um sicherzustellen, dass das ID-Token nicht abgelaufen ist.

Es gibt auch noch einige andere Überprüfungen, die Sie durchführen sollten. Diese sind in der [OpenID Connect Core-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html) ausführlich beschrieben. Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer/die Organisation für die App registriert hat.
* Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und die richtigen Berechtigungen verfügt.
* Sicherstellen, dass eine bestimmte Authentifizierungsmethode verwendet wird, z.B. Multi-Factor Authentication von Azure.

Weitere Informationen zu den Ansprüchen in einem ID-Token finden Sie im [Azure AD B2C-Tokenverweis](active-directory-b2c-reference-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie eine Sitzung mit dem Benutzer beginnen. Verwenden Sie in Ihrer App die Ansprüche im ID-Token, um Informationen über den Benutzer abzurufen. Diese Informationen können für die Anzeige, für Aufzeichnungen, für die Autorisierung usw. verwendet werden.

## <a name="get-access-tokens"></a>Abrufen von Zugriffstoken
Wenn Ihre Web-Apps lediglich Richtlinien ausführen müssen, können Sie die nächsten Abschnitte überspringen. Die Informationen in diesen Abschnitten gelten nur für Web-Apps, die authentifizierte Aufrufe an eine Web-API durchführen müssen und die auch von Azure AD B2C geschützt werden.

Nachdem Sie den Benutzer bei der einseitigen App angemeldet haben, können Sie Zugriffstoken zum Aufrufen der durch Azure AD gesicherten Web-APIs abrufen. Auch wenn Sie mithilfe des Antworttyps `token` bereits ein Token erhalten haben, können Sie diese Methode zum Abrufen von Token für zusätzliche Ressourcen verwenden, ohne den Benutzer zur erneuten Anmeldung umzuleiten.

In einem typischen Web-App-Fluss senden Sie dazu eine Anforderung an den `/token`-Endpunkt.  Der Endpunkt unterstützt jedoch keine CORS-Anforderungen, daher kommen AJAX-Aufrufe zum Abrufen und Aktualisieren von Token nicht infrage. Stattdessen können Sie den impliziten Fluss in einem ausgeblendeten HTML-IFrame-Element verwenden, um neue Token für andere Web-APIs zu erhalten. Hier sehen Sie ein Beispiel, mit Zeilenumbrüchen für bessere Lesbarkeit:

```

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

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| client_id |Erforderlich |Die Anwendungs-ID, die Ihrer App im [Azure-Portal](https://portal.azure.com) zugewiesen wird. |
| response_type |Erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten.  Kann auch den Antworttyp `token` enthalten. Mithilfe von `token` kann Ihre App ein Zugriffstoken direkt vom Autorisierungsendpunkt abrufen, ohne dass eine zweite Anforderung an den Autorisierungsendpunkt erforderlich ist. Wenn Sie den Antworttyp `token` verwenden, muss der Parameter `scope` einen Bereich enthalten, in dem angegeben wird, für welche Ressource das Token ausgegeben wird. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, den Sie im Portal registriert haben – mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen.  Beziehen Sie zum Abrufen von Token alle Bereiche ein, die für die gewünschte Ressource erforderlich sind. |
| response_mode |Empfohlen |Gibt die Methode an, die zum Zurücksenden des resultierenden Tokens an Ihre App verwendet wird.  Kann `query`, `form_post` oder `fragment` sein. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der in der Tokenantwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen zu verwendenden Inhalt handeln.  Normalerweise wird ein zufällig generierter eindeutiger Wert verwendet, um eine websiteübergreifende Anforderungsfälschung zu verhindern.  Der Status wird außerdem verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist. Beispiel: Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| nonce |Erforderlich |Ein Wert in der Anforderung, der von der App generiert wird und im resultierenden ID-Token als Anspruch enthalten ist.  Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die den Ursprung der Anforderung identifiziert. |
| Eingabeaufforderung |Erforderlich |Verwenden Sie `prompt=none` zum Aktualisieren und Abrufen von Token in einem ausgeblendeten IFrame, um sicherzustellen, dass das IFrame auf der Anmeldeseite nicht hängenbleibt, sondern direkt zurückgegeben wird. |
| login_hint |Erforderlich |Beziehen Sie zum Aktualisieren und Abrufen von Token in einem ausgeblendete IFrame den Benutzernamen des Benutzers in diesem Hinweis mit ein, damit zwischen verschiedenen Sitzungen unterschieden werden kann, die der Benutzer möglicherweise ausführt. Sie können den Benutzernamen aus einer früheren Anmeldung mithilfe des Anspruchs `preferred_username` extrahieren. |
| domain_hint |Erforderlich |Kann `consumers` oder `organizations` sein.  Schließen Sie zum Aktualisieren und Abrufen von Token in einem ausgeblendete IFrame den Wert `domain_hint` in die Anforderung mit ein.  Extrahieren Sie den Anspruch `tid` aus dem ID-Token einer früheren Anmeldung, um zu bestimmen, welcher Wert verwendet werden soll.  Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat.  Verwenden Sie andernfalls `domain_hint=organizations`. |

Durch Festlegen des Parameters `prompt=none` ist diese Anforderung entweder erfolgreich, oder sie führt direkt zu einem Fehler und kehrt zu Ihrer Anwendung zurück.  Eine erfolgreiche Antwort wird an Ihre App an den angegebenen Umleitungs-URI gesendet. Dabei wird die im Parameter `response_mode` angegebene Methode verwendet.

### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=fragment` sieht wie folgt aus:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das von der App angeforderte Token |
| token_type |Der Tokentyp wird immer Träger sein. |
| state |Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die `state`-Werte in der Anforderung und in der Antwort identisch sind. |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| Bereich |Die Bereiche, für die das Zugriffstoken gültig ist. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den Umleitungs-URI gesendet werden, damit die App diese angemessen behandeln kann.  Bei `prompt=none` sieht ein erwarteter Fehler wie folgt aus:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Zeichenfolge mit einem Fehlercode, die zum Klassifizieren der auftretenden Fehlertypen verwendet werden kann. Sie können mit der Zeichenfolge auch auf Fehler reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Wenn Sie diesen Fehler in der IFrame-Anforderung erhalten, muss sich der Benutzer erneut anmelden, um ein neues Token abzurufen. Sie können damit auf eine Weise umgehen, die für Ihre Anwendung sinnvoll ist.

## <a name="refresh-tokens"></a>Aktualisierungstoken
ID-Token und Zugriffstoken laufen nach einem kurzen Zeitraum ab. Ihre App muss darauf vorbereitet sein, diese Token in regelmäßigen Abständen zu aktualisieren.  Führen Sie zum Aktualisieren beider Tokentypen die oben erwähnte verborgene IFrame-Anforderung unter Verwendung des Parameters `prompt=none` aus, um die Schritte von Azure AD zu steuern.  Verwenden Sie `response_type=id_token` und `scope=openid` sowie einen `nonce`-Parameter, um einen neuen `id_token`-Wert zu erhalten.

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldeanforderung
Wenn Sie einen Benutzer von der App abmelden möchten, leiten Sie den Benutzer zum Abmelden an Azure AD weiter. Wenn Sie dies versäumen, kann sich der Benutzer möglicherweise wieder bei Ihrer App authentifizieren, ohne die Anmeldeinformationen erneut eingeben zu müssen. Die Ursache ist, das er über eine gültige SSO-Sitzung bei Azure AD verfügt.

Sie können den Benutzer einfach an den `end_session_endpoint` umleiten, der im gleichen OpenID Connect-Metadatendokument aufgeführt wird, das weiter oben im Abschnitt [Überprüfen des ID-Tokens](#validate-the-id-token) beschrieben wird. Beispiel:

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Erforderlich | Beschreibung |
| --- | --- | --- |
| p |Erforderlich |Die Richtlinie, die zum Abmelden des Benutzers von der Anwendung verwenden werden soll. |
| post_logout_redirect_uri |Empfohlen |Die URL, an die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll. Wenn sie nicht angegeben ist, zeigt Azure AD B2C dem Benutzer eine generische Meldung an. |

> [!NOTE]
> Durch die Weiterleitung des Benutzers zu `end_session_endpoint` wird der SSO-Status des Benutzers in Azure AD B2C teilweise aufgehoben. Allerdings wird der Benutzer nicht von der Sitzung des sozialen Netzwerks als Identitätsanbieter abgemeldet. Wenn der Benutzer bei einer nachfolgenden Anmeldung denselben Identitätsanbieter auswählt, wird der Benutzer ohne erneute Eingabe seiner Anmeldeinformationen wieder authentifiziert. Wenn ein Benutzer sich von der Azure AD B2C-Anwendung abmelden möchte, bedeutet dies beispielsweise nicht unbedingt, dass er sich auch vollständig von seinem Facebook-Konto abmelden möchte. Bei lokalen Konten wird die Sitzung des Benutzers jedoch ordnungsgemäß beendet.
> 
> 

## <a name="use-your-own-azure-ad-b2c-tenant"></a>Verwenden Sie Ihren eigenen Azure AD B2C-Mandanten
Führen Sie die folgenden drei Schritte aus, um diese Anforderungen selbst zu testen. Ersetzen Sie die in diesem Artikel verwendeten Beispielwerte durch Ihre eigenen Werte:

1. [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) Verwenden Sie den Namen Ihres Mandanten in den Anforderungen.
2. [Erstellen einer Anwendung](active-directory-b2c-app-registration.md) zum Abrufen einer Anwendungs-ID und eines `redirect_uri`-Werts. Fügen Ihrer App eine Web-App oder Web-API hinzu. Optional können Sie einen geheimen Anwendungsschlüssel erstellen.
3. [Erstellen der Richtlinien](active-directory-b2c-reference-policies.md) zum Abrufen der Richtliniennamen.

## <a name="samples"></a>Beispiele

* [Erstellen einer einseitigen App mithilfe von Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-nodejs-webapi)
* [Erstellen einer einseitigen App mithilfe von .NET](https://github.com/Azure-Samples/active-directory-b2c-javascript-singlepageapp-dotnet-webapi)


