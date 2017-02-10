---
title: 'Tutorial: Azure Active Directory-Integration mit Boomi | Microsoft Docs'
description: "Erfahren Sie, wie Sie Boomi mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a315916e-8bfd-4390-bad2-ec9029314ab6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 51ea1ededc7b850e3dff24c9f735789d34391434


---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Lernprogramm: Azure Active Directory-Integration mit Boomi
In diesem Lernprogramm wird die Integration von Azure und Boomi erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Boomi-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Boomi zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Boomi-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Boomi
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-boomi-tutorial/IC791134.png "Scenario")

## <a name="enabling-the-application-integration-for-boomi"></a>Aktivieren der Anwendungsintegration für Boomi
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Boomi aktivieren.

### <a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Boomi
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-boomi-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-boomi-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-boomi-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Boomi** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-boomi-tutorial/IC790822.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Boomi** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Boomi](./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Boomi zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Boomi** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/IC790824.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Boomi anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/IC790825.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Boomi-Antwort-URL** Ihre **Boomi AtomSphere-Anmelde-URL ein** (z.B. „*https://platform.boomi.com/sso/AccountName/saml*”), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-boomi-tutorial/IC790826.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um Boomi** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/IC790827.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Boomi-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste im oberen Bereich, auf den Namen Ihres Unternehmens und dann auf **Setup**.
   
   ![Setup](./media/active-directory-saas-boomi-tutorial/IC790828.png "Setup")
7. Klicken Sie auf **SSO Options**.
   
   ![SSO Options](./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO Options")
8. Führen Sie im Abschnitt **Singel Sign-On Options** die folgenden Schritte aus:
   
   ![Singel Sign-On Options](./media/active-directory-saas-boomi-tutorial/IC790830.png "Single Sign-On Options")
   
   1. Aktivieren Sie **Enable SAML Single Sign-On**.
   2. Klicken Sie auf **Import**, um das heruntergeladene Zertifikat hochzuladen.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Boomi** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.
   4. Wählen Sie als **Federation Id Location** die Option **Federation Id is in NameID element of the Subject** aus.
   5. Klicken Sie auf **Speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/IC775560.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Boomi anmelden können, müssen sie in Boomi bereitgestellt werden.  
Im Fall von Boomi ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **Boomi** -Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Benutzerverwaltung \> Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-boomi-tutorial/IC790831.png "Users")
3. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-boomi-tutorial/IC790832.png "Add User")
4. Führen Sie auf der Dialogfeldseite **Add User Roles** die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-boomi-tutorial/IC790833.png "Add User")
   
   1. Geben Sie **Vorname**, **Nachname** und **E-Mail-Adresse** eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf OK.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Boomi-Benutzerkonten oder mithilfe der von Boomi bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-boomi-perform-the-following-steps"></a>So weisen Sie Boomi Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Boomi** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-boomi-tutorial/IC790834.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-boomi-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


