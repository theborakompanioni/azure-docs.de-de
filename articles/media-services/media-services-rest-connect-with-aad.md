---
title: Verwenden der Azure AD-Authentifizierung zum Zugreifen auf die Azure Media Services-API per REST | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie auf die Azure Media Services-API mit Azure Active Directory-Authentifizierung per REST zugreifen.
services: media-services
documentationcenter: 
author: willzhan
manager: SyntaxC4
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: a4531b69c44337c4863016810123f7f89bf7f98f
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---

# <a name="use-azure-ad-authentication-to-access-the-azure-media-services-api-with-rest"></a>Verwenden der Azure AD-Authentifizierung zum Zugreifen auf die Azure Media Services-API per REST

Das Azure Media Services-Team hat die Azure Active Directory-Authentifizierungsunterstützung (Azure AD) für den Azure Media Services-Zugriff veröffentlicht. Außerdem wurde angekündigt, dass geplant ist, die Azure Access Control-Dienstauthentifizierung für den Media Services-Zugriff als veraltet einzustufen. Da jedes Azure-Abonnement und jedes Media Services-Konto einem Azure AD-Mandanten zugeordnet ist, ist die Unterstützung der Azure AD-Authentifizierung mit vielen Vorteilen in Bezug auf die Sicherheit verbunden. Details zu dieser Änderung und Migration (bei Verwendung des Media Services-.NET-SDK für Ihre App) finden Sie in den folgenden Blogbeiträgen und Artikeln:

- [Azure Media Services announces support for AAD and deprecation of ACS authentication](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation) (Azure Media Services-Ankündigung: Unterstützung für AAD und Einstellung der ACS-Authentifizierung)
- [Access the Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md) (Zugreifen auf die Azure Media Services-API mit Azure AD-Authentifizierung)
- [Use Azure AD authentication to access Azure Media Services API with .NET](media-services-dotnet-get-started-with-aad.md) (Verwenden der Azure AD-Authentifizierung zum Zugreifen auf die Azure Media Services-API per .NET)
- [Get started with Azure AD authentication by using the Azure portal](media-services-portal-get-started-with-aad.md) (Erste Schritte mit der Azure AD-Authentifizierung mit dem Azure-Portal)

Einige Kunden müssen ihre Media Services-Lösungen unter den folgenden Einschränkungen entwickeln:

*   Sie verwenden eine Programmiersprache, die nicht Microsoft .NET oder C# ist, oder die Laufzeitumgebung ist nicht Windows.
*   Azure AD-Bibliotheken, z.B. Active Directory-Authentifizierungsbibliotheken, sind für die Programmiersprache nicht verfügbar oder können für die Laufzeitumgebung nicht genutzt werden.

Einige Kunden haben Anwendungen entwickelt, indem die REST-API sowohl für die Access Control-Authentifizierung als auch für den Azure Media Services-Zugriff verwendet wurde. Für diese Kunden benötigen Sie einen Ansatz, bei dem nur die REST-API für die Azure AD-Authentifizierung und den nachfolgenden Azure Media Services-Zugriff verwendet wird. Hierbei besteht keine Abhängigkeit von den Azure AD-Bibliotheken oder vom Media Services-.NET-SDK. In diesem Artikel wird eine Lösung beschrieben und Beispielcode für das Szenario bereitgestellt. Da der Code auf REST-API-Aufrufe beschränkt ist und keine Abhängigkeit von Azure AD- oder Azure Media Services-Bibliotheken besteht, kann er leicht in andere Programmiersprachen übersetzt werden.

> [!IMPORTANT]
> Derzeit wird für Media Services das Azure Access Control-Dienstauthentifizierungsmodell unterstützt. Die Access Control-Authentifizierung gilt aber ab dem 1. Juni 2018 als veraltet. Es wird empfohlen, möglichst bald zum Azure AD-Authentifizierungsmodell zu migrieren.


## <a name="design"></a>Entwurf

In diesem Artikel wird für die Authentifizierung und Autorisierung der folgende Aufbau verwendet:

*  Autorisierungsprotokoll: OAuth 2.0. OAuth 2.0 ist ein Websicherheitsstandard, der sowohl für die Authentifizierung als auch für die Autorisierung gilt. Er wird von Google, Microsoft, Facebook und PayPal unterstützt. Die Ratifizierung ist im Oktober 2012 erfolgt. Microsoft setzt voll auf die Unterstützung von OAuth 2.0 und OpenID Connect. Beide Standards werden in mehreren Diensten und Clientbibliotheken unterstützt, z.B. Azure Active Directory, Open Web Interface for .NET (OWIN) Katana und Azure AD-Bibliotheken.
*  Gewährungstyp: Gewährungstyp „Clientanmeldeinformationen“. Clientanmeldeinformationen stellen in OAuth 2.0 einen von vier Gewährungstypen dar. Er wird häufig für den Azure AD Microsoft Graph-API-Zugriff genutzt.
*  Authentifizierungsmodus: Dienstprinzipal. Der andere Authentifizierungsmodus ist die Benutzerauthentifizierung oder die interaktive Authentifizierung.

