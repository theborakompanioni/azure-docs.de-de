---
title: 'Tutorial: Azure Active Directory-Integration mit Work.com | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Work.com mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: 69d94659f4eff72e1c449fd915616d81fd4712de


---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Tutorial: Azure Active Directory-Integration mit Work.com
In diesem Tutorial wird die Integration von Azure und Work.com erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Work.com-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die AAD-Benutzer, denen Sie den Zugriff auf Work.com erteilt haben, mittels einmaliger Anmeldung auf der Work.com-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können Sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Work.com
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-work-com-tutorial/IC794105.png "Scenario")

## <a name="enabling-the-application-integration-for-workcom"></a>Aktivieren der Anwendungsintegration für Work.com
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Work.com aktivieren.

### <a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Work.com
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-work-com-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-work-com-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-work-com-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** die Wörter **Work.com** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-work-com-tutorial/IC794106.png "Application Gallery")

7. Wählen Sie im Ergebnisbereich **Work.com** aus, und klicken Sie anschließend auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Work.com](./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Work.com zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie ein Zertifikat auf Work.com hochladen.

> [!NOTE]
> Um einmaliges Anmelden zu konfigurieren, müssen Sie einen benutzerdefinierten Domänennamen von Work.com einrichten. Sie müssen mindestens einen Domänennamen definieren, testen und dann in Ihrer gesamten Organisation bereitstellen.
> 
> 

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Melden Sie sich bei Ihrem Work.com-Mandanten als Administrator an.
2. Wechseln Sie zu **Setup**.
   
    ![Einrichtung](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

3. Klicken Sie im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
   
    ![Meine Domäne](./media/active-directory-saas-work-com-tutorial/IC767825.png "My Domain")

4. Um zu überprüfen, ob Ihre Domäne richtig eingerichtet wurde, stellen Sie sicher, dass sich die Domäne in **Schritt 4 – bereitgestellt für Benutzer** befindet, und überprüfen Sie die **Domäneneinstellungen**.
   
    ![Domäne für Benutzer bereitgestellt](./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman Deployed to User")

5. Melden Sie sich in einem anderen Webbrowserfenster beim klassischen Azure-Portal an.

6. Klicken Sie auf der Anwendungsintegrationsseite für **Work.com** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-work-com-tutorial/IC794109.png "Configure Single Sign-On")

7. Wählen Sie auf der Seite **How would you like users to sign on to Work.com?** (Wie sollen sich Benutzer bei Work.com anmelden?) die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie anschließend auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-work-com-tutorial/IC794110.png "Configure Single Sign-On")

8. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Work.com-Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei Work.com verwenden (z.B. „*http://company.my.salesforce.com*“), und klicken Sie anschließend auf **Weiter**: 
   
    ![App-URL konfigurieren](./media/active-directory-saas-work-com-tutorial/IC794111.png "Configure App URL")

9. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Work.com** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-work-com-tutorial/IC794112.png "Configure Single Sign-On")

10. Melden Sie sich bei Ihrem Work.com-Mandanten an.

11. Wechseln Sie zu **Setup**.
    
    ![Einrichtung](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")

12. Erweitern Sie das Menü **Sicherheitskontrollen**, und klicken Sie anschließend auf **Single Sign-On Settings** (Einstellungen für einmaliges Anmelden).
    
    ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-work-com-tutorial/IC794113.png "Single Sign-On Settings")

13. Führen Sie auf der Dialogfeldseite **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
    
    ![SAML aktiviert](./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML Enabled")
    
    a. Wählen Sie **SAML aktiviert**aus.
    
    b. Klicken Sie auf **Neu**.

14. Führen Sie im Abschnitt **Einstellungen für einmalige Anmelden für SAML** die folgenden Schritte aus:
    
    ![SAML-Einstellung für einmaliges Anmelden](./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML Single Sign-On Setting")
    
    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein.  
       
    > [!NOTE]
    > Bei Eingabe eines Werts für **Name** wird automatisch das Textfeld **API-Name** gefüllt.
    > 
    > 
    
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at Work.com** (Einmaliges Anmelden für Work.com konfigurieren) den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
    
    c. Klicken Sie auf **Durchsuchen**, um das heruntergeladene Zertifikat hochzuladen.
    
    d. Geben Sie im Textfeld **Entitäts-ID** die folgende URL ein:**https://salesforce-work.com**.
    
    e. Wählen Sie für **SAML Identity Type** (SAML-Identitätstyp) die Option **Assertion contains the Federation ID from the User object** (Assertion enthält die Verbund-ID aus dem Benutzerobjekt) aus.
    
    f. Wählen Sie für **SAML Identity Location** (Speicherort der SAML-Identität) die Option **Identity is in the NameIdentifier element of the Subject statement** (Identität ist im NameIdentifier-Element der Subject-Anweisung enthalten) aus.
    
    g. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at Work.com** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.
    
    h. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at Work.com** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.
    
    i. Wählen Sie für **Service Provider Initiated Request Binding** (Vom Dienstanbieter initiierte Anforderungsbindung) die Option **HTTP Post** aus.
    
    j. Klicken Sie auf **Speichern**.

15. Klicken Sie im linken Navigationsbereich des klassischen Work.com-Portals auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
    
    ![Meine Domäne](./media/active-directory-saas-work-com-tutorial/IC794115.png "My Domain")

16. Klicken Sie auf der Seite **Meine Domäne** im Abschnitt **Anmeldeseitenbranding** auf **Bearbeiten**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-work-com-tutorial/IC767826.png "Login Page Branding")

17. Im Abschnitt **Authentifizierungsdienst** der Seite **Anmeldeseitenbranding** wird der Name Ihrer **SAML-SSO-Einstellungen** angezeigt. Wählen Sie den Namen aus, und klicken Sie auf **Speichern**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-work-com-tutorial/IC784366.png "Login Page Branding")

18. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-work-com-tutorial/IC794116.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure Active Directory-Benutzer anmelden können, müssen sie in Work.com bereitgestellt werden.  
Im Fall von Work.com ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der Work.com-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Setup**.
   
    ![Einrichtung](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")
3. Navigieren Sie zu **Benutzer verwalten \> Benutzer**.
   
    ![Benutzer verwalten](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")

4. Klicken Sie auf **Neuer Benutzer**.
   
    ![Alle Benutzer](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")

5. Führen Sie im Abschnitt „Benutzer bearbeiten“ die folgenden Schritte aus:
   
    ![Benutzer bearbeiten](./media/active-directory-saas-work-com-tutorial/IC794118.png "User Edit")
   
    a. Geben Sie die Attribute **Nachname**, **Alias**, **E-Mail**, **Benutzername** und **Spitzname** eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   
    b. Wählen Sie **Rolle**, **Benutzerlizenz** und **Profil** aus.
   
    c. Klicken Sie auf **Speichern**.  
      
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    > 
    > 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-workcom-perform-the-following-steps"></a>So weisen Sie Work.com Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für Work.com auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-work-com-tutorial/IC794119.png "Assign Users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-work-com-tutorial/IC767830.png "Yes")

Nach 10 Minuten können Sie überprüfen, ob das Konto mit Work.com synchronisiert wurde.

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


