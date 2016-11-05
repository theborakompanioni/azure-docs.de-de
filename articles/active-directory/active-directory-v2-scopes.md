---
title: 'Azure AD v2.0: Bereiche, Berechtigungen und Zustimmung | Microsoft Docs'
description: Beschreibung der Autorisierung im Azure AD v2.0-Endpunkt, einschließlich Bereichen, Berechtigungen und Zustimmung.
services: active-directory
documentationcenter: ''
author: dstrockis
manager: mbaldwin
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: dastrock

---
# <a name="scopes,-permissions,-&-consent-in-the-v2.0-endpoint"></a>Bereiche, Berechtigungen und Zustimmung im v2.0-Endpunkt
Apps, die sich in Azure AD integrieren lassen, folgen einem bestimmten Autorisierungsmodell, mit dem Benutzer festlegen können, wie eine App auf ihre Daten zugreift.  Die v2.0-Implementierung dieses Autorisierungsmodells wurde aktualisiert, sodass die Art und Weise, wie eine App mit Azure AD interagieren muss, nun anders ist.  In diesem Thema werden die grundlegenden Konzepte dieses Autorisierungsmodells einschließlich der Bereiche, Berechtigungen und Zustimmung behandelt.

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt.  Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
> 
> 

## <a name="scopes-&-permissions"></a>Bereiche und Berechtigungen
Azure AD implementiert das [OAuth 2.0](active-directory-v2-protocols.md) -Autorisierungsprotokoll – eine Methode, mit der eine Drittanbieter-App im Auftrag eines Benutzers auf im Web gehostete Ressourcen zugreifen kann.  Alle im Web gehosteten Ressourcen, die in Azure AD integriert werden können, verfügen über eine Ressourcen-ID oder einen **App-ID-URI**.  Zu den von Microsoft im Web gehosteten Ressourcen zählen beispielsweise:

* die Office 365 Unified Mail-API: `https://outlook.office.com`
* die Azure AD Graph-API: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

Dasselbe gilt für alle Ressourcen von Drittanbietern, die in Azure AD integriert wurden.  Diese Ressourcen können auch einen Satz von Berechtigungen definieren, die zum Unterteilen der Funktionalität der Ressource in kleinere Einheiten verwendet werden können.  Beispielsweise sind für Microsoft Graph folgende Berechtigungen definiert:

