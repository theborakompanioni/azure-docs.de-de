---
title: "Den Im-Auftrag-Fluss von OAuth 2.0 für die Dienst-zu-Dienst-Authentifizierung in Azure AD verwenden | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie HTTP-Nachrichten zum Implementieren der Dienst-zu-Dienst-Authentifizierung über den Im-Auftrag-von-Fluss von OAuth 2.0 verwenden."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 09f6f318-e88b-4024-9ee1-e7f09fb19a82
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: nacanuma
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 4836593cf11deb50463778d457227804cb3f0b56
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="service-to-service-calls-using-delegated-user-identity-in-the-on-behalf-of-flow"></a>Dienst-zu-Dienst-Aufrufe unter Verwendung einer Stellvertreter-Benutzeridentität im Im-Auftrag-Fluss
Der Im-Auftrag-Fluss von OAuth 2.0 wird verwendet, wenn eine Anwendung eine Dienst- oder Web-API aufruft, die wiederum einen andere Dienst- oder Web-API aufrufen muss. Die Idee dabei ist, die Stellvertreter-Benutzeridentität und -Berechtigungen über die Anforderungskette weiterzugeben. Damit der Dienst auf der mittleren Ebene Authentifizierungsanforderungen an den Downstream-Dienst stellen kann, muss für den Benutzer ein Zugriffstoken aus Azure Active Directory (Azure AD) abgesichert werden.

## <a name="on-behalf-of-flow-diagram"></a>„Im Auftrag von“-Ablauf
Es wird davon ausgegangen, dass der Benutzer in einer Anwendung authentifiziert wurde, die den [Autorisierungscode-Vergabefluss von OAuth 2.0](active-directory-protocols-oauth-code.md) verwendet. In diesem Fall verfügt die Anwendung über ein Zugriffstoken (Token A), das die Benutzeransprüche und die Genehmigung für den Zugriff auf die API der mittleren Ebene enthält (API A). API A muss nun eine authentifizierte Anfrage an die API des Downstream-Webs stellen (API B).

Die folgenden Schritte entsprechen dem Im-Auftrag-von-Fluss und werden anhand des folgenden Diagramms erläutert.

![Im-Auftrag-Fluss von OAuth 2.0](media/active-directory-protocols-oauth-on-behalf-of/active-directory-protocols-oauth-on-behalf-of-flow.png)


1. Die Clientanwendung stellt mit Token A eine Anforderung an API A.
2. API A wird gegenüber dem Azure AD-Tokenausstellungs-Endpunkt authentifiziert und fordert ein Token für den Zugriff auf API B an.
3. Der Azure AD-Tokenausstellungs-Endpunkt überprüft mit Token A die Anmeldeinformationen von API A und stellt das Zugriffstoken für API B aus (Token B).
4. Token B wird im Autorisierungsheader der Anforderung an API B belegt.
5. API B gibt die Daten aus der gesicherten Ressource zurück.

## <a name="register-the-application-and-service-in-azure-ad"></a>Registrieren der Anwendung und des Diensts in Azure AD
Registrieren der Clientanwendungen des Diensts auf der mittleren Ebene in Azure AD.
### <a name="register-the-middle-tier-service"></a>Registrieren des Diensts auf der mittleren Ebene
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto, und wählen Sie in der Liste **Verzeichnis** den Active Directory-Mandanten aus, für den Sie Ihre Anwendungen registrieren möchten.
3. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Neue Awendungsregistrierung**.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, und wählen Sie den Anwendungstyp aus. Geben Sie auf Basis des Anwendungsnamens die Anmelde-URL an, oder leiten Sie die URL zur Basis-URL um. Klicken Sie auf **Erstellen**, um die Anwendung zu erstellen.
6. Wählen Sie im Azure-Portal Ihre Anwendung aus, und klicken Sie auf **Einstellungen**. Wählen Sie im Menü Einstellungen **Schlüssel**, und fügen Sie einen Schlüssel mit einer Schlüsseldauer von ein oder zwei Jahren hinzu. Wenn Sie diese Seite speichern, wird der Schlüsselwert angezeigt. Kopieren und speichern Sie diesen an einem sicheren Ort – Sie benötigen den Schlüssel später, um die Anwendungseinstellungen in Ihrer Implementierung zu konfigurieren. Der Schlüsselwert wird nicht wieder angezeigt und kann auch nicht auf andere Weise ausgegeben werden. Deshalb ist es wichtig, ihn sofort zu speichern, wenn er im Azure-Portal angezeigt wird.

