---
title: 'Tutorial: Azure Active Directory-Integration mit Igloo Software | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Igloo Software mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/20/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c9e5dfa98e47c6a7a4e34f008fabd1f63b93937c


---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Tutorial: Azure Active Directory-Integration mit Igloo Software
In diesem Tutorial wird die Integration von Azure und Igloo Software erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein [Igloo Software](http://www.igloosoftware.com/) -Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Igloo Software zugewiesen haben, mittels einmaliger Anmeldung auf der Igloo Software-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Igloo Software
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Scenario")

## <a name="enabling-the-application-integration-for-igloo-software"></a>Aktivieren der Anwendungsintegration für Igloo Software
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Igloo Software aktivieren.

### <a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Igloo Software
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** **Igloo Software** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Igloo Software** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Igloo](./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Igloo Software zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei in Ihren Central Desktop-Mandanten hochladen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Igloo Software** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Igloo Software anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Igloo Software-Anmelde-URL** die URL im Format „*https://company.igloocommunities.com/?signin*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Igloo Software** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Igloo Software-Unternehmenswebsite als Administrator an.
6. Wechseln Sie zur **Systemsteuerung**.
   
   ![Systemsteuerung](./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Control Panel")
7. Klicken Sie auf der Registerkarte **Mitgliedschaft** auf **Anmeldeeinstellungen**.
   
   ![Anmeldeeinstellungen](./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Sign in Settings")
8. Klicken Sie im Abschnitt für die SAML-Konfiguration auf **SAML-Authentifizierung konfigurieren**.
   
   ![SAML-Konfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML Configuration")
9. Führen Sie im Abschnitt **Allgemeine Konfiguration** die folgenden Schritte aus:
   
   ![Allgemeine Konfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783970.png "General Configuration")
   
   1. Geben Sie im Textfeld **Verbindungsname** einen Namen für die Konfiguration ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Igloo Software** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP-Anmelde-URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Igloo Software** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **IdP-Abmelde-URL** ein.
   4. Wählen Sie für **Abmeldeantwort und HTTP-Anforderungstyp** die Option **POST** aus.
   5. Erstellen Sie aus dem heruntergeladenen Zertifikat eine Textdatei.
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   6. Entfernen Sie die erste Zeile und die letzte Zeile aus der Textdateiversion des Zertifikats, kopieren Sie den restlichen Text des Zertifikats, und fügen Sie ihn in das Textfeld **Öffentliches Zertifikat** ein.
10. Führen Sie unter **Antwort- und Authentifizierungskonfiguration**die folgenden Schritte aus:
    
    ![Antwort- und Authentifizierungskonfiguration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")
    
    1. Wählen Sie für **Identitätsanbieter** die Option **Microsoft ADFS** aus.
    2. Wählen Sie für **Bezeichnertyp** die Option **E-Mail-Adresse** aus.
    3. Geben Sie im Textfeld **E-Mail-Attribut** die Zeichenfolge **emailaddress** ein.
    4. Geben Sie **givenname** in das Textfeld **Vornamen-Attribut** ein.
    5. Geben Sie **surname** in das Textfeld **Nachnamen-Attribut** ein.
11. Führen Sie die folgenden Schritte aus, um die Konfiguration abzuschließen:
    
    ![Benutzererstellung beim Anmelden](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")
    
    1. Wählen Sie für **Benutzererstellung beim Anmelden** die Option **Bei der Anmeldung neuen Benutzer in der Website erstellen** aus.
    2. Wählen Sie für **Anmeldeeinstellungen** die Option **SAML-Schaltfläche auf dem Anmeldebildschirm verwenden** aus.
    3. Klicken Sie auf **Speichern**.
12. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Configure Single Sign-On")
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Für das Konfigurieren der Benutzerbereitstellung in Igloo Software steht kein Aktionselement zur Verfügung.  
Wenn ein zugewiesener Benutzer versucht, sich über den Zugriffsbereich bei Igloo Software anzumelden, überprüft Igloo Software, ob der Benutzer vorhanden ist.  
Ist noch kein Benutzerkonto verfügbar, wird es von Igloo Software automatisch erstellt.

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>So weisen Sie Igloo Software Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Igloo Software** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


