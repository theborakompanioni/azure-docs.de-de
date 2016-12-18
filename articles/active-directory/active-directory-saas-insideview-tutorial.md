---
title: 'Tutorial: Azure Active Directory-Integration mit InsideView | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie InsideView mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0da7c8f20fb6bbe78294aab2fb69c24a2dddd877


---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Tutorial: Azure Active Directory-Integration mit InsideView
In diesem Tutorial wird die Integration von Azure und InsideView erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen InsideView-Mandanten

Nach Abschluss dieses Tutorials können sich die InsideView zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf der InsideView-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für InsideView
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-insideview-tutorial/IC794128.png "Scenario")

## <a name="enabling-the-application-integration-for-insideview"></a>Aktivieren der Anwendungsintegration für InsideView
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für InsideView aktivieren.

### <a name="to-enable-the-application-integration-for-insideview-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für InsideView
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-insideview-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-insideview-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-insideview-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** das Wort **InsideView** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-insideview-tutorial/IC794129.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **InsideView** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![InsideView](./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei InsideView zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **InsideView** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794131.png "Configure Single SignOn")
2. **Wählen Sie auf der Seite Wie sollen sich Benutzer bei InsideView anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794132.png "Configure Single SignOn")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **InsideView-Antwort-URL** Ihre InsideView-SSO-URL ein (z.B. `https://my.insideview.com/iv/<STS Name>/login.iv`) ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794133.png "Configure App URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für InsideView** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794134.png "Configure Single SignOn")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der InsideView-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Administrator**, **Einstellungen für einmaliges Anmelden** und dann auf **SAML hinzufügen**.
   
   ![SAML-Einstellungen für einmaliges Anmelden](./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML Single Sign On Settings")
7. Führen Sie im Abschnitt **Neue SAML hinzufügen** die folgenden Schritte aus:
   
   ![Neue SAML hinzufügen](./media/active-directory-saas-insideview-tutorial/IC794136.png "Add a New SAML")
   
   1. Geben Sie im Textfeld **STS-Name** einen Namen für die Konfiguration ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für InsideView** den Wert von **Vom Dienstanbieter initiierter Endpunkt**, und fügen Sie ihn in das Textfeld **SamlP/WS-Fed Unsolicated EndPoint** ein.
   3. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **STS-Zertifikat** ein.
   5. Geben Sie in das Textfeld **Crm User Id Mapping** (Crm User ID-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
   6. Geben Sie in das Textfeld **Crm Email Mapping** (Crm E-Mail-Adressen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
   7. Geben Sie in das Textfeld **Crm First Name Mapping** (Crm Vornamen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
   8. Geben Sie in das Textfeld **Crm lastName Mapping** (Crm Nachnamen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
   9. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794137.png "Configure Single SignOn")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei InsideView anmelden können, müssen sie in InsideView bereitgestellt werden.  
Im Fall von InsideView ist die Bereitstellung eine manuelle Aufgabe.

Wenden Sie sich an den Kundenservicemanager, oder senden Sie eine E-Mail an **support@insideview.com**

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von InsideView-Benutzerkonten oder mithilfe der von InsideView bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-insideview-perform-the-following-steps"></a>So weisen Sie InsideView Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **InsideView** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-insideview-tutorial/IC794138.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-insideview-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


