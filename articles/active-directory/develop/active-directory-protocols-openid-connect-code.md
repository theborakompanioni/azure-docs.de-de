---
title: Grundlegendes zum OpenID Connect-Authentifizierungscodefluss in Azure AD | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mithilfe von Azure Active Directory und OpenID Connect verwenden.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 29142f7e-d862-4076-9a1a-ecae5bcd9d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: 3d5ad974c01e0ee3954da4f990da87338b2d1756
ms.openlocfilehash: e41620d3192dbb77a26b79663494e441ccd96d40
ms.lasthandoff: 02/23/2017


---
# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Autorisieren des Zugriffs auf Webanwendungen mit OpenID Connect und Azure Active Directory
[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) ist eine einfache Identitätsebene, die auf dem OAuth 2.0-Protokoll aufbaut. OAuth 2.0 definiert Mechanismen zum Beziehen und Verwenden von **Zugriffstoken** für den Zugriff auf geschützte Ressourcen, jedoch keine Standardmethoden zum Bereitstellen von Identitätsinformationen. OpenID Connect implementiert die Authentifizierung als eine Erweiterung zur OAuth 2.0-Autorisierung. Es stellt Informationen über den Endbenutzer in Form eines `id_token` bereit, durch das die Identität des Benutzers überprüft wird und grundlegende Informationen aus dem Benutzerprofil bereitgestellt werden.

OpenID Connect wird für Webanwendungen empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird.


[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)] 

## <a name="authentication-flow-using-openid-connect"></a>Authentifizierungsfluss bei OpenID Connect
Der grundlegende Anmeldevorgang umfasst die folgenden Schritte – sie werden unten jeweils im Detail beschrieben.

![OpenId Connect-Authentifizierungsfluss](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)

## <a name="openid-connect-metadata-document"></a>OpenID Connect-Metadatendokument

OpenID Connect beschreibt ein Metadatendokument, das den Großteil der erforderlichen Informationen für die Anmeldung einer App enthält. Hierzu gehören Informationen wie z.B. die zu verwendenden URLs und der Speicherort der öffentlichen Signaturschlüssel des Dienstes. Sie finden das OpenID Connect-Metadatendokument unter:

