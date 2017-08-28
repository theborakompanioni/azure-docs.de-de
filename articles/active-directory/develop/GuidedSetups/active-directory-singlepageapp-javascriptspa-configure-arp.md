---
title: "Angeleitetes Setup für Azure AD v2 JS SPA – Konfigurieren (ARP) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie JavaScript SPA-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt (ARP) anfordert."
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
ms.openlocfilehash: 735559464db4c237ff7498b7350f2385090fa4e4
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
## <a name="add-the-applications-registration-information-to-your-app"></a>Hinzufügen von Registrierungsinformationen für die Anwendung zu Ihrer App

In diesem Schritt müssen Sie die Umleitungs-URL der Registrierungsinformationen für die Anwendung konfigurieren und dann die Anwendungs-ID zu Ihrer JavaScript-Einzelseitenanwendung hinzufügen.

### <a name="configure-redirect-url"></a>Konfigurieren der Umleitungs-URL

Konfigurieren Sie das Feld `Redirect URL` mit der URL für Ihre Seite „index.html“ basierend auf Ihrem Webserver, und klicken Sie dann auf *Aktualisieren*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio-Anweisungen zum Abrufen der Umleitungs-URL
> Zum Abrufen Ihrer Umleitungs-URL befolgen Sie die unten stehenden Anweisungen:
> 1.    Wählen Sie im *Projektmappen-Explorer* das Projekt aus. Untersuchen Sie das Fenster `Properties` (wenn es nicht angezeigt wird, drücken Sie `F4`).
> 2.    Kopieren Sie den Wert von `URL` in die Zwischenablage:<br/> ![Projekteigenschaften](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Fügen Sie den Wert als `Redirect URL` oben auf dieser Seite ein, und klicken Sie dann auf `Update`.

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Festlegen der Umleitungs-URL für Python
> Für Python können Sie den Webserverport über die Befehlszeile festlegen. Diese ausführliche Anleitung verwendet den Port 8080. Sie können aber auch beliebige andere verfügbare Ports verwenden. Befolgen Sie in jedem Fall die nachstehenden Anweisungen zum Einrichten einer Umleitungs-URL in den Registrierungsinformationen der Anwendung:<br/>
> Legen Sie `http://localhost:8080/` oben auf dieser Seite als eine `Redirect URL` fest, oder verwenden Sie `http://localhost:[port]/` bei einem benutzerdefinierten TCP-Port (wobei *[port]* die benutzerdefinierte TCP-Portnummer ist). Klicken Sie anschließend auf „Aktualisieren“.

### <a name="configure-your-javascript-spa-application"></a>Konfigurieren Ihrer JavaScript-Einzelseitenanwendung

1.  Erstellen Sie eine Datei namens `msalconfig.js` mit den Informationen für die Anwendungsregistrierung. Wenn Sie Visual Studio verwenden, wählen Sie das Projekt aus (Stammordner des Projekts), klicken Sie mit der rechten Maustaste, und wählen Sie Folgendes aus: `Add` > `New Item` > `JavaScript File`. Vergeben Sie den Namen `msalconfig.js`.
2.  Fügen Sie Ihrer Datei `msalconfig.js` den folgenden Code hinzu:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 

