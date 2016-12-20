---
title: 'Tutorial: Azure Active Directory-Integration mit Greenhouse | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Greenhouse mit Azure Active Directory verwenden können, um einmalige Anmeldung, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 55a9a01414d930b40f60119318f5c023172141e1


---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a>Tutorial: Azure Active Directory-Integration mit Greenhouse
In diesem Tutorial wird die Integration von Azure und Greenhouse erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Greenhouse-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Greenhouse zugewiesen haben, mittels einmaligen Anmeldens auf der Greenhouse-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Greenhouse
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-greenhouse-tutorial/IC790783.png "Scenario")

## <a name="enabling-the-application-integration-for-greenhouse"></a>Aktivieren der Anwendungsintegration für Greenhouse
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Greenhouse aktivieren.

### <a name="to-enable-the-application-integration-for-greenhouse-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Greenhouse
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-greenhouse-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-greenhouse-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-greenhouse-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-greenhouse-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **greenhouse** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-greenhouse-tutorial/IC790784.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Greenhouse** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![greenhouse](./media/active-directory-saas-greenhouse-tutorial/IC790785.png "Greenhouse")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Greenhouse zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Greenhouse** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-greenhouse-tutorial/IC790786.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Greenhouse anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-greenhouse-tutorial/IC790787.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Anmelde-URL** Ihre URL im Format *https://unternehmen.greenhouse.io* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-greenhouse-tutorial/IC790788.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Greenhouse** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-greenhouse-tutorial/IC790789.png "Configure single sign-on")
5. Leiten Sie die Metadatendatei an das Supportteam von  Greenhouse weiter.
   
   > [!NOTE]
   > Das einmalige Anmelden muss vom Supportteam von Greenhouse aktiviert werden.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-greenhouse-tutorial/IC790790.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Greenhouse anmelden können, müssen sie in Greenhouse bereitgestellt werden.  
Im Fall von Greenhouse ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei der **Greenhouse** -Unternehmenswebsite als Administrator an.
2. Klicken Sie oben im Menü auf **Konfigurieren** und dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-greenhouse-tutorial/IC790791.png "Users")
3. Klicken Sie auf **Neue Benutzer**.
   
   ![Neuer Benutzer](./media/active-directory-saas-greenhouse-tutorial/IC790792.png "New User")
4. Führen Sie im Abschnitt **Neuen Benutzer hinzufügen** die folgenden Schritte aus:
   
   ![Neuen Benutzer hinzufügen](./media/active-directory-saas-greenhouse-tutorial/IC790793.png "Add New User")
   
   1. Geben Sie im Textfeld **E-Mail-Adressen des Benutzers eingeben** die E-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.
      
      > [!NOTE]
      > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
      > 
      > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Greenhouse-Benutzerkonten oder mithilfe der von Greenhouse bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-greenhouse-perform-the-following-steps"></a>So weisen Sie Greenhouse Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Greenhouse** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-greenhouse-tutorial/IC790794.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-greenhouse-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


