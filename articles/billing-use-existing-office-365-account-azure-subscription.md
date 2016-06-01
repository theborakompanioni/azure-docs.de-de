<properties
	pageTitle="Office 365- und Azure-Abonnements übergreifendes Freigeben eines einzelnen Azure AD-Mandanten | Microsoft Azure"
	description="Erfahren Sie, wie Sie Ihren Office 365-Azure AD-Mandanten und seine Benutzer mit Ihrem Azure-Abonnement freigeben – oder umgekehrt."
	services="billing"
	documentationCenter=""
	authors="jiangchen79"
	manager="mbaldwin"
	editor=""
	tags="top-support-issue"/>

<tags
	ms.service="billing"
	ms.workload="na"
	ms.tgt_pltfrm="ibiza"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/12/2016"
	ms.author="cjiang"/>

# Verwenden Ihres vorhandenen Office 365-Kontos mit Ihrem Azure-Abonnement (oder umgekehrt)
Szenario: Sie verfügen bereits über ein Office 365-Abonnement und wünschen ein Azure-Abonnement, möchten jedoch das vorhandene Office 365-Benutzerkonto (bzw. die Benutzerkonten) für Ihr Azure-Abonnement verwenden. Alternativ sind Sie Azure-Abonnent und wünschen ein Office 365-Abonnement für die Benutzer in Ihrem vorhandenen Azure Active Directory. In diesem Artikel erfahren Sie, wie einfach beides zu erreichen ist.

> [AZURE.NOTE] Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA).

## Kurzanleitung

