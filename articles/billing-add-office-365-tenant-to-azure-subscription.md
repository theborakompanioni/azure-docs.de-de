---
title: Verwenden eines Office 365-Mandanten mit einem Azure-Abonnement | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie ein Office 365-Verzeichnis (Mandant) zu einem Azure-Abonnement hinzufügen, um die Verknüpfung zu erstellen."
services: 
documentationcenter: 
author: JiangChen79
manager: mbaldwin
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cjiang
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 36cd9eac7be2d99971d8d2e227cd4b761df82d08


---
# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Zuordnen eines Office 365-Mandanten zu einem Azure-Abonnement
Wenn Sie das Azure-Abonnement und das Office 365-Abonnement in der Vergangenheit separat erworben haben und jetzt über das Azure-Abonnement auf den Office 365-Mandanten zugreifen möchten, kann dies problemlos umgesetzt werden. In diesem Artikel erfahren Sie, welche Schritte erforderlich sind.

> [!NOTE]
> Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA).
> 
> 

## <a name="quick-guidance"></a>Kurzanleitung
Um Ihren Office 365-Mandanten Ihrem Azure-Abonnement zuzuordnen, verwenden Sie Ihr Azure-Konto zum Hinzufügen des Office 365-Mandanten. Ordnen Sie anschließend das Azure-Abonnement dem Office 365-Mandanten zu.

## <a name="detailed-steps"></a>Ausführliche Schritte
In diesem Szenario ist Kelley Wall eine Benutzerin mit einem Azure-Abonnement im Konto kelley.wall@outlook.com. Kelley verfügt auch über ein Office 365-Abonnement unter dem Konto kelley.wall@contoso.onmicrosoft.com. Kelley möchte jetzt mit dem Azure-Abonnement auf den Office 365-Mandanten zugreifen.

