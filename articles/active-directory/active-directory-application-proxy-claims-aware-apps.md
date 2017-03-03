---
title: "Ansprüche unterstützende Apps – Azure AD-App-Proxy | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie lokale ASP.NET-Anwendungen veröffentlichen, die AD FS-Ansprüche für sicheren Remotezugriff durch Ihre Benutzer akzeptieren."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 91e6211b-fe6a-42c6-bdb3-1fff0312db15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 081e45e0256134d692a2da7333ddbaafc7366eaa
ms.openlocfilehash: ff07a52f6a503f07f5919b63f345878571742cac
ms.lasthandoff: 02/06/2017


---
# <a name="working-with-claims-aware-apps-in-application-proxy"></a>Arbeiten mit Ansprüche unterstützenden Apps im Anwendungsproxy
Ansprüche unterstützende Apps führen eine Umleitung zum Sicherheitstokendienst (STS, Security Token Service) durch, der wiederum Anmeldeinformationen vom Benutzer im Austausch gegen ein Token anfordert, bevor der Benutzer zur Anwendung umgeleitet wird. Damit der Anwendungsproxy mit diesen Umleitungen arbeiten kann, müssen die folgenden Schritte ausgeführt werden.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie vor dem Ausführen dieses Verfahrens sicher, dass der STS, zu dem die Ansprüche unterstützende App umgeleitet wird, außerhalb Ihres lokalen Netzwerks zur Verfügung steht.

## <a name="azure-classic-portal-configuration"></a>Konfiguration im klassischen Azure-Portal
1. Veröffentlichen Sie Ihre Anwendung entsprechend den Anweisungen unter [Veröffentlichen von Anwendungen mit einem Anwendungsproxy](active-directory-application-proxy-publish.md).
2. Wählen Sie in der Liste der Anwendungen die  Ansprüche unterstützende App aus, und klicken Sie auf **Konfigurieren**.
3. Wenn Sie **Passthrough** als **Präauthentifizierungsmethode** gewählt haben, stellen Sie sicher, dass Sie **HTTPS** als Schema für die **Externe URL** auswählen.
4. Wenn Sie **Azure Active Directory** als **Präauthentifizierungsmethode** gewählt haben, wählen Sie **Keine** als **Interne Authentifizierungsmethode** aus.

## <a name="adfs-configuration"></a>AD FS-Konfiguration
1. Öffnen Sie die AD FS-Verwaltung.
2. Wechseln Sie zu **Vertrauensstellungen der vertrauenden Seite**, klicken Sie mit der rechten Maustaste auf die App, die Sie mit dem Anwendungsproxy veröffentlichen, und wählen Sie **Eigenschaften** aus.  

   ![Vertrauensstellungen der vertrauenden Seite: mit der rechten Maustaste auf den App-Namen klicken – Screenshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyrelyingpartytrust.png)  

3. Wählen Sie auf der Registerkarte **Endpunkte** unter **Endpunkttyp** die Option **WS-Verbund** aus.
4. Geben Sie unter **Vertrauenswürdige URL** die URL ein, die Sie im Anwendungsproxy unter **Externe URL** eingegeben haben, und klicken Sie auf **OK**.  

   ![Hinzufügen eines Endpunkts – Wert "Vertrauenswürdige URL" festlegen – Screenshot](./media/active-directory-application-proxy-claims-aware-apps/appproxyendpointtrustedurl.png)  

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)
* [Aktivieren von nativen Client-Apps für die Interaktion mit Proxyanwendungen](active-directory-application-proxy-native-client.md)



