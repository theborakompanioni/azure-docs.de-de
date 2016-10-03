<properties
	pageTitle="Verwenden eines Office 365-Mandanten mit einem Azure-Abonnement | Microsoft Azure"
	description="Erfahren Sie, wie Sie ein Office 365-Verzeichnis (Mandant) zu einem Azure-Abonnement hinzufügen, um die Verknüpfung zu erstellen."
	services=""
	documentationCenter=""
	authors="jiangchen79"
	manager="mbaldwin"
	editor=""
	tags="billing,top-support-issue"/>

<tags
	ms.service="billing"
	ms.workload="na"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="cjiang"/>

# Zuordnen eines Office 365-Mandanten zu einem Azure-Abonnement
Szenarien: Sie haben das Azure-Abonnement und das Office 365-Abonnement in der Vergangenheit separat erworben und möchten aus dem Azure-Abonnement auf den Office 365-Mandanten zugreifen. In diesem Artikel erfahren Sie, wie einfach das geht.

> [AZURE.NOTE] Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA).

## Kurzanleitung
Wenn Sie Ihren Office 365-Mandanten Ihrem Azure-Abonnement zuordnen möchten, verwenden Sie Ihr Azure-Konto zum Hinzufügen des Office 365-Mandanten. Ordnen Sie anschließend das Azure-Abonnement dem Office 365-Mandanten zu. Weitere Informationen finden Sie unter „Ausführliche Schritte“.

## Ausführliche Schritte
In diesem Szenario ist Kelley Wall eine Benutzerin mit einem Azure-Abonnement im Konto kelly.wall@outlook.com. Kelley verfügt auch über ein Office 365-Abonnement unter dem Konto kelley.wall@contoso.onmicrosoft.com. Kelley möchte jetzt mit dem Azure-Abonnement auf den Office 365-Mandanten zugreifen.

