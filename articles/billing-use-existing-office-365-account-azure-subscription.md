---
title: Office 365- und Azure-Abonnements übergreifendes Freigeben eines einzelnen Azure AD-Mandanten | Microsoft Docs
description: Erfahren Sie, wie Sie Ihren Office 365-Azure AD-Mandanten und seine Benutzer mit Ihrem Azure-Abonnement freigeben – oder umgekehrt.
services: ''
documentationcenter: ''
author: JiangChen79
manager: mbaldwin
editor: ''
tags: billing,top-support-issue

ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cjiang

---
# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Verwenden eines vorhandenen Office 365-Kontos mit Ihrem Azure-Abonnement (oder umgekehrt)
Szenario: Sie verfügen bereits über ein Office 365-Abonnement und wünschen ein Azure-Abonnement, möchten jedoch die vorhandenem Office 365-Benutzerkonten für Ihr Azure-Abonnement verwenden. Alternativ sind Sie Azure-Abonnent und wünschen ein Office 365-Abonnement für die Benutzer in Ihrem vorhandenen Azure Active Directory. In diesem Artikel erfahren Sie, wie einfach beides zu erreichen ist.

> [!NOTE]
> Dieser Artikel gilt nicht für Enterprise Agreement-Kunden (EA). Wenn Sie bei einem Verfahren in diesem Artikel weitere Hilfe benötigen, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
> 
> 

