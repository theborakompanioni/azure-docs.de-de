---
title: 'Tutorial: Azure Active Directory-Integration mit BlueJeans| Microsoft Docs'
description: "Erfahren Sie, wie Sie BlueJeans mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: e8df27b17ac0e839efdd302ffa40fafe688a22b4
ms.openlocfilehash: aaa1c06da8b53a45a1e675f175dd3adcee20e910
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-ad-integration-with-bluejeans"></a>Lernprogramm: Azure AD-Integration mit BlueJeans
In diesem Lernprogramm wird die Integration von Azure und BlueJeans erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein BlueJeans-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die BlueJeans zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer BlueJeans-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für BlueJeans
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Szenario")

## <a name="enable-the-application-integration-for-bluejeans"></a>Aktivieren der Anwendungsintegration für BlueJeans
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für BlueJeans aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für BlueJeans die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **BlueJeans** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **BlueJeans** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![BlueJeans](./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei BlueJeans zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **BlueJeans** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **How would you like users to sign on to BlueJeans?** (Wie sollen sich Benutzer bei BlueJeans anmelden?) die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **BlueJeans-Anmelde-URL** Ihre URL im Format *https://company.BlueJeans.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-bluejeans-tutorial/IC785865.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Configure single sign-on at BlueJeans** (Einmaliges Anmelden bei BlueJeans konfigurieren) auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der **BlueJeans** -Unternehmenswebsite als Administrator an.
6. Wechseln Sie zu **ADMIN (Administrator) \> Group Settings (Gruppeneinstellungen) \> Security (Sicherheit)**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")
7. Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden für SAML](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "Einmaliges Anmelden für SAML")   
   1. Wählen Sie **SAML Single Sign On**aus.
   2. Wählen Sie **Enable automatic provisioning**aus.
8. Führen Sie die folgenden Schritte aus:
   
   ![Zertifikatpfad](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Zertifikatpfad")
   
   1. Klicken Sie auf **Datei auswählen**, und laden Sie dann das heruntergeladene Zertifikat hoch.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at BlueJeans** den Wert für **Remote Login URL** (Remoteanmelde-URL), und fügen Sie ihn ins Textfeld **Login URL** (Anmelde-URL) ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at BlueJeans** den Wert für **Change Password URL** (Kennwort-URL ändern), und fügen Sie ihn in das Textfeld **Password Change URL** (Änderung der Kennwort-URL) ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at BlueJeans** den Wert für **Remote Logout URL** (Remoteabmelde-URL), und fügen Sie ihn ins Textfeld **Logout URL** (Abmelde-URL) ein.
9. Führen Sie die folgenden Schritte aus:
   
   ![Änderungen speichern](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Änderungen speichern")
   1. Geben Sie ins Text **User ID** (Benutzer-ID) **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name** ein.
   2. Geben Sie ins Text **Email** (E-Mail) **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name** ein.
   3. Klicken Sie auf **Änderungen speichern**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei BlueJeans anmelden können, müssen sie in BlueJeans bereitgestellt werden.  

* Im Fall von BlueJeans ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der **BlueJeans** -Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **ADMIN (Administrator) \> Manage Users (Benutzer verwalten) \> Add User (Benutzer hinzufügen)**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")
   
   >[!IMPORTANT]
   >Die Registerkarte **Add User** ist nur verfügbar, wenn auf der Registerkarte **Security** (Sicherheit) die Option **Enable automatic provisioning** (Automatische Bereitstellung aktivieren) deaktiviert ist. 
   > 
3. Führen Sie im Abschnitt **Add User** die folgenden Schritte aus:
   
  ![Benutzer hinzufügen](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Benutzer hinzufügen")
   
  1. Geben Sie für die Bereitstellung einen **BlueJeans-Benutzernamen**, eine **E-Mail-Adresse**, eine **BlueJeans-Meeting-ID**, eine **Moderatorenkennung**, den **vollständigen Namen** und das **Unternehmen** eines gültigen AAD-Kontos in die entsprechenden Textfelder ein.
  2. Klicken Sie auf **Benutzer hinzufügen**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von BlueJeans-Benutzerkonten oder mithilfe von APIs erstellen, die von BlueJeans zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in BlueJeans folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **BlueJeans** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


