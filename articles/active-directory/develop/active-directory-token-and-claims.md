---
title: "Weitere Informationen zu den verschiedenen von Azure AD unterstützten Token- und Anspruchstypen | Microsoft-Dokumentation"
description: "Anleitung zum Verstehen und Auswerten der Ansprüche in den SAML 2.0- und JWT-Token (JSON Web Tokens), die Azure Active Directory (Azure AD) ausstellt."
documentationcenter: na
author: bryanla
services: active-directory
manager: mbaldwin
editor: 
ms.assetid: 166aa18e-1746-4c5e-b382-68338af921e2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/07/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: ba958d029e5bf1bc914a2dff4b6c09282d578c67
ms.openlocfilehash: 4612c1f516dca51aea925343f79649761448c05d


---
# <a name="azure-ad-token-reference"></a>Azure AD-Tokenreferenz
Azure Active Directory (Azure AD) stellt bei der Verarbeitung der einzelnen Authentifizierungsflüsse verschiedene Arten von Sicherheitstoken aus. In diesem Dokument sind das Format, die Sicherheitsmerkmale und der Inhalt aller Tokentypen beschrieben.

## <a name="types-of-tokens"></a>Tokentypen
Azure AD unterstützt das [OAuth 2.0-Autorisierungsprotokoll](active-directory-protocols-oauth-code.md), das sowohl Zugriffstoken (access_tokens) als auch Aktualisierungstoken (refresh_tokens) verwendet.  Er unterstützt darüber hinaus die Authentifizierung und Anmeldung über [OpenID Connect](active-directory-protocols-openid-connect-code.md). Dabei wird ein dritter Tokentyp, das ID-Token, eingeführt.  Alle diese Token werden als Bearertoken dargestellt.

