---
title: 'Tutorial: Azure Active Directory-Integration mit xMatters OnDemand | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie xMatters OnDemand mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: 143e4a856a31a44a9ec909d07d5f24710a6e2803


---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Tutorial: Azure Active Directory-Integration mit xMatters OnDemand
In diesem Tutorial wird die Integration von Azure und xMatters OnDemand erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen xMatters OnDemand-Mandanten

Nach Abschluss dieses Tutorials können sich die xMatters OnDemand zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer xMatters OnDemand-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für xMatters OnDemand
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Scenario")

## <a name="enabling-the-application-integration-for-xmatters-ondemand"></a>Aktivieren der Anwendungsintegration für xMatters OnDemand
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für xMatters OnDemand aktivieren.

### <a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für xMatters OnDemand
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** **xMatters OnDemand** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Application gallery")

7. Wählen Sie im Ergebnisbereich **xMatters OnDemand** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![xMatters OnDemand](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei xMatters OnDemand zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **xMatters OnDemand** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Configure single sign-on")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei xMatters OnDemand anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Configure single sign-on")

3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Configure app URL")
   
    a. Geben Sie im Textfeld **xMatters OnDemand-Anmelde-URL** die URL in folgendem Format ein: `https://<tenant-name>.XMattersOnDemandapp.com`
   
    b. Klicken Sie auf **Weiter**.

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für xMatters OnDemand** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal als: **c:\\XMatters OnDemand.cer**.
   
    > [!IMPORTANT]
    > Sie müssen das Zertifikat an das xMatters-Supportteam weiterleiten. Das Zertifikat muss vom xMatters-Supportteam hochgeladen werden, bevor Sie die Konfiguration der einmaligen Anmeldung abschließen können.
    > 
    > 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Configure single sign on")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der xMatters OnDemand-Unternehmenswebsite als Administrator an.

6. Klicken Sie in der Symbolleiste oben auf **Administrator**, und klicken Sie dann in der Navigationsleiste auf der linken Seite auf **Unternehmensdetails**.
   
    ![Administrator](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Admin")

7. Führen Sie auf der Seite **SAML-Konfiguration** die folgenden Schritte aus:
   
    ![SAML-Konfiguration](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML configuration")
   
    a. Wählen Sie **SAML aktivieren**.
   
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für xMatters OnDemand** den Wert der **Identitätsanbieter-ID**, und fügen Sie ihn in das Textfeld **Identitätsanbieter-ID** ein.
   
    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für xMatters OnDemand** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **URL für einmaliges Anmelden** ein.
   
    d. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für xMatters OnDemand** den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **URL für einmaliges Abmelden** ein.
   
    e. Klicken Sie auf der Seite „Unternehmensdetails“ oben auf **Speichern**.
    
    ![Unternehmensdetails](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Company details")

8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Configure single sign on")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Um Azure AD-Benutzern die Anmeldung bei xMatters OnDemand zu ermöglichen, müssen sie in xMatters OnDemand bereitgestellt werden.  
Im Fall von xMatters OnDemand ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **XMatters OnDemand** -Mandanten an.

2. Klicken Sie auf die Registerkarte **Benutzer** .

3. Klicken Sie auf **Benutzer hinzufügen**.
  
    ![Benutzer](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Users")

4. Wählen Sie **Aktiv**.

5. Führen Sie im Abschnitt **Benutzer hinzufügen** die folgenden Schritte aus:
   
    ![Benutzer hinzufügen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Add a User")
   
    a. Geben Sie die **Benutzer-ID**, den **Vornamen**, den **Nachnamen** und die **Website** eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
    
    b. Klicken Sie auf **Speichern**.


## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>So weisen Sie xMatters OnDemand Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **xMatters OnDemand** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Assign users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


