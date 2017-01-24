---
title: 'Tutorial: Azure Active Directory-Integration mit UserVoice | Microsoft Docs'
description: "Erfahren Sie, wie Sie UserVoice mit Azure Active Directory verwenden können, um einmaliges Anmelden, die automatisierte Bereitstellung u. v. m. zu aktivieren."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 684a405b-8932-46f6-b43a-4d97a42b6b87
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1cef7ff21a8d076c89688f1fe75cebdb7c468199
ms.openlocfilehash: 593decb7595371396787a04e4a3252cfa65b2643


---
# <a name="tutorial-azure-active-directory-integration-with-uservoice"></a>Tutorial: Azure Active Directory-Integration mit UserVoice
In diesem Tutorial wird die Integration von Azure und UserVoice erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein UserVoice-Mandant

Nach Abschluss dieses Tutorials können sich die UserVoice zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer UserVoice-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für UserVoice
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-uservoice-tutorial/IC777514.png "Scenario")

## <a name="enabling-the-application-integration-for-uservoice"></a>Aktivieren der Anwendungsintegration für UserVoice
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für UserVoice aktivieren.

### <a name="to-enable-the-application-integration-for-uservoice-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für UserVoice:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-uservoice-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-uservoice-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-uservoice-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-uservoice-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** als Suchbegriff **UserVoice** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-uservoice-tutorial/IC777513.png "Application gallery")

7. Wählen Sie im Ergebnisbereich **UserVoice** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![UserVoice](./media/active-directory-saas-uservoice-tutorial/IC777810.png "UserVoice")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei UserVoice zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für UserVoice müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **UserVoice** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-uservoice-tutorial/IC777515.png "Configure single sign-on")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei UserVoice anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-uservoice-tutorial/IC777516.png "Configure single sign-on")

3. Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **UserVoice-Anmelde-URL** die URL im Format „*https://\<Mandantenname\>.UserVoice.com*“ ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-uservoice-tutorial/IC777517.png "Configure App URL")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für UserVoice** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal unter **C:\\UserVoice.cer**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-uservoice-tutorial/IC777518.png "Configure single sign-on")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der UserVoice-Unternehmenswebsite als Administrator an.

6. Klicken Sie in der Symbolleiste oben die Option „Einstellungen“, und wählen Sie dann aus dem Menü die Option „Webportal“ aus.
   
    ![Einstellungen](./media/active-directory-saas-uservoice-tutorial/IC777519.png "Settings")

7. Klicken Sie auf der Registerkarte **Webportal** des Abschnitts **Benutzerauthentifizierung** auf **Bearbeiten**, um die Dialogfeldseite **Benutzerauthentifizierung bearbeiten** zu öffnen.
   
    ![Webportal](./media/active-directory-saas-uservoice-tutorial/IC777520.png "Web portal")

8. Führen Sie auf der Dialogseite **Benutzerauthentifizierung bearbeiten** die folgenden Schritte aus:
   
    ![Benutzerauthentifizierung bearbeiten](./media/active-directory-saas-uservoice-tutorial/IC777521.png "Edit user authentication")
   
    a. Klicken Sie auf **Einmaliges Anmelden (SSO)**.
 
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für UserVoice** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Einmalige Remoteanmeldung** ein.

    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für UserVoice** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Einmalige Remoteabmeldung** ein.
 
    d. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **SHA1-Fingerabdruck des aktuellen Zertifikats** ein.  
      
    > [!TIP]
    > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
    > 
    > 
  
    e. Klicken Sie auf **Authentifizierungseinstellungen speichern**.

9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Configure single sign-on](./media/active-directory-saas-uservoice-tutorial/IC777522.png "Configure single sign-on")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei UserVoice anmelden können, müssen sie in UserVoice bereitgestellt werden.  
Im Fall von UserVoice ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **UserVoice** -Mandanten an.

2. Wechseln Sie zu **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-uservoice-tutorial/IC777811.png "Settings")

3. Klicken Sie auf **Allgemein**.

4. Klicken Sie auf **Agents und Berechtigungen**.
   
    ![Agents und Berechtigungen](./media/active-directory-saas-uservoice-tutorial/IC777812.png "Agents and permissions")

5. Klicken Sie auf **Administratoren hinzufügen**.
   
    ![ Administratoren hinzufügen](./media/active-directory-saas-uservoice-tutorial/IC777813.png "Add admins")

6. Führen Sie im Dialogfeld **Benutzer einladen** die folgenden Schritte aus:
   
    ![Benutzer einladen](./media/active-directory-saas-uservoice-tutorial/IC777814.png "Invite admins")
   
    a. Geben Sie in das Textfeld „E-Mails“ die E-Mail-Adresse eines vorhandenen Azure AD-Kontos ein, das Sie bereitstellen möchten, und klicken Sie auf **Hinzufügen**.
   
    b. Klicken Sie auf **Invite**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von UserVoice-Benutzerkonten oder mithilfe der von UserVoice bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-uservoice-perform-the-following-steps"></a>So weisen Sie UserVoice Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **UserVoice** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-uservoice-tutorial/IC777523.png "Assign users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-uservoice-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


