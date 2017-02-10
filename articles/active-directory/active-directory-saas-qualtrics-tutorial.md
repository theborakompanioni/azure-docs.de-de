---
title: 'Tutorial: Azure Active Directory-Integration mit Qualtrics | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Qualtrics mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4df889ab-2685-4d15-a163-1ba26567eeda
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3fb6227aaa50816d49372e9b43fdf5faadf1bded


---
# <a name="tutorial-azure-active-directory-integration-with-qualtrics"></a>Tutorial: Azure Active Directory-Integration mit Qualtrics
In diesem Tutorial wird die Integration von Azure und Qualtrics erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Qualtrics-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Qualtrics zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Qualtrics-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Qualtrics
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-qualtrics-tutorial/IC789542.png "Scenario")

## <a name="enabling-the-application-integration-for-qualtrics"></a>Aktivieren der Anwendungsintegration für Qualtrics
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Qualtrics aktivieren.

### <a name="to-enable-the-application-integration-for-qualtrics-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Qualtrics
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-qualtrics-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-qualtrics-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-qualtrics-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-qualtrics-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Qualtrics** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-qualtrics-tutorial/IC789543.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Qualtrics** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Qualtrics](./media/active-directory-saas-qualtrics-tutorial/IC789544.png "Qualtrics")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Qualtrics zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Qualtrics** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qualtrics-tutorial/IC789545.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Qualtrics anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qualtrics-tutorial/IC789546.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Qualtrics-Anmelde-URL** Ihre URL ein (z.B. *https://ssotest2ut1.qualtrics.com*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-qualtrics-tutorial/IC789547.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Qualtrics** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qualtrics-tutorial/IC789548.png "Configure Single Sign-On")
5. Senden Sie die Metadatendatei an das Supportteam von Qualtrics.
   
   > [!NOTE]
   > Das einmalige Anmelden muss vom Supportteam von Qualtrics konfiguriert werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist.
   > 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qualtrics-tutorial/IC789549.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Für das Konfigurieren der Benutzerbereitstellung in Qualtrics steht kein Aktionselement zur Verfügung.  
Wenn ein zugewiesener Benutzer versucht, sich über den Zugriffsbereich bei Qualtrics anzumelden, überprüft Qualtrics, ob der Benutzer vorhanden ist.  
Ist noch kein Benutzerkonto verfügbar, wird es von Qualtrics automatisch erstellt.

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-qualtrics-perform-the-following-steps"></a>So weisen Sie Qualtrics Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Qualtrics** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-qualtrics-tutorial/IC789550.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-qualtrics-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