- Wenn Sie bereits über ein Office 365-Abonnement verfügen und sich bei Azure anmelden möchten, verwenden Sie die Option **Melden Sie sich mit Ihrem Organisationskonto an**, und setzen Sie die Azure-Anmeldung mit Ihrem Office 365-Konto fort. Weitere Informationen finden Sie unter [Ausführliche Schritte](#s1).
- Wenn Sie bereits über ein Azure-Abonnement verfügen und ein Office 365-Abonnement wünschen, melden Sie sich mit Ihrem Azure-Konto bei Office 365 an, und fahren Sie mit den Anmeldeschritten fort. Nach Abschluss wird das Office 365-Abonnement dem gleichen Azure Active Directory hinzugefügt, dem Ihr Azure-Abonnement angehört. Weitere Informationen finden Sie unter [Ausführliche Schritte](#s2).

>[AZURE.NOTE] Damit Sie ein Office 365-Abonnement erhalten können, muss das Konto, das Sie zum Anmelden verwenden, in Ihrem Azure AD-Mandanten Mitglied der Rolle „Globaler Administrator“ oder „Abrechnungsadministrator“ sein. [Erfahren Sie, wie Sie die Rolle Ihres Azure Active Directory kennen lernen](#how-to-know-your-role-in-your-azure-active-directory)

In den [Hintergrundinformationen](#background-information) weiter unten in diesem Artikel erfahren Sie, was passiert, wenn Sie einem Konto ein Abonnement hinzufügen.

## Ausführliche Schritte
<a id="s1"></a>
### Szenario 1: Office 365-Benutzer möchten Azure kaufen
In diesem Szenario nehmen wir an, dass Kelley Wall eine Benutzerin ist, die ein Office 365-Abonnement besitzt und Azure abonnieren möchte. Es gibt zwei zusätzliche aktive Benutzerinnen, Jane und Tricia. Kelleys Konto ist admin@contoso.onmicrosoft.com.

![office365-users-admin-center.png](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Melden Sie sich folgendermaßen bei Azure an:

1. Melden Sie sich unter [Azure.com](https://azure.microsoft.com/) bei Azure an. Klicken Sie auf **Kostenlos testen**. Klicken Sie auf der nächsten Seite auf **Jetzt starten**.

	![azure-signup-try-free](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Klicken Sie auf **Melden Sie sich mit Ihrem Organisationskonto an**.

	![sign-in-to-azure](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Melden Sie sich mit Ihrem Office 365-Konto an. In diesem Fall handelt es sich dabei um Kelleys Office 365-Konto.

	![sign-in-with-org-account](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Geben Sie die Informationen ein, und schließen Sie den Anmeldeprozess ab.

	![azure-sign-up-fill-information](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

5. Klicken Sie auf **Mit der Verwaltung meines Diensts beginnen**, und Sie sind startklar.

	![azure-start-managing-my-service](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Jetzt sind Sie soweit. Sie sehen, dass im Azure-Portal die gleichen Benutzer im gleichen Verzeichnis angezeigt werden. Gehen Sie dazu wie folgt vor:

1. Klicken Sie im obigen Screenshot auf **Mit der Verwaltung meines Diensts beginnen**.
2. Klicken Sie auf **Durchsuchen** und dann auf **Active Directory**.

	![azure-portal-browse-ad](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klicken Sie auf **BENUTZER**.

	![azure-portal-ad-users-tab](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Wie erwartet werden alle Benutzer einschließlich Kelley aufgeführt.

	![azure-portal-ad-users](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### Szenario 2: Azure-Benutzer planen, Office 365 zu kaufen

In diesem Szenario ist Kelley Wall eine Benutzerin mit einem Azure-Abonnement unter dem Konto admin@contoso.onmicrosoft.com. Kelley möchte Office 365 abonnieren und das gleiche Verzeichnis verwenden, über das sie bereits für Azure verfügt.

>[AZURE.NOTE] Damit Sie ein Office 365-Abonnement erhalten können, muss das Konto, das Sie zum Anmelden verwenden, in Ihrem Azure AD-Mandanten Mitglied der Rolle „Globaler Administrator“ oder „Abrechnungsadministrator“ sein. [Erfahren Sie, wie Sie die Rolle Ihres Azure Active Directory kennen lernen](#how-to-know-your-role-in-your-azure-active-directory)

![azure-portal-settings-subscription](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![azure-portal-ads-users](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Gehen Sie folgendermaßen vor, um Office 365 zu abonnieren:

1. Wechseln Sie zur [Office 365-Produktseite](https://products.office.com/business), und wählen Sie dann einen Plan aus, der für Sie geeignet ist.
2. Nachdem Sie den Plan ausgewählt haben, wird die folgende Seite angezeigt. Füllen Sie das Formular nicht aus. Klicken Sie oben rechts auf der Seite auf **Anmelden**.

	![office-365-trial-page](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Melden Sie sich mit den Anmeldeinformationen Ihres Kontos an. In diesem Fall handelt es sich dabei um Kelleys Konto.

	![office-365-sign-in](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klicken auf **Jetzt ausprobieren**.

	![office-365-confirm-your-order](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Klicken Sie auf der Bestellbestätigungsseite auf **Weiter**.

	![office-365-order-receipt](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Jetzt sind Sie soweit. Sie sehen, dass im Office 365 Admin Center die gleichen Benutzer aus dem Verzeichnis „Contoso“ als aktive Benutzer angezeigt werden. Gehen Sie dazu wie folgt vor:

1. Öffnen Sie das Office 365 Admin Center.
2. Erweitern Sie **BENUTZER**, und klicken Sie dann auf **Aktive Benutzer**.

	![office-365-admin-center-users](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### So lernen Sie Ihre Rolle in Ihrem Azure Active Directory kennen

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/) an.
2. Klicken Sie auf **Durchsuchen** und dann auf **Active Directory**.

	![azure-portal-browse-ad](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klicken Sie auf **BENUTZER**.

	![azure-portal-default-ad-users](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klicken Sie auf den Benutzer. In diesem Beispiel ist dies Kelley Wall.
5. Beachten Sie das Feld der **ORGANISATIONSROLLE**.

	![azure-portal-user-identity](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## Hintergrundinformationen
Office 365 und Azure verwenden den Azure Active Directory-Dienst (AAD) zum Verwalten von Benutzern und Abonnements. Betrachten Sie ein Azure-Verzeichnis als Container, in dem Sie Benutzer und Abonnements gruppieren können. Damit Sie für Ihre Microsoft Azure- und Office 365-Abonnements das gleiche Benutzerkonto verwenden können, müssen Sie sicherstellen, dass die Abonnements in dem gleichen Verzeichnis erstellt werden.

- Ein Abonnement wird in einem Verzeichnis erstellt, nicht umgekehrt.
- Benutzer gehören zu Verzeichnissen, nicht umgekehrt.
- Ein Abonnement wird in dem Verzeichnis des Benutzers untergebracht, der das Abonnement erstellt. Folglich ist Ihr Office 365-Abonnement an das Konto Ihres Azure-Abonnements gebunden, wenn Sie das Konto verwenden, um das Office 365-Abonnement zu erstellen.

![background-information](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

**Hinweise:**

- Azure-Abonnements gehören einzelnen Benutzern im Verzeichnis.
- Office 365-Abonnements gehören dem Verzeichnis selbst. Benutzer im Verzeichnis können Vorgänge an diesen Abonnements ausführen, wenn sie über die erforderlichen Berechtigungen verfügen.

<!---HONumber=AcomDC_0518_2016-->