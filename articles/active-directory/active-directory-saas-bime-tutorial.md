---
title: 'Tutorial: Azure Active Directory-Integration mit Bime | Microsoft Docs'
description: "Erfahren Sie, wie Sie Bime mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c1adfe732b88a6a48a3aa9ad3ca7785703667e3d


---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a>Lernprogramm: Azure Active Directory-Integration mit Bime
In diesem Lernprogramm wird die Integration von Azure und Bime erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Bime-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Bime zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Bime-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Bime
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bime-tutorial/IC775552.png "Scenario")

## <a name="enabling-the-application-integration-for-bime"></a>Aktivieren der Anwendungsintegration für Bime
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Bime aktivieren.

### <a name="to-enable-the-application-integration-for-bime-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Bime
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bime-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bime-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bime-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Bime** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bime-tutorial/IC775553.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Bime** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Bime](./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Bime zu authentifizieren.  
Zum Konfigurieren des einmaligen Anmeldens für Bime müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI)(in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Bime** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC771709.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bime anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775555.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Bime-Anmelde-URL** die URL im Format *https://\<Mandantenname\>.Bimeapp.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-bime-tutorial/IC775556.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um Bime** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal unter **C:\\Bime.cer**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775557.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Bime-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste auf **Administration** und dann auf **Konto**.
   
   ![Administrator](./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")
7. Führen Sie auf der Kontenkonfigurationsseite die folgenden Schritte aus:
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775559.png "Configure Single Sign-On")
   
   1. Aktivieren Sie **Enable SAML authentication**.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bime** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteanmelde-URL** ein.
   3. Kopieren Sie den Wert für **Fingerabdruck** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** ein.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)
      > 
      > 
   4. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775560.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Bime anmelden können, müssen sie in Bime bereitgestellt werden.  
Im Fall von Bime ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei Ihrem **Bime** -Mandanten an.
2. Klicken Sie auf der Symbolleiste auf **Administration** und dann auf **Benutzer**.
   
   ![Administrator](./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")
3. Klicken Sie unter **Benutzerliste** auf **Neuen Benutzer hinzufügen** („+“).
   
   ![Benutzer](./media/active-directory-saas-bime-tutorial/IC775562.png "Users")
4. Führen Sie auf der Dialogfeldseite **User Details** die folgenden Schritte aus:
   
   ![User Details](./media/active-directory-saas-bime-tutorial/IC775563.png "User Details")
   
   1. Geben Sie den "Vornamen", den "Nachnamen", den "Anmeldenamen" und die "E-Mail-Adresse" eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf Speichern.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Bime-Benutzerkonten oder mithilfe der von Bime bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-bime-perform-the-following-steps"></a>So weisen Sie Bime Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Bime** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-bime-tutorial/IC775564.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bime-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


