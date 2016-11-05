
---
title: Azure AD v2.0-OAuth-Clientanmeldeinformations-Flow | Microsoft Docs
description: Erstellen von Webanwendungen mit der Azure AD-Implementierung des OAuth 2.0-Authentifizierungsprotokolls.
services: active-directory
documentationcenter: ''
author: dstrockis
manager: msmbaldwin
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: dastrock

---
# <a name="v2.0-protocols---oauth-2.0-client-credentials-flow"></a>v2.0-Protokolle – OAuth 2.0-Clientanmeldeinformations-Flow
Die [Gewährung von OAuth 2.0-Clientanmeldeinformationen](http://tools.ietf.org/html/rfc6749#section-4.4), gelegentlich als „zweibeinige OAuth“ bezeichnet, kann für den Zugriff auf webgehostete Ressourcen über die Identität einer Anwendung verwendet werden.  Sie wird häufig für Interaktionen zwischen Servern verwendet, die ohne Endbenutzereingriff im Hintergrund ausgeführt werden müssen.  Diese Anwendungstypen werden oft als **Daemons** oder **Dienstkonten** bezeichnet.

> [!NOTE]
> Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt.  Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
> 
> 

In der üblicheren „dreibeinigen OAuth“ wird einer Clientanwendung die Berechtigung zum Zugriff auf eine Ressource im Auftrag eines bestimmten Benutzers gewährt.  Die Berechtigung wird in der Regel während des **Zustimmungsprozesses** vom Benutzer an die Anwendung [delegiert](active-directory-v2-scopes.md) .  Allerdings werden Berechtigungen im Clientanmeldeinformations-Flow **direkt** an die Anwendung selbst erteilt.  Wenn die App einer Ressource ein Token anbietet, erzwingt die Ressource, dass die App selbst und nicht irgendein Benutzer für die Durchführung einer Aktion autorisiert ist.

## <a name="protocol-diagram"></a>Protokolldiagramm
Der vollständige Clientanmeldeinformations-Flow sieht in etwa wie folgt aus. Die einzelnen Schritte werden unten im Detail beschrieben.

![Clientanmeldeinformations-Flow](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Erhalten der direkten Autorisierung
Für eine App gibt es in der Regel zwei Möglichkeiten, eine direkte Autorisierung für den Zugriff auf eine Ressource zu erhalten: über eine Zugriffssteuerungsliste in der Ressource oder über die Zuweisung einer Anwendungsberechtigung in Azure AD.  Es gibt mehrere weitere Möglichkeiten, mit deren Hilfe eine Ressource ihre Clients autorisieren kann, und jeder Ressourcenserver kann die Methode wählen, die für seine Anwendung am sinnvollsten ist.  Diese beiden Methoden sind die häufigsten in Azure AD und werden für Clients und Ressourcen empfohlen, die den Clientanmeldeinformations-Flow ausführen sollen.

### <a name="access-control-lists"></a>Zugriffssteuerungslisten
Ein bestimmter Ressourcenanbieter erzwingt möglicherweise eine Autorisierungsprüfung basierend auf einer Liste von Anwendungs-IDs, die ihm bekannt sind, und erteilt eine bestimmte Zugriffsebene.  Erhält die Ressource ein Token vom v2.0-Endpunkt, kann sie das Token decodieren und die Anwendungs-ID des Clients aus den Ansprüchen `appid` und `iss` extrahieren.  Anschließend kann sie die Anwendung mit einer von ihr verwalteten Zugriffssteuerungsliste (ACL) vergleichen.  Die Granularität und die Methode der Zugriffssteuerungsliste können von Ressource zu Ressource erheblich variieren.

Ein häufiges Anwendungsbeispiel für solche ACLs sind Test Runners für eine Webanwendung oder Web-API.  Die Web-API kann ihren verschiedenen Clients nur eine Teilmenge der vollständigen Berechtigungen gewähren.  Zum Ausführen von End-to-End-Tests auf der API wird jedoch ein Testclient erstellt, der Token vom v2.0-Endpunkt erhält und diese an die API sendet.  Die API kann anschließend die Anwendungs-ID des Testclients anhand der ACL überprüfen, um Vollzugriff auf die gesamte Funktionalität der API zu gewähren.  Wenn Sie für Ihren Dienst eine solche Liste verwenden, sollten Sie unbedingt beachten, dass nicht nur die `appid` des Aufrufers überprüft werden muss, sondern auch, ob der `iss` des Tokens ebenfalls vertrauenswürdig ist.

Diese Art der Autorisierung wird häufig für Daemons und Dienstkonten eingesetzt, die auf Daten zugreifen müssen, die sich im Besitz von Privatnutzern mit persönlichen Microsoft-Konten befinden.  Für Daten, die Organisationen gehören, empfiehlt es sich, die erforderliche Autorisierung über Anwendungsberechtigungen zu erhalten.

### <a name="application-permissions"></a>Anwendungsberechtigungen
Anstelle der Verwendung von ACLs können APIs einen Satz von **Anwendungsberechtigungen** verfügbar machen, die einer Anwendung erteilt werden können.  Eine Anwendungsberechtigung wird einer Anwendung von einem Administrator einer Organisation erteilt und kann nur für den Zugriff auf Daten verwendet werden, die sich im Besitz der jeweiligen Organisation und deren Mitarbeiter befinden.  Microsoft Graph macht beispielsweise verschiedene Anwendungsberechtigungen verfügbar:

* Lesen von E-Mail in allen Postfächern
* Lesen und Schreiben von E-Mail in allen Postfächern
* Senden von E-Mail als beliebiger Benutzer
* Lesen von Verzeichnisdaten
* [und mehr](https://graph.microsoft.io)

Um diese Berechtigungen in Ihrer App zu erhalten, können Sie die folgenden Schritte ausführen.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Anfordern der Berechtigungen im App-Registrierungsportal
* Navigieren Sie zu Ihrer Anwendung unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com), oder [erstellen Sie eine App](active-directory-v2-app-registration.md), falls noch nicht geschehen.  Sie müssen sicherstellen, dass Ihre Anwendung mindestens einen geheimen Anwendungsschlüssel erstellt hat.
* Suchen Sie den Abschnitt **Direkte Anwendungsberechtigungen** , und fügen Sie die Berechtigungen hinzu, die von Ihrer App benötigt werden.
* Stellen Sie sicher, dass Sie die App-Registrierung **speichern** .

#### <a name="recommended:-sign-the-user-into-your-app"></a>Empfohlen: Anmelden des Benutzers bei Ihrer App
Beim Erstellen einer Anwendung, die Anwendungsberechtigungen verwendet, benötigt die App in der Regel eine Seite/Ansicht, die dem Administrator das Genehmigen der App-Berechtigungen gestattet.  Diese Seite kann Teil des Anmelde-Flows der App, Teil der App-Einstellungen oder ein dedizierter Flow „Verbinden“ sein.  In vielen Fällen ist es sinnvoll, wenn die App diese Ansicht „Verbinden“ erst anzeigt, wenn ein Benutzer sich mit einem Geschäfts-, Schul- oder Unikonto von Microsoft angemeldet hat.

Durch das Anmelden des Benutzers bei der App können Sie die Organisation identifizieren, der der Benutzer angehört, bevor Sie zur Genehmigung der Anwendungsberechtigungen auffordern.  Auch wenn es nicht unbedingt erforderlich ist, können Sie für Ihre Organisationsbenutzer auf diese Weise eine intuitivere Benutzeroberfläche erstellen.  Zum Anmelden des Benutzers befolgen Sie unsere [Tutorials zum v2.0-Protokoll](active-directory-v2-protocols.md).

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
| Mandant |erforderlich |Der Verzeichnismandant, von dem Sie die Berechtigung anfordern möchten.  Kann als GUID oder als Anzeigename bereitgestellt werden.  Wenn Sie nicht wissen, zu welchem Mandanten der Benutzer gehört, und wenn der Benutzer sich bei jedem Mandanten anmelden können soll, verwenden Sie `common`. |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com)) Ihrer Anwendung zugewiesen hat. |
| redirect_uri |erforderlich |Der redirect_uri, an den die Antwort zur Verarbeitung durch die App gesendet werden soll.  Er muss genau einem der redirect_uris entsprechen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss und zusätzliche Pfadsegmente aufweisen kann. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird.  Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln.  Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |

