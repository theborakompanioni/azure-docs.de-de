---
title: "Angeleitetes Setup für Azure AD v2 JS SPA – Konfigurieren | Microsoft-Dokumentation"
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
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: adff529bfdc40006666cc643d49a302d7f1d09ad
ms.contentlocale: de-de

---
## <a name="register-your-application"></a>Anwendung registrieren

Es gibt mehrere Möglichkeiten, eine Anwendung zu erstellen – bitte wählen Sie eine aus:

### <a name="option-1-register-your-application-express-mode"></a>Option 1: Registrieren Sie Ihre Anwendung (Expressmodus)
Nun müssen Sie Ihre Anwendung im *Microsoft-Anwendungsregistrierungsportal* registrieren:

1.  Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure).
2.  Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein.
3.  Stellen Sie sicher, dass die Option *Geführtes Setup* aktiviert ist.
4.  Befolgen Sie die Anweisungen zum Abrufen der Anwendungs-ID, und fügen Sie sie in Ihren Code ein.

### <a name="option-2-register-your-application-advanced-mode"></a>Option 2: Registrieren Sie Ihre Anwendung (Erweiterter Modus)

1. Registrieren Sie Ihre Anwendung im [Microsoft-Anwendungsregistrierungsportal](https://apps.dev.microsoft.com/portal/register-app).
2. Geben Sie einen Namen für Ihre Anwendung und Ihre E-Mail-Adresse ein. 
3. Stellen Sie sicher, dass die Option *Geführtes Setup* deaktiviert ist.
4.  Klicken Sie auf `Add Platform`, und wählen Sie dann `Web` aus.
5. Fügen Sie die `Redirect URL` hinzu, die der URL der Anwendung Ihrem Webserver gemäß entspricht. Informationen über Anweisungen zum Festlegen/Abrufen der Umleitungs-URL in Visual Studio und Python finden Sie in den folgenden Abschnitten.
6. Klicken Sie unten auf der Seite auf *Speichern*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-Anweisungen zum Abrufen der Umleitungs-URL
> Zum Abrufen Ihrer Umleitungs-URL befolgen Sie die folgenden Anweisungen:
> 1.    Wählen Sie im *Projektmappen-Explorer* das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie `F4`).
> 2.    Kopieren Sie den Wert von `URL` in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Wechseln Sie zurück zum *Portal für die Anwendungsregistrierung*, fügen Sie den Wert als `Redirect URL` ein, und klicken Sie auf „Speichern“.

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Festlegen der Umleitungs-URL für Python
> Für Python können Sie den Webserverport über die Befehlszeile festlegen. Diese ausführliche Anleitung verwendet den Port 8080. Sie können aber auch beliebige andere verfügbare Ports verwenden. Befolgen Sie in jedem Fall die nachstehenden Anweisungen zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> - Wechseln Sie zurück zum *Portal für die Anwendungsregistrierung*, und legen Sie `http://localhost:8080/` als `Redirect URL` fest, oder verwenden Sie `http://localhost:[port]/`, wenn Sie einen benutzerdefinierten TCP-Port einsetzen (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist), und klicken Sie auf „Speichern“.


#### <a name="configure-your-javascript-spa"></a>Konfigurieren Ihrer JavaScript-SPA

1.  Erstellen Sie eine Datei namens `msalconfig.js` mit den Informationen für die Anwendungsregistrierung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`. Vergeben Sie den Namen `msalconfig.js`.
2.  Fügen Sie Ihrer Datei `msalconfig.js` den folgenden Code hinzu:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Ersetzen Sie <code>Enter_the_Application_Id_here</code> durch die Anwendungs-ID, die Sie gerade registriert haben.
</li>
</ol>

