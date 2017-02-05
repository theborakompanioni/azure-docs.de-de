---
title: 'Tutorial: Azure Active Directory-Integration mit Pagerduty | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Pagerduty mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2b473d7edb531f4fdab9c2ea8bde969e37558072


---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Tutorial: Azure Active Directory-Integration mit Pagerduty
In diesem Tutorial wird die Integration von Azure und Pagerduty erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Pagerduty-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Pagerduty zugewiesen haben, mittels einmaliger Anmeldung auf der Pagerduty-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Pagerduty
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Scenario")

## <a name="enabling-the-application-integration-for-pagerduty"></a>Aktivieren der Anwendungsintegration für Pagerduty
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Pagerduty aktivieren.

### <a name="to-enable-the-application-integration-for-pagerduty-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Pagerduty
1. Klicken Sie im linken Navigationsbereich des Azure-Verwaltungsportals auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Pagerduty** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **Pagerduty** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![PagerDuty](./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Pagerduty zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Pagerduty** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Pagerduty anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Pagerduty-Anmelde-URL** die URL im Format „*https://\<tenant-name\>.Pagerduty.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778533.png "Configure app url")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Pagerduty** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Configure single sign-on")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Pagerduty-Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf **Kontoeinstellungen**.
   
   ![Kontoeinstellungen](./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Account Settings")
7. Klicken Sie auf **Einmaliges Anmelden**.
   
   ![Einmaliges Anmelden](./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Single sign-on")
8. Führen Sie auf der Seite „Einmaliges Anmelden aktivieren“ die folgenden Schritte aus:
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Enable single sign-on")
   
   1. Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   2. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Pagerduty** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Login URL** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Pagerduty** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Logout URL** ein.
   5. Wählen Sie **Einmaliges Anmelden aktivieren**.
   6. Klicken Sie auf **Änderungen speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Pagerduty anmelden können, müssen sie in Pagerduty bereitgestellt werden.  
Im Fall von Pagerduty ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **Pagerduty** -Mandanten an.
2. Klicken Sie im oberen Menü auf **Benutzer**.
3. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Add Users")
4. Geben Sie im Dialogfeld **Team einladen** den **Vor- und Nachnamen** und die **E-Mail-Adresse** des Azure AD-Benutzers ein, den Sie bereitstellen möchten, und klicken Sie auf **Hinzufügen** und anschließend auf **Einladungen senden**.
   
   ![Team einladen](./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Invite your team")
   
   > [!NOTE]
   > Alle hinzugefügten Benutzer erhalten eine Einladung zum Erstellen eines PagerDuty-Kontos.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Pagerduty-Benutzerkonten oder mithilfe der von Pagerduty bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-pagerduty-perform-the-following-steps"></a>So weisen Sie Pagerduty Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Pagerduty** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


