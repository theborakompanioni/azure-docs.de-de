---
title: 'Tutorial: Azure Active Directory-Integration mit Adaptive Suite | Microsoft Docs'
description: "Erfahren Sie, wie Sie Adaptive Suite mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 13af9d00-116a-41b8-8ca0-4870b31e224c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 652b0cef09625ed354c2c72178cc98a0482bb789


---
# <a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Lernprogramm: Azure Active Directory-Integration mit Adaptive Suite
In diesem Lernprogramm wird die Integration von Azure und Adaptive Suite erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Adaptive Suite-Mandanten

Nach Abschluss dieses Tutorials können sich die Adaptive Suite zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Adaptive Suite-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Sie können aber auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Adaptive Suite
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Scenario")

## <a name="enabling-the-application-integration-for-adaptive-suite"></a>Aktivieren der Anwendungsintegration für Adaptive Suite
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Adaptive Suite aktivieren.

### <a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Adaptive Suite
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Adaptive Suite** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Adaptive Suite** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Adaptive Suite](./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Adaptive Suite")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Adaptive Suite zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Adaptive Suite müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI)(in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Adaptive Suite** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Adaptive Suite anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Textfeld **Antwort-URL** Ihre URL im Format *https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE=* ein, und klicken Sie dann auf **Weiter**.
   
   > [!NOTE]
   > Sie finden diesen Wert auf der Seite **SAML SSO Settings** der Adaptive Suite.
   > 
   > 
   
   ![App-Einstellungen konfigurieren](./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Configure App Settings")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Adaptive Suite** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Adaptive Suite-Unternehmenswebsite als Administrator an.
6. Wechseln Sie zu **Admin**.
   
   ![Administrator](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")
7. Klicken Sie im Abschnitt **Benutzer und Rollen** auf **SAML-SSO-Einstellungen verwalten**.
   
   ![Manage SAML SSO Settings](./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "Manage SAML SSO Settings")
8. Führen Sie auf der Seite **SAML SSO Settings** die folgenden Schritte aus:
   
   ![SAML SSO Settings](./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO Settings")
   
   1. Geben Sie im Textfeld **Identity provider name** einen Namen für die Konfiguration ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adaptive Suite** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **Entitäts-ID des Identitätsanbieters** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adaptive Suite** den Wert **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **SSO-URL des Identitätsanbieters** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adaptive Suite** den Wert **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **Benutzerdefinierte Abmelde-URL** ein.
   5. Klicken Sie auf **Choose file**, um das heruntergeladene Zertifikat hochzuladen.
   6. Wählen Sie als **SAML-Benutzer-ID** die Option **Adaptive Insights-Benutzername des Benutzers** aus.
   7. Wählen Sie als **SAML-Benutzer-ID-Speicherort** die **Benutzer-ID in NameID von Subject** aus.
   8. Wählen Sie als **SAML-NameID-Format** die Option **E-Mail-Adresse** aus.
   9. Wählen Sie für **SAML aktivieren** die Option **SAML SSO und direkte Adaptive Insights-Anmeldung** aus.
   10. Klicken Sie auf **Speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Adaptive Suite anmelden können, müssen sie in Adaptive Suite bereitgestellt werden.  
Im Fall von Adaptive Suite ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **Adaptive Suite** -Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Admin**.
   
   ![Administrator](./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Admin")
3. Klicken Sie im Abschnitt **Benutzer und Rollen** auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Add User")
4. Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:
   
   ![Senden](./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Submit")
   
   1. Geben Sie in die Textfelder **Name**, **Anmeldename**, **E-Mail** und **Kennwort** die entsprechenden Informationen eines gültigen Azure Active Directory-Benutzerkontos ein, das Sie bereitstellen möchten.
   2. Wählen Sie eine **Role**aus.
   3. Klicken Sie auf **Senden**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Adaptive Suite-Benutzerkonten oder mithilfe der von Adaptive Suite bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>So weisen Sie Adaptive Suite Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Adaptive Suite** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


