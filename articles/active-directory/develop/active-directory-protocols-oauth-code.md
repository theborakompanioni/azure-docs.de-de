---
title: Grundlegendes zum Codefluss der OAuth 2.0-Autorisierung in Azure AD | Microsoft-Dokumentation
description: Dieser Artikel beschreibt, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mithilfe von Azure Active Directory und OAuth 2.0 verwenden.
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: de3412cb-5fde-4eca-903a-4e9c74db68f2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: dastrock
ms.custom: aaddev
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 35132eae4d6a7f85b19a7a49ad4034e795d7df13
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# Autorisieren des Zugriffs auf Webanwendungen mit OAuth 2.0 und Azure Active Directory
Azure Active Directory (Azure AD) verwendet OAuth 2.0, um den Zugriff auf Webanwendungen und Web-APIs in Ihrem Azure AD-Mandanten zu autorisieren. Diese sprachunabhängige Anleitung beschreibt das Senden und Empfangen von HTTP-Nachrichten ohne Verwendung unserer Open Source-Bibliotheken.

Der OAuth 2.0-Autorisierungscodefluss wird in [Abschnitt 4.1 der OAuth 2.0-Spezifikation](https://tools.ietf.org/html/rfc6749#section-4.1)beschrieben. Er wird zur Authentifizierung und Autorisierung bei den meisten Anwendungstypen verwendet, einschließlich Web-Apps und nativ installierten Apps.

[!INCLUDE [active-directory-protocols-getting-started](../../../includes/active-directory-protocols-getting-started.md)]

## OAuth 2.0-Autorisierungsfluss
Allgemein sieht der gesamte Autorisierungsfluss für eine Anwendung etwa wie folgt aus:

![OAuth-Autorisierungscodefluss](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)

## Anfordern eines Autorisierungscodes
Der Autorisierungscodefluss beginnt damit, dass der Client den Benutzer auf den `/authorize` -Endpunkt leitet. In dieser Anforderung gibt der Client die Berechtigungen an, die er vom Benutzer abrufen muss. Sie können die OAuth 2.0-Endpunkte von der Seite Ihrer Anwendung im klassischen Azure-Portal beziehen (unten über die Schaltfläche **Endpunkte anzeigen** ).

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| tenant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Die zulässigen Werte sind Mandantenbezeichner (also etwa `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder für mandantenunabhängige Token `common`). |
| client_id |erforderlich |Die Anwendungs-Id, die Ihrer App zugewiesen wird, wenn Sie sie mit Azure AD registrieren. Diese finden Sie im Azure-Portal. Klicken Sie auf **Active Directory**. Klicken Sie anschließend auf das Verzeichnis, wählen Sie die Anwendung aus, und klicken Sie dann auf **Konfigurieren**. |
| response_type |erforderlich |Muss `code` für den Autorisierungscodefluss enthalten. |
| redirect_uri |empfohlen |Der Umleitungs-URI der App, in dem Authentifizierungsantworten gesendet und von der App empfangen werden können.  Er muss genau mit einer der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben, mit dem Unterschied, dass er URL-codiert sein muss.  Für native und mobile Apps sollten Sie den Standardwert `urn:ietf:wg:oauth:2.0:oob` verwenden. |
| response_mode |empfohlen |Gibt die Methode an, die zum Senden des resultierenden Tokens zurück an Ihre App verwendet werden soll.  Kann `query` oder `form_post` sein. |
| state |empfohlen |Ein in der Anforderung enthaltener Wert, der ebenfalls in der Tokenantwort zurückgegeben wird. Ein zufällig generierter eindeutiger Wert wird normalerweise verwendet, um [websiteübergreifende Anforderungsfälschungsangriffe zu verhindern](http://tools.ietf.org/html/rfc6749#section-10.12).  Der Status wird auch verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z. B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
| resource |optional |Der App-ID-URI der Web-API (geschützte Ressource). Klicken Sie zum Ermitteln des App-ID-URI der Web-API im Azure-Portal nacheinander auf **Active Directory**, das Verzeichnis, die Anwendung und dann auf **Konfigurieren**. |
| prompt |optional |Geben Sie den Typ der erforderlichen Benutzerinteraktion an.<p> Gültige Werte sind: <p> *login*: Der Benutzer sollte zum erneuten Authentifizieren aufgefordert werden. <p> *consent*: Die Benutzerzustimmung wurde erteilt, muss aber aktualisiert werden. Der Benutzer sollte zur Erteilung der Zustimmung aufgefordert werden. <p> *admin_consent*: Ein Administrator sollte aufgefordert werden, die Zustimmung im Namen aller Benutzer der Organisation zu erteilen. |
| login_hint |optional |Dieser Wert kann verwendet werden, um das Feld für den Benutzernamen oder die E-Mail-Adresse auf der Anmeldeseite vorab für den Benutzer auszufüllen, wenn dessen Benutzername im Vorfeld bekannt ist.  Apps verwenden diesen Parameter häufig für die erneute Authentifizierung, nachdem sie den Benutzernamen aus einer vorherigen Anmeldung mithilfe des Anspruchs `preferred_username` extrahiert haben. |
| domain_hint |optional |Stellt einen Hinweis zum Mandanten oder zur Domäne bereit, die der Benutzer zum Anmelden verwenden sollte. Der Wert von „domain_hint“ ist eine registrierte Domäne für den Mandanten. Wenn der Mandant mit einem lokalen Verzeichnis verbunden ist, führt AAD eine Umleitung an den angegebenen Mandantenverbundserver durch. |

> [!NOTE]
> Wenn der Benutzer Teil einer Organisation ist, kann ein Administrator der Organisation im Auftrag des Benutzers zustimmen oder ablehnen oder dem Benutzer die Zustimmung erlauben. Der Benutzer erhält die Option zur Zustimmung nur dann, wenn der Administrator dies zulässt.
>
>

Nun wird der Benutzer zur Eingabe der Anmeldeinformationen und zur Zustimmung zu den im Abfrageparameter `scope` angegebenen Berechtigungen aufgefordert. Nach der Authentifizierung und der Zustimmung durch den Benutzer versendet Azure AD eine Antwort an Ihre App unter der `redirect_uri` -Adresse in Ihrer Anforderung.

### Erfolgreiche Antwort
Eine erfolgreiche Antwort sieht wie folgt aus:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parameter | Beschreibung |
| --- | --- |
| admin_consent |Der Wert ist „true“, wenn ein Administrator einer Aufforderung zu einer Zustimmungsanforderung zugestimmt hat. |
| code |Der Autorisierungscode, den die App angefordert hat. Die Anwendung kann den Autorisierungscode zum Anfordern eines Zugriffstokens für die Zielressource verwenden. |
| session_state |Ein eindeutiger Wert, der die aktuelle Benutzersitzung identifiziert. Dieser Wert ist eine GUID, sollte jedoch als opaker Wert behandelt werden, der ohne Prüfung übergeben wird. |
| state |Wenn ein Statusparameter in der Anforderung enthalten ist, sollte der gleiche Wert in der Antwort angezeigt werden. Es hat sich bewährt, wenn die Anwendung überprüft, ob die Statuswerte in der Anforderung und in der Antwort identisch sind, bevor die Antwort verwendet wird. Dies trägt zur Erkennung von [Angriffen vom Typ „websiteübergreifende Anforderungsfälschung“](https://tools.ietf.org/html/rfc6749#section-10.12) auf den Client bei. |

### Fehlerantwort
Fehlerantworten können auch an den `redirect_uri` gesendet werden, damit die App diese entsprechend behandeln kann.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameter | Beschreibung |
| --- | --- |
| error |Ein in Abschnitt 5.2 definierter Fehlercodewert des [OAuth 2.0-Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749). In der folgenden Tabelle finden Sie die Fehlercodes, die von Azure AD ausgegeben werden. |
| error_description |Detailliertere Beschreibung des Fehlers. Diese Meldung ist nicht für den Endbenutzer ausgelegt. |
| state |Der Statuswert ist ein zufällig generierter, nicht wiederverwendeter Wert, der in der Anforderung versendet und in der Antwort zurückgegeben wird, um webseitenübergreifende Anforderungsfälschungen (CSFR) zu vermeiden. |

#### Fehlercodes beim Autorisierungsendpunktfehler
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

## Fordern Sie ein Zugriffstoken mithilfe des Autorisierungscodes an.
Wenn Sie einen Autorisierungscode erworben und die Berechtigung vom Benutzer erhalten haben, können Sie den Code für ein Zugriffstoken auf die gewünschte Ressource einlösen, indem Sie eine POST-Anforderung an den `/token` -Endpunkt senden:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| tenant |erforderlich |Mit dem `{tenant}` -Wert im Pfad der Anforderung kann festgelegt werden, welche Benutzer sich bei der Anwendung anmelden können.  Die zulässigen Werte sind Mandantenbezeichner (also etwa `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`, `contoso.onmicrosoft.com` oder für mandantenunabhängige Token `common`). |
| client_id |erforderlich |Die Anwendungs-Id, die Ihrer App zugewiesen wird, wenn Sie sie mit Azure AD registrieren. Diese finden Sie im klassischen Azure-Portal. Klicken Sie auf **Active Directory**. Klicken Sie anschließend auf das Verzeichnis, wählen Sie die Anwendung aus, und klicken Sie dann auf **Konfigurieren**. |
| grant_type |erforderlich |Muss der `authorization_code` für den Autorisierungscodefluss sein. |
| code |erforderlich |Der `authorization_code` , den Sie im vorherigen Abschnitt abgerufen haben. |
| redirect_uri |erforderlich |Der `redirect_uri`-Wert, den Sie zum Abrufen von `authorization_code` verwendet haben. |
| client_secret |erforderlich für Web-Apps |Der geheime App-Schlüssel, den Sie im App-Registrierungsportal für Ihre App erstellt haben.  Er sollte nicht in einer systemeigenen App verwendet werden, da geheime Client-Schlüssel nicht zuverlässig auf Geräten gespeichert werden können.  Er ist für Web-Apps und Web-APIs erforderlich, die die Möglichkeit haben, `client_secret` sicher auf dem Server zu speichern. |
| Ressource |Erforderlich bei Angabe in der Autorisierungscodeanforderung, ansonsten optional |Der App-ID-URI der Web-API (geschützte Ressource). |

Klicken Sie zum Ermitteln des App-ID-URI im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis, die Anwendung und dann auf **Konfigurieren**.

### Erfolgreiche Antwort
Azure AD gibt bei einer erfolgreichen Antwort ein Zugriffstoken zurück. Um die Anzahl von Netzwerkaufrufen der Clientanwendung und die damit verbundene Latenz zu verringern, sollte die Clientanwendung Zugriffstoken für die Tokenlebensdauer zwischenspeichern, die in der OAuth 2.0-Antwort angegeben ist. Verwenden Sie zum Bestimmen der Tokenlebensdauer entweder den Parameterwert `expires_in` oder `expires_on`.

Wenn eine Web-API-Ressource den Fehlercode `invalid_token` zurückgibt, kann dies darauf hinweisen, dass von der Ressource ein abgelaufenes Token ermittelt wurde. Falls sich die Zeiten der Client- und Ressourcenuhr unterscheiden (als „zeitlicher Versatz“ bezeichnet), wird das Token von der Ressource ggf. als abgelaufen angesehen, bevor das Token aus dem Clientcache entfernt wird. Löschen Sie das Token in diesem Fall auch dann aus dem Cache, wenn der berechnete Lebensdauerzeitraum noch nicht abgelaufen ist.

Eine erfolgreiche Antwort sieht wie folgt aus:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
  "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ."
}

```

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Die App kann dieses Token zur Authentifizierung auf geschützten Ressourcen verwenden, wie z. B. eine Web-API. |
| token_type |Gibt den Wert des Tokentyps an. Bearertoken ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth2.0 Authorization Framework: Bearer Token Usage (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt) |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| expires_on |Die Uhrzeit, zu der das Zugriffstoken abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. Dieser Wert wird verwendet, um die Lebensdauer von zwischengespeicherten Token zu bestimmen. |
| resource |Der App-ID-URI der Web-API (geschützte Ressource). |
| scope |Die Identitätswechselberechtigungen, die der Clientanwendung gewährt wurden. Die Standardberechtigung ist `user_impersonation`. Der Besitzer der gesicherten Ressource kann zusätzliche Werte in Azure AD registrieren. |
| refresh_token |Ein Aktualisierungstoken von OAuth 2.0. Die App kann dieses Token verwenden, um nach Ablauf der aktuellen Zugriffstoken zusätzliche Zugriffstoken zu erhalten.  Aktualisierungstoken sind langlebig und können verwendet werden, um den Zugriff auf Ressourcen für längere Zeit beizubehalten. |
| id_token |Ein unsigniertes JSON-Webtoken (JWT). Die App kann die Segmente dieses Tokens mit einer base64-URL decodieren, um Informationen über den angemeldeten Benutzer abzurufen. Die App kann die Werte zwischenspeichern und sie anzeigen, sollte sich jedoch nicht für Autorisierungs- und Sicherheitsgrenzen auf sie verlassen. |

### JWT-Tokenansprüche
Das JWT-Token im Wert des Parameters `id_token` kann in die folgenden Ansprüche decodiert werden:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

Weitere Informationen zu JSON-Webtoken finden Sie im [JWT IETF-Spezifikationsentwurf](http://go.microsoft.com/fwlink/?LinkId=392344). Weitere Informationen zu den Tokentypen und Ansprüchen finden Sie unter [Unterstützte Token- und Anspruchstypen](active-directory-token-and-claims.md).

Der Parameter `id_token` umfasst die folgenden Anspruchstypen:

| Anspruchstyp | Beschreibung |
| --- | --- |
| aud |Zielgruppe des Tokens. Wenn das Token für eine Clientanwendung ausgestellt wird, ist die Zielgruppe die `client_id` des Clients. |
| exp |Ablaufzeit. Die Uhrzeit, zu der das Token abläuft. Damit das Token gültig ist, darf das aktuelle Datum/die aktuelle Uhrzeit nicht größer sein als der Wert `exp`. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| family_name |Nachname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| given_name |Vorname des Benutzers. Die Anwendung kann diesen Wert anzeigen. |
| iat |„Issued at time“ (Ausgestellt um). Der Zeitpunkt, zu dem das JWT ausgestellt wurde. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| iss |Identifiziert den Herausgeber des Tokens. |
| nbf |„Not before time“ (Nicht vor). Der Zeitpunkt, ab dem das Token gültig ist. Damit das Token gültig ist, darf das aktuelle Datum/die aktuelle Uhrzeit nicht kleiner sein als der Wert „Nbf“. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| oid |Objektbezeichner (ID) des Benutzerobjekts in Azure AD. |
| sub |Der Token-Antragstellerbezeichner. Dies ist ein dauerhafter und unveränderlicher Bezeichner für den Benutzer, den das Token beschreibt. Verwenden Sie diesen Wert in der Logik für das Zwischenspeichern. |
| tid |Mandantenbezeichner (ID) des Azure AD-Mandanten, der das Token ausgestellt hat. |
| unique_name |Ein eindeutiger Bezeichner, der dem Benutzer angezeigt werden kann. Dies ist in der Regel ein Benutzerprinzipalname (UPN). |
| upn |Benutzerprinzipalname. |
| ver |Version. Die Version des JWT-Tokens, in der Regel 1.0. |

### Fehlerantwort
Die Fehler am Tokenausstellungs-Endpunkt sind HTTP-Fehlercodes, da der Client den Tokenausstellungs-Endpunkt direkt aufruft. Zusätzlich zum HTTP-Statuscode gibt der Azure AD-Tokenausstellungs-Endpunkt auch ein JSON-Dokument mit Objekten zurück, die den Fehler beschreiben.

Eine Beispiel für eine Fehlerantwort sieht wie folgt aus:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parameter | Beschreibung |
| --- | --- |
| error |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error_codes |Eine Liste der STS-spezifischen Fehlercodes, die bei der Diagnose helfen können. |
| timestamp |Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann |

#### HTTP-Statuscodes
Die folgende Tabelle enthält die HTTP-Statuscodes, die vom Tokenausstellungs-Endpunkt zurückgegeben werden. In einigen Fällen ist der Fehlercode ausreichend, um die Antwort zu beschreiben. Wenn jedoch Fehler auftreten, müssen Sie die zugehörigen JSON-Dokumente analysieren und den Fehlercode überprüfen.

| HTTP-Code | Beschreibung |
| --- | --- |
| 400 |Standard-HTTP-Code. Wird in den meisten Fällen verwendet und wird in der Regel aufgrund einer fehlerhaften Anforderung ausgegeben. Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| 401 |Fehler bei der Authentifizierung. Tritt beispielsweise auf, wenn die Anforderung den Parameter „client_secret“ nicht enthält. |
| 403 |Fehler bei der Autorisierung. Der Benutzer verfügt beispielsweise nicht über die Berechtigung zum Zugriff auf die Ressource. |
| 500 |Ein interner Fehler ist beim Dienst aufgetreten. Wiederholen Sie die Anforderung. |

#### Fehlercodes für Token-Endpunktfehler
| Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- |
| invalid_request |Protokollfehler, z.B. ein fehlender erforderlicher Parameter. |Korrigieren Sie die Anforderung, und senden Sie sie erneut. |
| invalid_grant |Der Autorisierungscode ist ungültig oder abgelaufen. |Versuchen Sie, eine neue Anforderung an den `/authorize`-Endpunkt zu senden. |
| unauthorized_client |Der authentifizierte Client ist zur Verwendung dieses Autorisierungsgewährungstyps nicht autorisiert. |Dies tritt in der Regel auf, wenn die Clientanwendung nicht in Azure AD registriert ist oder dem Azure AD-Mandanten des Benutzers nicht hinzugefügt wird. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| invalid_client |Clientauthentifizierung fehlgeschlagen. |Die Client-Anmeldeinformationen sind nicht gültig. Um das Problem zu beheben, aktualisiert der Anwendungsadministrator die Anmeldeinformationen. |
| unsupported_grant_type |Der Autorisierungsserver unterstützt den Autorisierungsgewährungstyp nicht. |Ändern Sie den Gewährungstyp in der Anforderung. Diese Art von Fehler sollte nur während der Entwicklung auftreten und bei den ersten Tests erkannt werden. |
| invalid_resource |Die Zielressource ist ungültig, da sie nicht vorhanden ist, Azure AD sie nicht findet oder sie nicht ordnungsgemäß konfiguriert ist. |Dies gibt an, dass die Ressource, falls vorhanden, im Mandanten nicht konfiguriert wurde. Die Anwendung kann den Benutzer zum Installieren der Anwendung und zum Hinzufügen zu Azure AD auffordern. |
| interaction_required |Die Anforderung erfordert eine Benutzerinteraktion. Beispielsweise ist ein zusätzlicher Schritt zur Authentifizierung erforderlich. | Wiederholen Sie die Anforderung derselben Ressource mit einer interaktiven Autorisierungsanforderung anstelle einer nicht interaktiven Anforderung. |
| temporarily_unavailable |Der Server ist vorübergehend überlastet und kann die Anforderung nicht verarbeiten. |Wiederholen Sie die Anforderung. Die Clientanwendung kann dem Benutzer erklären, dass ihre Antwort aufgrund einer temporären Bedingung verzögert ist. |

## Verwenden des Zugriffstokens für den Zugriff auf die Ressource
Nachdem Sie erfolgreich ein `access_token` abgerufen haben, können Sie das Token für Anforderungen an Web-APIs verwenden, indem Sie es in den `Authorization`-Header einschließen: Die Spezifikation [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) erklärt, wie Bearertoken in HTTP-Anforderungen für den Zugriff auf geschützte Ressourcen verwendet werden.

### Beispiel für eine Anforderung
```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### Fehlerantwort
Gesicherte Ressourcen, die RFC 6750 implementieren, geben HTTP-Statuscodes zurück. Wenn die Anforderung keine Authentifizierungsinformationen enthält oder wenn das Token fehlt, enthält die Antwort einen `WWW-Authenticate` -Header. Wenn eine Anforderung fehlschlägt, antwortet der Ressourcenserver mit einem HTTP-Statuscode und einem Fehlercode.

Im Folgenden finden Sie ein Beispiel für eine nicht erfolgreiche Antwort, wenn die Clientanforderung das Bearertoken nicht enthält:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.microsoftonline.com/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### Fehlerparameter
| Parameter | Beschreibung |
| --- | --- |
| authorization_uri |Der URI (physische Endpunkt) des Autorisierungsservers. Dieser Wert wird auch als Suchschlüssel verwendet, um weitere Informationen über den Server aus einem Discovery-Endpunkt zu erhalten. <p><p> Der Client muss überprüfen, ob der Autorisierungsserver vertrauenswürdig ist. Wenn die Ressource von Azure AD geschützt wird, ist die Prüfung ausreichend, ob die URL mit https://login.microsoftonline.com oder einem anderen Hostnamen beginnt, den Azure AD unterstützt. Eine mandantenspezifische Ressource sollte immer einen mandantenspezifischen Autorisierungs-URI zurückgeben. |
| error |Ein in Abschnitt 5.2 definierter Fehlercodewert des [OAuth 2.0-Autorisierungsframeworks](http://tools.ietf.org/html/rfc6749). |
| error_description |Detailliertere Beschreibung des Fehlers. Diese Meldung ist nicht für den Endbenutzer ausgelegt. |
| resource_id |Gibt den eindeutigen Bezeichner der Ressource zurück. Die Clientanwendung kann diesen Bezeichner als Wert für den `resource` -Parameter verwenden, wenn sie ein Token für die Ressource anfordert. <p><p> Es ist wichtig, dass die Clientanwendung diesen Wert überprüft, da andernfalls ein schädlicher Dienst möglicherweise einen Angriff mit einer **Erhöhung von Rechten** durchführt. <p><p> Die empfohlene Strategie zur Verhinderung eines Angriffs besteht darin sicherzustellen, dass die `resource_id` mit dem Basiselement der Web-API-URL übereinstimmt, auf die zugegriffen wird. Wenn beispielsweise auf „https://service.contoso.com/data“ zugegriffen wird, kann die `resource_id` „htttps://service.contoso.com/“ lauten. Die Clientanwendung muss eine `resource_id` ablehnen, die nicht mit der Basis-URL beginnt, sofern es kein zuverlässiges alternatives Verfahren zum Überprüfen der ID gibt. |

#### Bearerschema-Fehlercodes
Die Spezifikation RFC 6750 definiert die folgenden Fehler für Ressourcen, die in der Antwort den WWW-Authenticate-Header und das Bearer-Schema verwenden.

| HTTP-Statuscode | Fehlercode | Beschreibung | Clientaktion |
| --- | --- | --- | --- |
| 400 |invalid_request |Die Anforderung ist nicht richtig formatiert. Möglicherweise fehlt ein Parameter, oder der gleiche Parameter wird zweimal verwendet. |Beheben Sie den Fehler, und wiederholen Sie die Anforderung. Diese Art von Fehler sollte nur während der Entwicklung auftreten und bei den ersten Tests erkannt werden. |
| 401 |invalid_token |Das Zugriffstoken ist nicht vorhanden, ungültig oder wurde widerrufen. Der Wert des Parameters „error_description“ enthält weitere Details. |Fordern Sie ein neues Token vom Autorisierungsserver an. Wenn das neue Token fehlschlägt, ist ein unerwarteter Fehler aufgetreten. Sendet eine Fehlermeldung an den Benutzer. Eine Wiederholung erfolgt nach einer beliebigen Zeitspanne. |
| 403 |insufficient_scope |Das Zugriffstoken enthält nicht die erforderlichen Berechtigungen zum Identitätswechsel, um auf die Ressource zuzugreifen. |Senden Sie eine neue Autorisierungsanforderung an den Autorisierungsendpunkt. Wenn die Antwort den Bereichsparameter enthält, verwenden Sie den Bereichswert in der Anforderung für die Ressource. |
| 403 |insufficient_access |Der Antragsteller des Tokens besitzt nicht die Berechtigungen, die für den Zugriff auf die Ressource erforderlich sind. |Der Benutzer wird aufgefordert, ein anderes Konto zu verwenden oder Berechtigungen für die angegebene Ressource anzufordern. |

## Aktualisieren der Zugriffstoken
Zugriffstoken sind kurzlebig und müssen nach Ablauf aktualisiert werden, damit Sie weiterhin auf Ressourcen zugreifen können. Übermitteln Sie zum Aktualisieren von `access_token` eine weitere `POST`-Anforderung an den `/token`-Endpunkt, und zwar dieses Mal unter Angabe des `refresh_token` anstelle von `code`.

Für Aktualisierungstoken werden keine Lebensdauern angegeben. Normalerweise verfügen Aktualisierungstoken über relativ lange Lebensdauern. In einigen Fällen laufen Aktualisierungstoken aber ab, werden widerrufen oder verfügen nicht über ausreichende Berechtigungen für die gewünschte Aktion. Von Ihrer Anwendung müssen Fehler, die vom Tokenausstellungs-Endpunkt zurückgegeben werden, erwartet und richtig behandelt werden.

Wenn Sie eine Antwort mit einem Aktualisierungstokenfehler erhalten, sollten Sie das aktuelle Aktualisierungstoken verwerfen und einen neuen Autorisierungscode oder ein neues Zugriffstoken anfordern. Wenn Sie ein Aktualisierungstoken im Ablauf der Autorisierungscodeerteilung verwenden und eine Antwort mit dem Fehlercode `interaction_required` oder `invalid_grant` erhalten, sollten Sie besonders darauf achten, dass Sie das Aktualisierungstoken verwerfen und einen neuen Autorisierungscode anfordern.

Ein Beispiel für eine Anforderung an den **mandantenspezifischen** Endpunkt (Sie können auch den **allgemeinen** Endpunkt verwenden), um ein neues Zugriffstoken mithilfe eines Aktualisierungstokens abzurufen, sieht wie folgt aus:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

### Erfolgreiche Antwort
Eine erfolgreiche Tokenantwort sieht wie folgt aus:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```
| Parameter | Beschreibung |
| --- | --- |
| token_type |Der Tokentyp. Der einzige derzeit unterstützte Wert ist **bearer**. |
| expires_in |Die verbleibende Gültigkeitsdauer des Tokens in Sekunden. Ein typischer Wert wäre etwa 3600 (eine Stunde). |
| expires_on |Datum und Uhrzeit, wenn das Token abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. |
| resource |Gibt die gesicherte Ressource an, die das Zugriffstoken für den Zugriff verwenden kann. |
| scope |Die Identitätswechselberechtigungen, die der nativen Clientanwendung gewährt wurden. Die Standardberechtigung lautet **user_impersonation**. Der Besitzer der Zielressource kann alternative Werte in Azure AD registrieren. |
| access_token |Das neue Zugriffstoken, das angefordert wurde. |
| refresh_token |Ein neues OAuth 2.0-Aktualisierungstoken, das zum Anfordern neuer Zugriffstoken verwendet werden kann, wenn das Token in dieser Antwort abläuft. |

### Fehlerantwort
Eine Beispiel für eine Fehlerantwort sieht wie folgt aus:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parameter | Beschreibung |
| --- | --- |
| error |Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren. |
| error_description |Eine spezifische Fehlermeldung, mit der Entwickler die Hauptursache eines Authentifizierungsfehlers identifizieren können. |
| error_codes |Eine Liste der STS-spezifischen Fehlercodes, die bei der Diagnose helfen können. |
| timestamp |Der Zeitpunkt, zu dem der Fehler aufgetreten ist |
| trace_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der Diagnose helfen kann |
| correlation_id |Ein eindeutiger Bezeichner für die Anforderung, die bei der komponentenübergreifenden Diagnose helfen kann |

Eine Beschreibung der Fehlercodes und der jeweils empfohlenen Clientaktion finden Sie unter [Fehlercodes für Token-Endpunktfehler](#error-codes-for-token-endpoint-errors).

