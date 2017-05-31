---
title: "Dienst-zu-Dienst-Authentifizierung für Azure AD mit OAuth 2.0 | Microsoft Docs"
description: "In diesem Artikel wird beschrieben, wie Sie HTTP-Nachrichten zum Implementieren der Dienst-zu-Dienst-Authentifizierung über den Fluss zum Gewähren von OAuth 2.0-Clientanmeldeinformationen verwenden."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: priyamo
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 9f8513c64cf605ab339dc3dfb8129a7fad123918
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="service-to-service-calls-using-client-credentials-shared-secret-or-certificate"></a>Dienst-zu-Dienst-Aufrufe mit Clientanmeldeinformationen (freigegebenes Geheimnis oder Zertifikat)
Beim Fluss zur Gewährung von OAuth 2.0-Clientanmeldeinformationen kann ein Webdienst (ein *vertraulicher Client*) seine eigenen Anmeldeinformationen zum Authentifizieren verwenden, wenn ein anderer Webdienst aufgerufen wird, anstatt die Identität eines anderen Benutzers anzunehmen. In diesem Szenario ist der Client normalerweise ein Webdienst der mittleren Ebene, ein Daemondienst oder eine Website. Für ein höheres Maß an Sicherheit kann der aufrufende Dienst anstelle eines gemeinsamen Geheimnisses auch ein Zertifikat als Anmeldeinformationen verwenden.

## <a name="client-credentials-grant-flow-diagram"></a>Diagramm: Fluss zur Gewährung von Clientanmeldeinformationen
Im folgenden Diagramm wird veranschaulicht, wie der Fluss zur Gewährung von Clientanmeldeinformationen in Azure Active Directory (Azure AD) funktioniert.

