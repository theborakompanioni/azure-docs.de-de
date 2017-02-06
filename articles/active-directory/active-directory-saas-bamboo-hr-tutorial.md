---
title: 'Tutorial: Azure Active Directory-Integration mit BambooHR | Microsoft Docs'
description: "Erfahren Sie, wie Sie BambooHR mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e675b92ea53c31c014e20735c039db3dbad325cc


---
# <a name="tutorial-azure-active-directory-integration-with-bamboo-hr"></a>Lernprogramm: Azure Active Directory-Integration mit BambooHR
In diesem Lernprogramm wird die Integration von Azure und BambooHR erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein BambooHR-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die BambooHR zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer BambooHR-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für BambooHR
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")

## <a name="enabling-the-application-integration-for-bamboohr"></a>Aktivieren der Anwendungsintegration für BambooHR
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für BambooHR aktivieren.

### <a name="to-enable-the-application-integration-for-bamboohr-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für BambooHR
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-bamboo-hr-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bamboo-hr-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bamboo-hr-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bamboo-hr-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** als Suchbegriff **BambooHR** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bamboo-hr-tutorial/IC796686.png "Application gallery")
7. Wählen Sie im Ergebnisbereich **BambooHR** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![BambooHR](./media/active-directory-saas-bamboo-hr-tutorial/IC796687.png "BambooHR")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei BambooHR zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **BambooHR** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Szenario](./media/active-directory-saas-bamboo-hr-tutorial/IC796685.png "Scenario")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei BambooHR anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bamboo-hr-tutorial/IC796688.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **BambooHR-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer BambooHR-Anwendung verwendete URL ein (z.B.: https://company.bamboohr.com), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-bamboo-hr-tutorial/IC796689.png "Configure app URL")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren um BambooHR** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bamboo-hr-tutorial/IC796690.png "Configure single sign-on")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der BambooHR-Unternehmenswebsite als Administrator an.
6. Führen Sie auf der Startseite die folgenden Schritte aus:
   
   ![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/IC796691.png "Single Sign-On")
   
   1. Klicken Sie auf **Apps**.
   2. Klicken Sie im Apps-Menü links auf **Single Sign-On**.
   3. Klicken Sie auf **SAML Single Sign-On**.
7. Führen Sie im Abschnitt **SAML Single Sign-On** die folgenden Schritte aus:
   
   ![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/IC796692.png "SAML Single Sign-On")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für BambooHR** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **SSO Login URL** ein.
   2. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   3. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509 Certificate** ein.
   4. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-bamboo-hr-tutorial/IC796693.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei BambooHR anmelden können, müssen sie in BambooHR bereitgestellt werden.  
Im Fall von BambooHR ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei Ihrer **BambooHR** -Unternehmenswebsite als Administrator an.
2. Klicken Sie oben auf der Symbolleiste auf **Settings**.
   
   ![Einstellung](./media/active-directory-saas-bamboo-hr-tutorial/IC796694.png "Setting")
3. Klicken Sie auf **Overview**.
4. Wechseln Sie im linken Navigationsbereich zu **Sicherheit \> Benutzer**.
5. Geben Sie Benutzername, Kennwort und E-Mail-Adresse eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
6. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von BambooHR-Benutzerkonten oder mithilfe der von BambooHR bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-bamboohr-perform-the-following-steps"></a>So weisen Sie BambooHR Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **BambooHR** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-bamboo-hr-tutorial/IC796695.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bamboo-hr-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO4-->


