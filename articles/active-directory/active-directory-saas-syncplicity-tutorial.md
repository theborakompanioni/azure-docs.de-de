---
title: 'Tutorial: Azure Active Directory-Integration mit Syncplicity | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Syncplicity mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 896a3211-f368-46d7-95b8-e4768c23be08
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d8464b1ca5051b2bdac61f0e637ed1b2ff3df3ab


---
# <a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Lernprogramm: Azure Active Directory-Integration mit Syncplicity
In diesem Tutorial wird erläutert, wie einmaliges Anmelden zwischen Azure Active Directory (Azure AD) und Syncplicity eingerichtet wird.

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Syncplicity-Mandant

Nach Abschluss dieses Tutorials können sich die Syncplicity zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Syncplicity-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen.

1. Aktivieren der Anwendungsintegration für Syncplicity
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Scenario")

## <a name="enabling-the-application-integration-for-syncplicity"></a>Aktivieren der Anwendungsintegration für Syncplicity
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Syncplicity aktivieren.

### <a name="to-enable-the-application-integration-for-syncplicity-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Syncplicity:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie in das **Suchfeld** **Syncplicity** ein.
   
   ![Syncplicity-Anwendungskatalog](./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Syncplicity application gallery")
7. Wählen Sie im Ergebnisbereich **Syncplicity** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Konto in Azure Active Directory bei Syncplicity zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Syncplicity** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Syncplicity anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** in das Textfeld **Syncplicity-Anmelde-URL** die URL ein, mit der sich die Benutzer bei der Syncplicity-Anwendung anmelden. Klicken Sie dann auf **Weiter**. 
   
   Bei der App-URL handelt es sich um die URL Ihres Syncplicity-Mandanten (z.B. *http://company.Syncplicity.com*):
   
   ![App-URL konfigurieren](./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Configure app URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Syncplicity** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Configure single sign-on")
5. Melden Sie sich bei Ihrem **Syncplicity** -Mandanten an.
6. Klicken Sie im Menü oben auf **Admin**, wählen Sie **Einstellungen**, und klicken Sie dann auf **Benutzerdefinierte Domäne und einmaliges Anmelden**.
   
   ![Syncplicity](./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")
7. Führen Sie auf der Dialogfeldseite **Einmaliges Anmelden (SSO)** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden \((SSO)\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \\\(SSO\\\)")
   
   1. Geben Sie in das Textfeld **Benutzerdefinierte Domäne** den Namen  Ihrer Domäne ein.
   2. Wählen Sie **Aktiviert** als **Status der einmaligen Anmeldung**.
   3. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für Syncplicity** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **Entitäts-ID** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Syncplicity** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **URL der Anmeldeseite** ein.
   5. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für Syncplicity** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **URL der Abmeldeseite** ein.
   6. Klicken Sie unter **Zertifikat des Identitätsanbieters** auf **Datei auswählen**, und laden Sie dann das Zertifikat hoch, das Sie aus dem klassischen Azure-Portal heruntergeladen haben.
   7. Klicken Sie auf **Änderungen speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Bestätigung](./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Confirmation")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich AAD-Benutzer anmelden können, müssen sie in der Syncplicity-Anwendung bereitgestellt werden. In diesem Abschnitt wird beschrieben, wie in Syncplicity AAD-Benutzerkonten erstellt werden.

### <a name="to-provision-a-user-account-to-syncplicity-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten in Syncplicity die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **Syncplicity**-Mandanten an (z.B. *http://company.Syncplicity.com*).
2. Klicken Sie auf **Admin**, und wählen Sie **Benutzerkonten** aus.
3. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer verwalten](./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Manage Users")
4. Geben Sie die **E-Mail-Adresse** eines AAD-Kontos ein, das Sie bereitstellen möchten, und wählen Sie dann **Benutzer** als **Rolle** aus. Klicken Sie abschließend auf **Weiter**.
   
   ![Azure-Kontoinformationen](./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Account Information")
   
   > [!NOTE]
   > Der AAD-Kontoinhaber erhält eine E-Mail mit einem Link zur Bestätigung und Aktivierung des Kontos.
   > 
   > 
5. Wählen Sie eine Gruppe in Ihrem Unternehmen aus, bei der Ihr neuer Benutzer Mitglied werden soll. Klicken Sie dann auf **Weiter**.
   
   ![Gruppenmitgliedschaft](./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Group Membership")
   
   > [!NOTE]
   > Wenn keine Gruppen aufgelistet sind, klicken Sie einfach auf **Weiter**.
   > 
   > 
6. Wählen Sie die Ordner aus, die Sie auf dem Computer des Benutzers unter die Kontrolle von Syncplicity stellen möchten. Klicken Sie dann auf **Weiter**.
   
   ![Syncplicity-Ordner](./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity Folders")

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Syncplicity-Benutzerkonten oder mithilfe der von Syncplicity bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-syncplicity-perform-the-following-steps"></a>So weisen Sie Syncplicity Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Syncplicity** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


