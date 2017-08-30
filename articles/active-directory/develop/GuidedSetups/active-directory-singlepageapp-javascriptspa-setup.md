---
title: "Geführte Einrichtung von Azure AD v2 JS SPA: Setup | Microsoft-Dokumentation"
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
ms.sourcegitcommit: 847eb792064bd0ee7d50163f35cd2e0368324203
ms.openlocfilehash: a399d35358ecf188070e699f4d975a415bd4649d
ms.contentlocale: de-de

---
## <a name="setting-up-your-web-server-or-project"></a>Einrichten Ihres Webservers oder Projekts

> Möchten Sie stattdessen das Projekt dieses Beispiels herunterladen? 
> - [Laden Sie das Visual Studio-Projekt herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/VisualStudio.zip)
>
> oder
> - [Laden Sie die Projektdateien für einen lokalen Webserver herunter](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/core.zip), z.B. Python
>
> Fahren Sie dann mit dem Schritt [Konfiguration](#create-an-application-express) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen
Ein lokaler Webserver wie z.B. [Python http.server](https://www.python.org/downloads/), [http-server](https://www.npmjs.com/package/http-server/), [.NET Core](https://www.microsoft.com/net/core) oder eine IIS Express-Integration in [Visual Studio 2017](https://www.visualstudio.com/downloads/) ist für die Ausführung dieser geführten Einrichtung erforderlich. 

Die Anweisungen in diesem Leitfaden basieren auf Python und Visual Studio 2017. Sie können aber auch eine andere Entwicklungsumgebung oder einen anderen Webserver verwenden.

## <a name="create-your-project"></a>Erstellen Ihres Projekts 

> ### <a name="option-1-visual-studio"></a>Option 1: Visual Studio 
> Wenn Sie Visual Studio verwenden und ein neues Projekt erstellen, führen Sie die folgenden Schritte aus, um eine neue Visual Studio-Projektmappe zu erstellen:
> 1.    In Visual Studio:  `File` > `New` > `Project`
> 2.    Wählen Sie unter `Visual C#\Web` die Option `ASP.NET Web Application (.NET Framework)` aus.
> 3.    Geben Sie der Anwendung einen Namen, und klicken Sie auf *OK*.
> 4.    Wählen Sie unter `New ASP.NET Web Application` die Option `Empty` aus.

<p/><!-- -->

> ### <a name="option-2-python-other-web-servers"></a>Option 2: Python/Andere Webserver
> Vergewissern Sie sich, dass Sie [Python](https://www.python.org/downloads/) installiert haben, und führen Sie dann den folgenden Schritt aus:
> - Erstellen Sie einen Ordner zum Hosten Ihrer Anwendung.


## <a name="create-your-single-page-applications-ui"></a>Erstellen der Benutzeroberfläche für die Einzelseitenanwendung
1.  Erstellen Sie die Datei *index.html* für Ihre JavaScript-Einzelseitenanwendung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `HTML page`. Geben Sie der Datei den Namen „index.html“.
2.  Fügen Sie Ihrer Seite den folgenden Code hinzu:
```html
<!DOCTYPE html>
<html>
<head>
    <!-- bootstrap reference used for styling the page -->
    <link href="//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <title>JavaScript SPA Guided Setup</title>
</head>
<body style="margin: 40px">
    <button id="callGraphButton" type="button" class="btn btn-primary" onclick="callGraphApi()">Call Microsoft Graph API</button>
    <div id="errorMessage" class="text-danger"></div>
    <div class="hidden">
        <h3>Graph API Call Response</h3>
        <pre class="well" id="graphResponse"></pre>
    </div>
    <div class="hidden">
        <h3>Access Token</h3>
        <pre class="well" id="accessToken"></pre>
    </div>
    <div class="hidden">
        <h3>ID Token Claims</h3>
        <pre class="well" id="userInfo"></pre>
    </div>
    <button id="signOutButton" type="button" class="btn btn-primary hidden" onclick="signOut()">Sign out</button>

    <!-- This app uses cdn to reference msal.js (recommended). 
         You can also download it from: https://github.com/AzureAD/microsoft-authentication-library-for-js -->
    <script src="https://secure.aadcdn.microsoftonline-p.com/lib/0.1.1/js/msal.min.js"></script>

    <!-- The 'bluebird' and 'fetch' references below are required if you need to run this application on Internet Explorer -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js"></script>

    <script type="text/javascript" src="msalconfig.js"></script>
    <script type="text/javascript" src="app.js"></script>
</body>
</html>
````

