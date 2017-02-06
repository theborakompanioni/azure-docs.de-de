---
title: 'Tutorial: Azure Active Directory-Integration mit LogicMonitor | Microsoft Docs'
description: "Erfahren Sie, wie Sie LogicMonitor mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6dbb19605f92ff0c0d58478a56e564bab71123e4


---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a>Tutorial: Azure Active Directory-Integration mit LogicMonitor
In diesem Lernprogramm wird die Integration von Azure und LogicMonitor erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen LogicMonitor-Mandanten

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für LogicMonitor
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-logicmonitor-tutorial/IC790045.png "Scenario")

## <a name="enabling-the-application-integration-for-logicmonitor"></a>Aktivieren der Anwendungsintegration für LogicMonitor
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für LogicMonitor aktivieren.

### <a name="to-enable-the-application-integration-for-logicmonitor-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für LogicMonitor
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-logicmonitor-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-logicmonitor-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-logicmonitor-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-logicmonitor-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **logicmonitor** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-logicmonitor-tutorial/IC790046.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **LogicMonitor** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![logicmonitor](./media/active-directory-saas-logicmonitor-tutorial/IC790047.png "LogicMonitor")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei LogicMonitor zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **LogicMonitor** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-logicmonitor-tutorial/IC790048.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei LogicMonitor anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-logicmonitor-tutorial/IC790049.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei LogicMonitor verwendete URL ein\( (z.B. *„http://company.logicmonitor.com*“\)), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-logicmonitor-tutorial/IC790050.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für LogicMonitor** auf **Metadaten herunterladen**, und speichern Sie die Daten auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-logicmonitor-tutorial/IC790051.png "Configure Single Sign-On")
5. Melden Sie sich bei der **LogicMonitor** -Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf **Einstellungen**.
   
   ![Einstellungen](./media/active-directory-saas-logicmonitor-tutorial/IC790052.png "Settings")
7. Klicken Sie auf der Navigationsleiste links auf **Einmaliges Anmelden**
   
   ![Einmaliges Anmelden](./media/active-directory-saas-logicmonitor-tutorial/IC790053.png "Single Sign-On")
8. Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden (SSO)** die folgenden Schritte aus:
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-logicmonitor-tutorial/IC790054.png "Single Sign-On Settings")
   
   1. Wählen Sie **Einmaliges Anmelden aktivieren**aus.
   2. Wählen Sie für **Standardmäßige Rollenzuweisung** die Option **Schreibgeschützt** aus.
   3. Öffnen Sie die heruntergeladene Metadatendatei im Editor, und fügen Sie den Inhalt der Datei ins Textfeld **Identitätsanbietermetadaten** ein.
   4. Klicken Sie auf **Änderungen speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-logicmonitor-tutorial/IC790055.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich AAD-Benutzer anmelden können, müssen sie in der LogicMonitor-Anwendung unter Verwendung ihrer Azure Active Directory-Benutzernamen bereitgestellt werden.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der LogicMonitor-Unternehmenswebsite als Administrator an.
2. Klicken Sie im oberen Menü auf **Einstellungen** und dann auf **Rollen und Benutzer**.
   
   ![Rollen und Benutzer](./media/active-directory-saas-logicmonitor-tutorial/IC790056.png "Roles and Users")
3. Klicken Sie auf **Hinzufügen**.
4. Führen Sie im Abschnitt **Konto hinzufügen** die folgenden Schritte aus:
   
   ![Konto hinzufügen](./media/active-directory-saas-logicmonitor-tutorial/IC790057.png "Add an account")
   
   1. Geben Sie für den Azure Active Directory-Benutzer, den Sie bereitstellen möchten, Werte für **Benutzername**, **E-Mail**, **Kennwort** und **Kennwort erneut eingeben** in die entsprechenden Textfelder ein.
   2. Wählen Sie **Rollen**, **Berechtigungen anzeigen** und **Status**.
   3. Klicken Sie auf **Senden**.

> [!NOTE]
> Sie können Azure Active Directory-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von LogicMonitor-Benutzerkonten oder mithilfe der von LogicMonitor bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-logicmonitor-perform-the-following-steps"></a>So weisen Sie LogicMonitor Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **LogicMonitor** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-logicmonitor-tutorial/IC790058.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-logicmonitor-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


