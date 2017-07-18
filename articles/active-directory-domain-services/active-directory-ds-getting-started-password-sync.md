---
title: 'Azure Active Directory Domain Services: Aktivieren der Kennwortsynchronisierung | Microsoft-Dokumentation'
description: Erste Schritte mit Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 4b6da997f44860dccb2aa2571ce099ab2d0231f3
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Aktivieren der Kennwortsynchronisierung für Azure Active Directory Domain Services
In vorherigen Aufgaben haben Sie Azure Active Directory Domain Services für Ihren Azure AD-Mandanten (Azure Active Directory) aktiviert. Die nächste Aufgabe besteht darin, die Synchronisierung der Anmeldeinformationshashes, die für die NTLM- (NT LAN Manager) und Kerberos-Authentifizierung erforderlich sind, mit Azure AD Domain Services zu ermöglichen. Nach der Einrichtung der Synchronisierung von Anmeldeinformationen können sich Benutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden.

Für reine Cloudbenutzerkonten müssen andere Schritte ausgeführt werden als für Benutzerkonten, die aus Ihrem lokalen Verzeichnis mit Azure AD Connect synchronisiert werden.  Wenn Ihr Azure AD-Mandant über eine Kombination aus reinen Cloudbenutzern und Benutzern aus Ihrem lokalen AD verfügt, müssen beide Schritte ausgeführt werden.

<br>

> [!div class="op_single_selector"]
> * **Reine Cloudbenutzerkonten:** [Synchronisieren der Kennwörter reiner Cloudbenutzerkonten mit Ihrer verwalteten Domäne](active-directory-ds-getting-started-password-sync.md)
> * **Lokale Benutzerkonten:** [Synchronisieren der Kennwörter für synchronisierte Benutzerkonten aus Ihrem lokalen AD mit Ihrer verwalteten Domäne](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-cloud-only-user-accounts"></a>Aufgabe 5: Aktivieren der Kennwortsynchronisierung mit Ihrer verwalteten Domäne für reine Cloudbenutzerkonten
Für Azure Active Directory Domain Services müssen Anmeldeinformationshashes in einem für die NTLM- und Kerberos-Authentifizierung geeigneten Format vorliegen, um Benutzer für die verwaltete Domäne authentifizieren zu können. Azure AD generiert oder speichert erst dann Anmeldeinformationshashes in dem Format, das für die NTLM- oder Kerberos-Authentifizierung erforderlich ist, wenn Sie Azure Active Directory Domain Services für Ihren Mandanten aktivieren. Aus Sicherheitsgründen speichert Azure AD Kennwörter nicht als Klartext. Daher ist es in Azure AD nicht möglich, diese NTLM- oder Kerberos-Anmeldeinformationshashes automatisch auf der Grundlage bereits vorhandener Benutzeranmeldeinformationen zu generieren.

> [!NOTE]
> Falls Ihr Unternehmen über reine Cloudbenutzerkonten verfügt, müssen Benutzer, die Azure Active Directory Domain Services verwenden möchten, ihre Kennwörter ändern. Ein reines Cloudbenutzerkonto ist ein Konto, das in Ihrem Azure AD-Verzeichnis über das Azure-Portal oder mithilfe von Azure AD PowerShell-Cmdlets erstellt wurde. Solche Benutzerkonten werden nicht von einem lokalen Verzeichnis aus synchronisiert.
>
>

Diese Kennwortänderung führt dazu, dass die Anmeldeinformationshashes, die von Azure Active Directory Domain Services für die Kerberos- und NTLM-Authentifizierung benötigt werden, in Azure AD generiert werden. Sie können entweder die Kennwörter für alle Benutzer im Mandanten ablaufen lassen, die Azure Active Directory Domain Services verwenden müssen, oder diese Benutzer zum Ändern ihrer Kennwörter auffordern.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a>Aktivieren der Generierung von NTLM- und Kerberos-Anmeldeinformationshashes für ein reines Cloudbenutzerkonto
Stellen Sie Ihren Benutzern die folgende Anleitung zur Verfügung, damit sie ihre Kennwörter ändern können:

1. Rufen Sie den [Azure AD-Zugriffsbereich](http://myapps.microsoft.com) für Ihre Organisation auf.

    ![Starten des Azure AD-Zugriffsbereichs](./media/active-directory-domain-services-getting-started/access-panel.png)

2. Klicken Sie in der rechten oberen Ecke auf Ihren Namen, und wählen Sie im Menü die Option **Profil** aus.

    ![Auswählen des Profils](./media/active-directory-domain-services-getting-started/select-profile.png)

3. Klicken Sie auf der Seite **Profil** auf **Kennwort ändern**.

    ![Klicken auf „Kennwort ändern“](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Sollte die Option **Kennwort ändern** auf der Zugriffsbereichsseite nicht angezeigt werden, vergewissern Sie sich, dass in Ihrer Organisation die [Kennwortverwaltung in Azure AD](../active-directory/active-directory-passwords-getting-started.md) konfiguriert ist.
   >
   >
4. Geben Sie auf der Seite **Kennwort ändern** Ihr vorhandenes (altes) und ein neues Kennwort ein, und bestätigen Sie das neue Kennwort.

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klicken Sie auf **Senden**.

Das neue Kennwort kann wenige Minuten nach der Kennwortänderung in Azure Active Directory Domain Services verwendet werden. Nach einigen weiteren Minuten (in der Regel etwa 20 Minuten) können Sie sich mit dem neuen Kennwort bei Computern anmelden, die der verwalteten Domäne angehören.

## <a name="related-content"></a>Verwandte Inhalte
* [Aktualisieren Ihres eigenen Kennworts](../active-directory/active-directory-passwords-update-your-own-password.md)
* [Erste Schritte mit der Kennwortverwaltung](../active-directory/active-directory-passwords-getting-started.md)
* [Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Verwalten einer durch die Azure Active Directory-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
* [Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne](active-directory-ds-admin-guide-join-windows-vm.md)
* [Einbinden eines virtuellen Red Hat Enterprise Linux 7-Computers in eine verwaltete Domäne](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