![Screenshot des Azure Active Directory-Status](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Screenshot aktiver Benutzer im Office 365 Admin Center](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Voraussetzungen
Für eine ordnungsgemäße Funktionsweise der Zuordnung müssen folgende Voraussetzungen erfüllt sein:

* Sie benötigen die Anmeldeinformationen des Dienstadministrators für das Azure-Abonnement. Co-Administratoren können einen Teil der Schritte nicht ausführen.
* Sie benötigen die Anmeldeinformationen eines globalen Administrators des Office 365-Mandanten.
* Die E-Mail-Adresse des Dienstadministrators darf nicht im Office 365-Mandanten enthalten sein.
* Die E-Mail-Adresse des Dienstadministrators darf nicht mit der E-Mail-Adresse eines globalen Administrators des Office 365-Mandanten identisch sein.
* Wenn Sie derzeit eine E-Mail-Adresse verwenden, bei der es sich gleichzeitig um ein Microsoft-Konto und ein Organisationskonto handelt, legen Sie fest, dass der Dienstadministrator Ihres Azure-Abonnements vorübergehend ein anderes Microsoft-Konto verwendet. Sie können auf der [Seite für die Registrierung eines Microsoft-Kontos](https://signup.live.com/)ein neues Konto erstellen.

Führen Sie zum Ändern des Dienstadministrators die folgenden Schritte aus:

1. Melden Sie sich beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.
2. Wählen Sie das Abonnement aus, das Sie ändern möchten.
3. Wählen Sie **Abonnementdetails bearbeiten**.
   
    ![Screenshot der Azure-Abonnementinformationen mit ausgewählter Option „Abonnementdetails bearbeiten“](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)
4. Geben Sie im Feld **DIENSTADMINISTRATOR** die E-Mail-Adresse des neuen Dienstadministrators ein.
   
    ![Screenshot des Dialogfelds „Abonnement bearbeiten“](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Zuordnen des Office 365-Mandanten zum Azure-Abonnement
Führen Sie zum Zuordnen des Office 365-Mandanten zum Azure-Abonnement die folgenden Schritte aus:

1. Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.
2. Wählen Sie im linken Bereich **ACTIVE DIRECTORY** aus.
   
   ![Screenshot des Active Directory-Eintrags](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)
   
   > [!NOTE]
   > Der Office 365-Mandant sollte nicht angezeigt werden. Wird er angezeigt, überspringen Sie den nächsten Schritt.
   > 
   > 
   
   ![Screenshot des Standardverzeichnisses von Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)
3. Fügen Sie den Office 365-Mandanten zu Ihrem Azure-Abonnement hinzu.
   
    a. Wählen Sie **NEU** > **VERZEICHNIS** > **BENUTZERDEFINIERT ERSTELLEN**.
   
    ![Screenshot der benutzerdefinierten Erstellung in Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
    b. Wählen Sie auf der Seite **Verzeichnis hinzufügen** unter **VERZEICHNIS** die Option **Vorhandenes Verzeichnis verwenden** aus. Wählen Sie dann **Ich bin jetzt für die Abmeldung bereit** und anschließend **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Screenshot der Option „Vorhandenes Verzeichnis verwenden“](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
    c. Nachdem Sie abgemeldet wurden, melden Sie sich mit den Anmeldeinformationen des globalen Administrators des Office 365-Mandanten an.
   
    ![Screenshot zur Anmeldung des globalen Administrators bei Office 365](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
    d. Wählen Sie **Weiter**.
   
    ![Screenshot der Überprüfung](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
    e. Wählen Sie **Jetzt abmelden**.
   
    ![Screenshot zum Abmeldevorgang](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
    f. Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.
   
    ![Screenshot zur Anmeldung bei Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
    g. Ihr Office 365-Mandant sollte auf dem Dashboard angezeigt werden.
   
    ![Screenshot des Dashboards](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)
4. Ändern Sie das Verzeichnis, das mit dem Azure-Abonnement verknüpft ist.
   
    a. Wählen Sie **Settings**aus.
   
    ![Screenshot des Einstellungssymbols für das klassische Azure-Portal](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
    b. Wählen Sie Ihr Azure-Abonnement und anschließend **VERZEICHNIS BEARBEITEN**.
    ![Screenshot der Option zum Bearbeiten des Verzeichnisses im Azure-Abonnement](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
    c. Wählen Sie **Weiter** ![Weiter-Symbol](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Screenshot der Option „Zugeordnetes Verzeichnis ändern“](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
   > [!WARNING]
   > Eine Warnung wird angezeigt, dass alle Co-Administratoren entfernt werden.
   > 
   > 
   
    ![azure-confirm-directory-mapping](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
   > [!WARNING]
   > Darüber hinaus werden alle [RBAC](active-directory/role-based-access-control-configure.md) -Benutzer (Role-Based Access Control, rollenbasierte Zugriffssteuerung) mit zugewiesenem Zugriff in den vorhandenen Ressourcengruppen entfernt. In der Warnung wird jedoch nur auf die Entfernung der Co-Administratoren hingewiesen.
   > 
   > 
   
    ![assigned-users-removed-resource-groups](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
    d. Wählen Sie **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
5. Jetzt können Sie Ihre Office 365-Organisationskonten als Co-Administratoren zum Azure Active Directory-Mandanten hinzufügen.
   
    a. Wählen Sie die Registerkarte **ADMINISTRATOREN** und dann **HINZUFÜGEN**.
   
    ![Screenshot der Registerkarte „Administratoren“ im klassischen Azure-Portal](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
    b. Geben Sie ein Organisationskonto Ihres Office 365-Mandanten ein, wählen Sie das Azure-Abonnement, und wählen Sie dann **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Screenshot des Dialogfelds zum Hinzufügen eines Azure-Co-Administrators](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
    c. Wechseln Sie erneut auf die Registerkarte **ADMINISTRATOREN**. Hier sollte nun das Organisationskonto als Co-Administrator angezeigt werden.
   
    ![Screenshot der Registerkarte „Administratoren“](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
6. Als Nächstes können Sie den Zugriff als Co-Administrator testen.
   
    a. Melden Sie sich beim Kontoverwaltungsportal ab.
   
    b. Öffnen Sie das [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) oder das [Azure-Portal](https://portal.azure.com/).
   
    c. Wird auf der Azure-Anmeldeseite der Link **Mit Organisationskonto anmelden** angezeigt, klicken Sie auf den Link. Überspringen Sie andernfalls diesen Schritt.
   
    ![Screenshot der Azure-Anmeldeseite](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)
   
    d. Geben Sie die Anmeldeinformationen des Co-Administrators ein, und wählen Sie dann **Anmelden**.
   
    ![Screenshot der Azure-Anmeldeseite](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Nächste Schritte
Zu diesen Szenarien gehören:

* Sie verfügen bereits über ein Office 365-Abonnement und wünschen ein Azure-Abonnement, möchten jedoch die vorhandenem Office 365-Benutzerkonten für Ihr Azure-Abonnement verwenden.
* Sie sind Azure-Abonnent und wünschen ein Office 365-Abonnement für die Benutzer in Ihrer vorhandenen Azure Active Directory-Instanz.

Informationen hierzu finden Sie unter [Verwenden Ihres vorhandenen Office 365-Kontos mit Ihrem Azure-Abonnement (oder umgekehrt)](billing-use-existing-office-365-account-azure-subscription.md).




<!--HONumber=Nov16_HO3-->


