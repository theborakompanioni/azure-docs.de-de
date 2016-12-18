---
title: Azure Active Directory v2.0-Tokenreferenz | Microsoft Docs
description: Vom Azure AD v2.0-Endpunkt ausgestellte Token- und Anspruchstypen
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: dc58c282-9684-4b38-b151-f3e079f034fd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: a977cb509fb64d7c986e2e0f7e2b5e4e3e45dec0
ms.openlocfilehash: c40abe5364ca1198951a17b7df3084cf7d3cac45


---
# <a name="azure-active-directory-v20-tokens-reference"></a>Azure Active Directory v2.0-Tokenreferenz
Der Azure Active Directory (Azure AD) v2.0-Endpunkt stellt bei jedem [Authentifizierungsfluss](active-directory-v2-flows.md) verschiedene Arten von Sicherheitstoken aus. Dieses Dokument beschreibt das Format, die Sicherheitsmerkmale und den Inhalt der einzelnen Tokentypen.

> [!NOTE]
> Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure Active Directory. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
>
>

## <a name="types-of-tokens"></a>Tokentypen
Der v2.0-Endpunkt unterstützt das [OAuth 2.0-Autorisierungsprotokoll](active-directory-v2-protocols.md), das Zugriffstoken und Aktualisierungstoken verwendet. Der v2.0-Endpunkt unterstützt auch die Authentifizierung und die Anmeldung über [OpenID Connect](active-directory-v2-protocols.md). OpenID Connect führt einen dritten Tokentyp ein, das ID-Token. Alle diese Token werden als *Bearertoken* dargestellt.

