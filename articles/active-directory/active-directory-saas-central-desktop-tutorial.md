---
title: 'Tutorial: Azure Active Directory-Integration mit Central Desktop | Microsoft Docs'
description: "Erfahren Sie, wie Sie Central Desktop mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5923bf1012c862d500d3c9b0062f0d7662e9f1bc


---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Tutorial: Azure Active Directory-Integration mit Central Desktop
In diesem Tutorial wird die Integration von Azure und Central Desktop erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Abonnement für Central Desktop mit aktiviertem einmaligen Anmelden und einen Central Desktop-Mandanten

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Central Desktop
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Scenario")

## <a name="enabling-the-application-integration-for-central-desktop"></a>Aktivieren der Anwendungsintegration für Central Desktop
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Central Desktop aktivieren.

### <a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Central Desktop
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Central Desktop** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **Central Desktop** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Central Desktop zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei in Ihren Central Desktop-Mandanten hochladen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Central Desktop** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Central Desktop anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Configure single sign-on")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**: 
   
   * Geben Sie im Textfeld **Central Desktop-Anmelde-URL** die URL Ihres Central Desktop-Mandanten ein (z.B. *http://contoso.centraldesktop.com*).
   * Geben Sie im Textfeld „Central Desktop-Antwort-URL“ Ihre Central Desktop AssertionConsumerService-URL ein (z.B. https://contoso.centraldesktop.com/saml2-assertion.php).
   
   > [!NOTE]
   > Sie finden den Wert in den Central Desktop-Metadaten (z.B. *http://contoso.centraldesktop.com*).
   > 
   > 
   
   ![App-URL konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Configure app URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden bei Central Desktop konfigurieren** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Configure single sign-on")
5. Melden Sie sich bei Ihrem **Central Desktop** -Mandanten an.
6. Wechseln Sie zu **Einstellungen**, klicken Sie auf **Erweitert**, und klicken Sie dann auf **Einmaliges Anmelden**.
   
   ![Setup - Advanced](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup - Advanced")
7. Führen Sie im Dialogfeld **Single Sign On Settings** die folgenden Schritte aus:
   
   ![Single Sign On Settings](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Single Sign On Settings")
   
   1. Aktivieren Sie **Enable SAML v2 Single Sign On**.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **SSO-URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **SSO-Anmelde-URL** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **SSO-Abmelde-URL** ein.
8. Führen Sie im Abschnitt **Message Signature Verification Method** die folgenden Schritte aus:
   
   ![Message Signature Verification Method](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Message Signature Verification Method")
   
   1. Wählen Sie **Certificate**aus.
   2. Wählen Sie in der Liste **SSO-Zertifikat** die Option **RSH SHA256** aus.
   3. Erstellen Sie eine Textdatei aus dem heruntergeladenen Zertifikat, kopieren Sie den Inhalt der Textdatei, und fügen Sie ihn in das Feld **SSO Certificate** ein.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Aktivieren Sie **Display a link to your SAMLv2 login page**.
9. Klicken Sie auf **Aktualisieren**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Configure single sign-on](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Configure single sign-on")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich AAD-Benutzer anmelden können, müssen sie in der Central Desktop-Anwendung bereitgestellt werden. In diesem Abschnitt wird beschrieben, wie in Central Desktop AAD-Benutzerkonten erstellt werden.

### <a name="to-provision-user-accounts-to-central-desktop"></a>So stellen Sie Benutzerkonten für Central Desktop bereit
1. Melden Sie sich bei Ihrem Central Desktop-Mandanten an.
2. Wechseln Sie zu **Personen \> Interne Mitglieder**.
3. Klicken Sie auf **Add Internal Members**.
   
   ![Personen](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "People")
4. Geben Sie im Textfeld **E-Mail-Adresse des neuen Mitglieds** ein AAD-Konto ein, das Sie bereitstellen möchten, und klicken Sie dann auf **Weiter**.
   
   ![Email Addresses of New Members](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Email Addresses of New Members")
5. Klicken Sie auf **Add Internal member(s)**.
   
   ![Add Internal Member](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Add Internal Member")
   
   > [!NOTE]
   > Die Benutzer, die Sie hinzugefügt haben, erhalten eine E-Mail mit einem Link zur Bestätigung, auf den sie zur Bestätigung klicken müssen.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Central Desktop-Benutzerkonten oder mithilfe der von Central Desktop bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>So weisen Sie Central Desktop Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Central Desktop** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


