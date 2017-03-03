---
title: 'Tutorial: Azure Active Directory-Integration mit Cisco Webex | Microsoft Docs'
description: "Erfahren Sie, wie Sie Cisco Webex mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: ada8ffbbdc5565a517f5a06cb640a36aaf8879b4
ms.openlocfilehash: b44b1a5b3e988a51db3325ec8a181651fa84e768
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Lernprogramm: Azure Active Directory-Integration mit Cisco Webex
In diesem Lernprogramm wird die Integration von Azure und Cisco Webex erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Cisco Webex-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Cisco Webex zugewiesen haben, mittels einmaligen Anmeldens auf der Cisco Webex-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Sie können aber auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Cisco Webex
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Szenario")

## <a name="enable-the-application-integration-for-cisco-webex"></a>Aktivieren der Anwendungsintegration für Cisco Webex
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Cisco Webex aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Cisco Webex die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Begriff **Cisco Webex** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Cisco Webex** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Cisco Webex](./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Cisco Webex zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie ein Base64-codiertes Zertifikat erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Cisco Webex** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Cisco Webex anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "App-URL konfigurieren")   
   1. Geben Sie im Textfeld **Anmelde-URL** Ihre Cisco Webex-Mandanten-URL ein (z.B. *http://contoso.webex.com*).
   2. Geben Sie im Textfeld **Cisco Webex-Antwort-URL** Ihre **Cisco Webex-AssertionConsumerService-URL** ein (z.B.: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Cisco Webex** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Cisco Webex-Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf **Site Administration**.
   
   ![Websiteverwaltung](./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Websiteverwaltung")
7. Klicken Sie im Abschnitt **Website verwalten** auf **SSO-Konfiguration**.
   
   ![SSO-Konfiguration](./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-Konfiguration")
8. Führen Sie im Abschnitt "Federated Web SSO Configuratio" die folgenden Schritte aus:
   
   ![Verbund-SSO-Konfiguration](./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Verbund-SSO-Konfiguration")  
   1. Wählen Sie in der Liste **Verbundprotokoll** die Option **SAML 2.0** aus.
   2. Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
    >[!TIP]
    >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)
    >  
   3. Öffnen Sie das Base64-codierte Zertifikat in Editor, und kopieren Sie seinen Inhalt.
   4. Klicken Sie auf **Import SAML Metadata**, und fügen Sie dann das Base64-codierte Zertifikat ein.
   5. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Cisco Webex** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **SAML-Aussteller (IdP-ID)** ein.
   6. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Cisco Webex** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **SSO-Dienstanmelde-URL des Kunden** ein.
   7. Wählen Sie in der Liste **NameID-Format** die Option **E-Mail-Adresse** aus.
   8. Geben Sie in das Textfeld **AuthnContextClassRef** die Zeichenfolge **urn:oasis:names:tc:SAML:2.0:ac:classes:Password** ein.
   9. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Cisco Webex** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **SSO-Dienstabmelde-URL des Kunden** ein.
   10. Klicken Sie auf **Aktualisieren**.
9. Wählen Sie im klassischen Azure-Portal auf der Dialogseite **Einmaliges Anmelden konfigurieren für Cisco Webex** die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Cisco Webex anmelden können, müssen sie in Cisco Webex bereitgestellt werden.  

* Im Fall von Cisco Webex ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Cisco Webex** -Mandanten an.
2. Wechseln Sie zu **Benutzer verwalten \> Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Benutzer hinzufügen")
3. Führen Sie im Abschnitt "Add User" die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Benutzer hinzufügen")   
   1. Wählen Sie unter **Kontotyp** die Option **Host** aus.
   2. Geben Sie die Informationen eines vorhandenen Azure AD-Benutzers in die folgenden Textfelder ein: **Vorname, Nachname**, **Benutzername**, **E-Mail**, **Kennwort**, **Kennwort bestätigen**.
   3. Klicken Sie auf **Hinzufügen**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Cisco Webex-Benutzerkonten oder mithilfe der von Cisco Webex bereitgestellten APIs erstellen. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Cisco Webex folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Cisco Webex** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


