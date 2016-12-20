---
title: 'Tutorial: Azure Active Directory-Integration mit Innotas | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Innotas mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: eb9e9c2c-4b09-4177-bbab-423fd657448e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5d686f1ec3ec7a0ec5062e6da231ace8b47dbd83


---
# <a name="tutorial-azure-active-directory-integration-with-innotas"></a>Tutorial: Azure Active Directory-Integration mit Innotas
In diesem Tutorial wird die Integration von Azure und Innotas erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Innotas-Mandanten

Nach Abschluss dieses Tutorials können sich die Innotas zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Innotas-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Innotas
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-innotas-tutorial/IC777331.png "Scenario")

## <a name="enabling-the-application-integration-for-innotas"></a>Aktivieren der Anwendungsintegration für Innotas
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Innotas aktivieren.

### <a name="to-enable-the-application-integration-for-innotas-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Innotas
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-innotas-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-innotas-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-innotas-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-innotas-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Innotas** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-innotas-tutorial/IC777332.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **Innotas** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Innotas](./media/active-directory-saas-innotas-tutorial/IC777333.png "Innotas")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Innotas zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Innotas** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-innotas-tutorial/IC777334.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Innotas anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-innotas-tutorial/IC777335.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Innotas-Anmelde-URL** die URL im Format „*https://\<Mandantenname\>.Innotas.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-innotas-tutorial/IC777336.png "Configure app URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Innotas** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal unter **c:\\InnotasMetaData.xml**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-innotas-tutorial/IC777337.png "Configure single sign-on")
5. Leiten Sie die Metadatendatei an das Supportteam von Innotas weiter. Das Supportteam muss ein einmaliges Anmelden für Sie konfigurieren.
6. Bestätigen Sie die Konfiguration des einmaligen Anmeldens, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-innotas-tutorial/IC777338.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Für das Konfigurieren der Benutzerbereitstellung in Innotas steht kein Aktionselement zur Verfügung.  
Wenn ein zugewiesener Benutzer versucht, sich über den Zugriffsbereich bei Innotas anzumelden, überprüft Innotas, ob der Benutzer vorhanden ist.  
Ist noch kein Benutzerkonto verfügbar, wird es von Innotas automatisch erstellt.

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-innotas-perform-the-following-steps"></a>So weisen Sie Innotas Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Innotas** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-innotas-tutorial/IC777339.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-innotas-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


