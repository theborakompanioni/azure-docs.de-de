---
title: 'Tutorial: Azure Active Directory-Integration mit PolicyStat | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie PolicyStat mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 738184f4a253201a9aa7581e03d269a06d7cf48a


---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a>Tutorial: Azure Active Directory-Integration mit PolicyStat
In diesem Tutorial wird die Integration von Azure und PolicyStat erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen PolicyStat-Mandanten

Nach Abschluss dieses Tutorials können sich die PolicyStat zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer PolicyStat-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für PolicyStat
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-policystat-tutorial/IC808662.png "Scenario")

## <a name="enabling-the-application-integration-for-policystat"></a>Aktivieren der Anwendungsintegration für PolicyStat
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für PolicyStat aktivieren.

### <a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für PolicyStat
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-policystat-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-policystat-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-policystat-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** den Suchbegriff **PolicyStat** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-policystat-tutorial/IC808627.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich die Option **PolicyStat** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei PolicyStat zu authentifizieren.  
Die PolicyStat-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie der Konfiguration Ihrer **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Attribute](./media/active-directory-saas-policystat-tutorial/IC808628.png "Attributes")

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **PolicyStat** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808629.png "Configure Single Sign-On")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei PolicyStat anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808630.png "Configure Single Sign-On")
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Textfeld für die **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer PolicyStat-Anwendung verwendete URL ein (beispielsweise „*https://demo-azure.policystat.com*“), und klicken Sie dann auf **Weiter**.
   
   ![Konfigurieren von App-Einstellungen](./media/active-directory-saas-policystat-tutorial/IC808631.png "Configure App Settings")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für PolicyStat** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808632.png "Configure Single Sign-On")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der PolicyStat-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf die Registerkarte **Administrator** und anschließend links im Navigationsbereich auf **Einmaliges Anmelden – Konfiguration**.
   
   ![Administratormenü](./media/active-directory-saas-policystat-tutorial/IC808633.png "Administrator Menu")
7. Wählen Sie im Abschnitt **Einrichtung** die Option **Integration des einmaligen Anmeldens aktivieren** aus.
   
   ![Einmaliges Anmelden – Konfiguration](./media/active-directory-saas-policystat-tutorial/IC808634.png "Single Sign-On Configuration")
8. Klicken Sie auf **Attribute konfigurieren**, und führen Sie anschließend im Abschnitt **Attribute konfigurieren** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden – Konfiguration](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")
   
   1. Geben Sie im Textfeld **Benutzernamen-Attribut** die Zeichenfolge **uid** ein.
   2. Geben Sie im Textfeld **Vornamen-Attribut** die Zeichenfolge **firstname** ein.
   3. Geben Sie im Textfeld **Nachnamen-Attribut** die Zeichenfolge **lastname** ein.
   4. Geben Sie im Textfeld **E-Mail-Attribut** die Zeichenfolge **emailaddress** ein.
   5. Klicken Sie auf **Änderungen speichern**.
9. Klicken Sie auf **Ihre IDP-Metadaten**, und führen Sie anschließend im Abschnitt **Ihre IDP-Metadaten** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden – Konfiguration](./media/active-directory-saas-policystat-tutorial/IC808635.png "Single Sign-On Configuration")
   
   1. Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **Ihre Identitätsanbietermetadaten** ein.
   2. Klicken Sie auf **Änderungen speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC771723.png "Configure Single Sign-On")
11. 1. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
    
    ![Attribute](./media/active-directory-saas-policystat-tutorial/IC795920.png "Attributes")
12. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
    
    ![Attribute](./media/active-directory-saas-policystat-tutorial/IC804823.png "Attributes")
    
    1. Klicken Sie auf **Benutzerattribut hinzufügen**.
    2. Geben Sie im Textfeld **Attributname** die Zeichenfolge **uid** ein.
    3. Wählen Sie im Textfeld **Attributwert** die Option **ExtractMailPrefix()** aus.
    4. Wählen Sie in der Liste **E-Mail** die Option **User.mail** aus.
    5. Klicken Sie auf **Fertig stellen**.
       ##<a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei PolicyStat anmelden können, müssen sie in PolicyStat bereitgestellt werden.  
PolicyStat unterstützt die bedarfsabhängige Benutzerbereitstellung. Das bedeutet, dass Sie die Benutzer nicht manuell zu PolicyStat hinzufügen müssen.  
Die Benutzer werden bei der ersten Anmeldung mittels einmaligem Anmelden automatisch hinzugefügt.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von PolicyStat-Benutzerkonten oder mithilfe der von PolicyStat bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-policystat-perform-the-following-steps"></a>So weisen Sie PolicyStat Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **PolicyStat** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-policystat-tutorial/IC808636.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-policystat-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