```
https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration
```
Bei den Metadaten handelt es sich um ein einfaches JavaScript Object Notation-Dokument (JSON). Die folgenden Codeausschnitte zeigen Beispiele. Die vollständigen Inhalte der Codeausschnitte werden in der [OpenID Connect-Spezifikation](https://openid.net) beschrieben.

```
{
    "authorization_endpoint": "https://login.microsoftonline.com/common/oauth2/authorize",
    "token_endpoint": "https://login.microsoftonline.com/common/oauth2/token",
    "token_endpoint_auth_methods_supported":
    [
        "client_secret_post",
        "private_key_jwt"
    ],
    "jwks_uri": "https://login.microsoftonline.com/common/discovery/keys"
    
    ...
}
```

## <a name="send-the-sign-in-request"></a>Senden der Anmeldeanforderung
Wenn die Webanwendung den Benutzer authentifizieren muss, muss sie ihn an den `/authorize` -Endpunkt weiterleiten. Diese Anforderung ähnelt dem ersten Abschnitt des [OAuth 2.0-Autorisierungscodeflusses](active-directory-protocols-oauth-code.md), aber es gibt auch einige wichtige Unterschiede:

* Die Anforderung muss im `scope`-Parameter den Bereich `openid` enthalten.
* Der `response_type`-Parameter muss `id_token` enthalten.
* Die Anforderung muss den `nonce` -Parameter enthalten.

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

| Parameter |  | Beschreibung |
| --- | --- | --- |
| Mandant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Die zulässigen Werte sind Mandantenbezeichner (also etwa `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder für mandantenunabhängige Token `common`). |
| client_id |erforderlich |Die Anwendungs-Id, die Ihrer App zugewiesen wird, wenn Sie sie mit Azure AD registrieren. Diese finden Sie im Azure-Portal. Klicken Sie auf **Azure Active Directory** und auf **App-Registrierungen**, wählen Sie die Anwendung aus, und suchen Sie die Anwendungs-ID auf der Anwendungsseite. |
| response_type |erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten.  Es können auch andere Antworttypen enthalten sein, z. B. `code`. |
| Bereich |erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen.  Für OpenID Connect muss der Bereich `openid`enthalten sein, der auf der Zustimmungsbenutzeroberfläche die Anmeldeberechtigung ergibt.  Sie können in diese Anforderung auch andere Bereiche für das Anfordern der Zustimmung aufnehmen. |
| nonce |erforderlich |Ein Wert in der von der App erzeugten Anforderung, die im resultierenden `id_token`-Element als Anspruch enthalten ist.  Die App kann diesen Wert dann überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu vermindern.  Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge oder GUID, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| redirect_uri |empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können.  Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss. |
| response_mode |empfohlen |Gibt die Methode an, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll.  Unterstützte Werte sind `form_post` für *HTTP-Formularbereitstellung* und `fragment` für *URL-Fragment*.  Bei Webanwendungen empfiehlt sich die Verwendung von `response_mode=form_post`, um eine möglichst sichere Tokenübertragung an die Anwendung zu gewährleisten. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der in der Tokenantwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12).  Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung |optional |Gibt den Typ der erforderlichen Benutzerinteraktion an.  Zurzeit sind die einzigen gültigen Werte „login“, „none“ und „consent“.  `prompt=login` zwingt den Benutzer, die Anmeldeinformationen bei dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich.  `prompt=none` ist genau das Gegenteil: Dieser Wert stellt sicher, dass dem Benutzer keine interaktive Eingabeaufforderung angezeigt wird.  Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der Endpunkt einen Fehler aus.  `prompt=consent` löst nach der Anmeldung des Benutzers das OAuth-Zustimmungsdialogfeld aus, in dem der Benutzer aufgefordert wird, der App Berechtigungen zu gewähren. |
| login_hint |optional |Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist.  Apps verwenden diesen Parameter häufig für die erneute Authentifizierung, nachdem sie den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahiert haben. |

An dieser Stelle wird der Benutzer aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen.

### <a name="sample-response"></a>Beispiel für eine Antwort
Eine Antwort nach der Benutzerauthentifizierung sieht beispielsweise wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschreibung |
| --- | --- |
| id_token |Das von der App angeforderte `id_token` -Element. Mit dem `id_token` -Element können Sie die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. |
| state |Ein in der Anforderung enthaltener Wert, der ebenfalls in der Tokenantwort zurückgegeben wird. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12).  Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Fehlercodes beim Autorisierungsendpunktfehler
Die folgende Tabelle beschreibt die verschiedenen Fehlercodes, die im `error` -Parameter der Fehlerantwort zurückgegeben werden können:

| Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- |
| invalid_request |Protokollfehler, z.B. ein fehlender erforderlicher Parameter. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstests festgestellt wird. |
| unauthorized_client |Die Clientanwendung darf keinen Autorisierungscode anfordern. |Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| access_denied |Der Ressourcenbesitzer hat die Zustimmung verweigert. |Die Clientanwendung kann den Benutzer benachrichtigen, dass sie ohne Zustimmung des Benutzers nicht fortfahren kann. |
| unsupported_response_type |Der Autorisierungsserver unterstützt den Antworttyp in der Anforderung nicht. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstests festgestellt wird. |
| server_error |Der Server hat einen unerwarteten Fehler erkannt. |Wiederholen Sie die Anforderung. Diese Fehler werden durch temporäre Bedingungen verursacht. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund eines temporären Fehlers verzögert ist. |
| temporarily_unavailable |Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. |Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |
| invalid_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist. |Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |

## <a name="validate-the-idtoken"></a>Überprüfen des ID-Tokens
Das Empfangen eines `id_token`-Elements allein reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen die Signatur validieren und die Ansprüche im `id_token`-Element gemäß den Anforderungen der App überprüfen. Der Azure AD-Endpunkt verwendet JSON-Webtoken (JWTs) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Sie können `id_token` auch im Clientcode überprüfen. Es ist jedoch eine bewährte Methode, `id_token` an einen Back-End-Server zu senden und die Überprüfung dort auszuführen. Nach der Überprüfung der Signatur des `id_token`-Elements müssen noch einige Ansprüche überprüft werden.

Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer/die Organisation für die App angemeldet hat.
* Sicherstellen, dass der Benutzer über eine ordnungsgemäße Autorisierung und Berechtigungen verfügt.
* Sicherstellen, dass eine bestimmte Stärke der Authentifizierung aufgetreten ist, z.B. die mehrstufige Authentifizierung.

Nach der Überprüfung des `id_token`-Elements können Sie mit dem Benutzer eine Sitzung beginnen und die Ansprüche im `id_token`-Element zum Abrufen von Informationen über den Benutzer in der App verwenden. Diese Informationen kann für die Anzeige, für Datensätze, für die Autorisierung usw. verwendet werden. Weitere Informationen zu den Tokentypen und Ansprüchen finden Sie unter [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldeanforderung
Wenn Sie den Benutzer bei der App abmelden möchten, reicht es nicht, die Cookies der App zu löschen oder die Sitzung mit dem Benutzer auf andere Weise zu beenden.  Sie müssen den Benutzer für die Abmeldung außerdem an den `end_session_endpoint` umleiten.  Andernfalls kann sich der Benutzer erneut für Ihre Anwendung authentifizieren, ohne die Anmeldeinformationen erneut einzugeben, da der Benutzer nach wie vor über eine gültige SSO-Sitzung beim Azure AD-Endpunkt verfügt.

Sie können den Benutzer einfach an den `end_session_endpoint` umleiten, der im OpenID Connect-Metadatendokument aufgeführt wird:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| post_logout_redirect_uri |empfohlen |Die URL, an die der Benutzer nach erfolgreicher Abmeldung umgeleitet werden soll.  Wenn keine Angabe erfolgt, wird dem Benutzer eine generische Meldung angezeigt. |

## <a name="single-sign-out"></a>Einmaliges Abmelden
Wenn Sie Benutzer an `end_session_endpoint` umleiten, löscht Azure AD die Sitzung des Benutzers aus dem Browser. Allerdings kann der Benutzer weiterhin bei anderen Anwendungen angemeldet sein, die Azure AD für die Authentifizierung verwenden. Damit diese Anwendungen den Benutzer gleichzeitig abmelden können, sendet Azure AD eine HTTP GET-Anforderung an die registrierte `LogoutUrl` aller Anwendungen, bei denen der Benutzer zurzeit angemeldet ist. Anwendungen müssen auf diese Anforderung antworten, indem sie alle Sitzungen löschen, mit denen der Benutzer identifiziert wird, und eine `200`-Antwort zurückgeben.  Wenn Sie das einmalige Abmelden in Ihrer Anwendung unterstützen möchten, müssen Sie eine solche `LogoutUrl` im Code Ihrer Anwendung implementieren.  Sie können die `LogoutUrl` im Azure-Portal festlegen:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Wählen Sie Ihre Active Directory-Instanz aus, indem Sie in der rechten oberen Ecke der Seite auf Ihr Konto klicken.
3. Wählen Sie im linken Navigationsbereich **Azure Active Directory**, dann **App Registrierungen** und schließlich Ihre Anwendung aus.
4. Klicken Sie auf **Eigenschaften**, und suchen Sie das Textfeld **Abmelde-URL**. 

## <a name="token-acquisition"></a>Tokenbeschaffung
Viele Web-Apps müssen nicht nur den Benutzer anmelden, sondern auch mithilfe von OAuth im Namen dieses Benutzers auf einen Webdienst zugreifen. Bei diesem Szenario wird OpenID Connect für die Benutzerauthentifizierung verwendet und gleichzeitig ein Autorisierungscode (`authorization_code`) abgerufen, der mithilfe des OAuth-Autorisierungscodeflusses zum Abrufen von Zugriffstoken (`access_tokens`) verwendet werden kann.

## <a name="get-access-tokens"></a>Abrufen von Zugriffstoken
Zum Abrufen von Zugriffstoken müssen Sie die oben aufgeführte Anmeldeanforderung ändern:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                     
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

Durch das Einschließen von Berechtigungsbereichen in die Anforderung und die Verwendung von `response_type=code+id_token` stellt der `authorize`-Endpunkt sicher, dass der Benutzer den im `scope`-Abfrageparameter angegebenen Berechtigungen zugestimmt hat. Anschließend gibt der Endpunkt einen Autorisierungscode an die App zurück, der gegen ein Zugriffstoken ausgetauscht wird.

### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=form_post` sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| id_token |Das von der App angeforderte `id_token` -Element. Mit dem `id_token` -Element können Sie die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. |
| Code |Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. Autorisierungscodes sind kurzlebig und laufen in der Regel nach etwa zehn Minuten ab. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese angemessen behandeln kann:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| -Parameter enthalten. | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Eine Beschreibung der möglichen Fehlercodes und der jeweils empfohlenen Clientaktion finden Sie unter [Fehlercodes beim Autorisierungsendpunktfehler](#error-codes-for-authorization-endpoint-errors).

Nachdem Sie einen Autorisierungs`code` und einen `id_token` erhalten haben, können Sie den Benutzer anmelden und Zugriffstoken in seinem Namen abrufen.  Zum Anmelden des Benutzers müssen Sie das `id_token` genau wie oben beschrieben überprüfen. Um Zugriffstoken zu erhalten, können Sie die in der [OAuth-Protokolldokumentation](active-directory-protocols-oauth-code.md) im Abschnitt „Fordern Sie ein Zugriffstoken mithilfe des Autorisierungscodes an“ beschriebenen Schritte befolgen.

