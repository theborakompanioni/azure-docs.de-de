---
title: Anpassen Ihrer Anmeldeseite in der Azure Active Directory-Vorschau | Microsoft Docs
description: "Erfahren Sie, wie Sie der Anmeldeseite ein Unternehmensbranding hinzufügen."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3a054a01cf21bba4bbaf15c404f74086b2f25651


---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Hinzufügen eines Unternehmensbrandings zur Anmeldeseite in der Azure Active Directory-Vorschau
Um Verwirrung zu vermeiden, möchten viele Unternehmen über alle verwalteten Websites und Dienste hinweg für ein einheitliches Erscheinungsbild sorgen. Dies ist in der Azure Active Directory-Vorschau möglich, da Sie die Darstellung der Anmeldeseite mit Ihrem Firmenlogo und Ihren benutzerdefinierten Farbschemas anpassen können. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md)  Die Anmeldeseite wird angezeigt, wenn Sie sich an Office 365 oder anderen webbasierten Anwendungen anmelden, die Azure AD als Identitätsanbieter verwenden. Sie interagieren mit dieser Seite, um Ihre Anmeldeinformationen einzugeben.

Wenn Sie Ihre Unternehmensmarke, -farben und andere anpassbare Elemente auf dieser Seite anzeigen möchten, schauen Sie sich die folgenden Bilder an, um den Unterschied zwischen den beiden Eindrücken zu verstehen.

Der folgende Screenshot zeigt ein Beispiel für die Office 365-Anmeldeseite auf einem Desktopcomputer **vor** einer Anpassung:

![Office 365-Anmeldeseite vor der Anpassung](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Der folgende Screenshot zeigt ein Beispiel für die Office 365-Anmeldeseite auf einem Desktopcomputer **nach** einer Anpassung:

![Office 365-Anmeldeseite nach der Anpassung](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a>Anpassen der Anmeldeseite
Normalerweise verwenden Sie die Anmeldeseite, wenn Sie browserbasierten Zugriff auf Ihre Cloud-Apps und -dienste benötigen, die von Ihrer Organisation abonniert wurden.

Wenn Sie Änderungen auf Ihre Anmeldeseite angewendet haben, kann es bis zu einer Stunde dauern, bis die Änderungen angezeigt werden.

Eine Anmeldeseite mit Branding wird nur angezeigt, wenn Sie einen Dienst mit einer mandantenspezifischen URL besuchen, z. B. https://outlook.com/**contoso**.com oder https://mail.**contoso**.com.

Wenn Sie einen Dienst mit nicht mandantenspezifischen URLs besuchen (7. B. https://mail.office365.com), wird eine Anmeldeseite ohne Branding angezeigt. In diesem Fall wird Ihr Branding angezeigt, nachdem Sie die Benutzer-ID eingegeben oder eine Benutzerkachel ausgewählt haben.

> [!NOTE]
> * Ihr Domänenname muss im Abschnitt **Domänen** des Azure-Portals, in dem Sie das Branding konfiguriert haben, als „Aktiv“ angezeigt werden. Weitere Informationen finden Sie unter [Hinzufügen von benutzerdefinierten Domänennamen](active-directory-domains-add-azure-portal.md).
> * Die Anmeldeseite mit dem Branding wird nicht auf die Verbraucheranmeldeseite von Microsoft übertragen. Wenn Sie sich mit einem Microsoft-Konto anmelden, wird eine Liste mit von Azure AD gerenderten Benutzerkacheln mit Branding angezeigt, aber das Branding Ihrer Organisation wird nicht auf die Anmeldeseite des Microsoft-Kontos übertragen.
> 
> 

Dank des Kontrollkästchens **Angemeldet bleiben** auf der Anmeldeseite kann der Benutzer angemeldet bleiben, wenn er den Browser schließt und wieder öffnet. 

   ![Angemeldet bleiben](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Die Sitzungsdauer wird dadurch nicht beeinflusst. Das Kontrollkästchen kann auf der Azure Active Directory-Anmeldeseite ausgeblendet werden.
Ob das Kontrollkästchen angezeigt wird, hängt von der Einstellung **„Angemeldet bleiben“ deaktiviert** ab.

   ![Angemeldet bleiben](./media/active-directory-branding-custom-signon-azure-portal/02.png)

Legen Sie diese Einstellung zum Ausblenden des Kontrollkästchens auf **Ja** fest. 

> [!NOTE]
> Einige Features von SharePoint Online und Office 2010 setzen voraus, dass dieses Kontrollkästchen von den Benutzern aktiviert werden kann. Wenn Sie das Kontrollkästchen mithilfe dieser Einstellung ausblenden, werden den Benutzern unter Umständen zusätzliche und unerwartete Anmeldeaufforderungen angezeigt.
> 
> 

**So fügen Sie Ihrem Verzeichnis ein Unternehmensbranding hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die EINGABETASTE****.
   
   ![Öffnen der Benutzerverwaltung](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Unternehmensbranding** aus.
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Unternehmensbranding** den Befehl **Bearbeiten** aus.
   
    ![Bearbeiten des benutzerdefinierten Brandings](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. Ändern Sie die Elemente, die Sie anpassen möchten. Alle Elemente sind optional.
6. Klicken Sie auf **Speichern**.

Es kann bis zu einer Stunde dauern, bis Änderungen übernommen werden, die Sie am Branding der Anmeldeseite vorgenommen haben.

## <a name="next-steps"></a>Nächste Schritte
[Hinzufügen eines sprachspezifischen Unternehmensbrandings](active-directory-branding-localize-azure-portal.md)




<!--HONumber=Nov16_HO3-->


