---
title: "Azure AD v2.0 „Im Auftrag von“-OAuth2.0-Ablauf | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie HTTP-Nachrichten zum Implementieren der Dienst-zu-Dienst-Authentifizierung über den Im-Auftrag-von-Ablauf von OAuth2.0 verwenden."
services: active-directory
documentationcenter: 
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: nacanuma
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 356083fbaabfcd2ec7581adf319fa22b810df0d3
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017


---
# Azure Active Directory v2.0- und Im-Auftrag-von-Ablauf von OAuth2.0
Der Im-Auftrag-von-Ablauf von OAuth2.0 wird verwendet, wenn eine Anwendung eine Dienst- oder Web-API aufruft, die wiederum eine andere Dienst- oder Web-API aufrufen muss. Die Idee dabei ist, die delegierte Benutzeridentität und Berechtigungen über die Anforderungskette weiterzugeben. Damit der Dienst auf der mittleren Ebene Authentifizierungsanforderungen an den nachgelagerten Dienst stellen kann, muss für den Benutzer ein Zugriffstoken aus Azure Active Directory (Azure AD) gesichert werden.

> [!NOTE]
> Der v2.0-Endpunkt unterstützt nicht alle Szenarios und Funktionen von Azure Active Directory. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.
>
>

## Protokolldiagramm
Es wird davon ausgegangen, dass der Benutzer in einer Anwendung authentifiziert wurde, die den [Autorisierungscode-Vergabefluss von OAuth 2.0](active-directory-v2-protocols-oauth-code.md) verwendet. In diesem Fall verfügt die Anwendung über ein Zugriffstoken (Token A), das die Benutzeransprüche und die Genehmigung für den Zugriff auf die API der mittleren Ebene enthält (API A). API A muss nun eine authentifizierte Anfrage an die nachgelagerte Web-API stellen (API B).

Die folgenden Schritte entsprechen dem Im-Auftrag-von-Ablauf und werden anhand des folgenden Diagramms erläutert.

