---
title: Azure Active Directory v2. 0-Bereiche, Berechtigungen und Zustimmung | Microsoft Docs
description: "Beschreibung der Autorisierung im Azure AD v2.0-Endpunkt, einschließlich Bereichen, Berechtigungen und Zustimmung."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: e00ceb8dd87e00bbdd05146e107c72e6182eb474


---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Bereiche, Berechtigungen und Zustimmung im Azure Active Directory v2. 0-Endpunkt
Apps, die sich in Azure Active Directory (Azure AD) integrieren lassen, folgen einem bestimmten Autorisierungsmodell, mit dem Benutzer festlegen können, wie eine App auf ihre Daten zugreift. Die v2.0-Implementierung dieses Autorisierungsmodells wurde aktualisiert, sodass die Art und Weise, wie eine App mit Azure AD interagieren muss, nun anders ist. Dieser Artikel behandelt die grundlegenden Konzepte dieses Autorisierungsmodells einschließlich der Bereiche, Berechtigungen und Zustimmung.

> [!NOTE]
> Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure Active Directory. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
>
>

## <a name="scopes-and-permissions"></a>Bereiche und Berechtigungen
Azure AD implementiert das [OAuth 2.0](active-directory-v2-protocols.md)-Autorisierungsprotokoll. OAuth 2.0 ist eine Methode, über die eine Drittanbieter-App im Auftrag eines Benutzers auf im Web gehostete Ressourcen zugreifen kann. Alle im Web gehosteten Ressourcen, die in Azure AD integriert werden können, verfügen über eine Ressourcen-ID oder einen *Anwendungs-ID-URI*. Zu den von Microsoft im Web gehosteten Ressourcen zählen beispielsweise:

* die Office 365 Unified Mail-API: `https://outlook.office.com`
* die Azure AD Graph-API: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