Insgesamt sind vier Anwendungen oder Dienste am Azure AD-Authentifizierungs- und -Autorisierungsablauf für die Verwendung von Media Services beteiligt. In der folgenden Tabelle werden die Anwendungen und Dienste sowie der Ablauf beschrieben:

|Anwendungstyp |Anwendung |Ablauf|
|---|---|---|
|Client | Kunden-App oder -Lösung | Diese App (eigentlich: ihr Proxy) wird unter dem Azure AD-Mandanten registriert, in dem sich das Azure-Abonnement und das Mediendienstkonto befinden. Der Dienstprinzipal der registrierten App erhält dann die Rolle „Besitzer“ oder „Mitwirkender“ in der Access Control (IAM) des Mediendienstkontos. Der Dienstprinzipal wird durch die App-Client-ID und den geheimen Clientschlüssel dargestellt. |
|Identitätsanbieter (IDP) | Azure AD als IDP | Der registrierte App-Dienstprinzipal (Client-ID und geheimer Clientschlüssel) wird von Azure AD als IDP authentifiziert. Diese Authentifizierung wird intern und implizit durchgeführt. Wie beim Ablauf mit Clientanmeldeinformationen auch, wird anstelle des Benutzers der Client authentifiziert. |
|Secure Token Service (STS)/OAuth-Server | Azure AD als STS | Nach der Authentifizierung durch den IDP (bzw. im OAuth 2.0-Kontext den „OAuth-Server“) wird von Azure AD als STS/OAuth-Server ein Zugriffstoken oder JSON Web Token (JWT) für den Zugriff auf die Ressource der mittleren Ebene ausgestellt: in unserem Fall ist dies der Media Services-REST-API-Endpunkt. |
|Ressource | Media Services-REST-API | Jeder Aufruf der Media Services-REST-API wird von einem Zugriffstoken autorisiert, das von Azure AD als STS oder vom OAuth-Server ausgestellt wird. |

Wenn Sie den Beispielcode ausführen und ein JWT oder Zugriffstoken ausführen, verfügt das JWT über die folgenden Attribute:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Hier sind die Zuordnungen zwischen den Attributen im JWT und den vier Anwendungen bzw. Diensten der vorherigen Tabelle angegeben:

|Anwendungstyp |Anwendung |JWT-Attribut |
|---|---|---|
|Client- |Kunden-App oder -Lösung |appid: „02ed1e8e-af8b-477e-af3d-7e7219a99ac6“. Die Client-ID einer Anwendung, die Sie im nächsten Abschnitt für Azure AD registrieren. |
|Identitätsanbieter (IDP) | Azure AD als IDP |idp: „https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/“.  Die GUID ist die ID des Microsoft-Mandanten (microsoft.onmicrosoft.com). Jeder Mandant verfügt über eine eigene eindeutige ID. |
|Secure Token Service (STS)/OAuth-Server |Azure AD als STS | iss: „https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/“. Die GUID ist die ID des Microsoft-Mandanten (microsoft.onmicrosoft.com). |
|Ressource | Media Services-REST-API |aud: „https://rest.media.azure.net“. Der Empfänger oder die Zielgruppe des Zugriffstokens. |

## <a name="steps-for-setup"></a>Setup-Schritte

Führen Sie die folgenden Schritte aus, um eine Azure AD-Anwendung für die Azure AD-Authentifizierung zu registrieren und einzurichten und ein Zugriffstoken zum Aufrufen des Endpunkts der Azure Media Services-REST-API zu beschaffen:

1.  Registrieren Sie im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) eine Azure AD-Anwendung (z.B. „wzmediaservice“) für den Azure AD-Mandanten (z.B. „microsoft.onmicrosoft.com“). Es spielt keine Rolle, ob Sie die Registrierung als Web-App oder native App vornehmen. Außerdem können Sie eine beliebige Anmelde-URL und Antwort-URL auswählen (z.B. jeweils „http://wzmediaservice.com“).
2. Navigieren Sie im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) zur Registerkarte **Konfigurieren** Ihrer Anwendung. Notieren Sie sich die **Client-ID**. Generieren Sie dann unter **Schlüssel** einen **Clientschlüssel** (geheimer Clientschlüssel). 

    > [!NOTE] 
    > Notieren Sie sich den geheimen Clientschlüssel. Er wird nicht noch einmal angezeigt.
    
