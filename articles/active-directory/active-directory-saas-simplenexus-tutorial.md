---
title: 'Tutorial: Azure Active Directory-Integration mit SimpleNexus | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie SimpleNexus mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 89821a05-88e2-4579-b144-0123b2b9cb95
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 4226d82ac8f0b5e0d8806df2c93f1a1a73162316


---
# <a name="tutorial-azure-active-directory-integration-with-simplenexus"></a>Tutorial: Azure Active Directory-Integration mit SimpleNexus
In diesem Tutorial wird die Integration von Azure und SimpleNexus erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SimpleNexus-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die SimpleNexus zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer SimpleNexus-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SimpleNexus
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-simplenexus-tutorial/IC785893.png "Scenario")

## <a name="enabling-the-application-integration-for-simplenexus"></a>Aktivieren der Anwendungsintegration für SimpleNexus
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SimpleNexus aktivieren.

### <a name="to-enable-the-application-integration-for-simplenexus-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SimpleNexus:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-simplenexus-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-simplenexus-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-simplenexus-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-simplenexus-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie in das **Suchfeld** **simple nexus** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-simplenexus-tutorial/IC785894.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **SimpleNexus** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![simple nexus](./media/active-directory-saas-simplenexus-tutorial/IC809578.png "Simple Nexus")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SimpleNexus zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SimpleNexus** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-simplenexus-tutorial/IC785896.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SimpleNexus anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-simplenexus-tutorial/IC785897.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **SimpleNexus-Anmelde-URL** die URL im Format „*https://simplenexus.com/CompanyName\_login*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-simplenexus-tutorial/IC786904.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SimpleNexus** auf **Metadaten herunterladen**, und senden Sie die Metadatendatei an das SimpleNexus-Supportteam.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-simplenexus-tutorial/IC785899.png "Configure Single Sign-On")
   
   > [!NOTE]
   > Das einmalige Anmelden muss vom Supportteam von SimpleNexus aktiviert werden.
   > 
   > 
5. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-simplenexus-tutorial/IC785900.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei SimpleNexus anmelden können, müssen sie in SimpleNexus bereitgestellt werden.  
Im Fall von SimpleNexus ist die Bereitstellung eine manuelle Aufgabe, die vom Tenant-Administrator ausgeführt wird.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von SimpleNexus-Benutzerkonten oder mithilfe der von SimpleNexus bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-simplenexus-perform-the-following-steps"></a>So weisen Sie SimpleNexus Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **SimpleNexus** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-simplenexus-tutorial/IC785901.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-simplenexus-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


