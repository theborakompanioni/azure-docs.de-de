---
title: "Übersicht über das Azure AD .NET-Protokoll | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mit Azure AD verwenden.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
translationtype: Human Translation
ms.sourcegitcommit: f1e4b86a04a76513a2f0d9a9f89e49611c0447d5
ms.openlocfilehash: b31fa50a62d5b26a7346f212076ec3a2b0386f5e


---
<!--TODO: Introduction -->

## <a name="register-your-application-with-your-ad-tenant"></a>Registrieren der Anwendung beim AD-Mandanten
Zuerst müssen Sie Ihre Anwendung beim Azure Active Directory-Mandanten (AAD) registrieren. Hierbei erhalten Sie eine Anwendungs-ID für die Anwendung, und die Aktivierung für den Empfang von Token wird durchgeführt.

* Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
* Wählen Sie Ihren Azure AD-Mandanten aus, indem Sie in der oberen rechten Ecke der Seite auf Ihr Konto klicken.
* Klicken Sie im linken Navigationsbereich auf **Azure Active Directory**.
* Klicken Sie auf **App-Registrierungen**, und klicken Sie auf **Hinzufügen**.
* Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Es spielt bei diesem Tutorial keine Rolle, ob es sich um eine Webanwendung oder eine native Anwendung handelt. Falls Sie spezielle Beispiele für Webanwendungen oder native Anwendungen wünschen, können Sie sich unsere [Schnellstarts](../articles/active-directory/develop/active-directory-developers-guide.md) ansehen.
  * Geben Sie für Webanwendungen die **Anmelde-URL** an. Dies ist die Basis-URL Ihrer App, unter der sich Benutzer anmelden können, z.B. `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Geben Sie für native Anwendungen einen **Umleitungs-URI** an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.
* Nach Abschluss der Registrierung weist Azure AD Ihrer Anwendung eine eindeutige Client-ID zu, die Anwendungs-ID. Diesen Wert benötigen Sie in den nächsten Abschnitten, weswegen Sie ihn von der Anwendungsseite kopieren sollten.



<!--HONumber=Jan17_HO3-->