Ein Bearertoken ist ein einfaches Sicherheitstoken, das dem Inhaber („Bearer“) den Zugriff auf eine geschützte Ressource ermöglicht. Der Bearer ist jede beliebige Partei, die das Token vorweisen kann. Um das Bearertoken zu erhalten, muss sich die Partei zwar bei Azure AD authentifizieren, falls jedoch keine Maßnahmen ergriffen werden, um das Token bei der Übertragung und Speicherung zu schützen, kann es von einer fremden Partei abgefangen und verwendet werden. Einige Sicherheitstoken verfügen über einen integrierten Mechanismus, der eine Verwendung durch nicht autorisierte Parteien verhindert. Bearertoken besitzen diesen Mechanismus jedoch nicht. Bearertoken müssen über einen sicheren Kanal wie z.B. Transport Layer Security (HTTPS) übertragen werden. Wird ein Bearertoken ohne eine derartige Sicherheit gesendet, kann eine böswillige Partei das Token mithilfe eines Man-in-the-Middle-Angriffs abrufen und damit unautorisiert auf eine geschützte Ressource zugreifen. Die gleichen Sicherheitsprinzipien gelten für die (Zwischen-)Speicherung von Trägertoken zur späteren Verwendung. Stellen Sie daher sicher, dass Ihre App Bearertoken stets auf sichere Weise überträgt und speichert. Weitere Sicherheitsüberlegungen zu Bearertoken finden Sie unter [RFC 6750, Abschnitt 5](http://tools.ietf.org/html/rfc6750).

Viele der vom v2.0-Endpunkt ausgestellten Token werden als JSON-Webtoken (JWTs) implementiert. Ein JWT stellt eine kompakte, URL-sichere Methode zum Übertragen von Informationen zwischen zwei Parteien dar. Die in einem JWT abgerufenen Informationen werden als *Anspruch* bezeichnet. Hierbei handelt es sich um Assertionen von Informationen zum Träger und zum Antragsteller des Tokens. Bei den Ansprüchen in einem JWT handelt es sich um JavaScript Object Notation-Objekte (JSON), die für die Übertragung codiert und serialisiert wurden. Da die vom v2.0-Endpunkt ausgestellten JWTs signiert, aber nicht verschlüsselt sind, können Sie den Inhalt eines JWT problemlos für Debugzwecke untersuchen. Weitere Informationen zu JWTs finden Sie unter [JWT-Spezifikationen](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID-Token
Ein ID-Token ist eine Form eines Anmeldesicherheitstokens, das Ihre App empfängt, wenn sie die Authentifizierung mit [OpenID Connect](active-directory-v2-protocols.md) ausführt. ID-Token werden als [JWTs](#types-of-tokens)dargestellt und enthalten Ansprüche, die Sie zum Anmelden der Benutzer bei Ihrer App verwenden können. Sie können die Ansprüche in einem ID-Token auf unterschiedliche Weise verwenden. Im Allgemeinen setzen Administratoren ID-Token zum Anzeigen von Kontoinformationen oder für Entscheidungen hinsichtlich der Zugriffssteuerung in einer App ein. Der v2.0-Endpunkt stellt nur einen ID-Token-Typen aus. Dieser enthält unabhängig vom angemeldeten Benutzertyp einen konsistenten Satz von Ansprüchen. Das Format und der Inhalt der ID-Token sind für persönliche Microsoft-Kontobenutzer und für Geschäfts- oder Schulkonten gleich.

ID-Token sind zum gegenwärtigen Zeitpunkt zwar signiert, aber nicht verschlüsselt. Wenn Ihre App ein ID-Token empfängt, muss sie [die Signatur überprüfen](#validating-tokens), um die Authentizität des Tokens nachzuweisen. Zudem müssen einige Ansprüche im Token überprüft werden, um seine Gültigkeit zu belegen. Je nach den Anforderungen des Szenarios können die von einer App überprüften Ansprüche variieren. Einige [allgemeine Anspruchsüberprüfungen](#validating-tokens) muss Ihre App aber in jedem Szenario durchführen.

In den folgenden Abschnitten finden Sie die vollständigen Details zu Ansprüchen in ID-Token sowie ein ID-Beispieltoken. Beachten Sie, dass die Ansprüche in ID-Token nicht in einer bestimmten Reihenfolge zurückgegeben werden. Darüber hinaus können jederzeit neue Ansprüche in ID-Token eingeführt werden. Ihre App sollte nicht unterbrochen werden, wenn neue Ansprüche eingeführt werden. Die folgende Liste enthält die Ansprüche, die Ihre App gegenwärtig zuverlässig interpretieren kann. Weitere Informationen finden Sie in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-id-token"></a>ID-Beispieltoken
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [!TIP]
> Überprüfen Sie zu Übungszwecken die Ansprüche im ID-Beispieltoken, indem Sie das Token in [calebb.net](https://calebb.net) einfügen.
>
>

#### <a name="claims-in-id-tokens"></a>Ansprüche in ID-Token
| Name | Anspruch | Beispielwert | Beschreibung |
| --- | --- | --- | --- |
| audience |`aud` |`6731de76-14a6-49ae-97bc-6eba6914391e` |Identifiziert den vorgesehenen Empfänger des Tokens. Die Zielgruppe von ID-Token ist die Anwendungs-ID Ihrer App, die der App im Microsoft-Anwendungsregistrierungsportal zugewiesen wurde. Ihre App sollte diesen Wert überprüfen und das Token ablehnen, wenn der Wert nicht übereinstimmt. |
| Issuer (Aussteller) |`iss` |`https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` |Identifiziert den Sicherheitstokendienst (STS), der das Token und den Azure AD-Mandanten, in dem der Benutzer authentifiziert wurde, erstellt und zurückgibt. Ihre App sollte den Ausstelleranspruch überprüfen, um sicherzustellen, dass das Token vom v2.0-Endpunkt stammt. Sie sollte ferner den GUID-Abschnitt des Anspruchs nutzen, um die Gruppe der Mandanten zu beschränken, die sich bei der App anmelden können. Die GUID, die angibt, dass der Benutzer ein Consumer-Benutzer eines Microsoft-Kontos ist, lautet `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| issued at |`iat` |`1452285331` |Die Zeit, zu der das Token ausgestellt wurde (dargestellt als Epochenzeit) |
| expiration time |`exp` |`1452289231` |Die Zeit, zu der das Token ungültig wird (dargestellt als Epochenzeit). Ihre App sollte mit diesem Anspruch die Gültigkeit der Tokenlebensdauer überprüfen. |
| not before |`nbf` |`1452285331` |Die Zeit, zu der das Token gültig wird (dargestellt als Epochenzeit). Dieser Wert ist normalerweise mit dem Ausstellungszeitpunkt identisch. Ihre App sollte mit diesem Anspruch die Gültigkeit der Tokenlebensdauer überprüfen. |
| Version |`ver` |`2.0` |Die Version des ID-Tokens, die in Azure AD definiert wurde. Für den v2.0-Endpunkt lautet der Wert `2.0`. |
| tenant ID |`tid` |`b9419818-09af-49c2-b0c3-653adc1f376e` |Eine GUID, die den Azure AD-Mandanten darstellt, aus dem der Benutzer stammt. Bei Geschäfts- und Schulkonten ist die GUID die unveränderliche Mandanten-ID der Organisation, zu der der Benutzer gehört. Für persönliche Konten lautet der Wert `9188040d-6c67-4c5b-b112-36a304b66dad`. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |
| code hash |`c_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Der Codehash ist nur dann in ID-Token enthalten, wenn das ID-Token zusammen mit einem OAuth 2.0-Autorisierungscode ausgestellt wird. Mit seiner Hilfe kann die Authentizität eines Autorisierungscodes überprüft werden. Weitere Informationen zum Ausführen dieser Überprüfung finden Sie in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html). |
| access token hash |`at_hash` |`SGCPtt01wxwfgnYZy2VJtQ` |Der Zugriffstokenhash ist nur in ID-Token enthalten, wenn das ID-Token zusammen mit einem OAuth 2.0-Zugriffstoken ausgestellt wird. Mit seiner Hilfe kann die Authentizität eines Zugriffstokens überprüft werden. Weitere Informationen zum Ausführen dieser Überprüfung finden Sie in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html). |
| nonce |`nonce` |`12345` |Mithilfe der Nonce sollen Tokenwiedergabeangriffe abgewehrt werden. Ihre App kann eine Nonce in einer Autorisierungsanforderung mithilfe des `nonce`-Abfrageparameters angeben. Der in der Anforderung angegebene Wert wird (unverändert) im `nonce`-Anspruch des ID-Tokens ausgegeben. Ihre App kann den Wert anhand des Werts überprüfen, der in der Anforderung angegeben ist. Dabei wird die Sitzung der App einem bestimmten ID-Token zugeordnet. Ihre App sollte diese Überprüfung während des Überprüfungsvorgangs des ID-Tokens ausführen. |
| name |`name` |`Babe Ruth` |Der Namensanspruch gibt einen visuell lesbaren Wert an, der den Antragsteller des Tokens identifiziert. Der Wert ist nicht zwingend eindeutig, kann geändert werden und dient nur zu Anzeigezwecken. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |
| email |`email` |`thegreatbambino@nyy.onmicrosoft.com` |Die primäre E-Mail-Adresse, die mit dem Benutzerkonto verknüpft ist (sofern vorhanden). Der Wert kann geändert werden und sich im Laufe der Zeit ändern. Der Bereich `email` ist erforderlich, um diesen Anspruch zu empfangen. |
| preferred username |`preferred_username` |`thegreatbambino@nyy.onmicrosoft.com` |Der primäre Benutzername, der zum Darstellen des Benutzers im v2.0-Endpunkt verwendet wird. Dabei kann es sich um eine E-Mail-Adresse, eine Telefonnummer oder einen generischen Benutzernamen ohne bestimmtes Format handeln. Der Wert kann geändert werden und sich im Laufe der Zeit ändern. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |
| subject |`sub` |`MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` |Der Prinzipal, für den das Token Informationen zusichert, z. B. der Benutzer einer App. Dieser Wert ist unveränderlich und kann nicht erneut zugewiesen oder wiederverwendet werden. Er kann für die sichere Durchführung von Autorisierungsüberprüfungen verwendet werden, z.B. wenn das Token verwendet wird, um auf eine Ressource zuzugreifen. Da der Antragsteller immer in den Token vorhanden ist, die Azure AD ausstellt, wird die Nutzung dieses Werts in einem allgemeinen Autorisierungssystem empfohlen. |
| object ID |`oid` |`a1dbdde8-e4f9-4571-ad93-3059e3750d23` |Die Objekt-ID des Geschäfts- oder Schulkontos im Azure AD-System. Dieser Anspruch wird nicht für persönliche Microsoft-Konten ausgestellt. Der Bereich `profile` ist erforderlich, um diesen Anspruch zu empfangen. |

### <a name="access-tokens"></a>Zugriffstoken
Vom v2.0-Endpunkt ausgestellte Zugriffstoken können derzeit nur von Microsoft Services genutzt werden. Für alle gegenwärtig unterstützten Szenarios sollten Ihre Apps keine Validierung oder Überprüfung der Zugriffstoken ausführen müssen. Zugriffstoken können als vollkommen intransparent erachtet werden. Sie sind lediglich Zeichenfolgen, die Ihre App in HTTP-Anforderungen an Microsoft übergeben kann.

In Kürze wird es auf dem v2.0-Endpunkt für Ihre App möglich sein, Zugriffstoken von anderen Clients zu empfangen. Zum gegenwärtigen Zeitpunkt werden die Informationen in diesem Referenzthema mit den Informationen aktualisiert, die Ihre App für die Überprüfung von Zugriffstoken und andere ähnliche Aufgaben benötigt.

Beim Anfordern eines Zugriffstokens vom v2.0-Endpunkt gibt der Endpunkt auch Metadaten zum Zugriffstoken für die Nutzung durch Ihre App zurück. Diese Informationen umfassen die Ablaufzeit eines Zugriffstokens und die Bereiche, für die es gilt. Diese Metadaten ermöglichen Ihrer App das intelligente Zwischenspeichern von Zugriffstoken, ohne dass hierbei das Zugriffstoken selbst analysiert werden muss.

### <a name="refresh-tokens"></a>Aktualisierungstoken
Aktualisierungstoken sind Sicherheitstoken, mit denen Ihre App neue Zugriffstoken in einem OAuth 2.0-Fluss abrufen kann. Ihre App kann Aktualisierungstoken verwenden, um langfristig Zugriff auf Ressourcen im Auftrag eines Benutzers zu erhalten, ohne dass ein Benutzereingriff erforderlich ist.

Aktualisierungstoken bestehen aus mehreren Ressourcen. Ein Aktualisierungstoken, das bei einer Tokenanforderung für eine Ressource empfangen wurde, kann als Zugriffstoken bei einer völlig anderen Ressource eingelöst werden.

Um eine Aktualisierung bei einer Tokenantwort zu erhalten, muss Ihre App den `offline_acesss`-Bereich anfordern, und dieser Bereich muss gewährt werden. Weitere Informationen zum `offline_access`-Bereich finden Sie im Artikel zu [Bereichen und Zustimmungen](active-directory-v2-scopes.md).

Aktualisierungstoken sind für Ihre App niemals transparent. Sie werden vom Azure AD v2.0-Endpunkt ausgestellt und können nur von diesem v2.0-Endpunkt überprüft und interpretiert werden. Sie sind zwar sehr lange gültig, in Ihrer App darf aber nicht von einer unbegrenzten Gültigkeitsdauer ausgegangen werden. Aktualisierungstoken können jederzeit aus unterschiedlichen Gründen ungültig werden. Die einzige Möglichkeit für Ihre App, die Gültigkeit eines Aktualisierungstokens zu überprüfen, besteht in der Einlösung des Tokens. Führen Sie dazu eine Tokenanforderung auf dem v2.0-Endpunkt aus.

Wenn Sie ein Aktualisierungstoken für ein neues Zugriffstoken einlösen (und wenn Ihrer App der `offline_access`-Bereich zugeteilt wurde), erhalten Sie ein neues Aktualisierungstoken in der Tokenantwort. Speichern Sie das neu ausgestellte Aktualisierungstoken, um das in der Anforderung verwendete Token zu ersetzen. Dadurch wird sichergestellt, dass die Aktualisierungstoken möglichst lange gültig bleiben.

## <a name="validating-tokens"></a>Überprüfen von Token
Zum aktuellen Zeitpunkt müssen Ihre Apps nur ID-Token überprüfen. Zur Überprüfung eines ID-Tokens muss Ihre App sowohl die Signatur des ID-Tokens als auch die Ansprüche im ID-Token überprüfen.

<!-- TODO: Link -->
Microsoft bietet Bibliotheken und Codebeispiele, die zeigen, wie die Tokenüberprüfung problemlos ausgeführt werden kann. In den nächsten Abschnitten wird der zugrunde liegende Prozess beschrieben. Einige Drittanbieter-Open-Source-Bibliotheken sind auch für die JWT-Überprüfung verfügbar. Es gibt mindestens eine Bibliotheksoption für nahezu jede Plattform und Sprache.

### <a name="validate-the-signature"></a>Überprüfen der Signatur
Ein JWT enthält drei Segmente, die durch das Zeichen `.` getrennt sind. Das erste Segment wird als *Header*, das zweite als *Text* und das dritte als *Signatur* bezeichnet. Mit dem Signatursegment kann die Authentizität des ID-Tokens überprüft werden, sodass es für Ihre App als vertrauenswürdig eingestuft werden kann.

ID-Token werden mit branchenüblichen asymmetrischen Verschlüsselungsalgorithmen wie etwa RSA 256 signiert. Der Header des ID-Tokens enthält Informationen zum Schlüssel und zur Verschlüsselungsmethode, die zum Signieren des Tokens verwendet wird. Beispiel:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Der Anspruch `alg` gibt den Algorithmus an, mit dem das Token signiert wurde. Der Anspruch `kid` bezeichnet den öffentlichen Schlüssel, mit dem das Token signiert wurde.

Der v2.0-Endpunkt kann jederzeit ein ID-Token mithilfe eines bestimmten Satzes von Paaren aus öffentlichen und privaten Schlüsseln signieren. Der v2.0-Endpunkt wechselt regelmäßig durch die möglichen Sätze von Schlüsseln. Ihre App muss also auf die automatische Verarbeitung dieser Schlüsseländerungen ausgelegt sein. Die vom v2.0-Endpunkt verwendeten öffentlichen Schlüssel sollten idealerweise alle 24 Stunden auf Änderungen überprüft werden.

Sie können die Signaturschlüsseldaten, die zum Überprüfen der Signatur erforderlich sind, mithilfe des OpenID Connect-Metadatendokuments abrufen. Dieses Dokument befindet sich unter:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [!TIP]
> Fügen Sie die URL in einen Browser ein.
>
>

Bei diesem Metadatendokument handelt es sich um ein JSON-Objekt, das zahlreiche nützliche Informationen enthält, beispielsweise den Ort der verschiedenen Endpunkte, die für die OpenID Connect-Authentifizierung erforderlich sind.  Darüber hinaus enthält das Dokument einen *jwks_uri*, der den Speicherort des Satzes von öffentlichen Schlüsseln zum Signieren von Token angibt. Das JSON-Dokument am jwks_uri verfügt über alle Informationen zum gegenwärtig verwendeten öffentlichen Schlüssel. Ihre App kann mit dem `kid`-Anspruch im JWT-Header auswählen, welcher öffentliche Schlüssel in diesem Dokument zum Signieren eines Tokens verwendet wurde. Sie führt anschließend die Signaturüberprüfung mithilfe des korrekten öffentlichen Schlüssels und des angegebenen Algorithmus aus.

Die Signaturüberprüfung geht über den Rahmen dieses Dokuments hinaus. Hilfreiche Informationen stehen jedoch in zahlreichen Open Source-Bibliotheken zur Verfügung.

### <a name="validate-the-claims"></a>Überprüfen der Ansprüche
Wenn Ihre App bei der Benutzeranmeldung ein ID-Token empfängt, sollte sie auch die Ansprüche im ID-Token überprüfen. Dazu gehören unter anderem folgende Ansprüche:

* Anspruch **audience** (Zielgruppe): zum Überprüfen, ob das ID-Token an Ihre App übergeben werden sollte
* Ansprüche **not before** (Nicht vor) und **expiration time** (Ablaufzeit): zum Sicherstellen, dass das ID-Token nicht abgelaufen ist
* Anspruch **issuer** (Aussteller): zum Überprüfen, ob das Token tatsächlich vom v2.0-Endpunkt für Ihre App ausgestellt wurde
* **nonce**: zur Abwehr von Tokenwiedergabeangriffen

Eine vollständige Liste der Anspruchsüberprüfungen, die von Ihrer App ausgeführt werden sollten, finden Sie in der [OpenID Connect-Spezifikation](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Details zu den erwarteten Werten für diese Ansprüche finden Sie oben im Abschnitt zu den [ID-Token](# ID tokens).

## <a name="token-lifetimes"></a>Tokengültigkeitsdauer
Die folgenden Tokengültigkeitsdauern werden nur zu Informationszwecken angegeben. Diese Informationen können Sie beim Entwickeln und Debuggen von Apps unterstützen. Beim Schreiben Ihrer Apps darf nicht davon ausgegangen werden, dass die Gültigkeitsdauer konstant bleibt. Die Gültigkeitsdauer von Token kann und wird sich zu einem beliebigen Zeitpunkt ändern.

| Tokenverschlüsselung | Gültigkeitsdauer | Beschreibung |
| --- | --- | --- |
| ID-Token (Geschäfts- oder Schulkonten) |1 Stunde |ID-Token sind in der Regel eine Stunde lang gültig. Ihre Web-App kann dieselbe Lebensdauer für ihre eigene Sitzung mit dem Benutzer verwenden (empfohlen), oder Sie können eine andere Sitzungsdauer wählen. Falls Ihre App ein neues ID-Token abrufen muss, muss sie lediglich eine neue Anmeldeanforderung an den v2.0-Autorisierungsendpunkt senden. Wenn der Benutzer eine gültige Browsersitzung für den v2.0-Endpunkt nutzt, muss der Benutzer unter Umständen seine Anmeldeinformationen nicht erneut eingeben. |
| ID-Token (persönliche Konten) |24 Stunden |ID-Token für persönliche Konten gelten in der Regel 24 Stunden. Ihre Web-App kann dieselbe Lebensdauer für ihre eigene Sitzung mit dem Benutzer verwenden (empfohlen), oder Sie können eine andere Sitzungsdauer wählen. Falls Ihre App ein neues ID-Token abrufen muss, muss sie lediglich eine neue Anmeldeanforderung an den v2.0-Autorisierungsendpunkt senden. Wenn der Benutzer eine gültige Browsersitzung für den v2.0-Endpunkt nutzt, muss der Benutzer unter Umständen seine Anmeldeinformationen nicht erneut eingeben. |
| Zugriffstoken (Geschäfts- oder Schulkonten) |1 Stunde |Wird in Tokenantworten als Teil der Tokenmetadaten angegeben. |
| Zugriffstoken (persönliche Konten) |1 Stunde |Wird in Tokenantworten als Teil der Tokenmetadaten angegeben. Für Zugriffstoken, die für persönliche Konten ausgestellt werden, kann eine andere Gültigkeitsdauer konfiguriert werden. In der Regel sind sie jedoch 1 Stunde lang gültig. |
| Aktualisierungstoken (Geschäfts- oder Schulkonten) |Bis zu 14 Tage |Ein einzelnes Aktualisierungstoken ist maximal 14 Tage gültig. Das Aktualisierungstoken kann jedoch jederzeit aus verschiedenen Gründen ungültig werden. Ihre App sollte daher ein Aktualisierungstoken so lange verwenden, bis es fehlschlägt oder von Ihrer App durch ein neues Aktualisierungstoken ersetzt wird. Ein Aktualisierungstoken wird außerdem ungültig, wenn der Benutzer das letzte Mal vor 90 Tagen seine Anmeldeinformationen eingegeben hat. |
| Aktualisierungstoken (persönliche Konten) |Bis zu 1 Jahr |Ein einzelnes Aktualisierungstoken ist maximal 1 Jahr gültig. Das Aktualisierungstoken kann jedoch jederzeit aus verschiedenen Gründen ungültig werden. Ihre App muss daher ein Aktualisierungstoken verwenden, bis es fehlschlägt. |
| Autorisierungscodes (Geschäfts- oder Schulkonten) |10 Minuten |Autorisierungscodes haben absichtlich eine kurze Gültigkeitsdauer. Sie müssen umgehend für Zugriffs- und Aktualisierungstoken eingelöst werden, wenn die Token empfangen werden. |
| Autorisierungscodes (persönliche Konten) |5 Minuten |Autorisierungscodes haben absichtlich eine kurze Gültigkeitsdauer. Sie müssen umgehend für Zugriffs- und Aktualisierungstoken eingelöst werden, wenn die Token empfangen werden. Für persönliche Konten ausgestellte Autorisierungscodes können nur einmal verwendet werden. |



<!--HONumber=Nov16_HO3-->


