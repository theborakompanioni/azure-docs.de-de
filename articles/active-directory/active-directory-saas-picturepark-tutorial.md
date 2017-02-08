---
title: 'Tutorial: Azure Active Directory-Integration mit Picturepark | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Picturepark mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 98f2e5596a0af2fc9e633e005642cc3cd21621ce


---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Tutorial: Azure Active Directory-Integration mit Picturepark
In diesem Tutorial wird die Integration von Azure und Picturepark erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Picturepark-Mandanten

Nach Abschluss dieses Tutorials können sich die Picturepark zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Picturepark-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Picturepark
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-picturepark-tutorial/IC795055.png "Scenario")

## <a name="enabling-the-application-integration-for-picturepark"></a>Aktivieren der Anwendungsintegration für Picturepark
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Picturepark aktivieren.

### <a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Picturepark
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-picturepark-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-picturepark-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-picturepark-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Picturepark** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-picturepark-tutorial/IC795056.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Picturepark** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Picturepark](./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Picturepark zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Picturepark müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Picturepark** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/IC795058.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Picturepark anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/IC795059.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Picturepark-Anmelde-URL** Ihre URL im Format „*http://company.picturepark.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-picturepark-tutorial/IC795060.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Picturepark** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/IC795061.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Picturepark-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Verwaltungstools**, und klicken Sie dann auf **Verwaltungskonsole**.
   
   ![Verwaltungskonsole](./media/active-directory-saas-picturepark-tutorial/IC795062.png "Management Console")
7. Klicken Sie auf **Authentifizierung** und dann auf **Identitätsanbieter**.
   
   ![Authentifizierung](./media/active-directory-saas-picturepark-tutorial/IC795063.png "Authentication")
8. Führen Sie im Abschnitt **Identitätsanbieterkonfiguration** die folgenden Schritte aus:
   
   ![Identitätsanbieterkonfiguration](./media/active-directory-saas-picturepark-tutorial/IC795064.png "Identity provider configuration")
   
   1. Klicken Sie auf **Hinzufügen**.
   2. Geben Sie einen Namen für die Konfiguration ein.
   3. Wählen Sie **Als Standard festlegen**.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Picturepark** den Wert für **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **Aussteller-URI** ein.
   5. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck eines vertrauenswürdigen Ausstellers** ein.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
      > 
      > 
   6. Klicken Sie auf **JoinDefaultUsersGroup**.
   7. Geben Sie **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ein, um das Attribut **Emailaddress** im Textfeld **Anspruch** festzulegen.
      ![Konfiguration](./media/active-directory-saas-picturepark-tutorial/IC795065.png "Configuration")
   8. Klicken Sie auf **Speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/IC795066.png "Configure Single Sign-On")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Picturepark anmelden können, müssen sie in Picturepark bereitgestellt werden.  
Im Fall von Picturepark ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **Picturepark** -Mandanten an.
2. Klicken Sie oben in der Symbolleiste auf **Verwaltungstools**, und klicken Sie dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-picturepark-tutorial/IC795067.png "Users")
3. Klicken Sie auf der Registerkarte **Benutzerübersicht** auf **Neu**.
   
   ![Benutzerverwaltung](./media/active-directory-saas-picturepark-tutorial/IC795068.png "User management")
4. Führen Sie im Dialogfeld **Benutzer erstellen** die folgenden Schritte aus:
   
   ![Benutzer erstellen](./media/active-directory-saas-picturepark-tutorial/IC795069.png "Create User")
   
   1. Geben Sie die Informationen für **E-Mail-Adresse**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname**, **Unternehmen**, **Land**, **PLZ**, **Ort** eines gültigen Azure Active Directory-Benutzers, den Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Wählen Sie eine **Sprache**aus.
   3. Klicken Sie auf **Erstellen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Picturepark-Benutzerkonten oder mithilfe der von Picturepark bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>So weisen Sie Picturepark Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Picturepark** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-picturepark-tutorial/IC795070.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-picturepark-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


