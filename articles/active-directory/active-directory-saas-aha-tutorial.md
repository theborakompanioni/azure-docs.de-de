---
title: 'Lernprogramm: Azure Active Directory-Integration mit Aha! | Microsoft Docs'
description: "Erfahren Sie, wie Sie Aha! mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ad955d3d-896a-41bb-800d-68e8cb5ff48d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f95aecd18d5231413678669d01d16387737e9db2


---
# <a name="tutorial-azure-active-directory-integration-with-aha"></a>Lernprogramm: Azure Active Directory-Integration mit Aha!
In diesem Lernprogramm wird die Integration von Azure und Aha! erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Aha!-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Lernprogramms können sich die Aha! zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Aha!-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)nutzen.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Aha!
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-aha-tutorial/IC798944.png "Scenario")

## <a name="enabling-the-application-integration-for-aha"></a>Aktivieren der Anwendungsintegration für Aha!
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Aha! aktivieren.

### <a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Aha!
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-aha-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-aha-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-aha-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Aha!** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-aha-tutorial/IC798945.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Aha!** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Aha!](./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Aha! zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Aha!** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/IC798946.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Aha! anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/IC798947.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** in das Textfeld **Aha!- Anmelde-URL** den URL ein, den Ihre Benutzer zur Anmeldung bei der Aha!- Anwendung verwenden (z.B. *https://unternehmen.aha.io/session/new*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-aha-tutorial/IC798948.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Aha!** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/IC798949.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Aha!- Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf das Symbol **Einstellungen**.
   
   ![Settings](./media/active-directory-saas-aha-tutorial/IC798950.png "Settings")
7. Klicken Sie auf **Account**.
   
   ![Profil](./media/active-directory-saas-aha-tutorial/IC798951.png "Profile")
8. Klicken Sie auf **Security and single sign-on**.
   
   ![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798952.png "Security and single sign-on")
9. Wählen Sie im Abschnitt **Einmaliges Anmelden** als **Identitätsanbieter** die Option **SAML2.0** aus.
   
   ![Security and single sign-on](./media/active-directory-saas-aha-tutorial/IC798953.png "Security and single sign-on")
10. Führen Sie auf der Konfigurationsseite **Single Sign-On** die folgenden Schritte aus:
    
    ![Single Sign-On](./media/active-directory-saas-aha-tutorial/IC798954.png "Single Sign-On")
    
    1. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein.
    2. Wählen Sie für **Konfigurieren mit** die Option **Metadatendatei** aus.
    3. Klicken Sie auf **Browse**, um die heruntergeladene Metadatendatei hochzuladen.
    4. Klicken Sie auf **Aktualisieren**.
11. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-aha-tutorial/IC798955.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Aha! anmelden können, müssen sie in Aha! bereitgestellt werden.  
Im Fall von Aha! ist die Bereitstellung eine automatisierte Aufgabe.  
Für Sie steht kein Aktionselement zur Verfügung.

Benutzer werden beim Versuch des einmaligen Anmeldens bei Bedarf automatisch erstellt.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Aha!-Benutzerkonten oder mithilfe der von Aha! bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-aha-perform-the-following-steps"></a>So weisen Sie Aha! Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Aha!** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-aha-tutorial/IC798956.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-aha-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO4-->


