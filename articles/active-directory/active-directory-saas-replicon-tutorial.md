---
title: 'Tutorial: Azure Active Directory-Integration mit Replicon | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Replicon mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 2aeeceb61191962b62892b8409218684f76c6fa8
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Tutorial: Azure Active Directory-Integration mit Replicon
In diesem Tutorial wird die Integration von Azure und Replicon erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Replicon-Mandant

Nach Abschluss dieses Tutorials können sich die Replicon zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Replicon-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Replicon
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-replicon-tutorial/IC777798.png "Szenario")

## <a name="enable-the-application-integration-for-replicon"></a>Aktivieren der Anwendungsintegration für Replicon
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Replicon aktivieren.

**Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für Replicon zu aktivieren:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-replicon-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-replicon-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-replicon-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-replicon-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Begriff **Replicon** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-replicon-tutorial/IC777799.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Replicon** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Replicon](./media/active-directory-saas-replicon-tutorial/IC777800.png "Replicon")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Replicon zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Replicon** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-replicon-tutorial/IC777801.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Replicon anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-replicon-tutorial/IC777802.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-replicon-tutorial/IC777803.png "App-URL konfigurieren")
  1. Geben Sie im Textfeld **Replicon-Anmelde-URL** die URL Ihres Replicon-Mandanten ein (z.B. *https://na2.replicon.com/company/saml2/sp-sso/post*).
  2. Geben Sie im Textfeld **Replicon-Antwort-URL** Ihre **AssertionConsumerService**-URL für Replicon ein (z.B. *https://global.replicon.com/!/saml2/company/sso/post*).  
      
     >[!NOTE]
     >Sie können die URL aus den Replicon-Metadaten unter **https://global.replicon.com/!/saml2/\<IhrUnternehmensschlüssel\>** abrufen.
     > 
     > 
 
  3. Klicken Sie auf **Weiter**.

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Replicon** auf **Metadaten herunterladen**, und speichern Sie die Metadaten auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-replicon-tutorial/IC777804.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Replicon-Unternehmenswebsite als Administrator an.

6. Um SAML 2.0 zu konfigurieren, führen Sie die folgenden Schritte aus:
   
    ![SAML-Authentifizierung aktivieren](./media/active-directory-saas-replicon-tutorial/IC777805.png "SAML-Authentifizierung aktivieren")
  
  1. Zum Anzeigen des Dialogfelds **EnableSAML Authentication2** fügen Sie nach dem Unternehmensschlüssel Folgendes an Ihre URL an: **/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**  
    * Das folgende Beispiel zeigt das Schema der vollständigen URL:  
   **https://na2.replicon.com/\<IhrUnternehmensschlüssel\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2**
   2. Klicken Sie auf **+**, um den Abschnitt **v20Configuration** zu erweitern.
   3. Klicken Sie auf **+**, um den Abschnitt **metaDataConfiguration** zu erweitern.
   4. Klicken Sie auf **Datei auswählen**, um die Metadaten-XML-Datei Ihres Identitätsanbieters auszuwählen. Klicken Sie dann auf **Senden**.

7. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-replicon-tutorial/IC778418.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Replicon anmelden können, müssen sie in Replicon bereitgestellt werden.  

Im Fall von Replicon ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich in einem Webbrowserfenster bei der Replicon-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Verwaltung \> Benutzer**.
   
    ![Benutzer](./media/active-directory-saas-replicon-tutorial/IC777806.png "Benutzer")
3. Klicken Sie auf **+Benutzer**.
   
    ![Benutzer hinzufügen](./media/active-directory-saas-replicon-tutorial/IC777807.png "Benutzer hinzufügen")
4. Führen Sie im Abschnitt **Benutzerprofil** die folgenden Schritte aus:
   
    ![Benutzerprofil](./media/active-directory-saas-replicon-tutorial/IC777808.png "Benutzerprofil")
   
  1. Geben Sie im Textfeld **Anmeldungsname** die E-Mail-Adresse des Azure AD-Benutzers ein, den Sie bereitstellen möchten.
  2. Als **Authentifizierungstyp** wählen Sie **SSO** aus.
  3. Geben Sie in das Textfeld **Abteilung** die Abteilung des Benutzers ein.
  4. Wählen Sie als **Mitarbeitertyp** die Option **Administrator** aus.
  5. Klicken Sie auf **Benutzerprofil speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Replicon-Benutzerkonten oder mithilfe der von Replicon bereitgestellten APIs erstellen.
> 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie die folgenden Schritte aus, um Replicon Benutzer zuzuweisen:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Replicon** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-replicon-tutorial/IC777809.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-replicon-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


