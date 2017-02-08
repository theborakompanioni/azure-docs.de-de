---
title: 'Tutorial: Azure Active Directory-Integration mit Bonus.ly | Microsoft Docs'
description: "Erfahren Sie, wie Sie Bonus.ly mit Azure Active Directory verwenden können, um einmaliges Anmelden, die automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 25a4e33ab85139bd3f871f46cef1d12a7b9cd06b


---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Lernprogramm: Azure Active Directory-Integration mit Bonus.ly
In diesem Lernprogramm wird die Integration von Azure und Bonus.ly veranschaulicht. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Testkonto bei Bonus.ly

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Bonus.ly
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bonus-tutorial/IC773679.png "Scenario")

## <a name="enabling-the-application-integration-for-bonusly"></a>Aktivieren der Anwendungsintegration für Bonus.ly
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Bonus.ly aktivieren.

### <a name="to-enable-the-application-integration-for-bonusly-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Bonus.ly
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-bonus-tutorial/IC773680.png "Enable single sign-on")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bonus-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bonus-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bonus-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Bonus.ly** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bonus-tutorial/IC773681.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **Bonus.ly** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Bonus.ly](./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Bonus.ly zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Bonus.ly müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI)(in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Bonus.ly** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC749323.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bonus.ly anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773683.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Bonus.ly-Mandant-URL** die URL im Format "*https://\<Mandantenname\>.Bonus.ly*" ein, und klicken Sie dann auf **Weiter**: 
   
   ![App-URL konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773684.png "Configure app URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden für Bonus.ly konfigurieren** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal als **C:\\Bonusly.cer**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773685.png "Configure single sign-on")
5. Melden Sie sich in einem anderen Browserfenster mit Ihrem **Bonus.ly** -Mandanten an.
6. Klicken Sie in der Symbolleiste oben auf **Einstellungen**, und wählen Sie dann **Integrationen und Apps** aus.
   
   ![Bonus.ly](./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")
7. Wählen Sie unter **Einmaliges Anmelden** **SAML** aus.
8. Führen Sie auf der Dialogseite **SAML** die folgenden Schritte aus:
   
   ![Bonus.ly](./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP SSO target URL** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert für **Aussteller-ID**, und fügen Sie ihn in das Textfeld **IdP Issuer** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP Login URL** ein.
   4. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Cert-Fingerabdruck** ein.
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
      > 
      > 
9. Klicken Sie auf **Speichern**.
10. Wählen Sie im klassischen Microsoft Azure-Portal die Bestätigung der Konfiguration aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773689.png "Configure single sign-on")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Bonus.ly anmelden können, müssen sie in Bonus.ly bereitgestellt werden.  
Im Fall von Bonus.ly ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich in einem Browserfenster mit Ihrem Bonus.ly-Mandanten an.
2. Klicken Sie unten auf der Seite auf **Settings**
   
   ![Settings](./media/active-directory-saas-bonus-tutorial/IC781041.png "Settings")
3. Klicken Sie auf die Registerkarte **Users and bonuses** .
   
   ![Users and bonuses](./media/active-directory-saas-bonus-tutorial/IC781042.png "Users and bonuses")
4. Klicken Sie auf **Manage Users**.
   
   ![Manage Users](./media/active-directory-saas-bonus-tutorial/IC781043.png "Manage Users")
5. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-bonus-tutorial/IC781044.png "Add User")
6. Führen Sie im Dialogfeld **Add User** die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-bonus-tutorial/IC781045.png "Add User")
   
   1. Geben Sie in die entsprechenden Textfelder die „**E-Mail-Adresse**, den **Vornamen** und den **Nachnamen**“ eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.
   
   > [!NOTE]
   > Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Bonus.ly-Benutzerkonten oder mithilfe von APIs erstellen, die von Bonus.ly zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-bonusly-perform-the-following-steps"></a>So weisen Sie Bonus.ly Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für Bonus.ly auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-bonus-tutorial/IC773690.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bonus-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


