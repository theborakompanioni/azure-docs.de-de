---
title: 'Tutorial: Azure Active Directory-Integration mit Pagerduty | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Pagerduty mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: a7fae61f9b2dc400cbbd7054c11ec622f8683ccb
ms.lasthandoff: 04/03/2017


---

# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Tutorial: Azure Active Directory-Integration mit Pagerduty
In diesem Tutorial wird die Integration von Azure und Pagerduty erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Pagerduty-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Pagerduty zugewiesen haben, mittels einmaliger Anmeldung (SSO) auf der Pagerduty-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Pagerduty
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-pagerduty-tutorial/IC778528.png "Szenario")

## <a name="enabling-the-application-integration-for-pagerduty"></a>Aktivieren der Anwendungsintegration für Pagerduty
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Pagerduty aktivieren.

**Um die Anwendungsintegration für Pagerduty zu aktivieren, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im linken Navigationsbereich des Azure-Verwaltungsportals auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-pagerduty-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-pagerduty-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **Pagerduty** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-pagerduty-tutorial/IC778529.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Pagerduty** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![PagerDuty](./media/active-directory-saas-pagerduty-tutorial/IC778530.png "PagerDuty")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Pagerduty zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  

Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Pagerduty** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778531.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Pagerduty anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778532.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Pagerduty-Anmelde-URL** die URL im Format „*https://\<tenant-name\>.Pagerduty.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778533.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Pagerduty** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778534.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Pagerduty-Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf **Kontoeinstellungen**.
   
   ![Konteneinstellungen](./media/active-directory-saas-pagerduty-tutorial/IC778535.png "Konteneinstellungen")
7. Klicken Sie auf **Einmaliges Anmelden**.
   
   ![Einmaliges Anmelden](./media/active-directory-saas-pagerduty-tutorial/IC778536.png "Einmaliges Anmelden")
8. Führen Sie auf der Seite „Einmaliges Anmelden aktivieren“ die folgenden Schritte aus:
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-pagerduty-tutorial/IC778537.png "Einmaliges Anmelden aktivieren")
   
   1. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
      >[!TIP]
      >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
      >

  2. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Pagerduty** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Login URL** ein.
  4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Pagerduty** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Logout URL** ein.
  5. Wählen Sie **Einmaliges Anmelden aktivieren**.
  6. Klicken Sie auf **Änderungen speichern**.

9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pagerduty-tutorial/IC778538.png "Einmaliges Anmelden konfigurieren")
   

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Pagerduty anmelden können, müssen sie in Pagerduty bereitgestellt werden.  

* Im Fall von Pagerduty ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Pagerduty** -Mandanten an.
2. Klicken Sie im oberen Menü auf **Benutzer**.
3. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-pagerduty-tutorial/IC778539.png "Benutzer hinzufügen")
4. Geben Sie im Dialogfeld **Team einladen** den **Vor- und Nachnamen** und die **E-Mail-Adresse** des Azure AD-Benutzers ein, den Sie bereitstellen möchten, und klicken Sie auf **Hinzufügen** und anschließend auf **Einladungen senden**.
   
   ![Ihr Team einladen](./media/active-directory-saas-pagerduty-tutorial/IC778540.png "Ihr Team einladen")
   
   >[!NOTE]
   >Alle hinzugefügten Benutzer erhalten eine Einladung zum Erstellen eines PagerDuty-Kontos.
   > 
   > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Pagerduty-Benutzerkonten oder mithilfe der von Pagerduty bereitgestellten APIs erstellen.
> 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Um Pagerduty Benutzer zuzuweisen, führen Sie die folgenden Schritte durch:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Pagerduty** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-pagerduty-tutorial/IC778541.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-pagerduty-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