![Im-Auftrag-von-Fluss von OAuth2.0](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. Die Clientanwendung stellt mit Token A eine Anforderung an die API A.
2. Die API A wird beim Azure AD-Tokenausstellungs-Endpunkt authentifiziert und fordert ein Token für den Zugriff auf die API B an.
3. Der Azure AD-Tokenausstellungs-Endpunkt überprüft mit Token A die Anmeldeinformationen von API A und stellt das Zugriffstoken für API B aus (Token B).
4. Token B wird im Autorisierungsheader der Anforderung an die API B festgelegt.
5. API B gibt die Daten aus der gesicherten Ressource zurück.

> [!NOTE]
> In diesem Szenario interagiert der Dienst auf der mittleren Ebene nicht mit dem Benutzer, um dessen Zustimmung für den Zugriff auf die nachgelagerte API zu erlangen. Aus diesem Grund muss die Zugriffsgewährung auf die nachgelagerte API zuvor als Teil des Genehmigungsschrittes während der Authentifizierung als Option angezeigt werden.
>

## Dienst-zu-Dienst-Zugriffstokenanforderung
Verwenden Sie zum Anfordern eines Zugriffstokens ein HTTP POST-Element für den mandantenspezifischen Azure AD v2.0-Endpunkt. Verwenden Sie dabei die folgenden Parameter.

```
https://login.microsoftonline.com/<tenant>/oauth2/v2.0/token
```

Es sind zwei Fälle denkbar – je nachdem, ob die Clientanwendung durch ein gemeinsames Geheimnis oder durch ein Zertifikat geschützt wird.

### Erster Fall: Zugriffstokenanforderung mit einem gemeinsamen Geheimnis
Bei Verwendung eines gemeinsamen Geheimnisses enthält eine Dienst-zu-Dienst-Zugriffstokenanforderung die folgenden Parameter:

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich | Typ der Tokenanforderung Bei Anforderungen mit JWT muss der Wert **urn:ietf:params:oauth:grant-type:jwt-bearer** lauten. |
| client_id |erforderlich | Die Anwendungs-ID, die das [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Ihrer App zugewiesen hat. |
| client_secret |erforderlich | Das Anwendungsgeheimnis, das Sie im Anwendungsregistrierungsportal für Ihre App erstellt haben |
| Assertion |erforderlich | Der Wert des bei der Anforderung verwendeten Tokens. |
| Bereich |erforderlich | Eine durch Leerzeichen getrennte Scope-Liste für die Tokenanforderung. Weitere Informationen finden Sie unter [Bereiche](active-directory-v2-scopes.md).|
| requested_token_use |erforderlich | Gibt an, wie die Anforderung verarbeitet werden soll. Im Im-Auftrag-Fluss muss der Wert **on_behalf_of** lauten. |

#### Beispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken mit dem Bereich `user.read` für die Web-API https://graph.microsoft.com angefordert.

```
//line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer
&client_id=2846f71b-a7a4-4987-bab3-760035b2f389
&client_secret=BYyVnAt56JpLwUcyo47XODd
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE0OTM5MjA5MTYsIm5iZiI6MTQ5MzkyMDkxNiwiZXhwIjoxNDkzOTI0ODE2LCJhaW8iOiJBU1FBMi84REFBQUFnZm8vNk9CR0NaaFV2NjJ6MFFYSEZKR0VVYUIwRUlIV3NhcGducndMMnVrPSIsIm5hbWUiOiJOYXZ5YSBDYW51bWFsbGEiLCJvaWQiOiJkNWU5NzljNy0zZDJkLTQyYWYtOGYzMC03MjdkZDRjMmQzODMiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJuYWNhbnVtYUBtaWNyb3NvZnQuY29tIiwic3ViIjoiZ1Q5a1FMN2hXRUpUUGg1OWJlX1l5dVZNRDFOTEdiREJFWFRhbEQzU3FZYyIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInV0aSI6IjN5U3F4UHJweUVPd0ZsTWFFMU1PQUEiLCJ2ZXIiOiIyLjAifQ.TPPJSvpNCSCyUeIiKQoLMixN1-M-Y5U0QxtxVkpepjyoWNG0i49YFAJC6ADdCs5nJXr6f-ozIRuaiPzy29yRUOdSz_8KqG42luCyC1c951HyeDgqUJSz91Ku150D9kP5B9-2R-jgCerD_VVuxXUdkuPFEl3VEADC_1qkGBiIg0AyLLbz7DTMp5DvmbC09DhrQQiouHQGFSk2TPmksqHm3-b3RgeNM1rJmpLThis2ZWBEIPx662pjxL6NJDmV08cPVIcGX4KkFo54Z3rfwiYg4YssiUc4w-w3NJUBQhnzfTl4_Mtq2d7cVlul9uDzras091vFy32tWkrpa970UvdVfQ
&scope=https://graph.microsoft.com/user.read
&requested_token_use=on_behalf_of
```

### Zweiter Fall: Zugriffstokenanforderung mit einem Zertifikat
Eine Dienst-zu-Dienst-Zugriffstokenanforderung mit einem Zertifikat enthält die folgenden Parameter:

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich | Typ der Tokenanforderung Bei Anforderungen mit JWT muss der Wert **urn:ietf:params:oauth:grant-type:jwt-bearer** lauten. |
| client_id |erforderlich | Die Anwendungs-ID, die das [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) Ihrer App zugewiesen hat. |
| client_assertion_type |erforderlich |Der Wert muss `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` sein. |
| client_assertion |erforderlich | Eine Assertion (JSON Web Token), die Sie benötigen, um das Zertifikat, das Sie als Anmeldeinformationen für Ihre Anwendung registriert haben, zu erstellen und sich damit anzumelden.  Informationen zum Registrieren Ihres Zertifikats sowie zum Format der Assertion finden Sie im Abschnitt [Zertifikatanmeldeinformationen](active-directory-certificate-credentials.md).|
| Assertion |erforderlich | Der Wert des bei der Anforderung verwendeten Tokens. |
| requested_token_use |erforderlich | Gibt an, wie die Anforderung verarbeitet werden soll. Im Im-Auftrag-Fluss muss der Wert **on_behalf_of** lauten. |
| Bereich |erforderlich | Eine durch Leerzeichen getrennte Scope-Liste für die Tokenanforderung. Weitere Informationen finden Sie unter [Bereiche](active-directory-v2-scopes.md).|

Beachten Sie, dass die Parameter nahezu identisch mit den Parametern der Anforderung mit dem gemeinsamen geheimen Schlüssel sind. Einziger Unterschied: Anstelle des Parameters „client_secret“ werden die beiden Parameter „client_assertion_type“ und „client_assertion“ verwendet.

#### Beispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken mit dem Bereich `user.read` für die Web-API https://graph.microsoft.com mit einem Zertifikat angefordert.

```
// line breaks for legibility only

POST /oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer
&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=https://graph.microsoft.com/user.read
```

## Dienst-zu-Dienst-Zugriffstokenantwort
Eine erfolgreiche Antwort enthält eine JSON OAuth 2.0-Antwort mit den folgenden Parametern.

| Parameter | Beschreibung |
| --- | --- |
| token_type |Gibt den Wert des Tokentyps an. **Bearertoken**ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth 2.0 Authorization Framework: Bearer Token Usage (RFC 6750) (OAuth 2.0-Autorisierungsframework: Verwendung von Bearertoken (RFC 6750))](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Bereich |Der durch das Token gewährte Zugriffsbereich. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| access_token |Das angeforderte Zugriffstoken. Der aufrufende Dienst kann dieses Token verwenden, um die Authentifizierung für den empfangenden Dienst durchzuführen. |
| refresh_token |Das Aktualisierungstoken für das angeforderte Zugriffstoken. Der aufrufende Dienst kann dieses Token verwenden, um nach Ablauf des aktuellen Zugriffstokens ein neues Zugriffstoken anzufordern. |

### Beispiel für eine erfolgreiche Antwort
Das folgende Beispiel zeigt eine erfolgreiche Antwort auf eine Anforderung für ein Zugriffstoken auf die Web-API https://graph.microsoft.com.

```
{
  "token_type": "Bearer",
  "scope": "https://graph.microsoft.com/user.read",
  "expires_in": 3269,
  "ext_expires_in": 0,
  "access_token": "eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkQ0NDYy0tY0hGa18wZE50MVEtc2loVzRMd2RwQVZISGpnTVdQZ0tQeVJIaGlDbUN2NkdyMEpmYmRfY1RmMUFxU21TcFJkVXVydVJqX3Nqd0JoN211eHlBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMzA1LCJuYmYiOjE0OTM5MzAzMDUsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQU9KYnFFWlRNTnEyZFcxYXpKN1RZMDlYeDdOT29EMkJEUlRWMXJ3b2ZRc1k9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJWR1ItdmtEZlBFQ2M1dWFDaENRSkFBIiwidmVyIjoiMS4wIn0.cubh1L2VtruiiwF8ut1m9uNBmnUJeYx4x0G30F7CqSpzHj1Sv5DCgNZXyUz3pEiz77G8IfOF0_U5A_02k-xzwdYvtJUYGH3bFISzdqymiEGmdfCIRKl9KMeoo2llGv0ScCniIhr2U1yxTIkIpp092xcdaDt-2_2q_ql1Ha_HtjvTV1f9XR3t7_Id9bR5BqwVX5zPO7JMYDVhUZRx08eqZcC-F3wi0xd_5ND_mavMuxe2wrpF-EZviO3yg0QVRr59tE3AoWl8lSGpVc97vvRCnp4WVRk26jJhYXFPsdk4yWqOKZqzr3IFGyD08WizD_vPSrXcCPbZP3XWaoTUKZSNJg",
  "refresh_token": "OAQABAAAAAABnfiG-mA6NTae7CdWW7QfdAALzDWjw6qSn4GUDfxWzJDZ6lk9qRw4AnqPnvFqnzS3GiikHr5wBM1bV1YyjH3nUeIhKhqJWGwqJFRqs2sE_rqUfz7__3J92JDpi6gDdCZNNaXgreQsH89kLCVNYZeN6kGuFGZrjwxp1wS2JYc97E_3reXBxkHrA09K5aR-WsSKCEjf6WI23FhZMTLhk_ZKOe_nWvcvLj13FyvSrTMZV2cmzyCZDqEHtPVLJgSoASuQlD2NXrfmtcmgWfc3uJSrWLIDSn4FEmVDA63X6EikNp9cllH3Gp7Vzapjlnws1NQ1_Ff5QrmBHp_LKEIwfzVKnLLrQXN0EzP8f6AX6fdVTaeKzm7iw6nH0vkPRpUeLc3q_aNsPzqcTOnFfgng7t2CXUsMAGH5wclAyFCAwL_Cds7KnyDLL7kzOS5AVZ3Mqk2tsPlqopAiHijZaJumdTILDudwKYCFAMpUeUwEf9JmyFjl2eIWPmlbwU7cHKWNvuRCOYVqbsTTpJthwh4PvsL5ov5CawH_TaV8omG_tV6RkziHG9urk9yp2PH9gl7Cv9ATa3Vt3PJWUS8LszjRIAJmyw_EhgHBfYCvEZ8U9PYarvgqrtweLcnlO7BfnnXYEC18z_u5wemAzNBFUje2ttpGtRmRic4AzZ708tBHva2ePJWGX6pgQbiWF8esOrvWjfrrlfOvEn1h6YiBW291M022undMdXzum6t1Y1huwxHPHjCAA"
}
```

### Beispiel für eine fehlerhafte Antwort
Der Azure AD-Tokenendpunkt gibt bei dem Versuch, ein Zugriffstoken für eine nachgelagerte API zu erhalten, einen Fehler zurück, wenn der Zugriff auf die nachgelagerte API durch eine Richtlinie wie mehrstufige Authentifizierung begrenzt ist. Der Dienst der mittleren Ebene zeigt diesen Fehler der Clientanwendung an, so dass diese der Richtlinie entsprechend die geeignete Benutzerinteraktion bieten kann.

```
{
    "error":"interaction_required",
    "error_description":"AADSTS50079: Due to a configuration change made by your administrator, or because you moved to a new location, you must enroll in multi-factor authentication to access 'bf8d80f9-9098-4972-b203-500f535113b1'.\r\nTrace ID: b72a68c3-0926-4b8e-bc35-3150069c2800\r\nCorrelation ID: 73d656cf-54b1-4eb2-b429-26d8165a52d7\r\nTimestamp: 2017-05-01 22:43:20Z",
    "error_codes":[50079],
    "timestamp":"2017-05-01 22:43:20Z",
    "trace_id":"b72a68c3-0926-4b8e-bc35-3150069c2800",
    "correlation_id":"73d656cf-54b1-4eb2-b429-26d8165a52d7",
    "claims":"{\"access_token\":{\"polids\":{\"essential\":true,\"values\":[\"9ab03e19-ed42-4168-b6b7-7001fb3e933a\"]}}}"
}
```

## Verwenden des Zugriffstokens für den Zugriff auf die gesicherte Ressource
Der Dienst der mittleren Ebene kann nun mit dem oben erhaltenen Token bei der nachgelagerte Web-API authentifizierte Anforderungen stellen. Hierfür wird das Token in den `Authorization`-Header eingetragen.

### Beispiel
```
GET /v1.0/me HTTP/1.1
Host: graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJub25jZSI6IkFRQUJBQUFBQUFCbmZpRy1tQTZOVGFlN0NkV1c3UWZkSzdNN0RyNXlvUUdLNmFEc19vdDF3cEQyZjNqRkxiNlVrcm9PcXA2cXBJclAxZVV0QktzMHEza29HN3RzXzJpSkYtQjY1UV8zVGgzSnktUHZsMjkxaFNBQSIsImFsZyI6IlJTMjU2IiwieDV0IjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIiwia2lkIjoiejAzOXpkc0Z1aXpwQmZCVksxVG4yNVFIWU8wIn0.eyJhdWQiOiJodHRwczovL2dyYXBoLm1pY3Jvc29mdC5jb20iLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaWF0IjoxNDkzOTMwMDE2LCJuYmYiOjE0OTM5MzAwMTYsImV4cCI6MTQ5MzkzMzg3NSwiYWNyIjoiMCIsImFpbyI6IkFTUUEyLzhEQUFBQUlzQjN5ZUljNkZ1aEhkd1YxckoxS1dlbzJPckZOUUQwN2FENTVjUVRtems9IiwiYW1yIjpbInB3ZCJdLCJhcHBfZGlzcGxheW5hbWUiOiJUb2RvRG90bmV0T2JvIiwiYXBwaWQiOiIyODQ2ZjcxYi1hN2E0LTQ5ODctYmFiMy03NjAwMzViMmYzODkiLCJhcHBpZGFjciI6IjEiLCJmYW1pbHlfbmFtZSI6IkNhbnVtYWxsYSIsImdpdmVuX25hbWUiOiJOYXZ5YSIsImlwYWRkciI6IjE2Ny4yMjAuMC4xOTkiLCJuYW1lIjoiTmF2eWEgQ2FudW1hbGxhIiwib2lkIjoiZDVlOTc5YzctM2QyZC00MmFmLThmMzAtNzI3ZGQ0YzJkMzgzIiwib25wcmVtX3NpZCI6IlMtMS01LTIxLTIxMjc1MjExODQtMTYwNDAxMjkyMC0xODg3OTI3NTI3LTI2MTE4NDg0IiwicGxhdGYiOiIxNCIsInB1aWQiOiIxMDAzM0ZGRkEwNkQxN0M5Iiwic2NwIjoiVXNlci5SZWFkIiwic3ViIjoibWtMMHBiLXlpMXQ1ckRGd2JTZ1JvTWxrZE52b3UzSjNWNm84UFE3alVCRSIsInRpZCI6IjcyZjk4OGJmLTg2ZjEtNDFhZi05MWFiLTJkN2NkMDExZGI0NyIsInVuaXF1ZV9uYW1lIjoibmFjYW51bWFAbWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hY2FudW1hQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJzUVlVekYxdUVVS0NQS0dRTVFVRkFBIiwidmVyIjoiMS4wIn0.Hrn__RGi-HMAzYRyCqX3kBGb6OS7z7y49XPVPpwK_7rJ6nik9E4s6PNY4XkIamJYn7tphpmsHdfM9lQ1gqeeFvFGhweIACsNBWhJ9Nx4dvQnGRkqZ17KnF_wf_QLcyOrOWpUxdSD_oPKcPS-Qr5AFkjw0t7GOKLY-Xw3QLJhzeKmYuuOkmMDJDAl0eNDbH0HiCh3g189a176BfyaR0MgK8wrXI_6MTnFSVfBePqklQeLhcr50YTBfWg3Svgl6MuK_g1hOuaO-XpjUxpdv5dZ0SvI47fAuVDdpCE48igCX5VMj4KUVytDIf6T78aIXMkYHGgW3-xAmuSyYH_Fr0yVAQ
```

## Nächste Schritte
Erfahren Sie mehr über das OAuth 2.0-Protokoll und eine andere Möglichkeit, Dienst-zu-Dienst-Authentifizierungen mit Client-Anmeldedaten auszuführen.
* [Azure Active Directory v2.0 und der OAuth 2.0-Clientanmeldeinformations-Flow](active-directory-v2-protocols-oauth-client-creds.md)
* [v2.0-Protokolle – OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols-oauth-code.md)

