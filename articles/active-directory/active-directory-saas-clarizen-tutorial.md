---
title: 'Tutorial: Azure Active Directory-Integration mit Clarizen | Microsoft Docs'
description: "Erfahren Sie, wie Sie Clarizen mit Azure Active Directory verwenden können, um das einmalige Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9f8d2418fe86163a4022960f87803c82487c86fd


---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Lernprogramm: Azure Active Directory-Integration mit Clarizen
In diesem Lernprogramm wird die Integration von Azure und Clarizen erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Clarizen-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Clarizen zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Clarizen-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Clarizen
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-clarizen-tutorial/IC784679.png "Scenario")

## <a name="enabling-the-application-integration-for-clarizen"></a>Aktivieren der Anwendungsintegration für Clarizen
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Clarizen aktivieren.

### <a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Clarizen
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-clarizen-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-clarizen-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-clarizen-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Clarizen** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-clarizen-tutorial/IC784680.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Clarizen** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Clarizen](./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Clarizen zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Clarizen** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clarizen-tutorial/IC784682.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Clarizen anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clarizen-tutorial/IC784683.png "Configure Single Sign-On")
3. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um Clarizen** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clarizen-tutorial/IC784684.png "Configure Single Sign-On")
4. Melden Sie sich in einem anderen Webbrowserfenster bei der **Clarizen**-Unternehmenswebsite als Administrator an (z.B. *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).
5. Klicken Sie auf Ihren Benutzernamen, und klicken Sie dann auf **Settings**.
   
   ![Einstellungen](./media/active-directory-saas-clarizen-tutorial/IC784685.png "Settings")
6. Klicken Sie auf die Registerkarte **Globale Einstellungen**, und klicken Sie dann neben **Verbundauthentifizierung** auf **Bearbeiten**.
   
   ![Global Settings](./media/active-directory-saas-clarizen-tutorial/IC786906.png "Global Settings")
7. Führen Sie im Dialogfeld **Federated Authentication** die folgenden Schritte aus:
   
   ![Federated Authentication](./media/active-directory-saas-clarizen-tutorial/IC785892.png "Federated Authentication")
   
   1. Klicken Sie auf **Upload** , um das heruntergeladene Zertifikat hochzuladen.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Clarizen** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Abmelden konfigurieren für Clarizen** den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
   4. Aktivieren Sie **Use POST**.
   5. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clarizen-tutorial/IC784688.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Clarizen anmelden können, müssen sie in Clarizen bereitgestellt werden.  
Im Fall von Clarizen ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei der **Clarizen** -Unternehmenswebsite als Administrator an.
2. Klicken Sie auf **People**.
   
   ![People](./media/active-directory-saas-clarizen-tutorial/IC784689.png "People")
3. Klicken Sie auf **Invite User**.
   
   ![Invite Users](./media/active-directory-saas-clarizen-tutorial/IC784690.png "Invite Users")
4. Führen Sie auf der Dialogfeldseite "Invite People" die folgenden Schritte aus:
   
   ![Invite People](./media/active-directory-saas-clarizen-tutorial/IC784691.png "Invite People")
   
   1. Geben Sie im Textfeld **Email** die E-Mail-Adresse eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Invite**.
   
   > [!NOTE]
   > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>So weisen Sie Clarizen Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Clarizen** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-clarizen-tutorial/IC784692.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-clarizen-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


