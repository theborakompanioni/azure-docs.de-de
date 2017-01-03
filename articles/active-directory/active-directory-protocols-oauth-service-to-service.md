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
ms.date: 10/03/2016
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: 3fdd561bba898b0e790f69a82ed731d7fd87d8c8
ms.openlocfilehash: 7cf9fe659a91816e42394c1377232e122fe924a6


---
# <a name="service-to-service-calls-using-client-credentials"></a>Verwenden von Clientanmeldeinformationen für Dienst-zu-Dienst-Aufrufe
Beim Fluss zum Erteilen von OAuth 2.0-Clientanmeldeinformationen kann ein Webdienst (ein *vertraulicher Client*) seine eigenen Anmeldeinformationen zum Authentifizieren verwenden, wenn ein anderer Webdienst aufgerufen wird, anstatt die Identität eines anderen Benutzers anzunehmen. In diesem Szenario ist der Client normalerweise ein Webdienst der mittleren Ebene, ein Daemondienst oder eine Website.

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
Eine Dienst-zu-Dienst-Zugriffstokenanforderung enthält die folgenden Parameter:

| Parameter |  | Beschreibung |
| --- | --- | --- |
| grant_type |erforderlich |Gibt den angeforderten Antworttyp an. In einem Fluss zur Gewährung von Clientanmeldeinformationen muss der Wert **client_credentials** lauten. |
| client_id |erforderlich |Gibt die Azure AD-Client-ID des aufrufenden Webdiensts an. Klicken Sie zum Ermitteln der Client-ID der aufrufenden Anwendung im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis, die Anwendung und dann auf **Konfigurieren**. |
| client_secret |erforderlich |Geben Sie einen Schlüssel ein, der für den aufrufenden Webdienst in Azure AD registriert ist. Klicken Sie zum Erstellen eines Schlüssels im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis, die Anwendung und dann auf **Konfigurieren**. |
| resource |erforderlich |Geben Sie den App-ID-URI des empfangenden Webdiensts ein. Klicken Sie zum Ermitteln des App-ID-URI im Azure-Verwaltungsportal nacheinander auf **Active Directory**, das Verzeichnis, die Anwendung und dann auf **Konfigurieren**. |

## <a name="example"></a>Beispiel
Mit dem folgenden HTTP POST-Element wird ein Zugriffstoken für den Webdienst „https://service.contoso.com/“ angefordert. Mit der `client_id` wird der Webdienst identifiziert, der das Zugriffstoken anfordert.

```
POST contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

## <a name="service-to-service-access-token-response"></a>Dienst-zu-Dienst-Zugriffstokenantwort
Eine erfolgreiche Antwort enthält eine JSON OAuth 2.0-Antwort mit den folgenden Parametern:

| Parameter | Beschreibung |
| --- | --- |
| access_token |Das angeforderte Zugriffstoken. Der aufrufende Webdienst kann dieses Token verwenden, um die Authentifizierung für den empfangenden Webdienst durchzuführen. |
| access_type |Gibt den Wert des Tokentyps an. **Bearertoken**ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertoken finden Sie unter [OAuth 2.0 Authorization Framework: Bearer Token Usage (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)(OAuth 2.0-Autorisierungsframework: Verwendung von Bearertoken (RFC 6750)). |
| expires_in |Gibt an, wie lange das Zugriffstoken (in Sekunden) gültig ist. |
| expires_on |Die Uhrzeit, zu der das Zugriffstoken abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. Dieser Wert wird verwendet, um die Lebensdauer von zwischengespeicherten Token zu bestimmen. |
| resource |Der App-ID-URI des empfangenden Webdiensts. |

## <a name="example"></a>Beispiel
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



<!--HONumber=Dec16_HO4-->


