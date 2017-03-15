---
title: "Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Dropbox für Unternehmen mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 63502412-758b-4b46-a580-0e8e130791a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: ac53d0a2c1c0a257e86fe74e10c8781aa6305975
ms.openlocfilehash: 80a7280787086a91ceeea308c79f894bdfabb725
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-dropbox-for-business"></a>Tutorial: Azure Active Directory-Integration mit Dropbox für Unternehmen
In diesem Tutorial wird die Integration von Azure und Dropbox für Unternehmen erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Testmandanten in Dropbox für Unternehmen

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Dropbox für Unternehmen zugewiesen haben, mittels einmaliger Anmeldung auf der Unternehmenswebsite für Dropbox für Unternehmen bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Dropbox für Unternehmen
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769508.png "Szenario")

## <a name="enabling-the-application-integration-for-dropbox-for-business"></a>Aktivieren der Anwendungsintegration für Dropbox für Unternehmen
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Dropbox für Unternehmen aktivieren.

### <a name="to-enable-the-application-integration-for-dropbox-for-business-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Dropbox für Unternehmen
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-dropboxforbusiness-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **Dropbox für Unternehmen** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701010.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Dropbox für Unternehmen** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Dropbox für Unternehmen](./media/active-directory-saas-dropboxforbusiness-tutorial/IC701011.png "Dropbox für Unternehmen")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Dropbox für Unternehmen zu authentifizieren.

Im Rahmen dieses Verfahrens müssen Sie ein Base-64-codiertes Zertifikat in Ihren Dropbox für Unternehmen-Mandanten hochladen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Dropbox für Unternehmen anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749327.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
    a. Melden Sie sich bei Ihrem Dropbox für Unternehmen-Mandanten an. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769509.png "Einmaliges Anmelden konfigurieren")
   
    b. Klicken Sie im Navigationsbereich auf der linken Seite auf **Verwaltungskonsole**. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769510.png "Einmaliges Anmelden konfigurieren")
   
    c. Klicken Sie in der **Verwaltungskonsole** im linken Navigationsbereich auf **Authentifizierung**. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769511.png "Einmaliges Anmelden konfigurieren")
   
    d. Wählen Sie im Abschnitt **Einmaliges Anmelden** die Option **Einmaliges Anmelden aktivieren**, und klicken Sie dann auf **Mehr**, um diesen Abschnitt zu erweitern.  
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769512.png "Einmaliges Anmelden konfigurieren")
   
    e. Kopieren Sie die URL unter **Users can sign in by entering their email address or they can go directly to**(Benutzer können sich durch die Eingabe ihrer E-Mail-Adresse anmelden oder direkt zugreifen auf). 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769513.png "Einmaliges Anmelden konfigurieren")
   
    f. Fügen Sie die URL im klassischen Azure-Portal im Textfeld **Anmelde-URL für DropBox für Unternehmen** ein. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769514.png "Einmaliges Anmelden konfigurieren")  
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Dropbox für Unternehmen** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.  
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769515.png "Einmaliges Anmelden konfigurieren")
5. Gehen Sie auf Ihrem Dropbox für Unternehmen-Mandanten im Abschnitt **Einmaliges Anmelden** der Seite **Authentifizierung** wie folgt vor: 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769516.png "Einmaliges Anmelden konfigurieren")
   
    a. Klicken Sie auf **Erforderlich**.
   
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Dropbox für Unternehmen** den Wert für **URL der Anmeldeseite**, und fügen Sie ihn ins Textfeld **Anmelde-URL** ein.

    c. Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat. 

    > [!TIP] 
    > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o).

    d. Klicken Sie auf **Zertifikat wählen**, und wechseln Sie dann zu Ihrer **Base-64-codierten Zertifikatsdatei**.

    e. Klicken Sie auf **Änderungen speichern**, um die Konfiguration für Ihren DropBox für Unternehmen-Mandanten abzuschließen.

1. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-dropboxforbusiness-tutorial/IC749329.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für Dropbox für Unternehmen aktivieren.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** auf **Benutzerbereitstellung konfigurieren**, um das Dialogfeld **Benutzerbereitstellung konfigurieren** zu öffnen.
2. Klicken Sie auf der Seite „Benutzerbereitstellung aktivieren für DropBox für Unternehmen“ auf „Benutzerbereitstellung aktivieren“, um das Dialogfeld „Bei Dropbox anmelden zum Verbinden mit Azure AD“ zu öffnen.  
   
    ![Benutzerbereitstellung](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769517.png "Benutzerbereitstellung")
3. Melden Sie sich im Dialogfeld **Bei Dropbox anmelden zum Verbinden mit Azure AD** bei Ihrem Dropbox für Unternehmen-Mandanten an. 
   
    ![Benutzerbereitstellung](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769518.png "Benutzerbereitstellung")
4. Klicken Sie auf **Zulassen** , um Azure AD den Zugriff auf Dropbox zu gewähren. 
   
    ![Benutzerbereitstellung](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769519.png "Benutzerbereitstellung")
5. Klicken Sie zum Abschließen der Konfiguration auf die Schaltfläche **Abschließen** .  
   
    ![Benutzerbereitstellung](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769520.png "Benutzerbereitstellung")

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-dropbox-for-business-perform-the-following-steps"></a>So weisen Sie Dropbox für Unternehmen Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769521.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-dropboxforbusiness-tutorial/IC767830.png "Ja")

Nach 10 Minuten können Sie überprüfen, ob das Konto mit Citrix GoToMeeting synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellungsstatus überprüfen, indem Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Dropbox für Unternehmen** auf **Dashboard** klicken.

![Zuweisen von Benutzern](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769522.png "Zuweisen von Benutzern")

Ein erfolgreich abgeschlossener Benutzerbereitstellungszyklus wird durch einen entsprechenden Status angezeigt.

![Zuweisen von Benutzern](./media/active-directory-saas-dropboxforbusiness-tutorial/IC769523.png "Zuweisen von Benutzern")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