### <a name="register-the-client-application"></a>Registrieren der Clientanwendung
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto, und wählen Sie in der Liste **Verzeichnis** den Active Directory-Mandanten aus, für den Sie Ihre Anwendungen registrieren möchten.
3. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Neue Awendungsregistrierung**.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, und wählen Sie den Anwendungstyp aus. Geben Sie auf Basis des Anwendungsnamens die Anmelde-URL an, oder leiten Sie die URL zur Basis-URL um. Klicken Sie auf **Erstellen**, um die Anwendung zu erstellen.
6. Konfigurieren der Berechtigungen für Ihre Anwendung: Wählen Sie im Menü Einstellungen den Abschnitt **Erforderliche Berechtigungen**, klicken Sie auf **Hinzufügen**, **Select an API** (Hiermit wählen Sie eine API aus), und erfassen Sie im Textfeld den Namen der mittleren Ebene. Klicken Sie anschließend auf **Berechtigungen auswählen**, und wählen Sie *Auf den Dienstnamen zugreifen* aus.

### <a name="configure-known-client-applications"></a>Konfigurieren der bekannten Clientanwendungen
In diesem Szenario interagiert der Dienst auf der mittleren Ebene nicht mit dem Benutzer, um dessen Zustimmung für den Zugriff auf die Downstream-API zu erlangen. Aus diesem Grund muss die Zugriffsgewährung auf die Downstream-API zuvor als Teil des Genehmigungsschrittes während der Authentifizierung als Option angezeigt werden.
Führen Sie hierfür die folgenden Schritte aus. Dadurch wird die Registrierung der Clientanwendung in Azure AD explizit mit der Registrierung des Diensts auf der mittleren Ebene verknüpft, wodurch die von dem Client und der mittleren Ebene erzielten Genehmigungen zu einem einzigen Dialog zusammengeführt werden.
1. Navigieren Sie zu der Registrierung auf der mittleren Ebene, und klicken Sie auf **Manifest**, um den Manifest-Editor zu öffnen.
2. Suchen Sie im Manifest die Array-Eigenschaft `knownClientApplications`, und fügen Sie die Client-ID der Clientanwendung als Element hinzu.
3. Speichern Sie das Manifest, indem Sie auf die Schaltfläche „Speichern“ klicken.

## <a name="service-to-service-access-token-request"></a>Dienst-zu-Dienst-Zugriffstokenanforderung
Verwenden Sie zum Anfordern eines Zugriffstokens ein HTTP POST-Element für den mandantenspezifischen Azure AD-Endpunkt. Verwenden Sie dabei die folgenden Parameter.

```
https://login.microsoftonline.com/<tenant>/oauth2/token
```

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich | Typ der Tokenanforderung. Bei Anforderungen mit JWT muss der Wert **urn:ietf:params:oauth:grant-type:jwt-bearer** lauten. |
| Assertion |erforderlich | Der Wert des bei der Anforderung verwendeten Tokens. |
| client_id |erforderlich | Die dem aufrufenden Dienst während der Registrierung bei Azure AD zugewiesene App-ID. Klicken Sie zum Ermitteln der App-ID im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis und den Anwendungsnamen. |
| client_secret |erforderlich | Schlüssel, der für den aufrufenden Dienst in Azure AD registriert ist. Dieser Wert sollte zum Zeitpunkt der Registrierung notiert worden sein. |
| Ressource |erforderlich | Der App-ID-URI des empfangenden Diensts (geschützte Ressource). Klicken Sie zum Ermitteln des App-ID-URI im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis, den Anwendungsnamen, **Alle Einstellungen** und **Eigenschaften**. |
| requested_token_use |erforderlich | Gibt an, wie die Anforderung verarbeitet werden soll. Im Im-Auftrag-Fluss muss der Wert **on_behalf_of** lauten. |
| Bereich |erforderlich | Eine durch Leerzeichen getrennte Liste von Bereichen für die Tokenanforderung. Für OpenID Connect muss der Bereich **openid** angegeben werden.|

