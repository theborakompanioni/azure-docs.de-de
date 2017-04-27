---
title: 'Tutorial: Azure Active Directory-Integration mit SCC LifeCycle | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie SCC LifeCycle mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9748bf38-ffc3-4d51-a1ae-207ce57104fa
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9a30bcca720ff135d0180d73f46e78403e9bca43
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Tutorial: Azure Active Directory-Integration mit SCC LifeCycle
In diesem Tutorial wird die Integration von Azure und  SCC LifeCycle erläutert.  

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SCC LifeCycle-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie SCC LifeCycle zugewiesen haben, mittels einmaliger Anmeldung auf der SCC LifeCycle-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SCC LifeCycle
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Szenario")

## <a name="enable-the-application-integration-for-scc-lifecycle"></a>Aktivieren der Anwendungsintegration für SCC LifeCycle
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SCC LifeCycle aktivieren.

**Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für SCC LifeCycle zu aktivieren:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie in das **Suchfeld** den Namen **SCC LifeCycle** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **SCC LifeCycle** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![SCC LifeCycle](./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC LifeCycle")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SCC LifeCycle zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SCC LifeCycle** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SCC LifeCycle anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer SCC LifeCycle-Anwendung verwendete URL im Format *https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx* ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SCC LifeCycle** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Einmaliges Anmelden konfigurieren")
5. Leiten Sie die Metadatendatei an das Supportteam von  SCC LifeCycle weiter.
   
   >[!NOTE]
   >Das einmalige Anmelden muss vom Supportteam von SCC LifeCycle aktiviert werden.
   > 
   > 

6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei SCC LifeCycle anmelden können, müssen sie in SCC LifeCycle bereitgestellt werden. Für das Konfigurieren der Benutzerbereitstellung in SCC LifeCycle steht kein Aktionselement zur Verfügung.

Wenn ein zugewiesener Benutzer versucht, sich bei SCC LifeCycle anzumelden, wird ggf. automatisch ein SCC LifeCycle-Konto erstellt.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von SCC LifeCycle-Benutzerkonten oder mithilfe der von SCC LifeCycle bereitgestellten APIs erstellen.
> 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie die folgenden Schritte aus, um SCC LifeCycle Benutzer zuzuweisen:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **SCC LifeCycle** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