3.  Navigieren Sie im [Azure-Portal](http://ms.portal.azure.com) zum Media Services-Konto. Wählen Sie den Bereich **Zugriffssteuerung** (IAM). Fügen Sie ein neues Mitglied hinzu, das entweder über die Rolle „Besitzer“ oder „Mitwirkender“ verfügt. Suchen Sie für den Prinzipal nach dem Anwendungsnamen, den Sie in Schritt 1 registriert haben (in diesem Beispiel „wzmediaservice“).

## <a name="info-to-collect"></a>Zu sammelnde Informationen

Sammeln Sie als Vorbereitung auf die REST-Codierung die folgenden Datenpunkte zum Einfügen in den Code:

*   Azure AD als STS-Endpunkt: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Von diesem Endpunkt aus wird ein JWT-Zugriffstoken angefordert. Azure AD dient nicht nur als IDP, sondern auch als STS. Azure AD stellt ein JWT für den Ressourcenzugriff aus (ein Zugriffstoken). Ein JWT-Token verfügt über verschiedene Ansprüche.
*   Azure Media Services-REST-API als Ressource oder Zielgruppe: https://rest.media.azure.net.
*   Client-ID: Siehe Schritt 2 unter [Setup-Schritte](#steps-for-setup).
*   Geheimer Clientschlüssel: Siehe Schritt 2 unter [Setup-Schritte](#steps-for-setup).
*   Ihr REST-API-Endpunkt des Media Services-Kontos im folgenden Format:

    https://[mediendienstkonto-name].restv2.[rechenzentrum].media.azure.net/API 

    Dies ist der Endpunkt, über den alle Aufrufe der Media Services-REST-API in Ihrer Anwendung durchgeführt werden. Beispiel: https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Sie können diese fünf Parameter dann in Ihre web.config- oder app.config-Datei einfügen und im Code verwenden.

## <a name="sample-code"></a>Beispielcode

Den Beispielcode finden Sie unter [Azure AD Authentication for Azure Media Services Access: Both via REST API](https://github.com/willzhan/WAMSRESTSoln) (Azure AD-Authentifizierung für Azure Media Services-Zugriff: Jeweils per REST-API).

Der Beispielcode besteht aus zwei Teilen:

*   Einem DLL-Bibliotheksprojekt mit dem gesamten REST-API-Code für die Azure AD-Authentifizierung und -Autorisierung. Außerdem enthält er eine Methode zum Durchführen von REST-API-Aufrufen des Media Services-REST-API-Endpunkts mit dem Zugriffstoken.
*   Einem Konsolentestclient, mit dem die Azure AD-Authentifizierung initiiert und verschiedene Media Services-REST-API-Instanzen aufgerufen werden.

Das Beispielprojekt verfügt über drei Features:

*   Azure AD-Authentifizierungen über die Gewährung von Clientanmeldeinformationen unter ausschließlicher Verwendung der REST-API.
*   Azure Media Services-Zugriff unter ausschließlicher Verwendung der REST-API.
*   Azure Storage-Zugriff unter ausschließlicher Verwendung der REST-API (wie beim Erstellen eines Media Services-Kontos per REST-API).


## <a name="where-is-the-refresh-token"></a>Wo ist das Aktualisierungstoken?

Einige Leser fragen sich vielleicht Folgendes: Wo ist das Aktualisierungstoken? Warum wird hier kein Aktualisierungstoken genutzt?

Der Zweck eines Aktualisierungstokens besteht nicht darin, ein Zugriffstoken zu aktualisieren. Stattdessen ist es so konzipiert, dass die Endbenutzerauthentifizierung bzw. der Benutzereingriff umgangen wird und trotzdem ein gültiges Zugriffstoken verfügbar ist, wenn ein früheres Token abläuft. Ein besserer Name für ein Aktualisierungstoken wäre daher beispielsweise „Token zur Umgehung der erneuten Benutzeranmeldung“.

Wenn Sie den Ablauf zur Gewährung der OAuth 2.0-Autorisierung verwenden (Benutzername und Kennwort, im Namen eines Benutzers), dient ein Aktualisierungstoken zum Beschaffen eines erneuerten Zugriffstokens ohne Anforderung des Benutzereingriffs. Für den Ablauf zur Gewährung von OAuth 2.0-Clientanmeldeinformationen, den wir in diesem Artikel beschreiben, fungiert der Client in eigenem Namen. Sie benötigen keinerlei Benutzereingriff, und der Autorisierungsserver muss (und wird) kein Aktualisierungstoken für Sie bereitstellen. Wenn Sie die **GetUrlEncodedJWT**-Methode debuggen, fällt Ihnen auf, dass die Antwort vom Tokenendpunkt ein Zugriffstoken enthält, aber kein Aktualisierungstoken.

## <a name="next-steps"></a>Nächste Schritte

Beginnen Sie mit dem [Hochladen von Dateien in Ihr Konto](media-services-dotnet-upload-files.md).

