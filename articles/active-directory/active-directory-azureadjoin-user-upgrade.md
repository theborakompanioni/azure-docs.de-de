---
title: "Einrichten eines Windows 10-Geräts mit Azure AD in den Einstellungen| Microsoft Docs"
description: "Erläutert, wie Benutzer über das Menü „Einstellungen“ eine Verknüpfung mit Azure AD herstellen können."
services: active-directory
documentationcenter: 
author: femila
manager: swadhwa
editor: 
tags: azure-classic-portal
ms.assetid: b844e1d9-ad43-4e4a-a398-5c4a43bf2f5c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 70610ec2c5a056d63ed854b33d84e29951940c2f


---
# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Einrichten eines Windows 10-Geräts mit Azure AD in den Einstellungen
Wenn Sie bereits Windows 7 oder Windows 8 verwenden und Ihr Computer oder Gerät auf Windows 10 aktualisiert wurde, können Sie über das Menü „Einstellungen“ eine Verknüpfung zu Azure Active Directory (Azure AD) herstellen.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Herstellen einer Verknüpfung mit Azure AD über das Menü „Einstellungen“
1. Klicken Sie im Menü **Start** auf den Charm **Einstellungen**.
2. Navigieren Sie unter **Einstellungen** zu **System**->**Info**->**Azure AD beitreten**.
   
   <center>
   ![„Azure AD beitreten“ im Menü „Einstellungen“](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png) </center>
3. Klicken Sie im Meldungsfenster „Azure AD beitreten“ auf **Weiter** .
   
   <center>
   ![Meldungsfenster „Azure AD beitreten“](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png) </center>
4. Geben Sie Ihre Anmeldeinformationen ein. Die Anmeldeoberfläche enthält alle erforderlichen Schritte zum Abschließen der Authentifizierung. Wenn Sie einem Verbundmandanten angehören, stellt Ihnen Ihr Administrator die Verbundoberfläche bereit, die von Ihrer Organisation gehostet wird.
   <center>
   ![Anmeldeinformationen eingeben](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png) </center>
5. Wenn Ihre Organisation Azure Multi-Factor Authentication für das Herstellen einer Verknüpfung mit Azure AD konfiguriert hat, müssen Sie vor dem Fortfahren die zweite Stufe bereitstellen.
6. Klicken Sie auf dem Bildschirm **Erlauben, dass dieses Gerät verwaltet wird** auf **Annehmen**.
7. Die Meldung „Das Gerät ist Ihrer Organisation in Azure AD beigetreten“ sollte angezeigt werden.

## <a name="additional-information"></a>Zusätzliche Informationen
* [Weitere Informationen zu Verwendungsszenarios für Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Benutzererfahrungen beim Verknüpfen von in die Domäne eingebundenen Windows 10-Geräten mit Azure AD](active-directory-azureadjoin-devices-group-policy.md)
* [Einrichten von Azure AD Join](active-directory-azureadjoin-setup.md)
* [Authentifizieren von Identitäten ohne Kennwörter über Microsoft Passport](active-directory-azureadjoin-passport.md)




<!--HONumber=Dec16_HO4-->


