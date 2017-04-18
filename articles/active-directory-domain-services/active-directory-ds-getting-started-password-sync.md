---
title: 'Azure Active Directory Domain Services: Aktivieren der Kennwortsynchronisierung | Microsoft-Dokumentation'
description: "Erste Schritte mit Azure Active Directory-Domänendiensten"
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
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 6ed35ca1b83f5f7221824d99817800c1e42e68c1
ms.lasthandoff: 04/12/2017


---
# <a name="enable-password-synchronization-with-azure-active-directory-domain-services"></a>Aktivieren der Kennwortsynchronisierung mit Azure Active Directory Domain Services
In den vorherigen Aufgaben haben Sie Azure Active Directory Domain Services (AD DS) für Ihren Azure AD-Mandanten (Azure Active Directory) aktiviert. Die nächste Aufgabe besteht in der Aktivierung von Anmeldeinformationshashes. Diese werden benötigt, um die NTLM-Authentifizierung (NT LAN Manager) und die Kerberos-Authentifizierung mit Azure Active Directory Domain Services synchronisieren zu können. Nach der Einrichtung der Synchronisierung von Anmeldeinformationen können sich Benutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden.

Das jeweilige Verfahren hängt davon ab, ob Ihre Organisation über einen auf die Cloud beschränkten Azure AD-Mandanten verfügt oder ob die Synchronisierung mit Ihrem lokalen Verzeichnis über Azure AD Connect erfolgt.

> [!div class="op_single_selector"]
> * [Auf die Cloud beschränkter Azure AD-Mandant](active-directory-ds-getting-started-password-sync.md)
> * [Synchronisierter Azure AD-Mandant](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

## <a name="task-5-enable-password-synchronization-with-azure-active-directory-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Aufgabe 5: Aktivieren der Kennwortsynchronisierung mit Azure Active Directory Domain Services für einen auf die Cloud beschränkten Azure AD-Mandanten
Für Azure Active Directory Domain Services müssen Anmeldeinformationshashes in einem für die NTLM- und Kerberos-Authentifizierung geeigneten Format vorliegen, um Benutzer für die verwaltete Domäne authentifizieren zu können. Wenn Sie Azure Active Directory Domain Services für Ihren Mandanten nicht aktivieren, generiert oder speichert Azure AD keine Anmeldeinformationshashes in dem Format, das für die NTLM- oder Kerberos-Authentifizierung erforderlich ist. Aus Sicherheitsgründen werden Anmeldeinformationen von Azure AD natürlich auch nicht als Klartext gespeichert. Daher gibt es in Azure AD keine Option zum Generieren dieser Hashes von NTLM- oder Kerberos-Anmeldeinformationen basierend auf den vorhandenen Anmeldeinformationen von Benutzern.

> [!NOTE]
> Wenn Ihr Unternehmen über einen auf die Cloud beschränkten Azure AD-Mandanten verfügt, müssen Benutzer, die Azure Active Directory Domain Services verwenden, ihre Kennwörter ändern.
>
>

Diese Kennwortänderung führt dazu, dass die Anmeldeinformationshashes, die von Azure Active Directory Domain Services für die Kerberos- und NTLM-Authentifizierung benötigt werden, in Azure AD generiert werden. Sie können entweder die Kennwörter für alle Benutzer im Mandanten ablaufen lassen, die Azure Active Directory Domain Services verwenden müssen, oder diese Benutzer zum Ändern ihrer Kennwörter auffordern.

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Aktivieren der Hashgenerierung für NTLM- und Kerberos-Anmeldeinformationen für einen auf die Cloud beschränkten Azure AD-Mandanten
Stellen Sie Ihren Benutzern die folgende Anleitung zur Verfügung, damit sie ihre Kennwörter ändern können:

1. Rufen Sie den [Azure AD-Zugriffsbereich](http://myapps.microsoft.com) für Ihre Organisation auf.
2. Wählen Sie im Zugriffsbereich die Registerkarte **Profil** aus.
3. Klicken Sie auf die Kachel **Kennwort ändern**.

    ![Die Kachel „Kennwort ändern“ des Azure AD-Zugriffsbereichs](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > Sollte die Option **Kennwort ändern** auf der Zugriffsbereichsseite nicht angezeigt werden, vergewissern Sie sich, dass in Ihrer Organisation die [Kennwortverwaltung in Azure AD](../active-directory/active-directory-passwords-getting-started.md) konfiguriert ist.
   >
   >
4. Geben Sie auf der Seite **Kennwort ändern** Ihr vorhandenes (altes) und ein neues Kennwort ein, und bestätigen Sie das neue Kennwort. 

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. Klicken Sie auf **Senden**.

Das neue Kennwort kann wenige Minuten nach der Kennwortänderung in Azure Active Directory Domain Services verwendet werden. Nach einigen weiteren Minuten (in der Regel etwa 20 Minuten) können Sie sich mit dem neuen Kennwort bei Computern anmelden, die der verwalteten Domäne angehören.

## <a name="next-steps"></a>Nächste Schritte
* [Aktualisieren Ihres eigenen Kennworts](../active-directory/active-directory-passwords-update-your-own-password.md#reset-your-password)
* [Erste Schritte mit der Kennwortverwaltung](../active-directory/active-directory-passwords-getting-started.md)
* [Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [Verwalten einer durch die Azure Active Directory-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
* [Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne](active-directory-ds-admin-guide-join-windows-vm.md)
* [Einbinden eines virtuellen Red Hat Enterprise Linux 7-Computers in eine verwaltete Domäne](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

