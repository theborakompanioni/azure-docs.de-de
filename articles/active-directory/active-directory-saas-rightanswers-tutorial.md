---
title: 'Tutorial: Azure Active Directory-Integration mit RightAnswers | Microsoft Docs'
description: Hier erfahren Sie, wie Sie RightAnswers mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes

---
# <a name="tutorial:-azure-active-directory-integration-with-rightanswers"></a>Tutorial: Azure Active Directory-Integration mit RightAnswers
In diesem Tutorial wird die Integration von Azure und RightAnswers erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein RightAnswers-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die RightAnswers zugewiesenen Azure AD-Benutzer wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben mittels einmaliger Anmeldung bei der Anwendung anmelden.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für RightAnswers
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Scenario")

## <a name="enabling-the-application-integration-for-rightanswers"></a>Aktivieren der Anwendungsintegration für RightAnswers
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für RightAnswers aktivieren.

### <a name="to-enable-the-application-integration-for-rightanswers,-perform-the-following-steps:"></a>So aktivieren Sie die Anwendungsintegration für RightAnswers:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** den Suchbegriff **RightAnswers** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **RightAnswers** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei RightAnswers zu authentifizieren.

### <a name="to-configure-single-sign-on,-perform-the-following-steps:"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **RightAnswers** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer RightAnswers anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Textfeld für die **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer RightAnswers-Anwendung verwendete URL (z.B. *https://fortify.rightanswers.com/portal/ss/*) ein, und klicken Sie dann auf **Weiter**.
   
   ![Konfigurieren von App-Einstellungen](./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Configure App Settings")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für RightAnswers** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Configure Single Sign-On")
5. Senden Sie die heruntergeladene Metadatendatei an das Supportteam von RightAnswers.
   
   > [!NOTE]
   > Die eigentliche SSO-Konfiguration muss vom Supportteam von RightAnswers durchgeführt werden.
   > Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei RightAnswers anmelden können, müssen sie in RightAnswers bereitgestellt werden.  
Im Fall von RightAnswers ist die Bereitstellung eine automatisierte Aufgabe.  
Für Sie steht kein Aktionselement zur Verfügung.

Benutzer werden beim Versuch des einmaligen Anmeldens bei Bedarf automatisch erstellt.

> [!NOTE]
> Sie können RightAnswers-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von AAD-Benutzerkonten oder mithilfe der von RightAnswers bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-rightanswers,-perform-the-following-steps:"></a>So weisen Sie RightAnswers Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **RightAnswers** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!--HONumber=Oct16_HO2-->