Ein Trägertoken ist ein einfaches Sicherheitstoken, das dem „Träger“ den Zugriff auf eine geschützte Ressource ermöglicht. In diesem Kontext ist der „Träger“ jede beliebige Partei, die das Token vorweisen kann. Um das Bearertoken zu erhalten, ist zwar eine Authentifizierung bei Azure AD erforderlich, es müssen jedoch Maßnahmen zur Sicherung des Tokens ergriffen werden, um ein Abfangen durch eine nicht vorgesehene Partei zu verhindern. Da Bearertoken nicht über einen integrierten Mechanismus zum Verhindern einer unbefugten Verwendung durch nicht autorisierte Parteien verfügt, müssen sie über einen sicheren Kanal wie etwa Transport Layer Security (HTTPS) übertragen werden. Wird ein Bearertoken als Klartext gesendet, das Token mithilfe eines Man-in-the-Middle-Angriffs abgefangen und damit unautorisierter Zugriff auf eine geschützte Ressource erlangt werdet. Die gleichen Sicherheitsprinzipien gelten für die (Zwischen-)Speicherung von Trägertoken zur späteren Verwendung. Stellen Sie daher sicher, dass Ihre App Bearertoken stets auf sichere Weise überträgt und speichert. Weitere Sicherheitsüberlegungen zu Bearertoken finden Sie unter [RFC 6750, Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der von Azure AD ausgestellten Token werden als JSON Web Token (JWT) implementiert.  Ein JWT stellt eine kompakte, URL-sichere Methode zum Übertragen von Informationen zwischen zwei Parteien dar.  Die in JWTs enthaltenen Informationen werden als „Ansprüche“ oder Assertionen von Informationen zum Träger und Antragsteller des Tokens bezeichnet.  Die Ansprüche in JWTs sind JSON-Objekte, die für die Übertragung codiert und serialisiert wurden.  Da die von Azure AD ausgestellten JWTs signiert, aber nicht verschlüsselt sind, können Sie den Inhalt eines JWTs problemlos für Debugzwecke untersuchen.  Dafür stehen mehrere Tools zur Verfügung (etwa [jwt.calebb.net](http://jwt.calebb.net)). Weitere Informationen zu JWTs finden Sie in der [JWT-Spezifikation](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>ID-Token
ID-Token sind eine Form von Anmeldesicherheitstoken, die Ihre App empfängt, wenn die Authentifizierung mit [OpenID Connect](active-directory-protocols-openid-connect-code.md) ausgeführt wird.  Sie werden als [JWTs](#types-of-tokens) dargestellt und enthalten Ansprüche, die Sie für die Anmeldung der Benutzer bei der App verwenden können.  Sie können die Ansprüche nach Bedarf in einem ID-Token verwenden. Im Allgemeinen werden Sie in einer App zum Anzeigen von Kontoinformationen oder zum Treffen von Entscheidungen hinsichtlich der Zugriffssteuerung eingesetzt.

ID-Token sind zu diesem Zeitpunkt zwar signiert, aber nicht verschlüsselt.  Wenn Ihre App ein ID-Token empfängt, muss sie [die Signatur überprüfen](#validating-tokens), um die Authentizität des Tokens nachzuweisen. Zudem müssen einige Ansprüche im Token überprüft werden, um seine Gültigkeit zu belegen.  Die von einer App überprüften Ansprüche variieren je nach Szenarioanforderungen. Es gibt jedoch einige [allgemeine Anspruchsüberprüfungen](#validating-tokens), die Ihre App in jedem Szenario ausführen muss.

Informationen zu den Ansprüchen in ID-Token sowie ein Beispiel für ein ID-Token finden Sie im folgenden Abschnitt.  Beachten Sie, dass die Ansprüche in ID-Token nicht in einer bestimmten Reihenfolge zurückgegeben werden.  Darüber hinaus können neue Ansprüche in ID-Token zu einem beliebigen Zeitpunkt eingeführt werden. Ihre App darf jedoch nicht unterbrochen werden, wenn neue Ansprüche eingeführt werden.  Die folgende Liste enthält die Ansprüche, die Ihre App zum Zeitpunkt der Erstellung dieses Dokuments zuverlässig interpretieren kann.  Ausführlichere Informationen finden Sie ggf. in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>ID-Beispieltoken
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [!TIP]
> Überprüfen Sie zu Übungszwecken die Ansprüche im ID-Beispieltoken, indem Sie sie in [calebb.net](http://jwt.calebb.net) einfügen.
> 
> 

#### <a name="claims-in-idtokens"></a>Ansprüche in ID-Token
> [!div class="mx-codeBreakAll"]
| JWT-Anspruch | Name | Beschreibung |
| --- | --- | --- |
| `appid` |Application ID (Anwendungs-ID) |Identifiziert die Anwendung, die das Token verwendet, um auf eine Ressource zuzugreifen. Die Anwendung kann als sie selbst oder im Auftrag eines Benutzers agieren. Die Anwendungs-ID stellt in der Regel ein Anwendungsobjekt dar, kann aber auch ein Dienstprinzipalobjekt in Azure AD darstellen. <br><br> **JWT-Beispielwert**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud` |Zielgruppe |Der vorgesehene Empfänger des Tokens. Die Anwendung, die das Token empfängt, muss prüfen, ob der "Audience"-Wert ordnungsgemäß ist, und alle Token ablehnen, die für eine andere Zielgruppe vorgesehen sind. <br><br> **SAML-Beispielwert**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **JWT-Beispielwert**: <br> `"aud":"https://contoso.com"` |
| `appidacr` |Application Authentication Context Class Reference (Kontextklassenreferenz für die Anwendungsauthentifizierung) |Gibt an, wie der Client authentifiziert wurde. Bei einem öffentlichen Client ist der Wert 0. Wenn die Client-ID und der geheime Clientschlüssel verwendet werden, ist der Wert 1. <br><br> **JWT-Beispielwert**: <br> `"appidacr": "0"` |
| `acr` |Authentication Context Class Reference (Klassenreferenz des Anwendungskontexts) |Gibt an, wie der Antragsteller authentifiziert wurde (im Gegensatz zum Client im Anspruch „Kontextklassenreferenz für die Anwendungsauthentifizierung“). Der Wert "0" gibt an, dass die Endbenutzerauthentifizierung nicht die ISO/IEC 29115-Anforderungen erfüllt. <br><br> **JWT-Beispielwert**: <br> `"acr": "0"` |
| Authentication Instant (Authentifizierungszeitpunkt) |Erfasst Datum und Uhrzeit der Authentifizierung. <br><br> **SAML-Beispielwert**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` | |
| `amr` |Authentifizierungsmethode |Gibt an, wie der Antragsteller des Tokens authentifiziert wurde. <br><br> **SAML-Beispielwert**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **JWT-Beispielwert**: `“amr”: ["pwd"]` |
| `given_name` |First Name (Vorname) |Gibt den Vornamen des Benutzers entsprechend der Festlegung im Azure AD-Benutzerobjekt an. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"given_name": "Frank"` |
| `groups` |Gruppen |Enthält die Objekt-IDs, die die Gruppenmitgliedschaften des Antragstellers darstellen. Diese Werte sind eindeutig (siehe "Object ID") und eignen sich zum sicheren Verwalten des Zugriffs, z. B. für das Erzwingen der Autorisierung für den Zugriff auf eine Ressource. Die im Anspruch "Groups" enthaltenen Gruppen werden über die "GroupMembershipClaims"-Eigenschaft des Anwendungsmanifests anwendungsbezogen konfiguriert. Mit dem Wert "null" werden alle Gruppen ausgeschlossen. Beim Wert "SecurityGroup" sind nur Active Directory-Sicherheitsgruppenmitglieder enthalten. Beim Wert "All" sind sowohl Sicherheitsgruppen als auch Office 365-Verteilerlisten enthalten. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` |Identity Provider (Identitätsanbieter) |Der Identitätsanbieter, der den Antragsteller des Tokens authentifiziert hat. Dieser Wert ist identisch mit dem Wert des "Issuer "-Anspruchs, es sei denn, das Benutzerkonto gehört zu einem anderen Mandanten als der Aussteller. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` |IssuedAt (Ausgestellt um) |Speichert die Uhrzeit, zu der das Token ausgestellt wurde. Er wird häufig verwendet, um die Aktualität von Token zu messen. <br><br> **SAML-Beispielwert**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **JWT-Beispielwert**: <br> `"iat": 1390234181` |
| `iss` |Issuer (Aussteller) |Identifiziert den Sicherheitstokendienst (Security Token Service, STS), der das Token erstellt und zurückgibt. Bei den Token, die Azure AD zurückgibt, ist der Aussteller "sts.windows.net". Die GUID im "Issuer"-Anspruch ist die Mandanten-ID des Azure AD-Verzeichnisses. Die Mandanten-ID ist ein unveränderlicher und zuverlässiger Bezeichner des Verzeichnisses. <br><br> **SAML-Beispielwert**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **JWT-Beispielwert**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` |Last Name (Nachname) |Gibt den Nachnamen des Benutzers entsprechend der Definition im Azure AD-Benutzerobjekt an. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"family_name": "Miller"` |
| `unique_name` |Name |Ein lesbarer Wert, der Aufschluss über den Antragsteller des Tokens gibt. Es wird nicht garantiert, dass der Wert innerhalb eines Mandanten eindeutig ist, weshalb er nur zu Anzeigezwecken dient. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` |Object ID (Objekt-ID) |Enthält einen eindeutigen Bezeichner eines Objekts in Azure AD. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet werden. Verwenden Sie die Objekt-ID zum Identifizieren eines Objekts in Abfragen von Azure AD. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` |Rollen |Stellt alle Anwendungsrollen dar, die dem Antragsteller direkt und indirekt über eine Gruppenmitgliedschaft zugewiesen wurden und zum Erzwingen der rollenbasierten Zugriffssteuerung verwendet werden können. Anwendungsrollen werden über die `appRoles` -Eigenschaft des Anwendungsmanifests anwendungsspezifisch definiert. Die `value` -Eigenschaft der einzelnen Anwendungsrollen ist der Wert, der im Roles-Anspruch enthalten ist. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `“roles”: ["Admin", … ]` |
| `scp` |Umfang |Gibt die Identitätswechselberechtigungen an, die der Clientanwendung gewährt wurden. Standardberechtigung: `user_impersonation`. Der Besitzer der gesicherten Ressource kann zusätzliche Werte in Azure AD registrieren. <br><br> **JWT-Beispielwert**: <br> `"scp": "user_impersonation"` |
| `sub` |Betreff |Identifiziert den Prinzipal, für den das Token Informationen bestätigt (beispielsweise der Benutzer einer Anwendung). Dieser Wert ist unveränderlich und kann nicht neu zugewiesen oder wiederverwendet werden. Daher kann er für die sichere Durchführung von Autorisierungsüberprüfungen verwendet werden. Da der Antragsteller immer in den Token vorhanden ist, die Azure AD ausstellt, wird die Nutzung dieses Werts in einem allgemeinen Autorisierungssystem empfohlen. <br> `SubjectConfirmation` ist kein Anspruch. Mit dem Element wird beschrieben, wie der Antragsteller des Tokens verifiziert wird. `Bearer` gibt an, dass der Antragsteller durch den Besitz des Tokens bestätigt wird. <br><br> **SAML-Beispielwert**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **JWT-Beispielwert**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"` |
| `tid` |Mandanten-ID |Ein unveränderlicher, nicht wieder verwendbarer Bezeichner, der den Verzeichnismandanten identifiziert, welcher das Token ausgestellt hat. Sie können diesen Wert verwenden, um auf mandantenspezifische Verzeichnisressourcen in einer mehrinstanzenfähigen Anwendung zugreifen. Sie können z. B. diesen Wert verwenden, um den Mandanten in einem Aufruf der Graph-API zu identifizieren. <br><br> **SAML-Beispielwert**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **JWT-Beispielwert**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"` |
| `nbf`, `exp` |Token Lifetime (Tokengültigkeitsdauer) |Das Zeitintervall, für das ein Token gültig ist. Der Dienst, der das Token überprüft, muss bestätigen, dass das aktuelle Datum innerhalb der Gültigkeitsdauer des Tokens liegt. Andernfalls wird das Token abgelehnt. Der Dienst kann einen Puffer von bis zu fünf Minuten über die Tokengültigkeitsdauer hinaus zulassen, um Differenzen bei der Istzeit (d. h. den zeitlichen Versatz) zwischen Azure AD und dem Dienst zu berücksichtigen. <br><br> **SAML-Beispielwert**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **JWT-Beispielwert**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn` |User Principal Name (Benutzerprinzipalname) |Speichert den Benutzernamen des Benutzerprinzipals.<br><br> **JWT-Beispielwert**: <br> `"upn": frankm@contoso.com` |
| `ver` |Version |Speichert die Versionsnummer des Tokens. <br><br> **JWT-Beispielwert**: <br> `"ver": "1.0"` |

## <a name="access-tokens"></a>Zugriffstoken

Wenn Ihre App für den Zugriff auf APIs nur Zugriffstoken *verwendet*, können (und sollten) Sie Zugriffstoken als vollständig intransparent behandeln – sie sind lediglich Zeichenfolgen, die Ihre App in HTTP-Anforderungen an Ressourcen übergeben kann.

Wenn Sie ein Zugriffstoken anfordern, gibt Azure AD auch einige Metadaten zum Zugriffstoken zurück, die von Ihrer App genutzt werden können.  Diese Informationen umfassen die Ablaufzeit eines Zugriffstokens und die Bereiche, für die es gilt.  Dies ermöglicht Ihrer App das intelligente Zwischenspeichern von Zugriffstoken, ohne dass dabei das Zugriffstoken selbst analysiert werden muss.

Wenn Ihre App eine in Azure AD geschützte API ist, die in HTTP-Anforderungen Zugriffstoken erwartet, sollten Sie die Token, die Sie erhalten, überprüfen und validieren. Weitere Informationen dazu, wie dies mit .NET funktioniert, finden Sie unter [Schützen einer Web-API mit Bearertoken aus Azure AD](active-directory-devquickstarts-webapi-dotnet.md).

## <a name="refresh-tokens"></a>Aktualisierungstoken

Aktualisierungstoken sind Sicherheitstoken, mit denen Ihre App neue Zugriffstoken in einem OAuth 2.0-Fluss abrufen kann.  Dadurch erhält Ihre App langfristig Zugriff auf Ressourcen im Auftrag eines Benutzers, ohne dass ein Benutzereingriff erforderlich ist.

Aktualisierungstoken bestehen aus mehreren Ressourcen. Das bedeutet, dass sie bei einer Tokenanforderung für eine Ressource empfangen werden, aber bei einer völlig anderen Ressource für Zugriffstoken eingelöst werden können. Legen Sie den `resource`-Parameter in der Anforderung auf die Zielressource fest, um mehrere Ressourcen festzulegen.

Der Inhalt von Aktualisierungstoken ist für Ihre App niemals zugänglich. Sie sind zwar sehr lange gültig, in Ihrer App darf aber nicht von einer unbegrenzten Gültigkeitsdauer ausgegangen werden.  Aktualisierungstoken können jederzeit aus unterschiedlichen Gründen ungültig werden.  Die einzige Möglichkeit für Ihre App, die Gültigkeit eines Aktualisierungstokens zu überprüfen, besteht in der Einlösung des Tokens. Führen Sie dazu eine Tokenanforderung am Azure AD-Tokenendpunkt aus.

Wenn Sie ein Aktualisierungstoken für ein neues Zugriffstoken einlösen, erhalten Sie ein neues Aktualisierungstoken in der Tokenantwort.  Speichern Sie das neu ausgestellte Aktualisierungstoken, und ersetzen Sie das in der Anforderung verwendete Token.  Dadurch wird sichergestellt, dass die Aktualisierungstoken möglichst lange gültig bleiben.

## <a name="validating-tokens"></a>Überprüfen von Token

Bei der Überprüfung eines „id_token“ oder eines „access_token“ muss Ihre App sowohl die Signatur als auch die Ansprüche des Tokens überprüfen.

Wir stellen Bibliotheken und Codebeispiele bereit, die die Tokenüberprüfung veranschaulichen, falls Sie den zugrunde liegenden Prozess nachvollziehen möchten.  Für die JWT-Überprüfung stehen zudem verschiedene Open Source-Bibliotheken von Drittanbietern zur Verfügung. Für nahezu jede Plattform und Sprache ist mindestens eine Option verfügbar. Weitere Informationen zu Azure AD-Authentifizierungsbibliotheken sowie Codebeispiele finden Sie unter [Azure Active Directory-Authentifizierungsbibliotheken](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Überprüfen der Signatur

Ein JWT enthält drei Segmente, die durch das Zeichen `.` getrennt sind.  Das erste Segment wird als **Header**, das zweite als **Text** und das dritte als **Signatur** bezeichnet.  Mit dem Signatursegment kann die Authentizität des Tokens überprüft werden, sodass es für Ihre App als vertrauenswürdig eingestuft werden kann.

Von Azure AD ausgestellte Token werden mit asymmetrischen Verschlüsselungsalgorithmen nach Industriestandard signiert, z.B. RSA 256. Der Header des JWT enthält Informationen zum Schlüssel und zur Verschlüsselungsmethode, die zum Signieren des Tokens verwendet werden:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

Der `alg`-Anspruch gibt den Algorithmus an, mit dem das Token signiert wurde. Der `x5t`-Anspruch hingegen bezeichnet den bestimmten öffentlichen Schlüssel, mit dem das Token signiert wurde.

Zu einem beliebigen Zeitpunkt signiert Azure AD unter Umständen ein ID-Token mithilfe eines bestimmten Satzes von Paaren aus öffentlichen und privaten Schlüsseln. Azure AD wechselt regelmäßig durch die möglichen Sätze von Schlüsseln. Ihre App muss also auf die automatische Verarbeitung dieser Schlüsseländerungen ausgelegt sein.  Die von Azure AD verwendeten öffentlichen Schlüssel sollten alle 24 Stunden auf Änderungen überprüft werden.

Sie können die Signaturschlüsseldaten, die zum Überprüfen der Signatur erforderlich sind, mithilfe des OpenID Connect-Metadatendokuments abrufen. Dieses Dokument befindet sich hier:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Fügen Sie diese URL in einen Browser ein.
> 
> 

Bei diesem Metadatendokument handelt es sich um ein JSON-Objekt, das zahlreiche nützliche Informationen enthält, beispielsweise den Ort der verschiedenen Endpunkte, die zum Ausführen der OpenID Connect-Authentifizierung erforderlich sind.  

Darüber hinaus enthält es einen `jwks_uri`, der den Speicherort des Satzes von öffentlichen Schlüsseln zum Signieren von Token angibt.  Das JSON-Dokument unter `jwks_uri` enthält alle Informationen zu den zu diesem Zeitpunkt verwendeten öffentlichen Schlüsseln.  Ihre App kann mit dem Anspruch `kid` im JWT-Header auswählen, welcher öffentliche Schlüssel in diesem Dokument zum Signieren eines bestimmten Tokens verwendet wurde.  Sie kann anschließend die Signaturüberprüfung mithilfe des korrekten öffentlichen Schlüssels und des angegebenen Algorithmus ausführen.

Die Signaturüberprüfung wird in diesem Dokument nicht erläutert. Es stehen jedoch zahlreiche Open Source-Bibliotheken mit hilfreichen Informationen zur Verfügung.

#### <a name="validating-the-claims"></a>Überprüfen der Ansprüche

Wenn Ihre App ein Token (entweder ein ID-Token bei der Benutzeranmeldung oder ein Zugriffstoken als Bearertoken in der HTTP-Anforderung) erhält, sollten auch einige Überprüfungen der Ansprüche im Token durchgeführt werden.  Dazu gehören unter anderem folgende Ansprüche:

* Anspruch **Zielgruppe**: Zum Überprüfen, ob das Token an Ihre App übergeben werden sollte.
* Ansprüche **Nicht vor** und **Ablaufzeit**: Zum Sicherstellen, dass das Token nicht abgelaufen ist.
* Anspruch **Aussteller** : Zum Überprüfen, ob das Token tatsächlich von Azure AD für Ihre App ausgestellt wurde.
* **Nonce**: Zur Abwehr von Tokenwiedergabeangriffen.
* und vieles mehr...

Eine vollständige Liste mit Anspruchsüberprüfungen für ID-Token, die von Ihrer App ausgeführt werden sollten, finden Sie in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation). Details zu den erwarteten Werten für diese Ansprüche finden Sie im vorhergehenden Abschnitt [ID-Token](#id-tokens).

## <a name="sample-tokens"></a>Beispieltoken

Dieser Abschnitt enthält Beispiele von SAML- und JWT-Token, die Azure AD zurückgibt. Bei diesen Beispielen können Sie die Ansprüche im Kontext sehen.

### <a name="saml-token"></a>SAML-Token

Dies ist ein Beispiel eines typischen SAML-Tokens.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT-Token – Benutzeridentitätswechsel
Dies ist ein Beispiel für ein typisches JSON Web Token (JWT), das in einem Autorisierungscodegewährungs-Fluss verwendet wird.
Neben Ansprüchen enthält das Token eine Versionsnummer in **ver** und **appidacr** und die Klassenreferenz des Anwendungskontexts, die angibt, wie der Client authentifiziert wurde. Bei einem öffentlichen Client ist der Wert 0. Wenn eine Client-ID oder ein geheimer Clientschlüssel verwendet wird, ist der Wert 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Verwandte Inhalte
* In den [Richtlinienvorgängen](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) und der [Richtlinienentität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity) von Azure AD Graph finden Sie weitere Informationen zur Verwaltung der Richtlinie für die Tokengültigkeitsdauer über die Azure AD Graph-API.
* Weitere Informationen und Beispiele zur Verwaltung von Richtlinien über PowerShell-Cmdlets finden Sie unter [Konfigurierbare Tokengültigkeitsdauer in Azure AD](../active-directory-configurable-token-lifetimes.md). 




<!--HONumber=Jan17_HO4-->


