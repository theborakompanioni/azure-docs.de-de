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
ms.date: 01/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 04a045f41965b093aab71e59cd9b5f328b44de84
ms.openlocfilehash: 8290d90f22a80d5c585192b17cfa0aeba00aa891


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

![Szenario](./media/active-directory-saas-policystat-tutorial/IC808662.png "Szenario")

## <a name="enabling-the-application-integration-for-policystat"></a>Aktivieren der Anwendungsintegration für PolicyStat
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für PolicyStat aktivieren.

**Um die Anwendungsintegration für PolicyStat zu aktivieren, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-policystat-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-policystat-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-policystat-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **PolicyStat** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-policystat-tutorial/IC808627.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich die Option **PolicyStat** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![PolicyStat](./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei PolicyStat zu authentifizieren.  

Die PolicyStat-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie der Konfiguration Ihrer **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  

Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen.

![Attribute](./media/active-directory-saas-policystat-tutorial/IC808628.png "Attribute")

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **PolicyStat** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808629.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei PolicyStat anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808630.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Textfeld für die **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer PolicyStat-Anwendung verwendete URL ein (beispielsweise „*https://demo-azure.policystat.com*“), und klicken Sie dann auf **Weiter**.
   
   ![App-Einstellungen konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808631.png "App-Einstellungen konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für PolicyStat** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC808632.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der PolicyStat-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf die Registerkarte **Administrator** und anschließend links im Navigationsbereich auf **Einmaliges Anmelden – Konfiguration**.
   
   ![Menü „Administrator“](./media/active-directory-saas-policystat-tutorial/IC808633.png "Menü „Administrator“")
7. Wählen Sie im Abschnitt **Einrichtung** die Option **Integration des einmaligen Anmeldens aktivieren** aus.
   
   ![Konfiguration für einmaliges Anmelden](./media/active-directory-saas-policystat-tutorial/IC808634.png "Konfiguration für einmaliges Anmelden")
8. Klicken Sie auf **Attribute konfigurieren**, und führen Sie anschließend im Abschnitt **Attribute konfigurieren** die folgenden Schritte aus:
   
   ![Konfiguration für einmaliges Anmelden](./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einmaliges Anmelden")
   
   1. Geben Sie im Textfeld **Benutzernamen-Attribut** die Zeichenfolge **uid** ein.
   2. Geben Sie im Textfeld **Vornamen-Attribut** die Zeichenfolge **firstname** ein.
   3. Geben Sie im Textfeld **Nachnamen-Attribut** die Zeichenfolge **lastname** ein.
   4. Geben Sie im Textfeld **E-Mail-Attribut** die Zeichenfolge **emailaddress** ein.
   5. Klicken Sie auf **Änderungen speichern**.
9. Klicken Sie auf **Ihre IDP-Metadaten**, und führen Sie anschließend im Abschnitt **Ihre IDP-Metadaten** die folgenden Schritte aus:
   
   ![Konfiguration für einmaliges Anmelden](./media/active-directory-saas-policystat-tutorial/IC808635.png "Konfiguration für einmaliges Anmelden")
   
   1. Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **Ihre Identitätsanbietermetadaten** ein.
   2. Klicken Sie auf **Änderungen speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-policystat-tutorial/IC771723.png "Einmaliges Anmelden konfigurieren")
  
11. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
    
    ![Attribute](./media/active-directory-saas-policystat-tutorial/IC795920.png "Attribute")
12. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
    
    ![Attribute](./media/active-directory-saas-policystat-tutorial/IC804823.png "Attribute")
    
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

**Um PolicyStat Benutzer zuzuweisen, führen Sie die folgenden Schritte durch:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **PolicyStat** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-policystat-tutorial/IC808636.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-policystat-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO1-->


