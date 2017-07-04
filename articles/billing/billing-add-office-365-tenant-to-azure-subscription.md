---
title: Verwenden eines Office 365-Mandanten mit einem Azure-Abonnement | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie ein Office 365-Verzeichnis (Mandant) einem Azure-Abonnement hinzufügen."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.translationtype: Human Translation
ms.sourcegitcommit: 1cb57e5156dab976599ddfa9a58f26ca8ef1ee0e
ms.openlocfilehash: 42db903244e5642dbf273e71994d402f8aeda1e9
ms.contentlocale: de-de
ms.lasthandoff: 02/22/2017


---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a>Zuordnen eines Office 365-Mandanten zu einem Azure-Abonnement
Verknüpfen Sie Ihre separaten Azure- und Office 365-Abonnements, sodass Sie von Ihrem Azure-Abonnement aus auf den Office 365-Mandanten zugreifen können. Um Ihre Abonnements zu verknüpfen, melden Sie sich in Azure mit dem Azure-Dienstadministratorkonto an, fügen Sie ein Verzeichnis hinzu, und fügen Sie die Office 365-Organisationskonten dem Azure Active Directory-Mandanten hinzu.

Wenn Sie ein Office 365-Abonnement für Benutzer in Ihrer Azure Active Directory-Instanz wünschen, oder wenn Sie über ein Office 365-Konto verfügen, jedoch nicht über ein Azure-Konto, lesen Sie [Verwenden eines vorhandenen Office 365-Kontos mit Ihrem Azure-Abonnement (oder umgekehrt)](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Voraussetzungen
* Sie benötigen die Anmeldeinformationen des Azure-Abonnementdienstadministrators. Mit Co-Administratorkonten können einige der Schritte in diesem Artikel nicht ausgeführt werden. Informationen zum Ändern des Dienstadministrators finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Sie benötigen die Anmeldeinformationen eines globalen Administrators des Office 365-Mandanten.
* Die E-Mail-Adresse des Dienstadministrators darf nicht im Office 365-Mandanten enthalten sein.
* Die E-Mail-Adresse des Dienstadministrators darf nicht mit der E-Mail-Adresse eines globalen Administrators des Office 365-Mandanten identisch sein.
* Wenn Sie eine E-Mail-Adresse verwenden, bei der es sich gleichzeitig um ein Microsoft-Konto und ein Organisationskonto handelt, legen Sie fest, dass der Dienstadministrator Ihres Azure-Abonnements vorübergehend ein anderes Microsoft-Konto verwendet. Sie können auf der [Seite für die Registrierung eines Microsoft-Kontos](https://signup.live.com/) ein Konto erstellen.

## <a name="link-office-365-tenant-to-azure-subscription"></a>Verknüpfen des Office 365-Mandanten mit dem Azure-Abonnement
Führen Sie zum Zuordnen des Office 365-Mandanten zum Azure-Abonnement die folgenden Schritte aus:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>Schritt 1: Hinzufügen des Office 365-Mandanten zu Ihrem Azure-Abonnement

1. Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.

    ![Screenshot zur Anmeldung bei Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. Wählen Sie im linken Bereich **ACTIVE DIRECTORY** aus. Der Office 365-Mandant sollte nicht angezeigt werden. Wenn er angezeigt wird, fahren Sie mit [Schritt 2: Ändern des Verzeichnisses, das mit dem Azure-Abonnement verknüpft ist](#Step2) fort.
   
   ![Screenshot des Active Directory-Eintrags](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Wählen Sie **NEU** > **VERZEICHNIS** > **BENUTZERDEFINIERT ERSTELLEN**.
   
    ![Screenshot der benutzerdefinierten Erstellung in Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. Wählen Sie auf der Seite **Verzeichnis hinzufügen** unter **VERZEICHNIS** die Option **Vorhandenes Verzeichnis verwenden** aus. Wählen Sie dann **Ich bin jetzt für die Abmeldung bereit** und anschließend **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Screenshot der Option „Vorhandenes Verzeichnis verwenden“](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. Nachdem Sie abgemeldet wurden, melden Sie sich mit den Anmeldeinformationen des globalen Administrators des Office 365-Mandanten an.
   
    ![Screenshot zur Anmeldung des globalen Administrators bei Office 365](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Wählen Sie **Weiter**.
   
    ![Screenshot der Überprüfung](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Wählen Sie **Jetzt abmelden**.
   
    ![Screenshot zum Abmeldevorgang](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.
   
    ![Screenshot zur Anmeldung bei Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Ihr Office 365-Mandant sollte auf dem Dashboard angezeigt werden.
   
    ![Screenshot des Dashboards](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Schritt 2: Ändern des Verzeichnisses, das mit dem Azure-Abonnement verknüpft ist
   
1. Wählen Sie **Settings**aus.
   
    ![Screenshot des Einstellungssymbols für das klassische Azure-Portal](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Wählen Sie Ihr Azure-Abonnement und anschließend **VERZEICHNIS BEARBEITEN**.

    ![Screenshot der Option zum Bearbeiten des Verzeichnisses im Azure-Abonnement](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Wählen Sie **Weiter** ![Weiter-Symbol](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Screenshot der Option „Zugeordnetes Verzeichnis ändern“](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Überprüfen Sie die betroffenen Konten. Alle Co-Administratoren und [RBAC](../active-directory/role-based-access-control-configure.md)-Benutzer (Role-Based Access Control, rollenbasierte Zugriffssteuerung) mit zugewiesenem Zugriff in den vorhandenen Ressourcengruppen werden entfernt. In der Warnung wird nur auf die Entfernung der Co-Administratoren hingewiesen.
      
    ![Screenshot, der die zu entfernenden Co-Administratorkonten zeigt.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Screenshot, der ein Beispiel eines zu entfernenden Benutzerkontos zeigt.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Wählen Sie **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a>Schritt 3: Hinzufügen Ihrer Office 365-Organisationskonten als Co-Administratoren zum Azure Active Directory-Mandanten
   
1. Wählen Sie die Registerkarte **ADMINISTRATOREN** und dann **HINZUFÜGEN**.
   
    ![Screenshot der Registerkarte „Administratoren“ im klassischen Azure-Portal](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Geben Sie ein Organisationskonto Ihres Office 365-Mandanten ein, wählen Sie das Azure-Abonnement, und wählen Sie dann **Fertig stellen** ![Symbol zum Fertigstellen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Screenshot des Dialogfelds zum Hinzufügen eines Azure-Co-Administrators](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Wechseln Sie erneut auf die Registerkarte **ADMINISTRATOREN**. Hier sollte nun das Organisationskonto als Co-Administrator angezeigt werden.
   
    ![Screenshot der Registerkarte „Administratoren“](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  Testen Sie den Zugriff auf Azure mit dem Co-Administratorkonto.
   
    a. Melden Sie sich vom klassischen Azure-Portal ab.
   
    b. Öffnen Sie das [Azure-Portal](https://portal.azure.com/).
   
    c. Geben Sie die Anmeldeinformationen des Co-Administrators ein, und wählen Sie dann **Anmelden**.
   
    ![Screenshot der Azure-Anmeldeseite](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen.



