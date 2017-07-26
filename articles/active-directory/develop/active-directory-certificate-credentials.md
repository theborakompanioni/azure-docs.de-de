---
title: Zertifikatanmeldeinformationen in Azure AD | Microsoft-Dokumentation
description: "In diesem Artikel werden die Registrierung für die Anwendungsauthentifizierung und die Verwendung von Zertifikatanmeldeinformationen für diesen Zweck beschrieben."
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 88f0c64a-25f7-4974-aca2-2acadc9acbd8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: nacanuma
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 08bb5140bb35bbd120aaa506afeab8ad247f81e1
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---

# <a name="certificate-credentials-for-application-authentication"></a>Zertifikatanmeldeinformationen für die Anwendungsauthentifizierung

Azure Active Directory ermöglicht einer Anwendung, ihre eigenen Anmeldeinformationen für die Authentifizierung z.B. im Flow zum Erteilen der OAuth 2.0-Clientanmeldeinformationen und den Im-Auftrag-von-Flow zu verwenden.
Eine verwendbare Form von Anmeldeinformationen ist eine JWT-Assertion (JSON Web Token), die mit einem der Anwendung zugehörigen Zertifikat signiert ist.

## <a name="format-of-the-assertion"></a>Format der Assertion
Für die Berechnung der Assertion empfiehlt sich die Verwendung einer der zahlreichen [JSON Web Token](https://jwt.io/)-Bibliotheken in der Sprache Ihrer Wahl. Das Token enthält folgende Informationen:

#### <a name="header"></a>Header

| Parameter |  Anmerkung |
| --- | --- | --- |
| `alg` | Muss **RS256** sein. |
| `typ` | Muss **JWT** sein. |
| `x5t` | Muss der SHA-1-Fingerabdruck des X.509-Zertifikats sein. |

#### <a name="claims-payload"></a>Ansprüche (Nutzlast)

| Parameter |  Anmerkung |
| --- | --- | --- |
| `aud` | Zielgruppe: Muss **https://login.microsoftonline.com/*tenant_Id*/oauth2/token** sein. |
| `exp` | Ablaufdatum: Datum, an dem das Token abläuft. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) UTC bis zum Zeitpunkt dargestellt, an dem die Gültigkeit des Tokens abläuft.|
| `iss` | Aussteller: Muss die Client-ID (Anwendungs-ID des Clientdiensts) sein. |
| `jti` | GUID: Die JWT-ID. |
| `nbf` | Nicht vor: Datum, vor dem das Token nicht verwendet werden kann. Die Zeit wird als Anzahl der Sekunden ab dem 1. Januar 1970 (1970-01-01T0:0:0Z) (UTC) bis zur Zeit der Ausstellung des Tokens dargestellt. |
| `sub` | Antragsteller: Muss wie bei `iss` die Client-ID (Anwendungs-ID des Clientdiensts) sein. |

#### <a name="signature"></a>Signatur
Zur Berechnung der Signatur wird das Zertifikat wie in der [Spezifikation für JSON-Webtoken vom Typ „RFC7519“](https://tools.ietf.org/html/rfc7519) beschrieben angewendet.

### <a name="example-of-a-decoded-jwt-assertion"></a>Beispiel einer decodierten JWT-Assertion
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

### <a name="example-of-an-encoded-jwt-assertion"></a>Beispiel einer codierten JWT-Assertion
Die folgende Zeichenfolge ist ein Beispiel für eine codierte Assertion. Bei genauer Betrachtung sehen Sie drei Abschnitte, die jeweils durch einen Punkt getrennt sind.
Der erste Abschnitt codiert den Header, der zweite die Nutzlast. Bei dem letzten Abschnitt handelt es sich um die Signatur, die mit den Zertifikaten aus dem Inhalt der ersten beiden Abschnitte berechnet wurde.
```
"eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJhdWQiOiJodHRwczpcL1wvbG9naW4ubWljcm9zb2Z0b25saW5lLmNvbVwvam1wcmlldXJob3RtYWlsLm9ubWljcm9zb2Z0LmNvbVwvb2F1dGgyXC90b2tlbiIsImV4cCI6MTQ4NDU5MzM0MSwiaXNzIjoiOTdlMGE1YjctZDc0NS00MGI2LTk0ZmUtNWY3N2QzNWM2ZTA1IiwianRpIjoiMjJiM2JiMjYtZTA0Ni00MmRmLTljOTYtNjVkYmQ3MmMxYzgxIiwibmJmIjoxNDg0NTkyNzQxLCJzdWIiOiI5N2UwYTViNy1kNzQ1LTQwYjYtOTRmZS01Zjc3ZDM1YzZlMDUifQ.
Gh95kHCOEGq5E_ArMBbDXhwKR577scxYaoJ1P{a lot of characters here}KKJDEg"
```

### <a name="register-your-certificate-with-azure-ad"></a>Registrieren Ihres Zertifikats bei Azure AD
Sie müssen das Anwendungsmanifest bearbeiten, um die Zertifikatanmeldeinformationen der Clientanwendung in Azure AD zuzuordnen.
Wenn Sie über ein Zertifikat verfügen, berechnen Sie Folgendes:
- `$base64Thumbprint`: Die base64-Codierung des Zertifikathashs.
- `$base64Value`: Die base64-Codierung der Zertifikatrohdaten.

Geben Sie außerdem eine GUID an, um den Schlüssel im Anwendungsmanifest (`$keyId`) zu identifizieren.

Öffnen Sie bei der Registrierung der Azure-App für die Clientanwendung das Anwendungsmanifest, und ersetzen Sie mithilfe des folgenden Schemas die Eigenschaft *keyCredentials* durch Ihre neuen Zertifikatsinformationen:
```
"keyCredentials": [
    {
        "customKeyIdentifier": "$base64Thumbprint",
        "keyId": "$keyid",
        "type": "AsymmetricX509Cert",
        "usage": "Verify",
        "value":  "$base64Value"
    }
]
```

Speichern Sie die Änderungen am Anwendungsmanifest, und laden Sie es in Azure AD hoch. Da die keyCredentials-Eigenschaft mehrere Werte besitzen kann, können Sie mehrere Zertifikate hochladen, um eine vielfältigere Schlüsselverwaltung zu erreichen.