### <a name="example"></a>Beispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken für die Web-API https://graph.windows.net angefordert. Mit der `client_id` wird der Dienst identifiziert, der das Zugriffstoken anfordert.

```
// line breaks for legibility only

POST /oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=urn%3Aietf%3Aparams%3Aoauth%3Agrant-type%3Ajwt-bearer
&client_id=625391af-c675-43e5-8e44-edd3e30ceb15
&client_secret=0Y1W%2BY3yYb3d9N8vSjvm8WrGzVZaAaHbHHcGbcgG%2BoI%3D
&resource=https%3A%2F%2Fgraph.windows.net
&assertion=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2Rkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tLzE5MjNmODYyLWU2ZGMtNDFhMy04MWRhLTgwMmJhZTAwYWY2ZCIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzI2MDM5Y2NlLTQ4OWQtNDAwMi04MjkzLTViMGM1MTM0ZWFjYi8iLCJpYXQiOjE0OTM0MjMxNTIsIm5iZiI6MTQ5MzQyMzE1MiwiZXhwIjoxNDkzNDY2NjUyLCJhY3IiOiIxIiwiYWlvIjoiWTJaZ1lCRFF2aTlVZEc0LzM0L3dpQndqbjhYeVp4YmR1TFhmVE1QeG8yYlN2elgreHBVQSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiJiMzE1MDA3OS03YmViLTQxN2YtYTA2YS0zZmRjNzhjMzI1NDUiLCJhcHBpZGFjciI6IjAiLCJlX2V4cCI6MzAyNDAwLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidmVyIjoiMS4wIn0.R-Ke-XO7lK0r5uLwxB8g5CrcPAwRln5SccJCfEjU6IUqpqcjWcDzeDdNOySiVPDU_ZU5knJmzRCF8fcjFtPsaA4R7vdIEbDuOur15FXSvE8FvVSjP_49OH6hBYqoSUAslN3FMfbO6Z8YfCIY4tSOB2I6ahQ_x4ZWFWglC3w5mK-_4iX81bqi95eV4RUKefUuHhQDXtWhrSgIEC0YiluMvA4TnaJdLq_tWXIc4_Tq_KfpkvI004ONKgU7EAMEr1wZ4aDcJV2yf22gQ1sCSig6EGSTmmzDuEPsYiyd4NhidRZJP4HiiQh-hePBQsgcSgYGvz9wC6n57ufYKh2wm_Ti3Q
&requested_token_use=on_behalf_of
&scope=openid
```

## <a name="service-to-service-access-token-response"></a>Dienst-zu-Dienst-Zugriffstokenantwort
Eine erfolgreiche Antwort ist eine JSON OAuth 2.0-Antwort mit den folgenden Parametern.