Dasselbe gilt für alle Ressourcen von Drittanbietern, die in Azure AD integriert wurden. Diese Ressourcen können auch einen Satz von Berechtigungen definieren, die zum Unterteilen der Funktionalität der Ressource in kleinere Einheiten verwendet werden können. Beispielsweise verfügt [Microsoft Graph](https://graph.microsoft.io) über definierte Berechtigungen, um unter anderem folgende Aufgaben auszuführen:

* Lesen des Kalenders eines Benutzers
* Schreiben in den Kalender eines Benutzers
* Senden von E-Mails als Benutzer

Durch das Definieren dieser Berechtigungstypen hat die Ressource eine präzisere Kontrolle über die Daten und die Art und Weise, wie sie verfügbar gemacht werden. Eine Drittanbieter-App kann von einem App-Benutzer diese Berechtigungen anfordern. Der App-Benutzer muss die Berechtigungen genehmigen, bevor die App im Auftrag des Benutzers agieren kann. Durch Segmentieren der Ressourcenfunktionalität in kleinere Berechtigungssätze können Drittanbieter-Apps so erstellt werden, dass nur die spezifischen Berechtigungen angefordert werden, die diese Apps zum Durchführen ihrer Funktion benötigen. Endbenutzer können genau erkennen, wie eine App ihre Daten verwendet, sodass sie sicher sein können, dass sich die App ohne bösartige Absichten verhält.

In Azure AD und OAuth werden diese Berechtigungen *Bereiche* genannt. Manchmal werden sie auch als *OAuth2-Berechtigungen* bezeichnet. Ein Bereich wird in Azure AD als Zeichenfolgenwert dargestellt. Um beim Beispiel von Microsoft Graph zu bleiben, lautet der Bereichswert für jede Berechtigung wie folgt:

* Lesen des Kalenders eines Benutzers mit `Calendar.Read`
* Schreiben in den Kalender eines Benutzers mit `Mail.ReadWrite`
* Senden von E-Mails als Benutzer mit `Mail.Send`

Eine Anwendung kann diese Berechtigungen durch Angabe der Bereiche in den Anforderungen an den v2.0-Endpunkt anfordern.

## <a name="openid-connect-scopes"></a>OpenID Connect-Bereiche
Die v2.0-Implementierung von OpenID Connect bietet einige klar definierte Bereiche, die für keine bestimmte Ressource gelten: `openid`, `email`, `profile` und `offline_access`.

### <a name="openid"></a>openid
Bei einer App-Anmeldung mit [OpenID Connect](active-directory-v2-protocols.md) muss der `openid`-Bereich angefordert werden. Der `openid`-Bereich wird auf der Zustimmungsseite des Arbeitskontos als Berechtigung „Sie werden angemeldet“ angezeigt, und auf der Zustimmungsseite des persönlichen Microsoft-Kontos als Berechtigung „Ihr Profil anzeigen und mit Ihrem Microsoft-Konto eine Verbindung zu Apps und Diensten herstellen“. Mit dieser Berechtigung kann eine App einen eindeutigen Bezeichner für den Benutzer in Form des Anspruchs `sub` empfangen. Sie ermöglicht der App zudem Zugriff auf den Endpunkt mit den Benutzerinformationen. Der `openid`-Bereich kann auch auf dem v2.0-Tokenendpunkt zum Abrufen von ID-Token verwendet werden, die zum Sichern von HTTP-Aufrufen zwischen verschiedenen Komponenten einer App genutzt werden können.

### <a name="email"></a>email
Der Bereich `email` kann zusammen mit dem Bereich `openid` und weiteren Bereichen verwendet werden. Er gibt der App Zugriff auf die primäre E-Mail-Adresse des Benutzers in Form des Anspruchs `email` . Der Anspruch `email` ist nur in einem Token enthalten, wenn dem Benutzerkonto eine E-Mail-Adresse zugewiesen ist (dies ist nicht immer der Fall). Bei Verwendung des Bereichs `email` sollte die Verarbeitung durch Ihre App auch dann möglich sein, wenn der Anspruch `email` nicht im Token vorhanden ist.

### <a name="profile"></a>Profil
Der Bereich `profile` kann zusammen mit dem Bereich `openid` und weiteren Bereichen verwendet werden. Er ermöglicht der App Zugriff auf eine Vielzahl von Benutzerinformationen. Dazu gehören u.a. Vorname, Nachname, bevorzugter Benutzername und Objekt-ID des Benutzers. Eine vollständige Liste der im Parameter id_tokens verfügbaren Profilansprüche für einen bestimmten Benutzer finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
Mit dem [`offline_access`-Bereich](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) kann Ihre App im Auftrag des Benutzers für einen längeren Zeitraum auf Ressourcen zugreifen. Auf der Zustimmungsseite des Arbeitskontos erscheint dieser Bereich als „Jederzeit und überall auf Ihre Daten zugreifen“. Auf der Zustimmungsseite des persönlichen Microsoft-Kontos erscheint er als „Jederzeit auf Ihre Informationen zugreifen“. Wenn ein Benutzer den `offline_access`-Bereich genehmigt, kann Ihre App Aktualisierungstoken vom v2.0-Tokenendpunkt empfangen. Aktualisierungstoken sind langlebig. Ihrer App kann neue Zugriffstoken abrufen, wenn ältere ablaufen.

Wenn die App den `offline_access`-Bereich nicht anfordert, werden auch keine Aktualisierungstoken empfangen. Dies bedeutet, dass Sie beim Einlösen eines Autorisierungscodes im [OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols.md) nur ein Zugriffstoken vom `/token`-Endpunkt erhalten. Das Zugriffstoken ist für eine kurze Zeit gültig. Das Zugriffstoken läuft in der Regel nach einer Stunde ab. Zu diesem Zeitpunkt muss Ihre App den Benutzer zurück auf den `/authorize`-Endpunkt leiten, um einen neuen Autorisierungscode abzurufen. Während dieser Umleitung muss der Benutzer möglicherweise seine Anmeldeinformationen erneut eingeben oder den Berechtigungen erneut zustimmen, je nach App-Typ.

Weitere Informationen zum Abrufen und Verwenden von Aktualisierungstoken finden Sie in der [v2.0-Protokollreferenz](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Anfordern der Zustimmung einzelner Benutzer
In einer Autorisierungsanforderung von [OpenID Connect oder OAuth 2.0](active-directory-v2-protocols.md) kann eine App die erforderlichen Berechtigungen mithilfe des `scope`-Abfrageparameters anfordern. Wenn sich ein Benutzer beispielsweise in einer App anmeldet, sendet die App eine Anforderung wie die folgende (mit Zeilenumbrüchen zur besseren Lesbarkeit):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Der `scope` -Parameter ist eine durch Leerzeichen getrennte Liste von Bereichen, die von der App angefordert wird. Jeder Bereich wird durch Anhängen des Bereichswerts an den Ressourcenbezeichner (Anwendungs-ID-URI) angegeben. Die Anforderung im Beispiel gibt an, dass die Anwendung eine Berechtigung zum Lesen des Kalenders des Benutzers und Senden von E-Mails als Benutzer benötigt.

Nachdem der Benutzer seine Anmeldeinformationen eingegeben hat, überprüft der v2.0-Endpunkt, ob es einen übereinstimmenden Datensatz der *Benutzerzustimmung* gibt. Wenn der Benutzer den angeforderten Berechtigungen in der Vergangenheit nicht zugestimmt hat, fragt der v2.0-Endpunkt den Benutzer nach den erforderlichen Berechtigungen.

![Zustimmung im Arbeitskonto](../../media/active-directory-v2-flows/work_account_consent.png)

Wenn der Benutzer die Berechtigung genehmigt, wird die Zustimmung aufgezeichnet, damit der Benutzer bei nachfolgenden Anmeldevorgängen nicht erneut seine Zustimmung geben muss.

## <a name="requesting-consent-for-an-entire-tenant"></a>Anfordern der Zustimmung für einen gesamten Mandanten
Wenn eine Organisation Lizenzen oder Abonnements für eine Anwendung erwirbt, soll die Anwendung häufig in vollem Umfang den Mitarbeitern bereitgestellt werden. Im Rahmen dieses Prozesses kann ein Administrator die Zustimmung für die Anwendung im Namen aller Mitarbeiter erteilen. Wenn der Administrator seine Zustimmung für den gesamten Mandanten erteilt hat, sehen die Mitarbeiter des Unternehmens keine Zustimmungsseite für die Anwendung.

Um die Zustimmung für alle Benutzer in einem Mandanten anzufordern, kann Ihre App den Endpunkt für die Administratorzustimmung verwenden.

## <a name="admin-restricted-scopes"></a>Auf den Administrator beschränkte Bereiche
Einige hochrangige Berechtigungen im Microsoft-Ökosystem können als *auf den Administrator beschränkt* gesetzt werden. Beispiele für derartige Bereiche beinhalten die folgenden Berechtigungen:

* Lesen der Verzeichnisdaten einer Organisation mit `Directory.Read`
* Schreiben von Daten in das Verzeichnis einer Organisation mit `Directory.ReadWrite`
* Lesen von Sicherheitsgruppen im Verzeichnis einer Organisation mit `Groups.Read.All`

Obwohl ein Endbenutzer einer Anwendung den Zugriff auf diese Daten gewähren kann, dürfen Organisationsbenutzer den Zugriff auf denselben Satz sensibler Unternehmensdaten nicht erteilen. Wenn Ihre Anwendung von einem Organisationsbenutzer Zugriff auf eine dieser Berechtigungen anfordert, wird dem Benutzer in einer Fehlermeldung mitgeteilt, dass er nicht befugt ist, den Berechtigungen Ihrer App zuzustimmen.

Wenn Ihre App den Zugriff auf diese dem Administrator vorbehaltenen Bereiche für Organisationen benötigt, sollten Sie diese ebenfalls über den Endpunkt für die Administratorzustimmung direkt von einem Unternehmensadministrator anfordern, wie im Folgenden beschrieben.

Wenn ein Administrator diese Berechtigungen über den Endpunkt für die Administratorzustimmung erteilt, wird die Zustimmung allen Benutzern im Mandanten gewährt.

## <a name="using-the-admin-consent-endpoint"></a>Verwenden des Endpunkts für die Administratorzustimmung
Mithilfe dieser Schritte kann Ihre App Berechtigungen für alle Benutzer in einem Mandanten sammeln, einschließlich der auf Administratoren beschränkten Bereiche. Ein Codebeispiel, in dem die beschriebenen Schritte implementiert sind, finden Sie unter [Beispiel für auf Administratoren beschränkte Bereiche](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern der Berechtigungen im App-Registrierungsportal
1. Wechseln Sie zur Anwendung im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), oder zu [Erstellen einer App](active-directory-v2-app-registration.md), sofern noch nicht geschehen.
2. Suchen Sie den Abschnitt **Microsoft Graph-Berechtigungen**, und fügen Sie die Berechtigungen hinzu, die von Ihrer App benötigt werden.
3. Stellen Sie sicher, dass Sie die App-Registrierung **speichern** .

### <a name="recommended-sign-the-user-in-to-your-app"></a>Empfohlen: Anmelden des Benutzers bei Ihrer App
Beim Erstellen einer Anwendung, die den Endpunkt für die Administratorzustimmung verwendet, benötigt die App in der Regel eine Seite/Ansicht, die dem Administrator das Genehmigen der App-Berechtigungen gestattet. Diese Seite kann Teil des Anmelde-Flows der App, Teil der App-Einstellungen oder ein dedizierter Flow „Verbinden“ sein. In vielen Fällen ist es sinnvoll, wenn die App diese Ansicht „Verbinden“ erst anzeigt, wenn ein Benutzer sich mit einem Geschäfts-, Schul- oder Unikonto von Microsoft angemeldet hat.

Durch das Anmelden des Benutzers bei der App können Sie die Organisation identifizieren, der der Administrator angehört, bevor Sie zur Genehmigung der nötigen Berechtigungen auffordern. Auch wenn es nicht unbedingt erforderlich ist, können Sie für Ihre Organisationsbenutzer auf diese Weise eine intuitivere Benutzeroberfläche erstellen. Zum Anmelden des Benutzers befolgen Sie unsere [Tutorials zum v2.0-Protokoll](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>Anfordern der Berechtigungen von einem Verzeichnisadministrator
Wenn Sie dazu bereit sind, vom Administrator der Organisation Berechtigungen anzufordern, können Sie den Benutzer zum *Endpunkt für die Administratorzustimmung* von v2.0 umleiten.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Bedingung | Beschreibung |
| --- | --- | --- |
| Mandant |Erforderlich |Der Verzeichnismandant, von dem Sie die Berechtigung anfordern möchten. Kann als GUID oder als Anzeigename bereitgestellt werden. |
| client_id |Erforderlich |Die Anwendungs-ID, die das [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Ihrer App zugewiesen hat. |
| redirect_uri |Erforderlich |Der Umleitungs-URI, an den die Antwort zur Verarbeitung durch die App gesendet werden soll. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben. |
| state |Empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |

An diesem Punkt erzwingt Azure AD, dass sich nur ein Mandantenadministrator anmelden kann, um die Anforderung abzuschließen. Der Administrator wird aufgefordert, alle Berechtigungen zu genehmigen, die Sie für Ihre App im Registrierungsportal angefordert haben.

#### <a name="successful-response"></a>Erfolgreiche Antwort
Wenn der Administrator die Berechtigungen für Ihre Anwendung genehmigt, lautet die erfolgreiche Antwort wie folgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschreibung |
| --- | --- | --- |
| Mandant |Der Verzeichnismandant, der Ihrer Anwendung die angeforderten Berechtigungen erteilt hat, im GUID-Format. |
| state |Ein in der Anforderung enthaltener Wert, der auch in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| admin_consent |Wird auf **true** festgelegt. |

#### <a name="error-response"></a>Fehlerantwort
Wenn der Administrator die Berechtigungen für Ihre App nicht genehmigt, lautet die Fehlerantwort wie folgt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschreibung |
| --- | --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der ein Entwickler die Grundursache eines Fehlers identifizieren kann. |

Nachdem Sie eine erfolgreiche Antwort vom Endpunkt für die Administratorzustimmung erhalten haben, erhält Ihre App die Berechtigungen, die sie angefordert hat. Als Nächstes können Sie ein Token für die Ressource anfordern, die Sie möchten.

## <a name="using-permissions"></a>Verwenden von Berechtigungen
Nachdem der Benutzer den Berechtigungen für Ihre App zugestimmt hat, kann Ihre App Zugriffstoken abrufen, die für die Berechtigung der App stehen, in irgendeiner Weise auf die Ressource zugreifen zu dürfen. Ein Zugriffstoken kann nur für eine einzelne Ressource verwendet werden, allerdings ist darin jede Berechtigung codiert, die Ihrer App für diese Ressource erteilt wurde. Um ein Zugriffstoken zu erhalten, kann Ihre App eine Anforderung an den v2.0-Tokenendpunkt senden, zum Beispiel:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Sie können das resultierende Zugriffstoken in HTTP-Anforderungen an die Ressource verwenden. Es zeigt der Ressource zuverlässig, dass die App über die erforderliche Berechtigung verfügt, eine bestimmte Aufgabe auszuführen.  

Weitere Informationen zum OAuth 2.0-Protokoll und zum Abrufen von Zugriffstoken finden Sie in der [Protokollreferenz zum v2.0-Endpunkt](active-directory-v2-protocols.md).



<!--HONumber=Jan17_HO3-->


