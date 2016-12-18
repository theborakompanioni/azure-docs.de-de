---
title: 'Tutorial: Azure Active Directory-Integration mit Clever | Microsoft Docs'
description: "Erfahren Sie, wie Sie Clever mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fc748c70afede4be52e6c49db72932588be779ce


---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Lernprogramm: Azure Active Directory-Integration mit Clever
In diesem Lernprogramm wird die Integration von Azure und Clever erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Clever-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Clever zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Clever-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Clever
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-clever-tutorial/IC798977.png "Scenario")

## <a name="enabling-the-application-integration-for-clever"></a>Aktivieren der Anwendungsintegration für Clever
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Clever aktivieren.

### <a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Clever
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-clever-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-clever-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-clever-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **Clever** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-clever-tutorial/IC798978.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Clever** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Clever](./media/active-directory-saas-clever-tutorial/IC798979.png "Clever")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Clever zu authentifizieren.  
Die Clever-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Attribute](./media/active-directory-saas-clever-tutorial/IC798980.png "Attributes")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Clever** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC784682.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Clever anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798981.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Clever-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer Clever-Anwendung verwendete URL ein (z.B. *https://clever.com/in/azsandbox*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-clever-tutorial/IC798982.png "Configure App URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Clever** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798983.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Clever-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste auf **Instant Login**.
   
   ![Instant Login](./media/active-directory-saas-clever-tutorial/IC798984.png "Instant Login")
7. Führen Sie auf der Seite **Instant Login** die folgenden Schritte aus:
   
   ![Instant Login](./media/active-directory-saas-clever-tutorial/IC798985.png "Instant Login")
   
   1. Geben Sie die **Login URL**ein.  
      
      > [!NOTE]
      > Die **Login URL** ist ein benutzerdefinierter Wert.
      > Sie erhalten diesen Wert vom Clever-Supportteam.
      > 
      > 
   2. Wählen Sie als **Identitätssystem** die Option **ADFS** aus.
   3. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798986.png "Configure Single Sign-On")
9. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
   
   ![Attribute](./media/active-directory-saas-clever-tutorial/IC795920.png "Attributes")
10. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
    
    ![SAML-Tokenattribute](./media/active-directory-saas-clever-tutorial/IC795921.png "saml token attributes")
    
    | Attributname | Attributwert |
    | --- | --- |
    | clever.student.credentials.district\_benutzername |User.userprincipalname |
    
    1. Klicken Sie für jede Datenzeile in der obigen Tabelle auf **add user attribute**.
    2. Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
    3. Geben Sie im Textfeld **Attribute Value** den für die Zeile angezeigten Attributwert ein.
    4. Klicken Sie auf **Fertig stellen**.
11. Klicken Sie auf **Apply Changes**.

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei Clever anmelden können, müssen sie in Clever bereitgestellt werden.  
Im Fall von Clever ist die Bereitstellung eine manuelle Aufgabe, die von Ihrem Clever-Support-Team ausgeführt werden muss.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Clever-Benutzerkonten oder mithilfe der von Clever bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-clever-perform-the-following-steps"></a>So weisen Sie Clever Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Clever** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-clever-tutorial/IC798987.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-clever-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


