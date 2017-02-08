---
title: 'Tutorial: Azure Active Directory-Integration mit Kintone | Microsoft Docs'
description: "Erfahren Sie, wie Sie Kintone mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: c2b947dc-e1a8-4f5f-b40e-2c5180648e4f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/18/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7a25e030e8ade95db2ac9cd58fe4d94bbb7775ba


---
# <a name="tutorial-azure-active-directory-integration-with-kintone"></a>Tutorial: Azure Active Directory-Integration mit Kintone
In diesem Tutorial wird die Integration von Azure und Kintone erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Kintone-Software-Abonnement, für das das einmalige Anmelden aktiviert ist.

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Kintone zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Kintone-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Kintone
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-kintone-tutorial/IC785859.png "Scenario")

## <a name="enabling-the-application-integration-for-kintone"></a>Aktivieren der Anwendungsintegration für Kintone
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Kintone aktivieren.

### <a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Führen Sie zum Aktivieren der Anwendungsintegration für Kintone die folgenden Schritte aus:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-kintone-tutorial/IC700994.png "Applications")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-kintone-tutorial/IC749321.png "Add application")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-kintone-tutorial/IC749322.png "Add an application from gallerry")

6. Geben Sie im **Suchfeld** als Suchbegriff **Kintone** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-kintone-tutorial/IC785867.png "Application Gallery")

7. Wählen Sie im Ergebnisbereich **Kintone** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Kintone](./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Kintone zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Kintone** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kintone-tutorial/IC785872.png "Configure Single Sign-On")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Kintone anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kintone-tutorial/IC785873.png "Configure Single Sign-On")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Kintone-Anmelde-URL** Ihre URL im Format *https://unternehmen.kintone.com* ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-kintone-tutorial/IC785875.png "Configure App URL")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Kintone** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kintone-tutorial/IC785878.png "Configure Single Sign-On")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der **Kintone** -Unternehmenswebsite als Administrator an.

6. Klicken Sie auf **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-kintone-tutorial/IC785879.png "Settings")

7. Klicken Sie auf **Benutzer und Systemadministration**.
   
    ![Benutzer und Systemadministration](./media/active-directory-saas-kintone-tutorial/IC785880.png "Users & System Administration")

8. Klicken Sie unter **Systemadministration \> Sicherheit** auf **Anmelden**.
   
    ![Anmeldung](./media/active-directory-saas-kintone-tutorial/IC785881.png "Login")

9. Klicken Sie auf **SAML-Authentifizierung aktivieren**.
   
    ![SAML-Authentifizierung](./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML Authentication")

10. Führen Sie im Abschnitt für die SAML-Authentifizierung die folgenden Schritte aus:
    
    ![SAML-Authentifizierung](./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML Authentication")
    
    1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Kintone** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   
    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Kintone** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
    
    3. Klicken Sie auf **Durchsuchen** , um das heruntergeladene Zertifikat hochzuladen.
    
    4. Klicken Sie auf **Speichern**.

11. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kintone-tutorial/IC785884.png "Configure Single Sign-On")
    
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei Kintone anmelden können, müssen sie in Kintone bereitgestellt werden.  
Im Fall von Kintone ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei der **Kintone** -Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Einstellung**.
   
    ![Einstellungen](./media/active-directory-saas-kintone-tutorial/IC785879.png "Settings")

3. Klicken Sie auf **Benutzer und Systemadministration**.
   
    ![Benutzer und Systemadministration](./media/active-directory-saas-kintone-tutorial/IC785880.png "User & System Administration")

4. Klicken Sie unter **Benutzerverwaltung** auf **Abteilungen und Benutzer**.
   
    ![Abteilung und Benutzer](./media/active-directory-saas-kintone-tutorial/IC785888.png "Department & Users")

5. Klicken Sie auf **Neuer Benutzer**.
   
    ![Neue Benutzer](./media/active-directory-saas-kintone-tutorial/IC785889.png "New Users")

6. Führen Sie im Abschnitt **Neuer Benutzer** die folgenden Schritte aus:
   
    ![Neue Benutzer](./media/active-directory-saas-kintone-tutorial/IC785890.png "New Users")
   
    1. Geben Sie **Anzeigename**, **Anmeldename**, **Neues Kennwort**, **Kennwort bestätigen**, **E-Mail-Adresse** und weitere Details eines gültigen AAD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
 
    2. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Kintone-Benutzerkonten oder mithilfe der von Kintone bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-kintone-perform-the-following-steps"></a>So weisen Sie Kintone Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Kintone** auf **Benutzer zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-kintone-tutorial/IC785891.png "Assign Users")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-kintone-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


