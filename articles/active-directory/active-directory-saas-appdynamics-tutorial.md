---
title: 'Tutorial: Azure Active Directory-Integration mit AppDynamics | Microsoft Docs'
description: "Erfahren Sie, wie Sie AppDynamics mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 25fd1df0-411c-4f55-8be3-4273b543100f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 6d2c8c8170562f398ae0dbb7339b0e9bfce86313
ms.openlocfilehash: 1feed4aac0241096fcccd715e01418ff86b8c06d
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Lernprogramm: Azure Active Directory-Integration mit AppDynamics
In diesem Lernprogramm wird die Integration von Azure und AppDynamics erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein AppDynamics-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie AppDynamics zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer AppDynamics-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für AppDynamics
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Szenario")

## <a name="enabling-the-application-integration-for-appdynamics"></a>Aktivieren der Anwendungsintegration für AppDynamics
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für AppDynamics aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für AppDynamics die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **AppDynamics** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **AppDynamics** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![AppDynamics](./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei AppDynamics zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **AppDynamics** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **How would you like users to sign on to AppDynamics?** (Wie sollen sich Benutzer bei AppDynamics anmelden?) die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **AppDynamics Sign On URL** (AppDynamics-Anmelde-URL) die von Ihren Benutzern für die Anmeldung bei AppDynamics verwendete URL ein (z.B. *https://companyname.saas.appdynamics.com*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-appdynamics-tutorial/IC790214.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Configure single sign-on at AppDynamics** (Einmaliges Anmelden bei AppDynamics konfigurieren) auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der AppDynamics-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Einstellungen**, und klicken Sie dann auf **Verwaltung**.
   
   ![Verwaltung](./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Verwaltung")
7. Klicken Sie auf die Registerkarte **Authentication Provider** .
   
   ![Authentifizierungsanbieter](./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Authentifizierungsanbieter")
8. Führen Sie im Abschnitt **Authentication Provider** die folgenden Schritte aus:
   
   ![SAML-Konfiguration](./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML-Konfiguration")   
   1. Wählen Sie als **Authentication Provider** (Authentifizierungsanbieter) die Option **SAML** aus.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at AppDynamics** den Wert für **Remote Login URL** (Remoteanmelde-URL), und fügen Sie ihn ins Textfeld **Login URL** (Anmelde-URL) ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at AppDynamics** den Wert für **Remote Logout URL** (Remoteabmelde-URL), und fügen Sie ihn ins Textfeld **Logout URL** (Abmelde-URL) ein.
   4. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
   
      >[!TIP]
      >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
      > 
   5. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat** ein.
   6. Klicken Sie auf **Speichern**.

     ![Speichern](./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Speichern")
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei AppDynamics anmelden können, müssen sie in AppDynamics bereitgestellt werden.  
Im Fall von AppDynamics ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der AppDynamics-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Benutzer**, und klicken Sie auf das Symbol **+**, um das Dialogfeld **Benutzer erstellen** zu öffnen.
   
   ![Benutzer](./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Benutzer")
3. Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:
   
   ![Benutzer erstellen](./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Benutzer erstellen")
   
   1. Geben Sie die Textfelder **Username** (Benutzername), **Name** (Name), **Email** (E-Mail), **New Password** (Neues Kennwort) und **Repeat New Password** (Neues Kennwort wiederholen) die entsprechenden Informationen eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von AppDynamics-Benutzerkonten oder mithilfe der von AppDynamics bereitgestellten APIs erstellen.
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in AppDynamics folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **AppDynamics** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


