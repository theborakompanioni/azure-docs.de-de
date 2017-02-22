
---
title: Azure Active Directory v2.0 und das OpenID Connect-Protokoll | Microsoft Docs
description: Erstellen von Webanwendungen mit der Azure AD v2.0-Implementierung des OpenID Connect-Authentifizierungsprotokolls.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a4875997-3aac-4e4c-b7fe-2b4b829151ce
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: 1d81be4ba596f7bc0ed7d16cb8bb9b375bd1e223


---
# <a name="azure-active-directory-v20-and-the-openid-connect-protocol"></a>Azure Active Directory v2.0 und das OpenID Connect-Protokoll
OpenID Connect ist ein Authentifizierungsprotokoll auf Grundlage von OAuth 2.0, mit dem Benutzer sicher bei Webanwendungen angemeldet werden können. Die Implementierung von OpenID Connect im v2.0-Endpunkt ermöglicht es Ihnen, Anmeldungen und API-Zugriff für Ihre webbasierten Apps hinzuzufügen. In diesem Artikel erfahren Sie, wie dies unabhängig von der Sprache ausgeführt wird. Er beschreibt, wie HTTP-Nachrichten ohne Verwendung von Microsoft Open Source-Bibliotheken gesendet und empfangen werden.

> [!NOTE]
> Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure Active Directory. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
> 
> 

