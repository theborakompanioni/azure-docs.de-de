---
title: 'Tutorial: Azure Active Directory-Integration mit SCC LifeCycle | Microsoft Docs'
description: Hier erfahren Sie, wie Sie SCC LifeCycle mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen.
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
# <a name="tutorial:-azure-active-directory-integration-with-scc-lifecycle"></a>Tutorial: Azure Active Directory-Integration mit SCC LifeCycle
In diesem Tutorial wird die Integration von Azure und  SCC LifeCycle erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SCC LifeCycle-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie SCC LifeCycle zugewiesen haben, mittels einmaliger Anmeldung auf der SCC LifeCycle-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SCC LifeCycle
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Scenario")

## <a name="enabling-the-application-integration-for-scc-lifecycle"></a>Aktivieren der Anwendungsintegration für SCC LifeCycle
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SCC LifeCycle aktivieren.

### <a name="to-enable-the-application-integration-for-scc-lifecycle,-perform-the-following-steps:"></a>So aktivieren Sie die Anwendungsintegration für  SCC LifeCycle:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie in das **Suchfeld** den Namen **SCC LifeCycle** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **SCC LifeCycle** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SCC LifeCycle zu authentifizieren.

### <a name="to-configure-single-sign-on,-perform-the-following-steps:"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SCC LifeCycle** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SCC LifeCycle anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer SCC LifeCycle-Anwendung verwendete URL im Format *https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SCC LifeCycle** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Configure Single Sign-On")
5. Leiten Sie die Metadatendatei an das Supportteam von  SCC LifeCycle weiter.
   
   > [!NOTE]
   > Das einmalige Anmelden muss vom Supportteam von SCC LifeCycle aktiviert werden.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei SCC LifeCycle anmelden können, müssen sie in SCC LifeCycle bereitgestellt werden.

Für das Konfigurieren der Benutzerbereitstellung in SCC LifeCycle steht kein Aktionselement zur Verfügung.  
Wenn ein zugewiesener Benutzer versucht, sich bei SCC LifeCycle anzumelden, wird ggf. automatisch ein SCC LifeCycle-Konto erstellt.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von SCC LifeCycle-Benutzerkonten oder mithilfe der von SCC LifeCycle bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-scc-lifecycle,-perform-the-following-steps:"></a>So weisen Sie SCC LifeCycle Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **SCC LifeCycle** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!--HONumber=Oct16_HO2-->