An diesem Punkt erzwingt Azure AD, dass sich nur ein Mandantenadministrator anmelden kann, um die Anforderung abzuschließen.  Der Administrator wird aufgefordert, alle direkten Anwendungsberechtigungen zu genehmigen, die Sie für Ihre App im Registrierungsportal angefordert haben. 

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

Nachdem Sie eine erfolgreiche Antwort vom App-Bereitstellungsendpunkt erhalten haben, erhält Ihre App die direkten Anwendungsberechtigungen, die sie angefordert hat.  Sie können jetzt ein Token für die gewünschte Ressource anfordern.

## <a name="get-a-token"></a>Abrufen von Token
Sobald Sie die notwendige Autorisierung für Ihre Anwendung erhalten haben, können Sie mit dem Abrufen von Zugriffstoken für APIs fortfahren.  Um ein Token über die Gewährung von Clientanmeldeinformationen zu erhalten, senden Sie eine POST-Anforderung an den v2.0-Endpunkt `/token` :

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| client_id |erforderlich |Die Anwendungs-ID, die das Registrierungsportal ([apps.dev.microsoft.com](https://apps.dev.microsoft.com)) Ihrer Anwendung zugewiesen hat. |
| Bereich |erforderlich |Der Wert für den `scope`-Parameter in dieser Anforderung muss dem Ressourcenbezeichner (App-ID-URI) der gewünschten Ressource entsprechen, versehen mit dem `.default`-Suffix.  Für das angegebene Microsoft Graph-Beispiel muss der Wert daher `https://graph.microsoft.com/.default`lauten.  Dieser Wert informiert den v2.0-Endpunkt darüber, dass er von allen direkten Anwendungsberechtigungen, die Sie für Ihre App konfiguriert haben, ein Token für diejenigen ausstellen soll, die zur gewünschten Ressource gehören. |
| client_secret |erforderlich |Der geheime Anwendungsschlüssel, den Sie im Registrierungsportal für Ihre App erstellt haben. |
| grant_type |erforderlich |Muss `client_credentials`lauten. |

#### <a name="successful-response"></a>Erfolgreiche Antwort
Eine erfolgreiche Antwort weist folgendes Format auf:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung bei geschützten Ressourcen verwenden, wie z.B. eine Web-API. |
| token_type |Gibt den Wert des Tokentyps an. Der einzige Typ, der von Azure AD unterstützt wird, ist `Bearer`. |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |

#### <a name="error-response"></a>Fehlerantwort
Eine Fehlerantwort weist folgendes Format auf:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschreibung |
| --- | --- |
| Fehler |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error_codes |Eine Liste der spezifischen STS-Fehlercodes, die bei der Diagnose helfen können |
| timestamp |Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann |

## <a name="use-a-token"></a>Verwenden eines Tokens
Da Sie jetzt ein Token abgerufen haben, können Sie dieses Token verwenden, um Anforderungen an die Ressource zu stellen.  Wenn das Token abläuft, wiederholen Sie einfach die Anforderung an den `/token` -Endpunkt, um ein neues Zugriffstoken abzurufen.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Codebeispiel
Ein Beispiel für eine Anwendung, die die client_credentials-Gewährung über den Endpunkt für die Administratorzustimmung implementiert, finden Sie in unserem [v2.0-Daemon-Codebeispiel](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).

<!--HONumber=Oct16_HO2-->


