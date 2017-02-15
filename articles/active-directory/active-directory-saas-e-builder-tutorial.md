---
title: 'Tutorial: Azure Active Directory-Integration mit e-Builder | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie e-Builder mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: d5068007-5a54-40ac-9cb8-2ceca89fd8ab
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c08989aa8dc966fc731be7a53c1eb0f1d57b5053


---
# <a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Tutorial: Azure Active Directory-Integration mit e-Builder
In diesem Tutorial wird die Integration von Azure und e-Builder erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen e-Builder-Mandanten

Nach Abschluss dieses Tutorials können sich die e-Builder zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer e-Builder-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für e-Builder
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-e-builder-tutorial/IC777378.png "Scenario")

## <a name="enabling-the-application-integration-for-e-builder"></a>Aktivieren der Anwendungsintegration für e-Builder
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für e-Builder aktivieren.

### <a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für e-Builder
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-e-builder-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-e-builder-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-e-builder-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **e-Builder** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-e-builder-tutorial/IC777379.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **e-Builder** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![e-Builder](./media/active-directory-saas-e-builder-tutorial/IC777380.png "e-Builder")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei e-Builder zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **e-Builder** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-e-builder-tutorial/IC777381.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei e-Builder anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-e-builder-tutorial/IC777382.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **e-Builder-Anmelde-URL** die URL im Format „*https://\<Mandantenname\>.e-Builder.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-e-builder-tutorial/IC777383.png "Configure app URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für e-Builder** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal unter **c:\\e-BuilderMetaData.xml**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-e-builder-tutorial/IC777384.png "Configure single sign-on")
5. Leiten Sie die Metadatendatei an das Supportteam von e-Builder weiter. Das Supportteam muss ein einmaliges Anmelden für Sie konfigurieren.
6. Bestätigen Sie die Konfiguration des einmaligen Anmeldens, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-e-builder-tutorial/IC777385.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Für das Konfigurieren der Benutzerbereitstellung in e-Builder steht kein Aktionselement zur Verfügung.  
Wenn ein zugewiesener Benutzer versucht, sich über den Zugriffsbereich bei e-Builder anzumelden, überprüft e-Builder, ob der Benutzer vorhanden ist.  
Ist noch kein Benutzerkonto verfügbar, wird es von e-Builder automatisch erstellt.

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>So weisen Sie e-Builder Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **e-Builder** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-e-builder-tutorial/IC777386.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-e-builder-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


