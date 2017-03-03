---
title: 'Tutorial: Azure Active Directory-Integration mit Adobe EchoSign | Microsoft Docs'
description: "Erfahren Sie, wie Sie Adobe EchoSign mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/13/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7cc133d6289bffbc3b7fc591104bc51ebfc67ddd
ms.openlocfilehash: b4b215b17ff54bc04bae681fae8a7ea916bc9e3c
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Lernprogramm: Azure Active Directory-Integration mit Adobe EchoSign
In diesem Lernprogramm wird die Integration von Azure und Adobe EchoSign erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Adobe EchoSign-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Adobe EchoSign zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Adobe EchoSign-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Adobe EchoSign
2. Konfigurieren der einmaligen Anmeldung (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Szenario")

## <a name="enable-the-application-integration-for-adobe-echosign"></a>Aktivieren der Anwendungsintegration für Adobe EchoSign
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Adobe EchoSign aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Adobe EchoSign die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Adobe EchoSign** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Adobe EchoSign** aus, und klicken Sie dann auf **Fertig stellen**, um die Anwendung hinzuzufügen.
   
   ![Adobe EchoSign](./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Adobe EchoSign zu authentifizieren.

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Adobe EchoSign** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Adobe EchoSign anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Adobe EchoSign-Anmelde-URL** Ihre URL im Format „*https://company.echosign.com/*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Adobe EchoSign** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Adobe EchoSign-Unternehmenswebsite als Administrator an.
6. Klicken Sie im Menü oben auf **Account**, und klicken Sie dann im Navigationsbereich links unter **Account Settings** auf **SAML Settings**.
   
   ![Konto](./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Konto")
7. Führen Sie im Abschnitt für die SAML-Einstellungen die folgenden Schritte aus:
   
   ![SAML-Einstellungen](./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-Einstellungen")
   
   1. Wählen Sie als **SAML Mode** die Option **SAML Mandatory** aus.
   2. Aktivieren Sie **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**.
   3. Wählen Sie für **User Creation** die Option **Automatically add users authenticated through SAML** aus.
8. Fahren Sie mit den folgenden Schritten fort:
   
   ![SAML-Einstellungen](./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-Einstellungen")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adobe EchoSign** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **IdP Entity ID** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adobe EchoSign** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP Login URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Adobe EchoSign** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **IdP Logout URL** ein.
   4. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      >[!TIP]
      >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
      >  
   5. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **IdP Certificate** ein.
   6. Klicken Sie auf **Änderungen speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Einmaliges Anmelden konfigurieren")
   
   ## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Adobe EchoSign anmelden können, müssen sie in Adobe EchoSign bereitgestellt werden.  

* Im Fall von Adobe EchoSign ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer **Adobe EchoSign** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im Menü oben auf **Account**, und klicken Sie dann im Navigationsbereich links unter **Users & Groups** auf **Create a new user**.
   
   ![Konto](./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Konto")
3. Führen Sie im Abschnitt **Create New User** die folgenden Schritte aus:
   
   ![Benutzer erstellen](./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Benutzer erstellen")
   
   1. Geben Sie in die Textfelder **Email Address**, **First Name** und **Last Name** die entsprechenden Informationen eines gültigen Azure Active Directory-Benutzerkontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Benutzer erstellen**.
      
      >[!NOTE]
      >Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. 
      > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Adobe EchoSign-Benutzerkonten oder mithilfe von APIs erstellen, die von Adobe EchoSign zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Adobe EchoSign folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Adobe EchoSign** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