* Lesen des Kalenders eines Benutzers
* Schreiben in den Kalender eines Benutzers
* Senden von E-Mails als Benutzer
* [und mehr](https://graph.microsoft.io)

Durch das Definieren dieser Berechtigungen erhält die Ressource eine präzisere Kontrolle über die Daten und die Art und Weise, wie sie der Außenwelt verfügbar gemacht werden.  Eine Drittanbieter-App kann diese Berechtigungen dann von einem Endbenutzer anfordern, und der Endbenutzer muss die Berechtigungen genehmigen, bevor die App in seinem Auftrag fungieren kann.  Durch Segmentieren der Ressourcenfunktionalität in kleinere Berechtigungssätze können Drittanbieter-Apps so erstellt werden, dass nur die spezifischen Berechtigungen angefordert werden, die diese Apps zum Durchführen ihrer Aufgabe benötigen.  Dadurch können Endbenutzer auch genau erkennen, wie eine App ihre Daten verwendet, sodass sie sicher sein können, dass sich die App ohne bösartige Absichten verhält.

In Azure AD und OAuth sind diese Berechtigungen als **Bereiche**bekannt.  Manchmal werden sie auch als **OAuth2-Berechtigungen**bezeichnet.  Ein Bereich wird in Azure AD als Zeichenfolgenwert dargestellt.  Um beim Beispiel von Microsoft Graph zu bleiben, lautet der Bereichswert für jede Berechtigung wie folgt:

* Lesen des Kalenders eines Benutzers: `Calendar.Read`
* Schreiben in den Kalender eines Benutzers: `Mail.ReadWrite`
* Senden von E-Mails als Benutzer: `Mail.Send`

Eine Anwendung kann diese Berechtigungen wie unten beschrieben durch Angabe der Bereiche in den Anforderungen an den v2.0-Endpunkt anfordern.

## <a name="openid-connect-scopes"></a>OpenId Connect-Bereiche
Die v2.0-Implementierung von OpenID Connect bietet einige klar definierte Bereiche, die für keine bestimmte Ressource gelten: `openid`, `email`, `profile` und `offline_access`.

#### <a name="openid"></a>OpenId
Bei einer App-Anmeldung mit [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow) muss der `openid`-Bereich angefordert werden.  Der `openid` -Bereich wird auf dem Zustimmungsbildschirm des Arbeitskontos als Berechtigung "Sie werden angemeldet" angezeigt, und auf dem Zustimmungsbildschirm des persönlichen Microsoft-Kontos als Berechtigung "Ihr Profil anzeigen und mit Ihrem Microsoft-Konto eine Verbindung zu Apps und Diensten herstellen".  Mit dieser Berechtigung kann eine App einen eindeutigen Bezeichner für den Benutzer in Form des Anspruchs `sub` empfangen.  Sie ermöglicht der App zudem Zugriff auf den Endpunkt mit den Benutzerinformationen.  Der `openid`-Bereich kann auch auf dem v2.0-Tokenendpunkt zum Abrufen von ID-Token verwendet werden, die zum Sichern von HTTP-Aufrufen zwischen verschiedenen Komponenten einer App genutzt werden können.

#### <a name="email"></a>E-Mail-Adresse
Der Bereich `email` kann zusammen mit dem Bereich `openid` und weiteren Bereichen eingefügt werden.  Er ermöglicht den App Zugriff auf die primäre E-Mail-Adresse des Benutzers in Form des Anspruchs `email` .  Der Anspruch `email` ist nur in Token enthalten, wenn dem Benutzerkonto eine E-Mail-Adresse zugewiesen ist (dies ist nicht immer der Fall).  Bei Verwendung des Bereichs `email` sollte die Verarbeitung durch Ihre App auch dann möglich sein, wenn der Anspruch `email` nicht im Token vorhanden ist.

#### <a name="profile"></a>Profil
Der Bereich `profile` kann zusammen mit dem Bereich `openid` und weiteren Bereichen eingefügt werden.  Er ermöglicht der App Zugriff auf eine Fülle von Benutzerinformationen.  Dazu gehören u. a. Vorname, Nachname, bevorzugter Benutzername und Objekt-ID des Benutzers.  Eine vollständige Liste der in ID-Token verfügbaren Profilansprüche für einen bestimmten Benutzer finden Sie in der [v2.0-Tokenreferenz](active-directory-v2-tokens.md).

#### <a name="offline_access"></a>Offline_access
Mit dem [`offline_access` -Bereich](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) kann Ihre App im Auftrag des Benutzers für einen längeren Zeitraum auf Ressourcen zugreifen.  Auf dem Zustimmungsbildschirm des Arbeitskontos erscheint dieser Bereich als "Jederzeit und überall auf Ihre Daten zugreifen".  Auf dem Zustimmungsbildschirm des persönlichen Microsoft-Kontos erscheint er als "Jederzeit auf Ihre Informationen zugreifen".  Wenn ein Benutzer den `offline_access` -Bereich genehmigt, ist Ihre App für den Empfang von Aktualisierungstoken vom v2.0-Tokenendpunkt aktiviert.  Aktualisierungstoken sind langlebig und ermöglichen es Ihrer App, neue Zugriffstoken zu erhalten, wenn ältere ablaufen.

Wenn die App den `offline_access`-Bereich nicht anfordert, werden auch keine Aktualisierungstoken empfangen.  Dies bedeutet, dass Sie beim Einlösen eines Autorisierungscodes im [OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols.md#oauth2-authorization-code-flow) nur ein Zugriffstoken vom `/token`-Endpunkt erhalten.  Dieses Zugriffstoken bleibt für einen kurzen Zeitraum (in der Regel eine Stunde) gültig, läuft aber anschließend ab.  Zu diesem Zeitpunkt muss Ihre App den Benutzer zurück auf den `/authorize`-Endpunkt leiten, um einen neuen Autorisierungscode abzurufen.  Während dieser Umleitung muss der Benutzer möglicherweise seine Anmeldeinformationen erneut eingeben oder den Berechtigungen erneut zustimmen, je nach Apptyp.

Weitere Informationen zum Abrufen und Verwenden von Aktualisierungstoken finden Sie in der [v2.0-Protokollreferenz](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Anfordern der Zustimmung einzelner Benutzer
In einer Autorisierungsanforderung von [OpenID Connect oder OAuth 2.0](active-directory-v2-protocols.md) kann eine App die erforderlichen Berechtigungen mithilfe des `scope`-Abfrageparameters anfordern.  Wenn sich ein Benutzer beispielsweise in einer App anmeldet, sendet die App eine Anforderung wie die folgende (mit Zeilenumbrüchen zur besseren Lesbarkeit):

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

Der `scope` -Parameter ist eine durch Leerzeichen getrennte Liste von Bereichen, die von der App angefordert wird.  Jeder einzelne Bereich wird durch Anhängen des Bereichswerts an den Ressourcenbezeichner (App-ID-URI) angegeben.  Die Anforderung oben gibt an, dass die Anwendung eine Berechtigung zum Lesen des Kalenders des Benutzers und Senden von E-Mails als Benutzer benötigt.

Nachdem der Benutzer seine Anmeldeinformationen eingegeben hat, überprüft der v2.0-Endpunkt, ob es einen übereinstimmenden Datensatz der **Benutzerzustimmung**gibt.  Wenn der Benutzer den angeforderten Berechtigungen in der Vergangenheit nicht zugestimmt hat, fragt der v2.0-Endpunkt den Benutzer nach den erforderlichen Berechtigungen.  

![Screenshot der Zustimmung im Arbeitskonto](../media/active-directory-v2-flows/work_account_consent.png)

Wenn der Benutzer die Berechtigung genehmigt, wird die Zustimmung aufgezeichnet, damit der Benutzer bei nachfolgenden Anmeldevorgängen nicht erneut seine Zustimmung geben muss.

## <a name="requesting-consent-for-an-entire-tenant"></a>Anfordern der Zustimmung für einen gesamten Mandanten
Wenn eine Organisation Lizenzen oder Abonnements für eine Anwendung erwirbt, sollen diese häufig in vollem Umfang den Mitarbeiter bereitgestellt werden.  Im Rahmen dieses Prozesses kann ein Unternehmensadministrator die Zustimmung für diese Anwendung im Namen aller Mitarbeiter erteilen.  Durch das Erteilen der Zustimmung für einen gesamten Mandanten wird den Mitarbeitern der Organisation der Zustimmungsbildschirm für die Anwendung nicht angezeigt.

Um die Zustimmung für alle Benutzer in einem Mandanten anzufordern, kann Ihre App den im Folgenden beschriebenen **Endpunkt für die Administratorzustimmung**verwenden.

## <a name="admin-restricted-scopes"></a>Auf den Administrator beschränkte Bereiche
Bestimmte hochrangige Berechtigungen im Microsoft-Ökosystem können als **auf den Administrator beschränkt**gekennzeichnet werden.  Beispiele für solche Bereiche:

* Lesen der Verzeichnisdaten einer Organisation: `Directory.Read`
* Schreiben von Daten in das Verzeichnis einer Organisation: `Directory.ReadWrite`
* Lesen von Sicherheitsgruppen im Verzeichnis einer Organisation: `Groups.Read.All`

Während ein Endbenutzer einer Anwendung den Zugriff auf diese Daten gewähren kann, dürfen Organisationsbenutzer den Zugriff auf denselben Satz sensibler Unternehmensdaten nicht erteilen.  Wenn Ihre Anwendung von einem Organisationsbenutzer Zugriff auf eine dieser Berechtigungen anfordert, wird dem Benutzer in einer Fehlermeldung mitgeteilt, dass er nicht befugt ist, den Berechtigungen Ihrer App zuzustimmen.

Wenn Ihre App den Zugriff auf diese dem Administrator vorbehaltenen Bereiche für Organisationen benötigt, sollten Sie diese ebenfalls über den **Endpunkt für die Administratorzustimmung**direkt von einem Unternehmensadministrator anfordern, wie im folgenden beschrieben.

Wenn ein Administrator diese Berechtigungen über den Endpunkt für die Administratorzustimmung erteilt, wird die Zustimmung, wie oben beschrieben, allen Benutzern im Mandanten gewährt.

## <a name="using-the-admin-consent-endpoint"></a>Verwenden des Endpunkts für die Administratorzustimmung
Mithilfe dieser Schritte kann Ihre App Berechtigungen für alle Benutzer in einen bestimmten Mandanten sammeln, einschließlich der auf Administratoren beschränkten Bereiche.  Ein Codebeispiel, in dem die unten beschriebenen Schritte implementiert sind, finden Sie unter [Beispiel für auf Administratoren beschränkte Bereiche](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern der Berechtigungen im App-Registrierungsportal
* Navigieren Sie zu Ihrer Anwendung unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com), oder [erstellen Sie eine App](active-directory-v2-app-registration.md), falls noch nicht geschehen.
* Suchen Sie den Abschnitt **Microsoft Graph-Berechtigungen** , und fügen Sie die Berechtigungen hinzu, die von Ihrer App benötigt werden.
* Stellen Sie sicher, dass Sie die App-Registrierung **speichern** .

#### <a name="recommended:-sign-the-user-into-your-app"></a>Empfohlen: Anmelden des Benutzers bei Ihrer App
Beim Erstellen einer Anwendung, die den Endpunkt für die Administratorzustimmung verwendet, benötigt die App in der Regel eine Seite/Ansicht, die dem Administrator das Genehmigen der App-Berechtigungen gestattet.  Diese Seite kann Teil des Anmelde-Flows der App, Teil der App-Einstellungen oder ein dedizierter Flow „Verbinden“ sein.  In vielen Fällen ist es sinnvoll, wenn die App diese Ansicht „Verbinden“ erst anzeigt, wenn ein Benutzer sich mit einem Geschäfts-, Schul- oder Unikonto von Microsoft angemeldet hat.

Durch das Anmelden des Benutzers bei der App können Sie die Organisation identifizieren, der der Administrator angehört, bevor Sie zur Genehmigung der nötigen Berechtigungen auffordern.  Auch wenn es nicht unbedingt erforderlich ist, können Sie für Ihre Organisationsbenutzer auf diese Weise eine intuitivere Benutzeroberfläche erstellen.  Zum Anmelden des Benutzers befolgen Sie unsere [Tutorials zum v2.0-Protokoll](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Anfordern der Berechtigungen von einem Verzeichnisadministrator
Wenn Sie dazu bereit sind, vom Administrator des Unternehmens Berechtigungen anzufordern, können Sie den Benutzer zum **Endpunkt für die Administratorzustimmung**von v2.0 umleiten.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| Mandant |erforderlich |Der Verzeichnismandant, von dem Sie die Berechtigung anfordern möchten.  Kann als GUID oder als Anzeigename bereitgestellt werden. |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com)) Ihrer Anwendung zugewiesen hat. |
| redirect_uri |erforderlich |Der redirect_uri, an den die Antwort zur Verarbeitung durch die App gesendet werden soll.  Er muss genau mit einem der redirect_uris übereinstimmen, die Sie im Portal registriert haben. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |

An diesem Punkt erzwingt Azure AD, dass sich nur ein Mandantenadministrator anmelden kann, um die Anforderung abzuschließen.  Der Administrator wird aufgefordert, alle Berechtigungen zu genehmigen, die Sie für Ihre App im Registrierungsportal angefordert haben. 

##### <a name="successful-response"></a>Erfolgreiche Antwort
Wenn der Administrator die Berechtigungen für Ihre Anwendung genehmigt, lautet die erfolgreiche Antwort wie folgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschreibung |
| --- | --- | --- |
| Mandant |Der Verzeichnismandant, der Ihrer Anwendung die angeforderten Berechtigungen erteilt hat, im GUID-Format. |
| state |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| admin_consent |Wird auf `True` festgelegt. |

##### <a name="error-response"></a>Fehlerantwort
Wenn der Administrator die Berechtigungen für Ihre Anwendung nicht genehmigt, lautet die Fehlerantwort wie folgt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschreibung |
| --- | --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der ein Entwickler die Grundursache eines Fehlers identifizieren kann. |

Nachdem Sie eine erfolgreiche Antwort vom Endpunkt für die Administratorzustimmung erhalten haben, erhält Ihre App die Berechtigungen, die sie angefordert hat.  Sie können jetzt ein Token für die gewünschte Ressource anfordern, wie unten beschrieben.

## <a name="using-permissions"></a>Verwenden von Berechtigungen
Nachdem der Benutzer den Berechtigungen für Ihre App zugestimmt hat, kann Ihre App Zugriffstoken abrufen, die für die Berechtigung der App stehen, in irgendeiner Weise auf die Ressource zugreifen zu dürfen.  Ein bestimmtes Zugriffstoken kann nur für eine einzelne Ressource verwendet werden, allerdings ist darin jede Berechtigung codiert, die Ihrer App für diese Ressource erteilt wurde.  Um ein Zugriffstoken zu erhalten, kann Ihre App eine Anforderung an den v2.0-Tokenendpunkt senden:

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

Das sich ergebende Zugriffstoken kann dann in HTTP-Anforderungen an die Ressource verwendet werden – es wird der Ressource zuverlässig anzeigen, dass Ihre Anwendung über die erforderliche Berechtigung zum Ausführen einer bestimmten Aufgabe verfügt.  

Weitere Informationen zum OAuth 2.0-Protokoll und zur Erfassung von Zugriffstoken finden Sie in der [Protokollreferenz zum v2.0-Endpunkt](active-directory-v2-protocols.md).

<!--HONumber=Oct16_HO2-->