[OpenID Connect](http://openid.net/specs/openid-connect-core-1_0.html) erweitert das OAuth 2.0-*Autorisierungsprotokoll* für die Verwendung als *Authentifizierungsprotokoll*, sodass Sie die einmalige Anmeldung mithilfe von OAuth durchführen können. OpenID Connect führt das Konzept eines *ID-Tokens* ein. Hierbei handelt es sich um ein Sicherheitstoken, mit dem der Client die Identität des Benutzers überprüfen kann. Ferner ruft das ID-Token auch Basisprofilinformationen über den Benutzer ab. Da OpenID Connect das OAuth 2.0-Protokoll erweitert, können Apps auf sichere Weise *Zugriffstoken* abrufen, die für den Zugriff auf Ressourcen verwendet werden können, welche mithilfe eines [Autorisierungsservers](active-directory-v2-protocols.md#the-basics) geschützt werden. OpenID Connect wird für das Erstellen von [Webanwendungen](active-directory-v2-flows.md#web-apps) empfohlen, die auf einem Server gehostet werden und auf die über einen Browser zugegriffen wird.

## <a name="protocol-diagram-sign-in"></a>Protokolldiagramm – Anmeldung
Der grundlegende Anmeldefluss besteht aus den in der nächsten Abbildung gezeigten Schritten. Jeder Schritt wird in diesem Artikel detailliert beschrieben.

![OpenID Connect-Protokoll: Anmeldung](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

## <a name="fetch-the-openid-connect-metadata-document"></a>Abrufen des OpenID Connect-Metadatendokuments
OpenID Connect beschreibt ein Metadatendokument, das den Großteil der erforderlichen Informationen für die Anmeldung einer App enthält. Hierzu gehören Informationen wie z.B. die zu verwendenden URLs und der Speicherort der öffentlichen Signaturschlüssel des Dienstes. Für den v2.0-Endpunkt sollten Sie folgendes OpenID Connect-Metadatendokument verwenden:

```
https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration
```

Der `{tenant}` kann einen von vier möglichen Werten annehmen:

| Wert | Beschreibung |
| --- | --- |
| `common` |Benutzer mit einem persönlichen Microsoft-Konto und einem Geschäfts-, Schul- oder Unikonto aus Azure Active Directory (Azure AD) können sich bei der Anwendung anmelden. |
| `organizations` |Nur Benutzer mit Geschäfts-, Schul- oder Unikonten aus Azure AD können sich bei der Anwendung anmelden. |
| `consumers` |Nur Benutzer mit einem persönlich Microsoft-Konto können sich bei der Anwendung anmelden. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` oder `contoso.onmicrosoft.com` |Nur Benutzer mit einem Geschäfts-, Schul- oder Unikonto eines bestimmten Azure AD-Mandanten können sich bei der Anwendung anmelden. Dabei kann entweder der Anzeigename der Domäne des Azure AD-Mandanten oder der GUID-Bezeichner des Mandanten verwendet werden. |

Bei den Metadaten handelt es sich um ein einfaches JavaScript Object Notation-Dokument (JSON). Die folgenden Codeausschnitte zeigen Beispiele. Die vollständigen Inhalte der Codeausschnitte werden in der [OpenID Connect-Spezifikation](https://openid.net) beschrieben.

```
{
  "authorization_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/authorize",
  "token_endpoint": "https:\/\/login.microsoftonline.com\/common\/oauth2\/v2.0\/token",
  "token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt"
  ],
  "jwks_uri": "https:\/\/login.microsoftonline.com\/common\/discovery\/v2.0\/keys",

  ...

}
```

In der Regel verwenden Sie dieses Metadatendokument zum Konfigurieren einer OpenID Connect-Bibliothek oder eines SDKs; die Bibliothek führt ihre Aufgaben mithilfe der Metadaten durch. Wenn Sie jedoch keine OpenID Connect-Prä-Build-Bibliothek verwenden, können Sie die Schritte weiter unten in diesem Artikel ausführen, um die Anmeldung bei einer Web-App mithilfe des v2.0-Endpunkts durchzuführen.

## <a name="send-the-sign-in-request"></a>Senden der Anmeldeanforderung
Wenn die Web-App den Benutzer authentifizieren muss, kann sie ihn direkt an den `/authorize` -Endpunkt weiterleiten. Diese Anforderung ähnelt dem ersten Abschnitt des [OAuth 2.0-Autorisierungscodeflusses](active-directory-v2-protocols-oauth-code.md), allerdings mit einigen wichtigen Unterschieden:

* Die Anforderung muss im `scope`-Parameter den `openid`-Bereich enthalten.
* Der `response_type`-Parameter muss `id_token` enthalten.
* Die Anforderung muss den `nonce` -Parameter enthalten.

Beispiel:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=678910
```

> [!TIP]
> Klicken Sie auf den folgenden Link, um diese Anforderung auszuführen. Nachdem Sie sich angemeldet haben, wird Ihr Browser an https://localhost/myapp/ mit einem ID-Token in der Adressleiste weitergeleitet. Hinweis: Diese Anforderung verwendet `response_mode=query` (nur zu Demonstrationszwecken). Wir empfehlen Ihnen, hierfür `response_mode=form_post` zu verwenden.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid&response_mode=query&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

| Parameter | Bedingung | Beschreibung |
| --- | --- | --- |
| Mandant |Erforderlich |Mit dem `{tenant}`-Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können. Zulässige Werte sind `common`, `organizations`, `consumers` und Mandantenbezeichner. Weitere Informationen finden Sie in den [Protokollgrundlagen](active-directory-v2-protocols.md#endpoints). |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Ihrer App zugewiesen hat. |
| response_type |Erforderlich |Muss das `id_token` für die OpenID Connect-Anmeldung enthalten. Es kann auch andere `response_types`-Werte enthalten, z.B. `code`. |
| redirect_uri |Empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können. Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben – mit dem Unterschied, dass er URL-codiert sein muss. |
| Bereich |Erforderlich |Eine durch Leerzeichen getrennte Liste von Bereichen. Für OpenID Connect muss der Bereich `openid`enthalten sein, der auf der Zustimmungsbenutzeroberfläche die Anmeldeberechtigung ergibt. Sie können in diese Anforderung auch andere Bereiche für das Anfordern der Zustimmung aufnehmen. |
| nonce |Erforderlich |Ein Wert in der Anforderung, der von der App erzeugt wird und im resultierenden ID-Tokenwert als Anspruch enthalten ist. Die App kann diesen Wert überprüfen, um die Gefahr von Tokenwiedergabeangriffen zu reduzieren. Der Wert ist in der Regel eine zufällige, eindeutige Zeichenfolge, die verwendet werden kann, um den Ursprung der Anforderung zu identifizieren. |
| response_mode |Empfohlen |Gibt die Methode an, die zum Senden des resultierenden Autorisierungscodes zurück an Ihre App verwendet werden soll. Kann `query`, `form_post` oder `fragment` sein. Bei Webanwendungen empfiehlt sich die Verwendung von `response_mode=form_post`, um eine möglichst sichere Tokenübertragung an die Anwendung zu gewährleisten. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Normalerweise wird ein zufällig generierter eindeutiger Wert verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12). Der Status wird außerdem verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| Eingabeaufforderung |Optional |Gibt den Typ der erforderlichen Benutzerinteraktion an. Die einzigen gültigen Werte sind gegenwärtig `login`, `none` und `consent`. Der Anspruch `prompt=login` zwingt den Benutzer, seine Anmeldeinformationen bei dieser Anforderung einzugeben. Einmaliges Anmelden ist dadurch nicht möglich. Der Anspruch `prompt=none` verhält sich genau entgegengesetzt. Dieser Wert stellt sicher, dass dem Benutzer keinerlei interaktive Eingabeaufforderung angezeigt wird. Wenn die Anforderung nicht über einmaliges Anmelden im Hintergrund abgeschlossen werden kann, gibt der v2.0-Endpunkt einen Fehler aus. Der Anspruch `prompt=consent` löst das OAuth-Zustimmungsdialogfeld aus, sobald sich der Benutzer angemeldet hat. Das Dialogfeld fordert den Benutzer zum Erteilen von Berechtigungen für die App auf. |
| login_hint |Optional |Sie können diesen Parameter verwenden, um das Feld für den Benutzernamen und die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist. Apps verwenden diesen Parameter häufig für die neuerliche Authentifizierung, nachdem sie den Benutzernamen bereits aus einer vorherigen Anmeldung mithilfe des `preferred_username`-Anspruchs extrahiert haben. |
| domain_hint |Optional |Mögliche Werte sind `consumers` oder `organizations`. Wenn dieser Parameter vorhanden ist, wird der E-Mail-basierte Ermittlungsvorgang übersprungen, den der Benutzer auf der v2.0-Anmeldeseite durchläuft, was die Benutzerfreundlichkeit verbessert. Apps verwenden diesen Parameter häufig für die neuerliche Authentifizierung, indem sie den Anspruch `tid` aus dem ID-Token extrahieren. Verwenden Sie `domain_hint=consumers`, wenn der Anspruch `tid` den Wert `9188040d-6c67-4c5b-b112-36a304b66dad` hat. Verwenden Sie andernfalls `domain_hint=organizations`. |

An dieser Stelle wird der Benutzer dazu aufgefordert, seine Anmeldeinformationen einzugeben und die Authentifizierung abzuschließen. Der v2.0-Endpunkt stellt sicher, dass der Benutzer den Berechtigungen zugestimmt hat, die im `scope`-Abfrageparameter angegeben sind. Wenn der Benutzer keiner der Berechtigungen zugestimmt hat, wird er vom v2.0-Endpunkt aufgefordert, den erforderlichen Berechtigungen zuzustimmen. Lesen Sie mehr über [Berechtigungen, die Zustimmung und mehrinstanzenfähige Apps](active-directory-v2-scopes.md).

Sobald der Benutzer authentifiziert wurde und seine Zustimmung erteilt hat, gibt der v2.0-Endpunkt mithilfe der im Parameter `response_mode` festgelegten Methode eine Antwort auf dem angegebenen Umleitungs-URI an Ihre App zurück.

### <a name="successful-response"></a>Erfolgreiche Antwort
Bei Verwendung von `response_mode=form_post` sieht eine erfolgreiche Antwort wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameter | Beschreibung |
| --- | --- |
| id_token |Das ID-Token, das die App angefordert hat. Sie können mit dem Parameter `id_token` die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md). |
| state |Wenn ein Parameter `state` in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den Umleitungs-URI gesendet werden, damit die App diese angemessen behandeln kann. Eine Fehlerantwort sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