![msa-aad-status](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![office-365-user](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

**Voraussetzungen:**

- Sie benötigen Anmeldeinformationen des Dienstadministrators für das Azure-Abonnement. Co-Administratoren können einen Teil der Schritte nicht ausführen.
- Sie benötigen Anmeldeinformationen eines globalen Administrators des Office 365-Mandanten.
- Die E-Mail-Adresse des Dienstadministrators darf nicht im Office 365-Mandanten enthalten sein.
- Die E-Mail-Adresse des Dienstadministrators darf nicht mit der E-Mail-Adresse eines globalen Administrators des Office 365-Mandanten identisch sein.
- Wenn Sie derzeit eine E-Mail-Adresse verwenden, bei der es sich gleichzeitig um ein Microsoft-Konto und ein Organisationskonto handelt, sollten Sie festlegen, dass der Dienstadministrator Ihres Azure-Abonnements vorübergehend ein anderes Microsoft-Konto verwendet. Dadurch verringern sich die bekannten Einschränkungen der Vorgehensweise. Sie können auf der [Seite für die Registrierung eines Microsoft-Kontos](https://signup.live.com/) ein neues Konto erstellen.

	Führen Sie zum Ändern des Dienstadministrators die folgenden Schritte aus:

	1. Melden Sie sich beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.
	2. Wählen Sie das Abonnement aus, das Sie ändern möchten.
	3. Klicken Sie auf der rechten Seite auf **Abonnementdetails bearbeiten**.

		![azure-edit-subscription-details](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

	4. Geben Sie im Feld **DIENSTADMINISTRATOR** die E-Mail-Adresse des neuen Dienstadministrators ein.

		![change-subscription-service-admin](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

Führen Sie zum Zuordnen des Office 365-Mandanten zum Azure-Abonnement die folgenden Schritte aus:

1. 	Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.
2.	Klicken Sie im linken Bereich auf **ACTIVE DIRECTORY**.

	![classic-portal-active-directory-entry](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

	> [AZURE.NOTE] Der Office 365-Mandant sollte nicht angezeigt werden. Wird er angezeigt, überspringen Sie den nächsten Schritt.

	![aad-tenant-default](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Fügen Sie den Office 365-Mandanten zu Ihrem Azure-Abonnement hinzu.
	1. Klicken Sie auf **NEU** > **VERZEICHNIS** > **BENUTZERDEFINIERT ERSTELLEN**.

		![aad-custom-create](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

	2. Wählen Sie auf der Seite **Verzeichnis hinzufügen** unter **VERZEICHNIS** die Option **Vorhandenes Verzeichnis verwenden** aus, aktivieren Sie **Ich bin jetzt für die Abmeldung bereit.**, und klicken Sie auf **Fertig stellen** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

		![add-directory-use-existing](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

	3. Nachdem Sie abgemeldet wurden, melden Sie sich mit den Anmeldeinformationen des globalen Administrators des Office 365-Mandanten an.

		![sign-in-global-admin-office-365](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

	4. Klicken Sie auf **Weiter**.

		![use-contoso-directory-azure-verify](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

	5. Klicken Sie auf **Jetzt abmelden**.

		![use-contoso-directory-azure-confirm-and-sign-out](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

	6. Melden Sie sich mit den Anmeldeinformationen des Dienstadministrators beim [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) an.

		![azure-sign-in-service-admin](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

	7. Ihr Office 365-Mandant sollte auf dem Dashboard angezeigt werden.

		![office-365-tenant-appear-in-azure](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Ändern Sie das Verzeichnis, das mit dem Azure-Abonnement verknüpft ist.

	1. Klicken Sie auf **Einstellungen**.

		![azure-classic-portal-settings-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

	2. Klicken Sie auf Ihr Azure-Abonnement und anschließend auf **VERZEICHNIS BEARBEITEN**.

		![azure-subscription-edit-directory](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

	3. Klicken Sie auf **Weiter** ![next-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

		![azure-change-associated-directory](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

		> [AZURE.WARNING] Eine Warnung wird angezeigt, dass alle Co-Administratoren entfernt werden.

		![azure-confirm-directory-mapping](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

		>[AZURE.WARNING] Darüber hinaus werden alle [RBAC](./active-directory/role-based-access-control-configure.md)-Benutzer (Role-Based Access Control, rollenbasierte Zugriffssteuerung) mit zugewiesenem Zugriff in den vorhandenen Ressourcengruppen entfernt. In der Warnung wird jedoch nur auf die Entfernung der Co-Administratoren hingewiesen.

		![assigned-users-removed-resource-groups](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

	4. Klicken Sie auf **Fertig stellen** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Jetzt können Sie Ihre Office 365-Organisationskonten als Co-Administratoren zum AAD-Mandanten hinzufügen.

	1. Klicken Sie auf die Registerkarte **ADMINISTRATOREN** und anschließend auf **HINZUFÜGEN**.

		![azure-classic-portal-settings-administrators](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

	2. Geben Sie ein Organisationskonto Ihres Office 365-Mandanten ein, klicken Sie zum Auswählen des Azure-Abonnements, und klicken Sie dann auf **Fertig stellen** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

		![azure-add-co-administrator](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

	3. Gehen Sie zurück zur Registerkarten **ADMINISTRATOREN**. Hier sollte nun das Organisationskonto als Co-Administrator angezeigt werden.

		![azure-co-administrator-added](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Als Nächstes können Sie den Zugriff als Co-Administrator testen.

	1. Melden Sie sich beim Kontoverwaltungsportal ab.
	2. Öffnen Sie das [Kontoverwaltungsportal](https://account.windowsazure.com/subscriptions) oder das [Azure-Portal](https://portal.azure.com/).
	3. Wird auf der Azure-Anmeldeseite der Link **Sign in with your organization account** (Mit dem Organisationskonto anmelden) angezeigt, klicken Sie auf den Link. Überspringen Sie andernfalls diesen Schritt.

		![Azure-sign-in-with-orgid-link](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

	4. Geben Sie die Anmeldeinformationen des Co-Administrators ein, und klicken Sie dann auf **Anmelden**.

		![Azure-sign-in-with-co-admin](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## Nächste Schritte
Es gibt ähnliche Szenarien, in denen Sie bereits über ein Office 365-Abonnement verfügen und ein Azure-Abonnement wünschen, jedoch das vorhandene Office 365-Benutzerkonto (bzw. die Benutzerkonten) für Ihr Azure-Abonnement verwenden möchten. Alternativ sind Sie Azure-Abonnent und wünschen ein Office 365-Abonnement für die Benutzer in Ihrem vorhandenen Azure Active Directory. Informationen hierzu finden Sie unter [Verwenden Ihres vorhandenen Office 365-Kontos mit Ihrem Azure-Abonnement (oder umgekehrt)](billing-use-existing-office-365-account-azure-subscription.md).

<!---HONumber=AcomDC_0921_2016-->