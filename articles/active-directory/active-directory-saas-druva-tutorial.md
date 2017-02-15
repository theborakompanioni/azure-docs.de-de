---
title: 'Tutorial: Azure Active Directory-Integration mit Druva | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Druva mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ab92b600-1fea-4905-b1c7-ef8e4d8c495c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 51e6da24517479c7fe47e811cc2355ccdeb961b3


---
# <a name="tutorial-azure-active-directory-integration-integration-with-druva"></a>Tutorial: Azure Active Directory-Integration mit Druva
In diesem Tutorial wird die Integration von Azure und Druva erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Druva-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Druva zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf der Druva-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich nutzen](active-directory-saas-access-panel-introduction.md).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Druva
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-druva-tutorial/IC795084.png "Scenario")

## <a name="enabling-the-application-integration-for-druva"></a>Aktivieren der Anwendungsintegration für Druva
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Druva aktivieren.

### <a name="to-enable-the-application-integration-for-druva-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Druva
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-druva-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-druva-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-druva-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-druva-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **Druva** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-druva-tutorial/IC795085.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Druva** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Druva](./media/active-directory-saas-druva-tutorial/IC795086.png "Druva")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Druva zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache) weitere Informationen.

Die Druva-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![SAML-Tokenattribute](./media/active-directory-saas-druva-tutorial/IC795087.png "SAML Token Attributes")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Druva** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-druva-tutorial/IC795027.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Druva anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-druva-tutorial/IC795088.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Druva-Anmelde-URL** die von Ihren Benutzer für die Anmeldung bei Ihrer Druva-Anwendung verwendete URL ein (z.B. „*https://cloud.druva.com/home/*“), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-druva-tutorial/IC795089.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Druva** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-druva-tutorial/IC795090.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Druva-Unternehmenswebsite als Administrator an.
6. Navigieren Sie zu **Verwalten \> Einstellungen**.
   
   ![Einstellungen](./media/active-directory-saas-druva-tutorial/IC795091.png "Settings")
7. Führen Sie im Dialogfeld „Einstellungen für einmaliges Anmelden“ die folgenden Schritte aus:
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-druva-tutorial/IC795092.png "Singl Sign-On Settings")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Druva** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **ID Provider Login URL** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Druva** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **ID Provider Logout URL** ein.
   3. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des ID-Anbieters** ein.
   5. Klicken Sie zum Öffnen der Seite **Einstellungen** auf **Speichern**.
8. Klicken Sie auf der Seite **Einstellungen** auf **SSO-Token generieren**.
   
   ![Einstellungen](./media/active-directory-saas-druva-tutorial/IC795093.png "Settings")
9. Führen Sie im Dialogfeld **Token für einmaliges Anmelden** die folgenden Schritte aus:
   
   ![SSO-Token](./media/active-directory-saas-druva-tutorial/IC795094.png "SSO Token")
   
   1. Klicken Sie auf **Kopieren**.
   2. Klicken Sie auf **Schließen**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-druva-tutorial/IC795095.png "Configure Single Sign-On")
11. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
    
    ![Attribute](./media/active-directory-saas-druva-tutorial/IC795096.png "Attributes")
12. Führen Sie zum Hinzufügen der erforderlichen Attributzuordnungen die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | --- | --- |
    | inSync\_auth\_token |<*Wert in der Zwischenablage*> |
    
    1. Klicken Sie für jede Datenzeile in der obigen Tabelle auf **Benutzerattribut hinzufügen**.
    2. Geben Sie im Textfeld **Attributname** den für die Zeile angezeigten Attributnamen ein.
    3. Geben Sie im Textfeld **Attributwert** den für die Zeile angezeigten Attributwert ein.
    4. Klicken Sie auf **Fertig stellen**.
13. Klicken Sie auf **Apply Changes**.
    
    ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Druva anmelden können, müssen sie in Druva bereitgestellt werden.  
Im Fall von Druva ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der **Druva** -Unternehmenswebsite als Administrator an.
2. Navigieren Sie zu **Benutzer \> verwalten**.
   
   ![Benutzer verwalten](./media/active-directory-saas-druva-tutorial/IC795097.png "Manage Users")
3. Klicken Sie auf **Neu erstellen**.
   
   ![Benutzer verwalten](./media/active-directory-saas-druva-tutorial/IC795098.png "Manage Users")
4. Führen Sie im Dialogfeld „Neuen Benutzer erstellen“ die folgenden Schritte aus:
   
   ![Neuen Benutzer erstellen](./media/active-directory-saas-druva-tutorial/IC795099.png "Create NewUser")
   
   1. Geben Sie die E-Mail-Adresse und den Namen eines gültigen Azure Active Directory-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf **Benutzer erstellen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Druva-Benutzerkonten oder mithilfe der von Druva bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-druva-perform-the-following-steps"></a>So weisen Sie Druva Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Druva** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-druva-tutorial/IC795100.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-druva-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


