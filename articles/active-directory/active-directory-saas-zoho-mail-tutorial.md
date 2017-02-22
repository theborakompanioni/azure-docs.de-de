---
title: 'Tutorial: Azure Active Directory-Integration mit Zoho Mail | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Zoho Mail mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/02/2016
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: 394777b77cbf2fe9fa779c270eec9dded21bdfae


---
# <a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Tutorial: Azure Active Directory-Integration mit Zoho Mail
In diesem Tutorial wird die Integration von Azure und Zoho Mail erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Zoho Mail-Mandanten

Nach Abschluss dieses Tutorials können sich die Zoho Mail zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Zoho Mail-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Zoho Mail
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Scenario")

## <a name="enabling-the-application-integration-for-zoho-mail"></a>Aktivieren der Anwendungsintegration für Zoho Mail
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Zoho Mail aktivieren.

### <a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Zoho Mail:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** **Zoho Mail** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Application Gallery")

7. Wählen Sie im Ergebnisbereich **Zoho Mail** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Zoho Mail zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Zoho Mail** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Configure Single Sign-On")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Zoho Mail anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Configure Single Sign-On")

3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Configure App URL")
   
    a. Geben Sie im Textfeld **Zoho Mail-Anmelde-URL`http://<company name>.ZohoMail.com` die URL in folgendem Format ein: **
   
    b. Klicken Sie auf **Weiter**.

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Zoho Mail** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Configure Single Sign-On")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der Zoho Mail-Unternehmenswebsite als Administrator an.

6. Wechseln Sie zur **Systemsteuerung**.
   
    ![Systemsteuerung](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Control Panel")

7. Klicken Sie auf die Registerkarte **SAML-Authentifizierung** .
   
    ![SAML-Authentifizierung](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML Authentication")

8. Führen Sie im Abschnitt für die **SAML-Authentifizierungsdetails** die folgenden Schritte aus:
   
    ![SAML-Authentifizierungsdetails](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML Authentication Details")
   
    a. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
   
    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Zoho Mail** den Wert für **Kennwort-URL ändern**, und fügen Sie ihn in das Textfeld **Kennwort-URL ändern** ein.
   
    d. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
    > [!TIP]
    > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
   
    e. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Öffentlicher Schlüssel** ein.
   
    f. Wählen Sie als **Algorithmus** **RSA**.
   
    g. Klicken Sie auf **OK**.

9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Zoho Mail anmelden können, müssen sie in Zoho Mail bereitgestellt werden.  
Im Fall von Zoho Mail ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei der **Zoho Mail** -Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Systemsteuerung \> E-Mails und Dokumente**.

3. Wechseln Sie zu **Benutzerdetails \> Benutzer hinzufügen**.
   
    ![Benutzer hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Add User")

4. Führen Sie im Dialogfeld **Benutzer hinzufügen** die folgenden Schritte aus:
   
    ![Benutzer hinzufügen](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Add User")
   
    a. Geben Sie **Vorname**, **Nachname**, **E-Mail-ID** und **Kennwort** eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   
    b. Klicken Sie auf **OK**.  
      
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    > 
    > 

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zoho Mail-Benutzerkonten oder mithilfe der von Zoho Mail bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>So weisen Sie Zoho Mail Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Zoho Mail** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Assign Users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


