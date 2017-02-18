---
title: Aktivieren oder Deaktivieren einer Rolle | Microsoft Docs
description: "Erfahren Sie, wie Sie mit der Anwendung Azure Privileged Identity Management Rollen für privilegierte Identitäten aktivieren."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 1ce9e2e7-452b-4f66-9588-0d9cd2539e45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 081e45e0256134d692a2da7333ddbaafc7366eaa
ms.openlocfilehash: f70fec829b273de97fc49e61ba42d511b6b141b3


---
# <a name="how-to-activate-or-deactivate-roles-in-azure-ad-privileged-identity-management"></a>Aktivieren oder Deaktivieren von Rollen in Azure AD Privileged Identity Management
Azure Active Directory (AD) Privileged Identity Management (PIM) vereinfacht die Art und Weise, in der Unternehmen den privilegierten Zugriff auf Ressourcen in Azure AD und anderen Onlinediensten von Microsoft wie Office 365 oder Microsoft Intune verwalten.  

Wenn Sie für eine Administratorrolle berechtigt sind, können Sie diese Rolle aktivieren, um Aufgaben durchzuführen, für die diese Rolle erforderlich ist. Wenn Sie z. B. gelegentlich Office 365-Funktionen verwalten, dürfen Ihre privilegierte Rollenadministratoren Ihrer Organisation Sie nicht als permanenten globalen Administrator festlegen, da sich diese Rolle auf andere Dienste auswirkt. Stattdessen gewähren sie Ihnen Berechtigungen für Azure AD-Rollen wie z. B. Exchange Online-Administrator. Sie können eine Aktivierung dieser Rolle anfordern, wenn Sie diese Berechtigungen benötigen, und verfügen damit für einen bestimmten Zeitraum über die administrative Kontrolle.

Dieser Artikel richtet sich an Administratoren, die ihre Rolle in Azure AD Privileged Identity Management aktivieren müssen. Der Artikel erläutert schrittweise, wie Sie eine Rolle aktivieren, wenn Sie die Berechtigungen benötigen, und wie Sie die Rolle wieder deaktivieren, wenn Sie die Berechtigungen nicht mehr benötigen.

## <a name="add-the-privileged-identity-management-application"></a>Hinzufügen der Anwendung Privileged Identity Management
Verwenden Sie die Anwendung Azure AD Privileged Identity Management im [Azure-Portal](https://portal.azure.com/) , um eine Rollenaktivierung anzufordern, auch wenn Sie in einem anderen Portal oder in PowerShell arbeiten werden. Wenn sich die Anwendung Azure AD Privileged Identity Management nicht in Ihrem Azure-Portal befindet, führen Sie die folgenden Schritte aus, um zu beginnen.

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/)an.
2. Wählen Sie in der oberen rechten Ecke des Azure-Portals Ihren Benutzernamen, und wählen Sie das Verzeichnis aus, in dem Sie arbeiten möchten.
3. Wählen Sie **Weitere Dienste** aus, und verwenden Sie das Textfeld „Filter“, um nach **Azure AD Privileged Identity Management** zu suchen.
4. Aktivieren Sie das Kontrollkästchen **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**. Die Anwendung Privileged Identity Management wird geöffnet.

## <a name="activate-a-role"></a>Aktivieren einer Rolle
Wenn Sie eine Rolle übernehmen müssen, können Sie die Aktivierung über die Schaltfläche **Meine Rollen aktivieren** in der Anwendung Azure AD Privileged Identity Management anfordern.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an, und wählen Sie die Kachel „Azure AD Privileged Identity Management“ aus.
2. Wählen Sie **Meine Rollen aktivieren**aus. Eine Liste der Ihnen zugewiesenen Rollen wird angezeigt.
3. Wählen Sie die Rolle aus, die Sie aktivieren möchten.
4. Wählen Sie **Aktivieren**aus. Das Blatt **Rollenaktivierung anfordern** wird angezeigt.
5. Für einige Rollen ist Multi-Factor Authentication (MFA) erforderlich, bevor die Rolle aktiviert werden kann. Sie müssen sich nur einmal pro Sitzung authentifizieren.
   
    ![Überprüfen mit MFA vor der Rollenaktivierung – Screenshot][2]
6. Geben Sie im Textfeld den Grund für die Aktivierungsanforderung ein.  Für einige Rollen müssen Sie eine Ticketnummer zur Problembehebung angeben.
7. Klicken Sie auf **OK**.  Die Rolle wurde jetzt aktiviert, und die Rollenänderung ist in den Oninediensten von Microsoft sichtbar.

## <a name="deactivate-a-role"></a>Deaktivieren einer Rolle
Wenn eine Rolle aktiviert wurde, wird sie nach Erreichen des Zeitlimits automatisch deaktiviert.

Wenn Sie vorher mit Ihren Aufgaben fertig sind, können Sie die Rolle in der Anwendung Azure AD Privileged Identity Management auch manuell deaktivieren.  Wählen Sie **Meine Rollen aktivieren** und dann die Rolle aus, die Sie nicht mehr verwenden. Wählen Sie anschließend **Deaktivieren** aus.  

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Azure AD Privileged Identity Management finden Sie unter folgenden Links.

[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-how-to-activate-role/PIM_activation_MFA.png



<!--HONumber=Feb17_HO1-->


