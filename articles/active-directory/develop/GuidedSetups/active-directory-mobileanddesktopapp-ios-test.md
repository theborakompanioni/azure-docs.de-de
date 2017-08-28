---
title: "Erste Schritte in Azure AD v2 iOS – Test | Microsoft-Dokumentation"
description: "Informationen, wie iOS (Swift)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
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
ms.date: 05/09/2017
ms.author: andret
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 2420723eed9ae8ba1ba6156482ef4b4de9ce2fc0
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testen Sie das Abfragen der Microsoft Graph-API von Ihrer iOS-Anwendung aus

Drücken Sie `Command` + `R`, um den Code im Simulator auszuführen.

![Beispielscreenshot](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Wenn Sie bereit zum Testen sind, tippen Sie auf *„Call Microsoft Graph API“* (Microsoft Graph-API aufrufen), und Sie werden zur Eingabe Ihres Benutzernamens und Kennworts aufgefordert.

### <a name="consent"></a>Zustimmung
Bei der ersten Anmeldung bei Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem Sie explizit Ihre Zustimmung geben müssen:

![Genehmigungsbildschirm](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Erwartete Ergebnisse
Es sollten Benutzerprofilinformationen angezeigt werden, die auf den Microsoft Graph-API-Aufruf im Abschnitt *Protokollierung* zurückgegeben werden.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen

Die Microsoft Graph-API benötigt den Bereich `user.read`, um das Benutzerprofil zu lesen. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die in unserem Registrierungsportal registriert wird. Einige andere Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern zusätzliche Bereiche. Für Microsoft Graph ist es z.B. erforderlich, dass der Bereich `Calendars.Read` die Kalender des Benutzers auflistet. Um auf den Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie den Informationen für die Anwendungsregistrierung die delegierte Berechtigung `Calendars.Read` hinzufügen und dann dem Aufruf von `acquireTokenSilent` den Bereich `Calendars.Read` hinzufügen. Wenn Sie die Anzahl der Bereiche erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

<!--end-collapse-->