| Parameter | Beschreibung |
| --- | --- |
| token_type |Gibt den Wert des Tokentyps an. **Bearertoken**ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth 2.0 Authorization Framework: Bearer Token Usage (RFC 6750) (OAuth 2.0-Autorisierungsframework: Verwendung von Bearertoken (RFC 6750))](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| Bereich |Der durch das Token gewährte Zugriffsbereich. |
| expires_in |Gibt an, wie lange das Zugriffstoken gültig ist (in Sekunden). |
| expires_on |Die Uhrzeit, zu der das Zugriffstoken abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. Dieser Wert wird verwendet, um die Lebensdauer von zwischengespeicherten Token zu bestimmen. |
| Ressource |Der App-ID-URI des empfangenden Diensts (geschützte Ressource). |
| access_token |Das angeforderte Zugriffstoken. Der aufrufende Dienst kann dieses Token verwenden, um die Authentifizierung für den empfangenden Dienst durchzuführen. |
| id_token |Das angeforderte ID-Token. Dies kann von dem aufrufenden Dienst dazu verwendet werden, die Identität des Benutzers zu überprüfen und eine Sitzung mit dem Benutzer zu beginnen. |
| refresh_token |Das Aktualisierungstoken für das angeforderte Zugriffstoken. Der aufrufende Dienst kann dieses Token verwenden, um nach Ablauf des aktuellen Zugriffstokens ein neues Zugriffstoken anzufordern. |

### <a name="success-response-example"></a>Beispiel für eine erfolgreiche Antwort
Das folgende Beispiel zeigt eine erfolgreiche Antwort auf eine Anforderung für ein Zugriffstoken auf die Web-API https://graph.windows.net.

```
{
    "token_type":"Bearer",
    "scope":"User.Read",
    "expires_in":"43482",
    "ext_expires_in":"302683",
    "expires_on":"1493466951",
    "not_before":"1493423168",
    "resource":"https://graph.windows.net",
    "access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ",
    "refresh_token":"AQABAAAAAABnfiG-mA6NTae7CdWW7QfdjKGu9-t1scy_TDEmLi4eLQMjJGt_nAoVu6A4oSu1KsRiz8XyQIPKQxSGfbf2FoSK-hm2K8TYzbJuswYusQpJaHUQnSqEvdaCeFuqXHBv84wjFhuanzF9dQZB_Ng5za9xKlUENrNtlq9XuLNVKzxEyeUM7JyxzdY7JiEphWImwgOYf6II316d0Z6-H3oYsFezf4Xsjz-MOBYEov0P64UaB5nJMvDyApV-NWpgklLASfNoSPGb67Bc02aFRZrm4kLk-xTl6eKE6hSo0XU2z2t70stFJDxvNQobnvNHrAmBaHWPAcC3FGwFnBOojpZB2tzG1gLEbmdROVDp8kHEYAwnRK947Py12fJNKExUdN0njmXrKxNZ_fEM33LHW1Tf4kMX_GvNmbWHtBnIyG0w5emb-b54ef5AwV5_tGUeivTCCysgucEc-S7G8Cz0xNJ_BOiM_4bAv9iFmrm9STkltpz0-Tftg8WKmaJiC0xXj6uTf4ZkX79mJJIuuM7XP4ARIcLpkktyg2Iym9jcZqymRkGH2Rm9sxBwC4eeZXM7M5a7TJ-5CqOdfuE3sBPq40RdEWMFLcrAzFvP0VDR8NKHIrPR1AcUruat9DETmTNJukdlJN3O41nWdZOVoJM-uKN3uz2wQ2Ld1z0Mb9_6YfMox9KTJNzRzcL52r4V_y3kB6ekaOZ9wQ3HxGBQ4zFt-2U0mSszIAA",
    "id_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC8yNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IvIiwiaWF0IjoxNDkzNDIzMTY4LCJuYmYiOjE0OTM0MjMxNjgsImV4cCI6MTQ5MzQ2Njk1MSwiYW1yIjpbInB3ZCJdLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJzdWIiOiJEVXpYbkdKMDJIUk0zRW5pbDFxdjZCakxTNUllQy0tQ2ZpbzRxS1MzNEc4IiwidGlkIjoiMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiIiwidW5pcXVlX25hbWUiOiJuYXZ5YUBkZG9iYWxpYW5vdXRsb29rLm9ubWljcm9zb2Z0LmNvbSIsInVwbiI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXRpIjoieEN3ZnpoYS1QMFdKUU9MeENHZ0tBQSIsInZlciI6IjEuMCJ9."
}
```

### <a name="error-response-example"></a>Beispiel für eine fehlerhafte Antwort
Der Azure AD-Tokenendpunkt gibt bei dem Versuch, ein Zugriffstoken für eine Downstream-API zu erhalten, einen Fehler zurück, wenn der Zugriff auf die Downstream-API durch eine Richtlinie wie mehrstufige Authentifizierung begrenzt ist. Der Dienst der mittleren Ebene zeigt diesen Fehler der Clientanwendung an, so dass diese der Richtlinie entsprechend die geeignete Benutzerinteraktion bieten kann.

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

## <a name="use-the-access-token-to-access-the-secured-resource"></a>Verwenden des Zugriffstokens für den Zugriff auf die geschützte Ressource
Der Dienst der mittleren Ebene kann nun mit dem oben erhaltenen Token bei der Downstream-Web-API authentifizierte Anforderungen stellen. Hierfür wird das Token in den `Authorization`-Header eingetragen.

### <a name="example"></a>Beispiel
```
GET /me?api-version=2013-11-08 HTTP/1.1
Host: graph.windows.net
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCIsImtpZCI6InowMzl6ZHNGdWl6cEJmQlZLMVRuMjVRSFlPMCJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRvd3MubmV0IiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvMjYwMzljY2UtNDg5ZC00MDAyLTgyOTMtNWIwYzUxMzRlYWNiLyIsImlhdCI6MTQ5MzQyMzE2OCwibmJmIjoxNDkzNDIzMTY4LCJleHAiOjE0OTM0NjY5NTEsImFjciI6IjEiLCJhaW8iOiJBU1FBMi84REFBQUE1NnZGVmp0WlNjNWdBVWwrY1Z0VFpyM0VvV2NvZEoveWV1S2ZqcTZRdC9NPSIsImFtciI6WyJwd2QiXSwiYXBwaWQiOiI2MjUzOTFhZi1jNjc1LTQzZTUtOGU0NC1lZGQzZTMwY2ViMTUiLCJhcHBpZGFjciI6IjEiLCJlX2V4cCI6MzAyNjgzLCJmYW1pbHlfbmFtZSI6IlRlc3QiLCJnaXZlbl9uYW1lIjoiTmF2eWEiLCJpcGFkZHIiOiIxNjcuMjIwLjEuMTc3IiwibmFtZSI6Ik5hdnlhIFRlc3QiLCJvaWQiOiIxY2Q0YmNhYy1iODA4LTQyM2EtOWUyZi04MjdmYmIxYmI3MzkiLCJwbGF0ZiI6IjMiLCJwdWlkIjoiMTAwMzNGRkZBMTJFRDdGRSIsInNjcCI6IlVzZXIuUmVhZCIsInN1YiI6IjNKTUlaSWJlYTc1R2hfWHdDN2ZzX0JDc3kxa1l1ekZKLTUyVm1Zd0JuM3ciLCJ0aWQiOiIyNjAzOWNjZS00ODlkLTQwMDItODI5My01YjBjNTEzNGVhY2IiLCJ1bmlxdWVfbmFtZSI6Im5hdnlhQGRkb2JhbGlhbm91dGxvb2sub25taWNyb3NvZnQuY29tIiwidXBuIjoibmF2eWFAZGRvYmFsaWFub3V0bG9vay5vbm1pY3Jvc29mdC5jb20iLCJ1dGkiOiJ4Q3dmemhhLVAwV0pRT0x4Q0dnS0FBIiwidmVyIjoiMS4wIn0.cqmUVjfVbqWsxJLUI1Z4FRx1mNQAHP-L0F4EMN09r8FY9bIKeO-0q1eTdP11Nkj_k4BmtaZsTcK_mUygdMqEp9AfyVyA1HYvokcgGCW_Z6DMlVGqlIU4ssEkL9abgl1REHElPhpwBFFBBenOk9iHddD1GddTn6vJbKC3qAaNM5VarjSPu50bVvCrqKNvFixTb5bbdnSz-Qr6n6ACiEimiI1aNOPR2DeKUyWBPaQcU5EAK0ef5IsVJC1yaYDlAcUYIILMDLCD9ebjsy0t9pj_7lvjzUSrbMdSCCdzCqez_MSNxrk1Nu9AecugkBYp3UVUZOIyythVrj6-sVvLZKUutQ
```

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über das OAuth 2.0-Protokoll und eine andere Möglichkeit, Dienst-zu-Dienst-Authentifizierungen mit Client-Anmeldedaten auszuführen.
* [Dienst-zu-Dienst-Authentifizierung mit Vergabe der OAuth 2.0-Client-Anmeldedaten in Azure AD](active-directory-protocols-oauth-service-to-service.md)
* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)