![Fluss zum Gewähren von OAuth 2.0-Clientanmeldeinformationen](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. Die Clientanwendung wird gegenüber dem Azure AD-Tokenausstellungs-Endpunkt authentifiziert und fordert ein Zugriffstoken an.
2. Der Azure AD-Tokenausstellungs-Endpunkt stellt das Zugriffstoken aus.
3. Das Zugriffstoken wird verwendet, um die geschützte Ressource zu authentifizieren.
4. Die Daten aus der geschützten Ressource werden an die Webanwendung zurückgegeben.

## <a name="register-the-services-in-azure-ad"></a>Registrieren der Dienste in Azure AD
Registrieren Sie sowohl den aufrufenden Dienst als auch den empfangenden Dienst in Azure Active Directory (Azure AD). Ausführlichere Informationen finden Sie unter [Integrieren von Anwendungen in Azure Active Directory](active-directory-integrating-applications.md).

## <a name="request-an-access-token"></a>Anfordern eines Zugriffstokens
Verwenden Sie zum Anfordern eines Zugriffstokens ein HTTP POST-Element für den mandantenspezifischen Azure AD-Endpunkt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Dienst-zu-Dienst-Zugriffstokenanforderung
Es sind zwei Fälle denkbar – je nachdem, ob die Clientanwendung durch ein gemeinsames Geheimnis oder durch ein Zertifikat geschützt wird.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Erster Fall: Zugriffstokenanforderung mit einem gemeinsamen Geheimnis
Bei Verwendung eines gemeinsamen Geheimnisses enthält eine Dienst-zu-Dienst-Zugriffstokenanforderung die folgenden Parameter:

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich |Gibt den angeforderten Gewährungstyp an. In einem Fluss zur Gewährung von Clientanmeldeinformationen muss der Wert **client_credentials** lauten. |
| client_id |erforderlich |Gibt die Azure AD-Client-ID des aufrufenden Webdiensts an. Klicken Sie zum Ermitteln der Client-ID der aufrufenden Anwendung im [Azure-Portal](https://portal.azure.com) auf **Active Directory**, wechseln Sie das Verzeichnis, und klicken Sie anschließend auf die Anwendung. Die Client-ID (client_id) ist die *Anwendungs-ID*. |
| client_secret |erforderlich |Geben Sie einen Schlüssel ein, der für den aufrufenden Webdienst oder für die aufrufende Daemon-Anwendung in Azure AD registriert ist. Klicken Sie zum Erstellen eines Schlüssels im Azure-Portal auf **Active Directory**, wechseln Sie das Verzeichnis, und klicken Sie auf die Anwendung. Klicken Sie anschließend auf **Einstellungen** > **Schlüssel**, und fügen Sie einen Schlüssel hinzu.|
| resource |erforderlich |Geben Sie den App-ID-URI des empfangenden Webdiensts ein. Klicken Sie zum Ermitteln des App-ID-URI im Azure-Portal auf **Active Directory**, wechseln Sie das Verzeichnis, klicken Sie auf die Dienstanwendung, und klicken Sie anschließend auf **Einstellungen** > **Eigenschaften**. |

#### <a name="example"></a>Beispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken für den Webdienst „https://service.contoso.com/“ angefordert. Mit der `client_id` wird der Webdienst identifiziert, der das Zugriffstoken anfordert.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Zweiter Fall: Zugriffstokenanforderung mit einem Zertifikat
Eine Dienst-zu-Dienst-Zugriffstokenanforderung mit einem Zertifikat enthält die folgenden Parameter:

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich |Gibt den angeforderten Antworttyp an. In einem Fluss zur Gewährung von Clientanmeldeinformationen muss der Wert **client_credentials** lauten. |
| client_id |erforderlich |Gibt die Azure AD-Client-ID des aufrufenden Webdiensts an. Klicken Sie zum Ermitteln der Client-ID der aufrufenden Anwendung im [Azure-Portal](https://portal.azure.com) auf **Active Directory**, wechseln Sie das Verzeichnis, und klicken Sie anschließend auf die Anwendung. Die Client-ID (client_id) ist die *Anwendungs-ID*. |
| client_assertion_type |erforderlich |Der Wert muss `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` sein. |
| client_assertion |erforderlich | Eine Assertion (ein JSON-Webtoken), die Sie benötigen, um das Zertifikat, das Sie als Anmeldeinformationen für Ihre Anwendung registriert haben, zu erstellen und sich damit anzumelden. Im Anschluss finden Sie ausführlichere Informationen zum Registrieren Ihres Zertifikats sowie zum Format der Assertion.|
| resource | erforderlich |Geben Sie den App-ID-URI des empfangenden Webdiensts ein. Klicken Sie zum Ermitteln des App-ID-URI im Azure-Portal nacheinander auf **Active Directory**, auf das Verzeichnis, auf die Anwendung und anschließend auf **Konfigurieren**. |

Wie Sie sehen, sind die Parameter nahezu identisch mit den Parametern der Anforderung mit dem gemeinsamen Geheimnis. Einziger Unterschied: Anstelle des Parameters „client_secret“ werden die beiden Parameter „client_assertion_type“ und „client_assertion“ verwendet.

#### <a name="format-of-the-assertion"></a>Format der Assertion
Für die Berechnung der Assertion empfiehlt sich die Verwendung einer der zahlreichen [JSON-Webtoken](https://jwt.io/)-Bibliotheken in der Sprache Ihrer Wahl. Das Token enthält folgende Informationen:

##### <a name="header"></a>Header

| Parameter |  Anmerkung |
| --- | --- | --- |
| `alg` | Muss **RS256** sein. |
| `typ` | Muss **JWT** sein. |
| `x5t` | Muss der SHA-1-Fingerabdruck des X.509-Zertifikats sein. |

##### <a name="claims-payload"></a>Ansprüche (Nutzlast)

| Parameter |  Anmerkung |
| --- | --- | --- |
| `aud` | Zielgruppe: Muss **https://login.microsoftonline/*Mandanten-ID*/oauth2/token** sein. |
| `exp` | Ablaufdatum |
| `iss` | Aussteller: Muss die Client-ID (Anwendungs-ID des Clientdiensts) sein. |
| `jti` | GUID: Die JWT-ID. |
| `nbf` | Nicht vor: Datum, vor dem das Token nicht verwendet werden kann. |
| `sub` | Antragsteller: Muss wie bei `iss` die Client-ID (Anwendungs-ID des Clientdiensts) sein. |

##### <a name="signature"></a>Signatur
Zur Berechnung der Signatur wird das Zertifikat wie in der [Spezifikation für JSON-Webtoken vom Typ „RFC7519“](https://tools.ietf.org/html/rfc7519) beschrieben angewendet.

##### <a name="example-of-a-decoded-jwt-assertion"></a>Beispiel einer decodierten JWT-Assertion
```
{
  "alg": "RS256",
  "typ": "JWT",
  "x5t": "gx8tGysyjcRqKjFPnd7RFwvwZI0"
} 
. 
{
  "aud": "https: //login.microsoftonline.com/contoso.onmicrosoft.com/oauth2/token",
  "exp": 1484593341,
  "iss": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05",
  "jti": "22b3bb26-e046-42df-9c96-65dbd72c1c81",
  "nbf": 1484592741,  
  "sub": "97e0a5b7-d745-40b6-94fe-5f77d35c6e05"
}
.
"Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"

```

##### <a name="example-of-encoded-jwt-assertion"></a>Beispiel einer codierten JWT-Assertion
Die folgende Zeichenfolge ist ein Beispiel für eine codierte Assertion. Bei genauer Betrachtung sehen Sie drei Abschnitte, die jeweils durch einen Punkt getrennt sind. Der erste Abschnitt codiert den Header, der zweite die Nutzlast. Bei dem letzten Abschnitt handelt es sich um die Signatur, die mit den Zertifikaten aus dem Inhalt der ersten beiden Abschnitte berechnet wurde.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

#### <a name="how-to-register-your-certificate-with-azure-ad"></a>So registrieren Sie Ihr Zertifikat bei Azure AD
Sie müssen das Anwendungsmanifest bearbeiten, um die Zertifikatanmeldeinformationen der Clientanwendung in Azure AD zuzuordnen. Wenn Sie über ein Zertifikat verfügen, berechnen Sie Folgendes:
- `$base64Thumbprint`: Die base64-Codierung des Zertifikathashs.
- `$base64Value`: Die base64-Codierung der Zertifikatrohdaten.

Geben Sie außerdem eine GUID an, um den Schlüssel im Anwendungsmanifest (`$keyId`) zu identifizieren.

Klicken Sie im Azure-Portal in der App-Registrierung für die Clientanwendung auf **Manifest** > **Herunterladen**.
Öffnen Sie die Manifestdateien in Ihrem bevorzugten Texteditor, und ersetzen Sie die Eigenschaft *keyCredentials* gemäß dem folgenden Schema durch Ihre neuen Zertifikatinformationen:
```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
],
```
Speichern Sie das geänderte Anwendungsmanifest, und klicken Sie auf **Manifest** > **Hochladen**, um es wieder in Azure AD hochzuladen. Da die keyCredentials-Eigenschaft mehrere Werte besitzen kann, können Sie mehrere Zertifikate hochladen, um eine vielfältigere Schlüsselverwaltung zu erreichen.


#### <a name="example-of-request"></a>Anforderungsbeispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken für den Webdienst „https://service.contoso.com/“ mit einem Zertifikat angefordert. Mit der `client_id` wird der Webdienst identifiziert, der das Zugriffstoken anfordert.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### <a name="service-to-service-access-token-response"></a>Dienst-zu-Dienst-Zugriffstokenantwort

Eine erfolgreiche Antwort enthält eine JSON OAuth 2.0-Antwort mit den folgenden Parametern:

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Der aufrufende Webdienst kann dieses Token verwenden, um die Authentifizierung für den empfangenden Webdienst durchzuführen. |
| token_type |Gibt den Wert des Tokentyps an. **Bearertoken**ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth 2.0 Authorization Framework: Bearer Token Usage (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)(OAuth 2.0-Autorisierungsframework: Verwendung von Bearertoken (RFC 6750)). |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| expires_on |Die Uhrzeit, zu der das Zugriffstoken abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. Dieser Wert wird verwendet, um die Lebensdauer von zwischengespeicherten Token zu bestimmen. |
| not_before |Der Zeitpunkt, ab dem das Zugriffstoken verwendet werden kann. Das Datum wird als Anzahl von Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z UTC) bis zum Zeitpunkt der Tokengültigkeit dargestellt.|
| resource |Der App-ID-URI des empfangenden Webdiensts. |

#### <a name="example-of-response"></a>Antwortbeispiel
Das folgende Beispiel zeigt eine erfolgreiche Antwort auf eine Anforderung für ein Zugriffstoken von einem Webdienst.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Weitere Informationen
* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
* [Beispiel des Dienst-zu-Dienst-Aufrufs mit einem gemeinsamen Geheimnis (C#)](https://github.com/Azure-Samples/active-directory-dotnet-daemon) und [Beispiel des Dienst-zu-Dienst-Aufrufs mit einem Zertifikat (C#)](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)

