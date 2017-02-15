---
title: 'Tutorial: Azure Active Directory-Integration mit Citrix ShareFile | Microsoft Docs'
description: "Erfahren Sie, wie Sie Citrix ShareFile mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 30f5ed9a5e9371193ef7663cdd8896793ca6359b


---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Lernprogramm: Azure Active Directory-Integration mit Citrix-ShareFile
In diesem Lernprogramm wird die Integration von Azure und Citrix ShareFile erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Citrix ShareFile-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Citrix ShareFile zugewiesen haben, mittels einmaligen Anmeldens auf der Citrix ShareFile-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Sie können aber auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Citrix ShareFile
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-citrix-sharefile-tutorial/IC773620.png "Scenario")

## <a name="enabling-the-application-integration-for-citrix-sharefile"></a>Aktivieren der Anwendungsintegration für Citrix ShareFile
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Citrix ShareFile aktivieren.

### <a name="to-enable-the-application-integration-for-citrix-sharefile-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Citrix ShareFile
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-citrix-sharefile-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-citrix-sharefile-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-citrix-sharefile-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-citrix-sharefile-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Citrix ShareFile** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-citrix-sharefile-tutorial/IC773621.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **Citrix ShareFile** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Citrix ShareFile](./media/active-directory-saas-citrix-sharefile-tutorial/IC773622.png "Citrix ShareFile")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Citrix ShareFile zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Citrix ShareFile** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-citrix-sharefile-tutorial/IC773623.png "Enable single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Citrix ShareFile anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-citrix-sharefile-tutorial/IC773624.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL für Citrix ShareFile** die URL im Format `https://<tenant-name>.shareFile.com` ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-citrix-sharefile-tutorial/IC773625.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Citrix ShareFile** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-citrix-sharefile-tutorial/IC773626.png "ConfigureSingle Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der **Citrix ShareFile** -Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Admin**.
7. Klicken Sie im linken Navigationsbereich auf **Configure Single Sign-On**.
   
   ![Kontoverwaltung](./media/active-directory-saas-citrix-sharefile-tutorial/IC773627.png "Account Administration")
8. Gehen Sie auf der Dialogseite **SSO-/SAML 2.0-Konfiguration** unter **Grundeinstellungen** folgendermaßen vor:
   
   ![Einmaliges Anmelden](./media/active-directory-saas-citrix-sharefile-tutorial/IC773628.png "Single sign-on")
   
   1. Aktivieren Sie **Enable SAML**.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Citrix ShareFile** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **Ihre IDP-Aussteller-/Entitäts-ID** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Citrix ShareFile** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Citrix ShareFile** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
   5. Klicken Sie auf **Ändern** neben dem Feld **X.509-Zertifikat**, und laden Sie dann das aus dem klassischen Azure AD-Portal heruntergeladene Zertifikat hoch.
      ![Basic Settings](./media/active-directory-saas-citrix-sharefile-tutorial/IC773629.png "Basic Settings")
9. Klicken Sie im Citrix ShareFile-Verwaltungsportal auf **Save** .
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Configure single sign-on](./media/active-directory-saas-citrix-sharefile-tutorial/IC773630.png "Configure single sign-on")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Citrix ShareFile anmelden können, müssen sie in Citrix ShareFile bereitgestellt werden.  
Im Fall von Citrix ShareFile ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei Ihrem **Citrix ShareFile** -Mandanten an.
2. Klicken Sie auf **Benutzer verwalten \> Benutzerbereich verwalten \> + Mitarbeiter erstellen**.
   
   ![Create Employee](./media/active-directory-saas-citrix-sharefile-tutorial/IC781050.png "Create Employee")
3. Geben Sie **E-Mail-Adresse**, **Vorname** und **Nachname** eines gültigen Azure AD-Kontos ein, das Sie bereitstellen möchten.
   
   ![Basic Information](./media/active-directory-saas-citrix-sharefile-tutorial/IC799951.png "Basic Information")
4. Klicken Sie auf **Add User**.
   
   > [!NOTE]
   > Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Citrix ShareFile-Benutzerkonten oder mithilfe der von Citrix ShareFile bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-citrix-sharefile-perform-the-following-steps"></a>So weisen Sie Citrix ShareFile Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Citrix ShareFile** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-citrix-sharefile-tutorial/IC773631.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-citrix-sharefile-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


