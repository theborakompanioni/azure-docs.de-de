---
title: 'Tutorial: Azure Active Directory-Integration mit EmpCenter | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie EmpCenter mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: a00ecf6e-917a-4284-b998-41506931585e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b24490585019c049af4e1808bf980e36c5143e71


---
# <a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Tutorial: Azure Active Directory-Integration mit EmpCenter
In diesem Tutorial wird die Integration von Azure und EmpCenter erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein EmpCenter-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie EmpCenter zugewiesen haben, mittels einmaligen Anmeldens auf der EmpCenter-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für EmpCenter
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-empcenter-tutorial/IC802916.png "Scenario")

## <a name="enabling-the-application-integration-for-empcenter"></a>Aktivieren der Anwendungsintegration für EmpCenter
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für EmpCenter aktivieren.

### <a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für EmpCenter
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-empcenter-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-empcenter-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-empcenter-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **EmpCenter** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-empcenter-tutorial/IC802917.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **EmpCenter** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![EmpCentral](./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
   

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei EmpCenter zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **EmpCenter** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-empcenter-tutorial/IC802919.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei EmpCenter anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-empcenter-tutorial/IC802920.png "Configure Single Sign-On")
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
   ![App-Einstellungen konfigurieren](./media/active-directory-saas-empcenter-tutorial/IC802921.png "Configure App Settings")
   
   1. Geben Sie im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer EmpCenter-Anwendung verwendete URL ein (z.B. *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
   2. Klicken Sie auf **Weiter**
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für EmpCenter** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-empcenter-tutorial/IC802922.png "Configure Single Sign-On")
5. Senden Sie die heruntergeladene Metadatendatei an das Supportteam von EmpCenter.
   
   > [!NOTE]
   > Die eigentliche SSO-Konfiguration muss vom Supportteam von EmpCenter durchgeführt werden.
   > Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-empcenter-tutorial/IC802923.png "Configure Single Sign-On")
   
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei EmpCenter anmelden können, müssen sie in EmpCenter bereitgestellt werden.  
Im Fall von EmpCenter müssen die Benutzerkonten vom Supportteam von EmpCenter erstellt werden.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von EmpCenter-Benutzerkonten oder mithilfe der von EmpCenter bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>So weisen Sie EmpCenter Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **EmpCenter** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-empcenter-tutorial/IC802924.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-empcenter-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


