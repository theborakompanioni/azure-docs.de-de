---
title: "Azure AD v2 – ASP.NET-Webserver: Erste Schritte – Konfiguration | Microsoft-Dokumentation"
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
ms.openlocfilehash: ad4160abfef748033eeb84a2f07a37124ce3b4b1
ms.contentlocale: de-de


---

## <a name="create-an-application-express"></a>Erstellen einer Anwendung (Express)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=serverSideWebApp&appTech=aspNetWebAppOwin&step=configure).
2.    Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.    Stellen Sie sicher, dass die Option für geführtes Setup aktiviert ist.
4.    Befolgen Sie die Anweisungen, um Ihrer Anwendung eine Umleitungs-URL hinzufügen.

## <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Hinzufügen der Registrierungsinformationen Ihrer Anwendung zur Projektmappe (für Fortgeschrittene)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:
1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3.    Stellen Sie sicher, dass die Option für geführtes Setup deaktiviert ist.
4.    Klicken Sie auf `Add Platforms`, und wählen Sie dann `Web` aus.
5.    Kehren Sie zurück zu Visual Studio, und wählen Sie im Projektmappen-Explorer das Projekt aus. Untersuchen Sie das Fenster „Eigenschaften“ (wenn es nicht angezeigt wird, drücken Sie F4).
6.    Ändern Sie „SSL-fähig“ in `True`:<br/><br/>![Projekteigenschaften](media/active-directory-serversidewebapp-aspnetwebappowin-configure/vsprojectproperties.png)<br />

7.    Kopieren Sie die SSL-URL, und fügen Sie diese URL zur Liste der Umleitungs-URLs im Registrierungsportal hinzu.
8.    Fügen Sie im Stammordner der Datei `web.config` im Abschnitt `configuration\appSettings` Folgendes hinzu:

```xml
<add key="ClientId" value="Enter_the_Application_Id_here" />
<add key="redirectUri" value="Enter_the_Redirect_URL_here" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
<!-- Workaround for Docs conversion bug -->
<ol start="9">
<li>
Ersetzen Sie `ClientId` durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
<li>
Ersetzen Sie `redirectUri` durch die SSL-URL Ihres Projekts.
</li>
</ol>
<!-- End Docs -->

