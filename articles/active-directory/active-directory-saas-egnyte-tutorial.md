---
title: 'Tutorial: Azure Active Directory-Integration mit Egnyte | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Egnyte mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 682fa3f4ce1387f20b065af88b54c3cde3f2b242


---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Tutorial: Azure Active Directory-Integration mit Egnyte
In diesem Tutorial wird die Integration von Azure und Egnyte erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Egnyte-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Egnyte zugewiesen haben, mittels einmaligen Anmeldens auf der Egnyte-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Egnyte
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-egnyte-tutorial/IC787812.png "Scenario")

## <a name="enabling-the-application-integration-for-egnyte"></a>Aktivieren der Anwendungsintegration für Egnyte
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Egnyte aktivieren.

### <a name="to-enable-the-application-integration-for-egnyte-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Egnyte
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-egnyte-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-egnyte-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-egnyte-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-egnyte-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **egnyte** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-egnyte-tutorial/IC787813.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Egnyte** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![egnyte](./media/active-directory-saas-egnyte-tutorial/IC787814.png "Egnyte")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Egnyte zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Egnyte** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-egnyte-tutorial/IC787815.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Egnyte anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-egnyte-tutorial/IC787816.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Egnyte-Anmelde-URL** Ihre URL im Format *https://unternehmen.egnyte.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-egnyte-tutorial/IC787817.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Egnyte** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-egnyte-tutorial/IC787818.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Egnyte-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf **Einstellungen**.
   
   ![Einstellungen](./media/active-directory-saas-egnyte-tutorial/IC787819.png "Settings")
7. Klicken Sie im Menü auf **Einstellungen**.
   
   ![Einstellungen](./media/active-directory-saas-egnyte-tutorial/IC787820.png "Settings")
8. Klicken Sie auf die Registerkarte **Konfiguration** und anschließend auf **Sicherheit**.
   
   ![Sicherheit](./media/active-directory-saas-egnyte-tutorial/IC787821.png "Security")
9. Führen Sie im Abschnitt **SSO-Authentifizierung** die folgenden Schritte aus:
   
   ![SSO-Authentifizierung](./media/active-directory-saas-egnyte-tutorial/IC787822.png "Single Sign On Authentication")
   
   1. Wählen Sie für **SSO-Authentifizierung** die Option **SAML 2.0** aus.
   2. Wählen Sie für **Identitätsanbieter** die Option **AzureAD** aus.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Egnyte** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Egnyte** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld für die **Entitäts-ID des Identitätsanbieters** ein.
   5. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   6. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.
   7. Wählen Sie für **Standardbenutzerzuordnung** die Option **E-Mail-Adresse** aus.
   8. Wählen Sie für **Domänenspezifischen Ausstellerwert verwenden** die Einstellung **Deaktiviert** aus.
   9. Klicken Sie auf **Speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-egnyte-tutorial/IC787823.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Egnyte anmelden können, müssen sie in Egnyte bereitgestellt werden.  
Im Fall von Egnyte ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei der **Egnyte** -Unternehmenswebsite als Administrator an.
2. Navigieren Sie zu **Einstellungen \> Benutzer und Gruppen**.
3. Klicken Sie auf **Neuen Benutzer hinzufügen**, und wählen Sie dann den Benutzertyp aus, den Sie hinzufügen möchten.
   
   ![Benutzer](./media/active-directory-saas-egnyte-tutorial/IC787824.png "Users")
4. Führen Sie im Abschnitt **Neuer Standardbenutzer** die folgenden Schritte aus:
   
   ![Neuer Standardbenutzer](./media/active-directory-saas-egnyte-tutorial/IC787825.png "New Standard User")
   
   1. Geben Sie die **E-Mail-Adresse**, den **Benutzernamen** und weitere Details eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.
   
   > [!NOTE]
   > Der Besitzer des Azure Active Directory-Kontos erhält eine Benachrichtigungs-E-Mail.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Egnyte-Benutzerkonten oder mithilfe der von Egnyte bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-egnyte-perform-the-following-steps"></a>So weisen Sie Egnyte Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Egnyte** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-egnyte-tutorial/IC787826.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-egnyte-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


