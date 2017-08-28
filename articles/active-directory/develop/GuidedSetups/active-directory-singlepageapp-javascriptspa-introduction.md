---
title: "Angeleitetes Setup für Azure AD v2 JS SPA – Einführung | Microsoft-Dokumentation"
description: "Informationen, wie JavaScript SPA-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: 4f40ba0eb6ce3e55a1e5458bc6e83060fcf6624c
ms.contentlocale: de-de

---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Aufrufen der Microsoft Graph-API aus einer JavaScript-Einzelseitenanwendung

Diese Anleitung veranschaulicht, wie eine JavaScript-Einzelseitenanwendung eine Anmeldung für private Konten und Geschäfts-, Schul- oder Unikonten durchführen, ein Zugriffstoken abrufen und die Microsoft Graph-API oder andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt benötigen.

### <a name="how-this-guide-works"></a>Funktionsweise dieser Anleitung

![Funktionsweise der über diesen Leitfaden generierten Beispiel-App](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Die über diesen Leitfaden erstellte Beispiel-App ermöglicht eine JavaScript-Einzelseitenanwendung das Abfragen der Microsoft Graph-API oder einer Web-API, die Token von einem Azure Active Directory v2-Endpunkt akzeptiert. In diesem Szenario wird nach der Benutzeranmeldung ein Zugriffstoken angefordert und über den Autorisierungsheader den HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotheken

In diesem Leitfaden werden die folgenden Bibliotheken verwendet:

|Bibliothek|Beschreibung|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Microsoft Authentication Library für JavaScript-Vorschau|

> [!NOTE]
> *msal.js* hat als Ziel den *Azure Active Directory-v2-Endpunkt*, der privaten und Geschäfts-, Schul- oder Unikonten das Anmelden sowie das Abrufen von Token ermöglicht. Der *Azure Active Directory-v2-Endpunkt* hat [einige Einschränkungen](..\active-directory-v2-limitations.md). Wenn Sie lediglich an Geschäfts-, Schul- oder Unikonten interessiert sind, verwenden Sie *adal.js* und den *v1-Endpunkt*. Um die Unterschiede zwischen v1- und v2-Endpunkten zu verstehen, lesen den [v1-v2-Vergleich](..\active-directory-v2-compare.md).

<!--end-collapse-->

