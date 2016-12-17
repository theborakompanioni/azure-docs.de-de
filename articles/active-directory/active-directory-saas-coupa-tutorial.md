---
title: 'Tutorial: Azure Active Directory-Integration mit Coupa | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Coupa mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 47f27746-9057-4b9c-991e-3abf77710f73
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 08331c68378caf7cf3b2bb9bcc14a824a58a249f


---
# <a name="tutorial-azure-active-directory-integration-with-coupa"></a>Tutorial: Azure Active Directory-Integration mit Coupa
In diesem Tutorial wird die Integration von Azure und Coupa erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Coupa-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Lernprogramms können sich die Coupa zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens bei der Anwendung mithilfe der [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)anmelden.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Coupa
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-coupa-tutorial/IC791897.png "Scenario")

## <a name="enabling-the-application-integration-for-coupa"></a>Aktivieren der Anwendungsintegration für Coupa
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Coupa aktivieren.

### <a name="to-enable-the-application-integration-for-coupa-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Coupa
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-coupa-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-coupa-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-coupa-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-coupa-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Coupa** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-coupa-tutorial/IC791898.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Coupa** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![coupa](./media/active-directory-saas-coupa-tutorial/IC791899.png "Coupa")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Coupa zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Coupa müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Melden Sie sich bei der Coupa-Unternehmenswebsite als Administrator an.
2. Navigieren Sie zu **Einrichtung \> Sicherheitskontrollen**.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")
3. Klicken Sie auf **SP-Metadaten herunterladen und importieren**, um die Coupa-Metadatendatei auf Ihren Computer herunterzuladen.
   
   ![Coupa-SP-Metadaten](./media/active-directory-saas-coupa-tutorial/IC791901.png "Coupa SP metadata")
4. Melden Sie sich in einem anderen Browserfenster beim klassischen Azure-Portal an.
5. Klicken Sie auf der Anwendungsintegrationsseite für **Coupa** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-coupa-tutorial/IC791902.png "Configure Single Sign-On")
6. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Coupa anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-coupa-tutorial/IC791903.png "Configure Single Sign-On")
7. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
   ![App-URL konfigurieren](./media/active-directory-saas-coupa-tutorial/IC791904.png "Configure App URL")
   
   1. Geben Sie im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer Coupa-Anwendung verwendete URL ein (z.B. *http://Unternehmen.Coupa.com*).
   2. Öffnen Sie die heruntergeladene Coupa-Metadatendatei, und kopieren Sie den Wert von **AssertionConsumerService index/URL**.
   3. Fügen Sie im Textfeld für die **Coupa-Antwort-URL** den Wert von **AssertionConsumerService index/URL** ein.
   4. Klicken Sie auf **Weiter**.
8. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Coupa** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-coupa-tutorial/IC791905.png "Configure Single Sign-On")
9. Navigieren Sie auf der Coupa-Unternehmenswebsite zu **Setup \> Sicherheitskontrollen**.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-coupa-tutorial/IC791900.png "Security Controls")
10. Führen Sie im Abschnitt **Mit Coupa-Anmeldeinformationen anmelden** die folgenden Schritte aus:
    
    ![Mit Coupa-Anmeldeinformationen anmelden](./media/active-directory-saas-coupa-tutorial/IC791906.png "Log in using Coupa credentials")
    
    1. Wählen Sie **Mit SAML anmelden**aus.
    2. Klicken Sie auf **Durchsuchen** , um die heruntergeladene Azure Active Directory-Metadatendatei hochzuladen.
    3. Klicken Sie auf **Speichern**.
11. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-coupa-tutorial/IC791907.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Coupa anmelden können, müssen sie in Coupa bereitgestellt werden.  
Im Fall von Coupa ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **Coupa** -Unternehmenswebsite als Administrator an.
2. Klicken Sie oben im Menü auf **Setup** und dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-coupa-tutorial/IC791908.png "Users")
3. Klicken Sie auf **Erstellen**.
   
   ![Benutzer erstellen](./media/active-directory-saas-coupa-tutorial/IC791909.png "Create Users")
4. Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:
   
   ![Benutzerdaten](./media/active-directory-saas-coupa-tutorial/IC791910.png "User Details")
   
   1. Geben Sie die Attribute **Benutzername**, **Vorname**, **Nachname**, **ID für einmaliges Anmelden** und **E-Mail** eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf **Erstellen**.
   
   > [!NOTE]
   > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 
   > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Coupa-Benutzerkonten oder mithilfe der von Coupa bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-coupa-perform-the-following-steps"></a>So weisen Sie Coupa Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Coupa** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-coupa-tutorial/IC791911.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-coupa-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


