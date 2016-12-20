---
title: 'Tutorial: Azure Active Directory-Integration mit Jitbit Helpdesk | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Jitbit Helpdesk mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 15ce27d4-0621-4103-8a34-e72c98d72ec3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: acffb1e0b3f1ac05034d130ca3a24246154a0cc2


---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Tutorial: Azure Active Directory-Integration mit Jitbit Helpdesk
In diesem Tutorial wird die Integration von Azure und Jitbit Helpdesk erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Jitbit Helpdesk-Mandanten

Nach Abschluss dieses Tutorials können sich die Jitbit Helpdesk zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf der Jitbit Helpdesk-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Jitbit Helpdesk
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777676.png "Scenario")

## <a name="enabling-the-application-integration-for-jitbit-helpdesk"></a>Aktivieren der Anwendungsintegration für Jitbit Helpdesk
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Jitbit Helpdesk aktivieren.

### <a name="to-enable-the-application-integration-for-jitbit-helpdesk-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Jitbit Helpdesk
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie im **Suchfeld** **Jitbit Helpdesk** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777677.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **Jitbit Helpdesk** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![JitBit](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC781008.png "JitBit")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Jitbit Helpdesk zu authentifizieren.  verfügbar.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache) weitere Informationen.

> [!IMPORTANT]
> Um einmaliges Anmelden für Ihren Jitbit Helpdesk-Mandanten konfigurieren zu können, müssen Sie sich zunächst an den technischen Support von Jitbit Helpdesk wenden und dieses Feature aktivieren lassen.
> 
> 

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Jitbit Helpdesk** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777678.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Jitbit Helpdesk anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777679.png "Configure single sign-on")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Jitbit Helpdesk-Anmelde-URL** die URL im Format „*https://\<tenant-name\>.Jitbit.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777528.png "Configure app URL")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Jitbit Helpdesk** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal unter **c:\\Jitbit Helpdesk.cer**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777680.png "Configure single sign-on")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Jitbit Helpdesk-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Verwaltung**.
   
   ![Verwaltung](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")
7. Klicken Sie auf **Allgemeine Einstellungen**.
   
   ![Benutzer, Unternehmen und Berechtigungen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")
8. Führen Sie im Konfigurationsabschnitt **Authentifizierungseinstellungen** die folgenden Schritte aus:
   
   ![Authentifizierungseinstellungen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777683.png "Authentication settings")
   
   1. Wählen Sie **Einmaliges Anmelden mit SAML 2.0 aktivieren** mit SSO mit **OneLogin** aus.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Jitbit Helpdesk** den Wert von **Vom Dienstanbieter initiierter Endpunkt**, und fügen Sie ihn in das Textfeld **EndPoint URL** ein.
   3. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.
      
      > [!TIP]
      > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      > 
      > 
   4. Öffnen Sie das Base-64-codierte Zertifikat, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
   5. Klicken Sie auf **Änderungen speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Configure single sign-on](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777684.png "Configure single sign-on")
   
   ## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Jitbit Helpdesk anmelden können, müssen sie in Jitbit Helpdesk bereitgestellt werden.  
Im Fall von Jitbit Helpdesk ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>So stellen Sie Benutzerkonten bereit
1. Melden Sie sich bei Ihrem **Jitbit Helpdesk** -Mandanten an.
2. Klicken Sie oben im Menü auf **Verwaltung**.
   
   ![Verwaltung](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777681.png "Administration")
3. Klicken Sie auf **Benutzer, Unternehmen und Berechtigungen**.
   
   ![Benutzer, Unternehmen und Berechtigungen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777682.png "Users, companies and permissions")
4. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777685.png "Add user")
5. Geben Sie im Abschnitt „Erstellen“ die Daten des Azure AD-Kontos, das Sie bereitstellen möchten, in die Textfelder für **Benutzername**, **E-Mail**, **Vorname** und **Nachname** ein.
   
   ![Erstellen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777686.png "Create")
6. Klicken Sie auf **Erstellen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Jitbit Helpdesk-Benutzerkonten oder mithilfe der von Jitbit Helpdesk bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-jitbit-helpdesk-perform-the-following-steps"></a>So weisen Sie Jitbit Helpdesk Benutzer zu
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Jitbit Helpdesk** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC777687.png "Assign users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-jitbit-helpdesk-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


