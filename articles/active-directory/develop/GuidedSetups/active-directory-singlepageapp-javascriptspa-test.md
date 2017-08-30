---
title: "Geführte Einrichtung für Azure AD v2 JS SPA: Testen| Microsoft-Dokumentation"
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
ms.openlocfilehash: c559c80781da3631a783d96539622c4c89fe7e17
ms.contentlocale: de-de
ms.lasthandoff: 08/19/2017

---
## <a name="test-your-code"></a>Testen Ihres Codes

> ### <a name="testing-with-visual-studio"></a>Testen mit Visual Studio
> Wenn Sie Visual Studio verwenden, drücken Sie `F5`, um Ihr Projekt auszuführen. Der Browser wird geöffnet und leitet Sie zu *http://localhost:{Port}*. Die Schaltfläche *Microsoft Graph-API aufrufen* wird hier angezeigt.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testen mit Python oder einem anderen Webserver
> Wenn Sie nicht Visual Studio verwenden, stellen Sie sicher, dass Ihr Webserver gestartet wurde und für das Lauschen an einem TCP-Port basierend auf dem Ordner mit Ihrer Datei *index.html* konfiguriert ist. Bei Python können Sie mit dem Lauschen am Port beginnen, indem Sie im Ordner der App den Befehl über die Eingabeaufforderung oder das Terminal ausführen:
> 
> ```bash
> python -m http.server 8080
> ```
>  Öffnen Sie den Browser, und geben Sie *http://localhost:8080* oder *http://localhost:{Port}* ein. Hierbei entspricht *Port* dem Port, an dem Ihr Webserver lauscht. Über die Schaltfläche *Microsoft Graph-API aufrufen* können Sie den Inhalt Ihrer Seite „index.html“ anzeigen.

## <a name="test-your-application"></a>Testen Ihrer Anwendung

Nachdem der Browser Ihre Datei *index.html* geladen hat, klicken Sie auf die Schaltfläche *Microsoft Graph-API aufrufen*. Wenn dies das erste Mal ist, leitet den Browser Sie zum Microsoft Azure Active Directory v2-Endpunkt um. Sie werden aufgefordert, sich anzumelden.
 
![Beispielscreenshot](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Zustimmung
Bei der allerersten Anmeldung bei Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem Sie Ihre Zustimmung geben müssen:

 ![Genehmigungsbildschirm](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Erwartete Ergebnisse
Es sollten Benutzerprofilinformationen angezeigt werden, die als Antwort auf den Microsoft Graph-API-Aufruf zurückgegeben werden.
 
 ![Ergebnisse](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Es werden außerdem in den Feldern *Zugriffstoken* und *ID-Tokenansprüche* grundlegende Informationen zum abgerufenen Token angezeigt.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich `user.read`, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die in unserem Registrierungsportal registriert wird. Einige andere Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern zusätzliche Bereiche. Für Microsoft Graph ist es z.B. erforderlich, dass der Bereich `Calendars.Read` die Kalender des Benutzers auflistet. Um auf den Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung `Calendars.Read` hinzufügen und dann dem Aufruf von `acquireTokenSilent` den Bereich `Calendars.Read` hinzufügen. Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

Wenn eine Back-End-API keinen Bereich erfordert (nicht empfohlen), können Sie `clientId` als Bereich in den Aufrufen `acquireTokenSilent` und/oder `acquireTokenRedirect` verwenden.

<!--end-collapse-->

