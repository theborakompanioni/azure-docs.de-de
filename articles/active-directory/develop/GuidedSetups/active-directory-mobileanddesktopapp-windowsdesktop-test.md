---
title: "Azure AD v2 – Windows-Desktop: Erste Schritte – Test | Microsoft-Dokumentation"
description: "Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
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
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 972cc48057c13271d725b0c973c3ccf651ad27c4
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
## <a name="test-your-code"></a>Testen Ihres Codes

Um Ihre Anwendung zu testen, drücken Sie `F5`, um das Projekt in Visual Studio auszuführen. Das Hauptfenster sollte angezeigt werden:

![Beispielscreenshot](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Wenn Sie zum Testen bereit sind, klicken Sie auf *Microsoft Graph-API aufrufen*. Verwenden Sie zum Anmelden ein Microsoft Azure Active Directory-Konto (Organisationskonto) oder ein Microsoft-Konto (live.com, outlook.com). Wenn Sie sich erstmals anmelden, wird ein Fenster eingeblendet, das den Benutzer auffordert, sich anzumelden:

![Anmeldung](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Zustimmung
Bei der ersten Anmeldung bei Ihrer Anwendung wird ein Genehmigungsbildschirm ähnlich dem folgenden angezeigt, auf dem Sie explizit Ihre Zustimmung geben müssen:

![Genehmigungsbildschirm](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Erwartete Ergebnisse
Es sollten Benutzerprofilinformationen angezeigt werden, die von einem Microsoft Graph-API-Aufruf auf dem Bildschirm „API-Aufrufergebnisse“ zurückgegeben werden.

Es sollten im Feld mit den Informationen zum Token auch grundlegende Informationen zum Token angezeigt werden, das über `AcquireTokenAsync` oder `AcquireTokenSilentAsync` abgerufen wurde:

|Eigenschaft  |Format  |Beschreibung |
|---------|---------|---------|
|Name | {Vollständiger Name des Benutzers} |Vor- und Nachname des Benutzers|
|Benutzername |<span>user@domain.com</span> |Benutzername zur Identifizierung des Benutzers|
|Ablaufdatum des Tokens |{DateTime}         |Uhrzeit, zu der das Token abläuft. Die MSAL verlängert das Ablaufdatum für Sie, indem das Token bei Bedarf verlängert wird|
|Zugriffstoken |{String}         |Die Tokenzeichenfolge, die bei HTTP-Anforderungen gesendet wird, die einen Autorisierungsheader erfordern|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Weitere Informationen zu Bereichen und delegierten Berechtigungen
Die Graph-API erfordert den Bereich `user.read` zum Lesen des Benutzerprofils. Dieser Bereich wird standardmäßig jeder Anwendung automatisch hinzugefügt, die in unserem Registrierungsportal registriert wird. Einige andere Graph-APIs sowie benutzerdefinierte APIs für Ihren Back-End-Server erfordern zusätzliche Bereiche. Beispielsweise ist für Graph `Calendars.Read` erforderlich, um Kalender von Benutzern aufzulisten. Um auf den Kalender des Benutzers im Kontext einer Anwendung zugreifen zu können, müssen Sie `Calendars.Read` den Informationen zur delegierten Anwendungsregistrierung und dann `Calendars.Read` dem Aufruf von `AcquireTokenAsync` hinzufügen. Wenn Sie die Anzahl der Bereiche weiter erhöhen, werden Benutzer ggf. zu weiteren Genehmigungen aufgefordert.

<!--end-collapse-->




