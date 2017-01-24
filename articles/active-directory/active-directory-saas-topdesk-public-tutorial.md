---
title: 'Tutorial: Azure Active Directory-Integration mit TOPdesk - Public | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie TOPdesk - Public mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1cef7ff21a8d076c89688f1fe75cebdb7c468199
ms.openlocfilehash: ee413f5044d541a7a127690fab0115341c7619f3


---
# <a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Tutorial: Azure Active Directory-Integration mit TOPdesk - Public
In diesem Tutorial wird die Integration von Azure und TOPdesk - Public erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein TOPdesk - Public-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie TOPdesk - Public zugewiesen haben, mittels einmaliger Anmeldung auf der TOPdesk - Public-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für TOPdesk - Public
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Scenario")

## <a name="enabling-the-application-integration-for-topdesk---public"></a>Aktivieren der Anwendungsintegration für TOPdesk - Public
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für TOPdesk - Public aktivieren.

### <a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für TOPdesk - Public:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie in das **Suchfeld** als Suchbegriff **TOPdesk - Public** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Application Gallery")

7. Wählen Sie im Ergebnisbereich **TOPdesk - Public** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![TOPdesk Public](./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk Public")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei TOPdesk - Public zu authentifizieren.  
Für das Konfigurieren des einmaligen Anmeldens für TOPdesk - Public müssen Sie eine Logosymboldatei hochladen. Um die Symboldatei zu erhalten, wenden Sie sich an das TOPdesk-Supportteam.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Melden Sie sich bei der **TOPdesk - Public** -Unternehmenswebsite als Administrator an.

2. Klicken Sie im Menü **TOPdesk** auf **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Settings")

3. Klicken Sie auf **Anmeldeeinstellungen**.
   
    ![Anmeldeeinstellungen](./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login Settings")

4. Erweitern Sie das Menü **Anmeldeeinstellungen**, und klicken Sie dann auf **Allgemein**.
   
    ![Allgemein](./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "General")

5. Führen Sie im Abschnitt **Öffentlich** des Konfigurationsabschnitts **SAML-Anmeldung** die folgenden Schritte aus:
   
    ![Technische Einstellungen](./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Technical Settings")
   
    a. Klicken Sie auf **Herunterladen** , um die  öffentliche Metadatendatei herunterzuladen und sie dann lokal auf Ihrem Computer zu speichern.
   
    b. Öffnen Sie die Metadatendatei, und suchen Sie den Knoten **AssertionConsumerService** .
    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
   
    c. Kopieren Sie den Wert **AssertionConsumerService** .  
      
    > [!NOTE]
    > Sie benötigen den Wert später im Tutorial im Abschnitt **App-URL konfigurieren** .
    > 
    > 

6. Melden Sie sich in einem anderen Webbrowserfenster beim **klassischen Azure-Portal** als Administrator an.

7. Klicken Sie auf der Anwendungsintegrationsseite für **TOPdesk - Public** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Configure Single Sign-On")

8. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei TOPdesk - Public anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Configure Single Sign-On")

9. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Configure App URL")
   
    a. Geben Sie in das Textfeld **TOPdesk - Public-Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei der TOPdesk - Public-Anwendung verwenden (z.B. *https://qssolutions.topdesk.net*).
   
    b. Fügen Sie in das Textfeld **TOPdesk - Public-Antwort-URL** die **TOPdesk - Public-AssertionConsumerService-URL** ein (z.B.: *https://qssolutions.topdesk.net/tas/public/login/saml*).
   
    c. Klicken Sie auf **Weiter**.

10. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für TOPdesk - Public** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Configure Single Sign-On")

11. Zum Erstellen einer Zertifikatsdatei führen Sie die folgenden Schritte durch:
    
    ![Zertifikat](./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Certificate")
    
    1. Öffnen Sie die heruntergeladene Metadatendatei.
    2. Erweitern Sie den Knoten **RoleDescriptor**, der über einen **xsi:type** von **fed:ApplicationServiceType** verfügt.
    3. Kopieren Sie den Wert des Knotens **X509Certificate** .
    4. Speichern Sie den kopierten **X509Certificate** -Wert lokal in einer Datei auf Ihrem Computer.
12. Klicken Sie auf Ihrer TOPdesk - Public-Unternehmenswebsite im Menü **TOPdesk** auf **Einstellungen**.
    
    ![Einstellungen](./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Settings")
13. Klicken Sie auf **Anmeldeeinstellungen**.
    
    ![Anmeldeeinstellungen](./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Login Settings")
14. Erweitern Sie das Menü **Anmeldeeinstellungen**, und klicken Sie dann auf **Allgemein**.
    
    ![Allgemein](./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "General")
15. Klicken Sie im Abschnitt **Öffentlich** auf **Hinzufügen**.
    
    ![SAML-Anmeldung](./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML Login")
16. Führen Sie auf der Seite **SAML Configuration Assistant** die folgenden Schritte aus:
    
    ![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML Configuration Assistant")
    
    a. Klicken Sie zum Hochladen der heruntergeladenen Metadatendatei unter **Verbundmetadaten** auf **Durchsuchen**.

    b. Klicken Sie zum Hochladen Ihrer Zertifikatsdatei unter **Zertifikat (RSA)** auf **Durchsuchen**.

    c. Klicken Sie zum Hochladen der Logodatei, die Sie vom TOPdesk-Supportteam erhalten haben, unter **Logosymbol** auf **Durchsuchen**.

    d. Geben Sie in das Textfeld **Benutzernamenattribut** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    e. Geben Sie in das Textfeld **Anzeigenname** einen Namen für die Konfiguration ein.

    f. Klicken Sie auf **Speichern**.

17. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei TOPdesk - Public anmelden können, müssen sie in TOPdesk - Public bereitgestellt werden.  
Im Fall von TOPdesk - Public ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei Ihrem **TOPdesk - Public** -Unternehmenswebsite als Administrator an.

2. Klicken Sie im Menü oben auf **TOPdesk \> Neu \> Support-Dateien \> Person**.
   
    ![Person](./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Person")

3. Führen Sie im Dialogfeld „Neue Person“ die folgenden Schritte aus:
   
    ![Neue Person](./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "New Person")
   
    a. Klicken Sie auf die Registerkarte „Allgemein“.

    b. Geben Sie in das Textfeld „Nachname“ den Nachnamen eines gültigen Azure Active Directory-Kontos ein, welches Sie bereitstellen möchten.
 
    c. Wählen Sie einen **Speicherort** für das Konto aus.
 
    d. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von TOPdesk - Public-Benutzerkonten oder mithilfe der von TOPdesk - Public bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>So weisen Sie  TOPdesk - Public Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **TOPdesk - Public** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Assign Users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