### <a name="error-codes-for-authorization-endpoint-errors"></a>Fehlercodes beim Autorisierungsendpunktfehler
Die folgende Tabelle beschreibt die Fehlercodes, die im Parameter `error` der Fehlerantwort zurückgegeben werden können:

| Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- |
| invalid_request |Protokollfehler, z.B. ein fehlender erforderlicher Parameter. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstests festgestellt wird. |
| unauthorized_client |Die Clientanwendung darf keinen Autorisierungscode anfordern. |Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| access_denied |Der Ressourcenbesitzer hat die Zustimmung verweigert. |Die Clientanwendung kann den Benutzer benachrichtigen, dass sie ohne Zustimmung des Benutzers nicht fortfahren kann. |
| unsupported_response_type |Der Autorisierungsserver unterstützt den Antworttyp in der Anforderung nicht. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. Dies ist ein Entwicklungsfehler, der in der Regel bei den Eingangstests festgestellt wird. |
| server_error |Der Server hat einen unerwarteten Fehler erkannt. |Wiederholen Sie die Anforderung. Diese Fehler werden durch temporäre Bedingungen verursacht. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund eines temporären Fehlers verzögert ist. |
| temporarily_unavailable |Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. |Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |
| invalid_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert wurde. |Dies zeigt an, dass die Ressource, sofern vorhanden, nicht im Mandanten konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |

## <a name="validate-the-id-token"></a>Überprüfen des ID-Tokens
Das Empfangen eines ID-Tokens reicht nicht aus, um den Benutzer zu authentifizieren. Sie müssen außerdem die Signatur des ID-Tokens validieren und die Ansprüche im Token gemäß den Anforderungen Ihrer App überprüfen. Der v2.0-Endpunkt verwendet [JSON-Webtoken (JSTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) und die Verschlüsselung mit öffentlichem Schlüssel, um Token zu signieren und deren Gültigkeit zu überprüfen.

Sie können das ID-Token auch im Clientcode überprüfen. Üblicherweise wird das ID-Token jedoch an einen Back-End-Server gesendet, um die Überprüfung dort vorzunehmen. Nachdem Sie die Signatur des ID-Tokens validiert haben, müssen Sie noch einige Ansprüche überprüfen. Weitere Informationen, unter anderem zum [Überprüfen von Token](active-directory-v2-tokens.md#validating-tokens), sowie [wichtige Informationen zum Signaturschlüsselrollover](active-directory-v2-tokens.md#validating-tokens) finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md). Es wird empfohlen, eine Bibliothek zu verwenden, um Token zu analysieren und zu validieren. Für die meisten Sprachen und Plattformen ist mindestens einer dieser Bibliotheken verfügbar.
<!--TODO: Improve the information on this-->

Sie können je nach Szenario auch zusätzliche Ansprüche überprüfen. Einige allgemeinen Überprüfungen umfassen:

* Sicherstellen, dass sich der Benutzer bzw. die Organisation für die App angemeldet hat.
* Sicherstellen, dass der Benutzer die Autorisierung oder Berechtigungen angefordert hat.
* Sicherstellen, dass eine bestimmte Stärke der Authentifizierung aufgetreten ist, z.B. die Multi-Factor Authentication.

Weitere Informationen zu den Ansprüchen in einem ID-Token finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md).

Nachdem Sie das ID-Token vollständig überprüft haben, können Sie eine Sitzung mit dem Benutzer beginnen. Verwenden Sie die Ansprüche im ID-Token, um Informationen über den Benutzer in Ihrer App zu erhalten. Diese Informationen können für die Anzeige, für Aufzeichnungen, für die Autorisierung usw. verwendet werden.

## <a name="send-a-sign-out-request"></a>Senden einer Abmeldeanforderung
Derzeit unterstützt der v2.0-Endpunkt nicht den OpenID Connect `end_session_endpoint`. Dies bedeutet, dass Ihre App keine Anforderung an den v2.0-Endpunkt zum Beenden der Sitzung eines Benutzers und zum Löschen von Cookies, die vom v2.0-Endpunkt festgelegt wurden, senden kann.
Um einen Benutzer abzumelden, beendet eine App ganz einfach die eigene Sitzung mit dem Benutzer und lässt die Sitzung des Benutzers mit dem v2.0-Endpunkt unverändert. Wenn der Benutzer das nächste Mal versucht, sich anzumelden, wird eine Seite mit einem Hinweis zum Auswählen eines Kontos angezeigt, auf der die aktiv angemeldeten Konten aufgeführt werden. Auf dieser Seite kann der Benutzer sich von jedem Konto abmelden, sodass die Sitzung mit dem v2.0-Endpunkt beendet wird.

<!--

When you want to sign out the user from your app, it isn't sufficient to clear your app's cookies or otherwise end the user's session. You must also redirect the user to the v2.0 endpoint to sign out. If you don't do this, the user re-authenticates to your app without entering their credentials again, because they will have a valid single sign-in session with the v2.0 endpoint.

You can redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | Condition | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | Recommended | The URL that the user is redirected to after successfully signing out. If the parameter is not included, the user is shown a generic message that's generated by the v2.0 endpoint. |

-->

## <a name="protocol-diagram-token-acquisition"></a>Protokolldiagramm – Tokenabruf
Viele Web-Apps müssen nicht nur den Benutzer anmelden, sondern auch mithilfe von OAuth im Namen dieses Benutzers auf einen Webdienst zugreifen. Dieses Szenario kombiniert OpenID Connect für die Benutzerauthentifizierung und ruft gleichzeitig einen Autorisierungscode ab, der mithilfe des OAuth-Autorisierungscodeflusses Zugriffstoken abrufen kann.

Der vollständige Ablauf für die Anmeldung mit OpenID Connect und den Abruf von Token sieht etwa wie folgt aus. Die einzelnen Schritte werden in den nächsten Abschnitten dieses Artikels im Detail beschrieben.

![OpenID Connect-Protokoll: Tokenabruf](../../media/active-directory-v2-flows/convergence_scenarios_webapp_webapi.png)

## <a name="get-access-tokens"></a>Abrufen von Zugriffstoken
Zum Abrufen von Zugriffstoken müssen Sie die Anmeldeanforderung abwandeln:

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e        // Your registered Application ID
&response_type=id_token%20code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered redirect URI, URL encoded
&response_mode=form_post                              // 'query', 'form_post', or 'fragment'
&scope=openid%20                                      // Include both 'openid' and scopes that your app needs  
offline_access%20                                         
https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345                                          // Any value, provided by your app
&nonce=678910                                         // Any value, provided by your app
```

> [!TIP]
> Klicken Sie auf den folgenden Link, um diese Anforderung auszuführen. Nachdem Sie sich angemeldet haben, wird Ihr Browser mit einem ID-Token und einem Code in der Adressleiste an https://localhost/myapp/ weitergeleitet. Hinweis: Diese Anforderung verwendet `response_mode=query` (nur zu Demonstrationszwecken). Wir empfehlen Ihnen, hierfür `response_mode=form_post` zu verwenden.
> <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token%20code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>
> 
> 

Durch das Einschließen von Berechtigungsbereichen in die Anforderung und durch die Verwendung von `response_type=id_token code` stellt der v2.0-Endpunkt sicher, dass der Benutzer den im `scope`-Abfrageparameter angegebenen Berechtigungen zugestimmt hat. Er gibt einen Autorisierungscode an Ihre App für den Austausch gegen ein Zugriffstoken zurück.

### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort mit `response_mode=form_post` sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameter | Beschreibung |
| --- | --- |
| id_token |Das ID-Token, das die App angefordert hat. Sie können mit dem ID-Token die Identität des Benutzers überprüfen und eine Sitzung mit dem Benutzer beginnen. Weitere Informationen zu ID-Token und deren Inhalt finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md). |
| Code |Der Autorisierungscode, den die App angefordert hat. Die App kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. Ein Autorisierungscode ist von sehr kurzer Lebensdauer. Ein Autorisierungscode läuft in der Regel nach ungefähr 10 Minuten ab. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Die Anwendung sollte überprüfen, ob die Statuswerte in der Anforderung und in der Antwort identisch sind. |

### <a name="error-response"></a>Fehlerantwort
Fehlerantworten können auch an den Umleitungs-URI gesendet werden, damit die App diese angemessen behandeln kann. Eine Fehlerantwort sieht wie folgt aus:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Sie die Hauptursache eines Authentifizierungsfehlers identifizieren können. |

Eine Beschreibung der möglichen Fehlercodes und der jeweils empfohlenen Clientantwort finden Sie unter [Fehlercodes beim Autorisierungsendpunktfehler](#error-codes-for-authorization-endpoint-errors).

Nachdem Sie einen Autorisierungscode und ein ID-Token erhalten haben, können Sie den Benutzer anmelden und Zugriffstoken in seinem Namen abrufen. Zum Anmelden des Benutzers müssen Sie das ID-Token [genau wie beschrieben](#validate-the-id-token) überprüfen. Zum Abrufen von Zugriffstoken können Sie die in der [OAuth-Protokolldokumentation](active-directory-v2-protocols-oauth-code.md#request-an-access-token) beschriebenen Schritte ausführen.




<!--HONumber=Jan17_HO3-->


