---
title: "Azure AD v2 – Android – Erste Schritte – Einführung | Microsoft-Dokumentation"
description: Informationen, wie eine Android-App ein Zugriffstoken abrufen und die Microsoft Graph-API oder APIs aufrufen kann, die Zugriffstoken vom Azure Active Directory-v2-Endpunkt erfordern
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
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 52037fc0cd20d283b1e88482cc0303feb81534f9
ms.contentlocale: de-de


---

# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Aufrufen der Microsoft Graph-API aus einer Android-App

Diese Anleitung veranschaulicht, wie eine native Android-Anwendung ein Zugriffstoken abrufen und die Microsoft Graph-API und andere APIs aufrufen kann, die Zugriffstoken von einem Azure Active Directory v2-Endpunkt anfordern.

Am Ende dieser Anleitung kann Ihre Anwendung eine geschützte API sowohl mit persönlichen Konten (z.B. outlook.com, live.com u.a.) als auch Geschäfts,- Schul- und Unikonten von Unternehmen oder Organisationen aufrufen, die mit Azure Active Directory arbeiten.  

### <a name="how-this-sample-works"></a>Funktionsweise dieses Beispiels
![Funktionsweise dieses Beispiels](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Das in dieser Anleitung erstellte Beispiel basiert auf einem Szenario, bei dem mit einer Android-Anwendung eine Web-API abgefragt wird, die Token vom Azure Active Directory v2-Endpunkt akzeptiert, in diesem Fall Microsoft Graph-API. In diesem Szenario wird ein Token über den Autorisierungsheader HTTP-Anforderungen hinzugefügt. Tokenabruf und -verlängerung werden von der Microsoft Authentication Library (MSAL) verarbeitet.

### <a name="pre-requisites"></a>Voraussetzungen
* Dieses geführte Setup konzentriert sich auf Android Studio. Andere Android-Anwendungsentwicklungsumgebungen sind jedoch ebenfalls akzeptabel. 
* Android SDK 21 oder höher ist erforderlich (SDK 25 empfohlen).
* Google Chrome oder ein Webbrowser mit benutzerdefinierten Registerkarten ist für diese Version der Microsoft Authentication Library (MSAL) für Android erforderlich.

> Hinweis: Google Chrome ist nicht im Visual Studio-Emulator für Android enthalten. Wir empfehlen Ihnen, diesen Code in einem Emulator mit API 25 oder einem Image mit API 21 oder höher zu testen, in dem Google Chrome installiert ist.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Verarbeiten des Tokenabrufs für den Zugriff auf eine geschützte Web-API

Nach der Authentifizierung des Benutzers empfängt die Beispielanwendung ein Token, mit dem eine Microsoft Graph-API oder Web-API abgefragt werden kann, die von Microsoft Azure Active Directory v2 geschützt wird.

APIs wie Microsoft Graph erfordern ein Zugriffstoken, um den Zugriff auf bestimmte Ressourcen zu ermöglichen, z.B. zum Lesen eines Benutzerprofils, Zugreifen auf den Kalender eines Benutzers oder Senden einer E-Mail. Ihre Anwendung kann ein Zugriffstoken mithilfe der MSAL anfordern, um auf diese Ressourcen durch Angeben von API-Bereichen zuzugreifen. Dieses Zugriffstoken wird dann dem HTTP-Autorisierungsheader für jeden Aufruf hinzugefügt, der für die geschützte Ressource erfolgt. 

Die MSAL verwaltet das Zwischenspeichern und Aktualisieren von Zugriffstoken, damit Ihre Anwendung dies nicht übernehmen muss.

### <a name="libraries"></a>Bibliotheken

In dieser Anleitung werden die folgenden Bibliotheken verwendet:

|Bibliothek|Beschreibung|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|