## <a name="quick-guidance"></a>Kurzanleitung
* Wenn Sie bereits über ein Office 365-Abonnement verfügen und sich bei Azure anmelden möchten, verwenden Sie die Option **Mit Organisationskonto anmelden**. Fahren Sie dann mit dem Azure-Anmeldevorgang über Ihr Office 365-Konto fort. Weitere Informationen finden Sie in den [ausführlichen Schritten weiter unten in diesem Artikel](#s1).
* Wenn Sie bereits über ein Azure-Abonnement verfügen und ein Office 365-Abonnement wünschen, melden Sie sich mit Ihrem Azure-Konto bei Office 365 an. Fahren Sie dann mit den Anmeldeschritten fort. Nach Abschluss der Anmeldung wird das Office 365-Abonnement der gleichen Azure Active Directory-Instanz hinzugefügt, der Ihr Azure-Abonnement angehört. Weitere Informationen finden Sie in den [ausführlichen Schritten weiter unten in diesem Artikel](#s2).

> [!NOTE]
> Damit Sie ein Office 365-Abonnement erhalten können, muss das für die Registrierung verwendete Konto Mitglied der Rolle „Globaler Administrator“ oder „Abrechnungsadministrator“ auf Ihrem Azure Active Directory-Mandanten sein. [Informationen zum Bestimmen der Rolle in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).
> 
> 

In den [Hintergrundinformationen weiter unten in diesem Artikel](#background-information) erfahren Sie, was passiert, wenn Sie einem Konto ein Abonnement hinzufügen.

## <a name="detailed-steps"></a>Ausführliche Schritte
<a id="s1"></a>

### <a name="scenario-1:-office-365-users-who-plan-to-buy-azure"></a>Szenario 1: Office 365-Benutzer, die Azure erwerben möchten
In diesem Szenario nehmen wir an, dass Kelley Wall eine Benutzerin ist, die ein Office 365-Abonnement besitzt und Azure abonnieren möchte. Es gibt zwei zusätzliche aktive Benutzerinnen, Jane und Tricia. Der Name von Kelleys Konto lautet „admin@contoso.onmicrosoft.com“.

![Admin Center für Office 365-Benutzer](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Melden Sie sich folgendermaßen bei Azure an:

1. Melden Sie sich unter [Azure.com](https://azure.microsoft.com/)bei Azure an. Klicken Sie auf **Kostenlos testen**. Klicken Sie auf der nächsten Seite auf **Jetzt starten**.
   
    ![Testen Sie Azure kostenlos.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)
2. Klicken Sie auf **Melden Sie sich mit Ihrem Organisationskonto an**.
   
    ![Melden Sie sich bei Azure an.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)
3. Melden Sie sich mit Ihrem Office 365-Konto an. In diesem Fall handelt es sich dabei um Kelleys Office 365-Konto.
   
    ![Melden Sie sich mit Ihrem Office 365-Konto an.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)
4. Geben Sie die Informationen ein, und schließen Sie den Anmeldeprozess ab.
   
    ![Geben Sie die erforderlichen Informationen ein, und schließen Sie die Registrierung ab.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)
   
    ![Klicken Sie auf „Mit der Verwaltung meines Diensts beginnen“.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Der Vorgang ist nun abgeschlossen. Die Benutzer sollten nun auch im Azure-Portal angezeigt werden. Gehen Sie dazu wie folgt vor:

1. Klicken Sie im zuvor gezeigten Screenshot auf **Mit der Verwaltung meines Diensts beginnen**.
2. Klicken Sie auf **Durchsuchen** und dann auf **Active Directory**.
   
    ![Klicken Sie auf „Durchsuchen“ und dann auf „Active Directory“.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)
3. Klicken Sie auf **BENUTZER**.
   
    ![Die Registerkarte „Benutzer“](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)
4. Alle Benutzer, einschließlich Kelley, werden wie erwartet aufgeführt.
   
    ![Liste der Benutzer](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>

### <a name="scenario-2:-azure-users-who-plan-to-buy-office-365"></a>Szenario 2: Azure-Benutzer, die Office 365 erwerben möchten
In diesem Szenario ist Kelley Wall eine Benutzerin mit einem Azure-Abonnement im Konto admin@contoso.onmicrosoft.com. Kelley möchte Office 365 abonnieren und das gleiche Verzeichnis verwenden, über das sie bereits für Azure verfügt.

> [!NOTE]
> Damit Sie ein Office 365-Abonnement erhalten können, muss das für die Anmeldung verwendete Konto Mitglied der Rolle „Globaler Administrator“ oder „Abrechnungsadministrator“ auf Ihrem Azure Active Directory-Mandanten sein. [Informationen zum Bestimmen der Rolle in Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).
> 
> 

![Abonnementeinstellungen im Azure-Portal](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Active Directory-Benutzer im Azure-Portal](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Gehen Sie folgendermaßen vor, um Office 365 zu abonnieren:

1. Wechseln Sie zur [Office 365-Produktseite](https://products.office.com/business), und wählen Sie dann einen Plan aus, der für Sie geeignet ist.
2. Nachdem Sie den Plan ausgewählt haben, wird die folgende Seite angezeigt. Füllen Sie das Formular nicht aus. Klicken Sie oben rechts auf der Seite auf **Anmelden** .
   
    ![Seite für den Office 365-Test](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)
3. Melden Sie sich mit Ihren Kontoanmeldeinformationen an. In diesem Fall handelt es sich dabei um Kelleys Konto.
   
    ![Office 365-Anmeldung](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)
4. Klicken auf **Jetzt ausprobieren**.
   
    ![Bestätigen Sie Ihre Bestellung von Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)
5. Klicken Sie auf der Bestellbestätigungsseite auf **Weiter**.
   
    ![Office 365-Bestellbestätigung](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Der Vorgang ist nun abgeschlossen. Im Office 365 Admin Center sollten Benutzer aus dem Verzeichnis „Contoso“ als aktive Benutzer angezeigt werden. Gehen Sie dazu wie folgt vor:

1. Öffnen Sie das Office 365 Admin Center.
2. Erweitern Sie **BENUTZER**, und klicken Sie dann auf **Aktive Benutzer**.
   
    ![Benutzer im Office 365 Admin Center](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>So lernen Sie Ihre Rolle in Ihrem Azure Active Directory kennen
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.
2. Klicken Sie auf **Durchsuchen** und dann auf **Active Directory**.
   
    ![Active Directory im Azure-Portal](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)
3. Klicken Sie auf **BENUTZER**.
   
    ![Active Directory-Standardbenutzer im Azure-Portal](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)
4. Klicken Sie auf den Benutzer. In diesem Beispiel ist der Benutzer Kelley Wall.
   
    Beachten Sie das Feld der **ORGANISATIONSROLLE**.
   
    ![Benutzeridentität im Azure-Portal](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Hintergrundinformationen zu Azure- und Office 365-Abonnements
Office 365 und Azure verwenden den Azure Active Directory-Dienst zum Verwalten von Benutzern und Abonnements. Betrachten Sie ein Azure-Verzeichnis als Container, in dem Sie Benutzer und Abonnements gruppieren können. Damit Sie für Ihre Azure- und Office 365-Abonnements das gleiche Benutzerkonto verwenden können, müssen Sie sicherstellen, dass die Abonnements im gleichen Verzeichnis erstellt werden. Berücksichtigen Sie dabei Folgendes:

* Ein Abonnement wird in einem Verzeichnis erstellt, nicht umgekehrt.
* Benutzer gehören zu Verzeichnissen, nicht umgekehrt.
* Ein Abonnement wird in dem Verzeichnis des Benutzers erstellt, der das Abonnement erstellt. Folglich ist Ihr Office 365-Abonnement an das Konto Ihres Azure-Abonnements gebunden, wenn Sie dieses Konto verwenden, um das Office 365-Abonnement zu erstellen.

![Hintergrundinformationen](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Weitere Informationen finden Sie unter [Beziehung zwischen Azure-Abonnements und Azure Active Directory](active-directory/active-directory-how-subscriptions-associated-directory.md).

> [!NOTE]
> Azure-Abonnements gehören einzelnen Benutzern im Verzeichnis.
> 
> [!NOTE]
> Office 365-Abonnements gehören dem Verzeichnis selbst. Wenn Benutzer innerhalb des Verzeichnisses über die erforderlichen Berechtigungen verfügen, können sie Vorgänge für diese Abonnements ausführen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie das Azure-Abonnement und das Office 365-Abonnement in der Vergangenheit separat erworben haben und jetzt über das Azure-Abonnement auf den Office 365-Mandanten zugreifen möchten, finden Sie unter [Zuordnen eines Office 365-Mandanten zu einem Azure-Abonnement](billing-add-office-365-tenant-to-azure-subscription.md) weitere Informationen.

> [!NOTE]
> Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.
> 
> 

<!--HONumber=Oct16_HO2-->


