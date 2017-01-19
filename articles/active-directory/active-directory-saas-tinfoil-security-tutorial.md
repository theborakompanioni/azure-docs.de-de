---
title: 'Tutorial: Azure Active Directory-Integration mit Tinfoil Security | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Tinfoil Security mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: da02da92-e3b0-4c09-ad6c-180882b0f9f8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/05/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1cef7ff21a8d076c89688f1fe75cebdb7c468199
ms.openlocfilehash: 11b14aee1ef28cd29976b138919ccea8b4763016


---
# <a name="tutorial-azure-active-directory-integration-with-tinfoil-security"></a>Tutorial: Azure Active Directory-Integration mit Tinfoil Security
In diesem Tutorial wird die Integration von Azure und Tinfoil Security erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Tinfoil Security-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Tinfoil Security zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Tinfoil Security-Unternehmenswebsite bei der Anwendung anmelden (durch den Identitätsanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Tinfoil Security
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798965.png "Configure Single Sign-On")

## <a name="enabling-the-application-integration-for-tinfoil-security"></a>Aktivieren der Anwendungsintegration für Tinfoil Security
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Tinfoil Security aktivieren.

### <a name="to-enable-the-application-integration-for-tinfoil-security-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Tinfoil Security:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-tinfoil-security-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-tinfoil-security-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-tinfoil-security-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-tinfoil-security-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** als Suchbegriff **Tinfoil Security** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-tinfoil-security-tutorial/IC798966.png "Application Gallery")

7. Wählen Sie im Ergebnisbereich **Tinfoil Security** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Tinfoil Security](./media/active-directory-saas-tinfoil-security-tutorial/IC802771.png "Tinfoil Security")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Tinfoil Security zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Tinfoil Security müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Tinfoil Security** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798967.png "Configure Single Sign-On")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Tinfoil Security anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798968.png "Configure Single Sign-On")

3. Geben Sie auf der Seite **App-URL konfigurieren** des Textfeldes **Tinfoil Security-Antwort-URL** Ihre Tinfoil Security Assertion Consumer Service-URL (ACS) ein (z.B. „*https://www.tinfoilsecurity.com/saml/consume*“), und klicken Sie dann auf **Weiter**.
   
    > [!NOTE]
    > Sie sollten in der Lage sein, die ACS-URL aus den Tinfoil Security-Metadaten abzurufen (https://www.tinfoilsecurity.com/saml/metadata).
    > 
    > 
   
    ![App-URL konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798969.png "Configure App URL")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Tinfoil Security** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal unter **c:\\Tinfoil Security.cer**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798970.png "Configure Single Sign-On")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der Tinfoil Security-Unternehmenswebsite als Administrator an.

6. Klicken Sie oben auf der Symbolleiste auf **Einstellungen**.
   
    ![Dashboard](./media/active-directory-saas-tinfoil-security-tutorial/IC798971.png "Dashboard")

7. Klicken Sie auf **Security**.
   
    ![Security](./media/active-directory-saas-tinfoil-security-tutorial/IC798972.png "Security")

8. Führen Sie auf der Konfigurationsseite **Single Sign-On** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden](./media/active-directory-saas-tinfoil-security-tutorial/IC798973.png "Single Sign-On")
   
    a. Wählen Sie **SAML aktivieren**.
   
    b. Klicken Sie auf **Manuelle Konfiguration**.
   
    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Tinfoil Security** den Wert der **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **SAML POST-URL** ein.
   
    d. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **SAML-Zertifikatsfingerabdruck** ein.  
      
    > [!TIP]
    > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
    > 
    > 
   
    e. Kopieren Sie **Ihre Konto-ID**.
   
    f. Klicken Sie auf **Speichern**.

9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tinfoil-security-tutorial/IC798974.png "Configure Single Sign-On")

10. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
    
    ![Attribute](./media/active-directory-saas-tinfoil-security-tutorial/IC795920.png "Attributes")

11. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
    
    ![Attribute](./media/active-directory-saas-tinfoil-security-tutorial/IC798975.png "Attributes")
    
    a. Klicken Sie auf **Benutzerattribut hinzufügen**.

    b. Geben Sie im Textfeld **Attributname** die Zeichenfolge **accountid** ein.

    c. Fügen Sie in das Textfeld **Attributwert** den Konto-ID-Wert ein, den Sie im vorherigen Abschnitt kopiert haben.

    d. Klicken Sie auf **Fertig stellen**.

12. Klicken Sie auf **Apply Changes**.

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Tinfoil Security anmelden können, müssen sie in Tinfoil Security bereitgestellt werden.  
Im Fall von Tinfoil Security ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-get-a-user-provisioned-perform-the-following-steps"></a>So rufen Sie einen bereitgestellten Benutzer ab:
1. Wenn der Benutzer einen Teil eines Enterprise-Kontos ist, müssen Sie sich an das Tinfoil Security-Supportteam wenden, um sich das Benutzerkonto anlegen zu lassen.
2. Wenn der Benutzer ein normaler Tinfoil Security-Benutzer ist, kann der Benutzer zu allen Websites des Benutzers einen Mitarbeiter hinzufügen. Dies löst einen Prozess aus, bei dem eine Einladung an die angegebene E-Mail-Adresse gesendet wird, um ein neues Tinfoil Security-Benutzerkonto anzulegen.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Tinfoil Security-Benutzerkonten oder mithilfe der von Tinfoil Security bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-tinfoil-security-perform-the-following-steps"></a>So weisen Sie Tinfoil Security Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Tinfoil Security** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-tinfoil-security-tutorial/IC798976.png "Assign Users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-tinfoil-security-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO1-->


