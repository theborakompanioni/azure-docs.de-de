---
title: "Azure AD v2 – ASP.NET-Webserver: Erste Schritte – Einführung | Microsoft-Dokumentation"
description: "Informationen zum Implementieren einer Microsoft-Anmeldung in einer ASP.NET-Projektmappe mittels einer herkömmlichen webbrowserbasierten Anwendung unter Verwendung des Standards OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: ef74361c7a15b0eb7dad1f6ee03f8df707a7c05e
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.contentlocale: de-de


---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Hinzufügen von „Mit Microsoft anmelden“ zu einer ASP.NET-Web-App

Diese Anleitung veranschaulicht das Implementieren von „Mit Microsoft anmelden“ mithilfe einer ASP.NET MVC-Projektmappe mit einer herkömmlichen browserbasierten Anwendung mittels OpenID Connect. 

Am Ende dieser Anleitung kann Ihre Anwendung Anmeldungen von sowohl persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen akzeptieren, die in Azure Active Directory integriert wurden. 

> Für diese Anleitung ist Visual Studio 2015 Update 3 oder Visual Studio 2017 erforderlich.  Sie haben beides nicht?  [Laden Sie Visual Studio 2017 kostenlos herunter](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise dieser Anleitung](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Diese Anleitung basiert auf dem Szenario, in dem ein Browser auf eine ASP.NET-Website zugreift und ein Benutzer aufgefordert wird, sich über die Schaltfläche „Anmelden“ anzumelden. In diesem Szenario wird ein Großteil der Arbeit zum Rendern der Webseite auf dem Server erledigt.

## <a name="libraries"></a>Bibliotheken

In dieser Anleitung werden die folgenden Bibliotheken verwendet:

|Bibliothek|Beschreibung|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware, die einer Anwendung das Verwenden von OpenIDConnect für die Authentifizierung ermöglicht|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware, die einer Anwendung das Beibehalten der Benutzersitzung mithilfe von Cookies ermöglicht|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Ermöglicht OWIN-basierten Anwendungen die Ausführung in IIS mithilfe der ASP.NET-Anforderungspipeline|


