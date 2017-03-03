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
ms.date: 02/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 48a8cb0afd9fc2f9201169b074cf10f6d910952f
ms.openlocfilehash: 235b1f11c60a595dfc760701213cfc1b5f792b61
ms.lasthandoff: 02/17/2017


---

# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Tutorial: Azure Active Directory-Integration mit InsideView
In diesem Tutorial wird die Integration von Azure und InsideView erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen InsideView-Mandanten

Nach Abschluss dieses Tutorials können sich die InsideView zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf der InsideView-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für InsideView
* Konfigurieren der einmaligen Anmeldung
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-insideview-tutorial/IC794128.png "Szenario")

## <a name="enable-the-application-integration-for-insideview"></a>Aktivieren der Anwendungsintegration für InsideView
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für InsideView aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für InsideView die folgenden Schritte aus:**
 
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-insideview-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-insideview-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-insideview-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-insideview-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **InsideView** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-insideview-tutorial/IC794129.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **InsideView** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![InsideView](./media/active-directory-saas-insideview-tutorial/IC794130.png "InsideView")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei InsideView zu authentifizieren.  
 
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **InsideView** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794131.png "Einmaliges Anmelden konfigurieren")
2. **Wählen Sie auf der Seite Wie sollen sich Benutzer bei InsideView anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794132.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **InsideView-Antwort-URL** Ihre InsideView-SSO-URL ein (z.B. `https://my.insideview.com/iv/<STS Name>/login.iv`) ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794133.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für InsideView** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794134.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der InsideView-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Administrator**, **Einstellungen für einmaliges Anmelden** und dann auf **SAML hinzufügen**.
   
   ![SAML-Einstellungen für einmaliges Anmelden](./media/active-directory-saas-insideview-tutorial/IC794135.png "SAML-Einstellungen für einmaliges Anmelden")
7. Führen Sie im Abschnitt **Neue SAML hinzufügen** die folgenden Schritte aus:
   
   ![Neue SAML hinzufügen](./media/active-directory-saas-insideview-tutorial/IC794136.png "Neue SAML hinzufügen")
   
   1. Geben Sie im Textfeld **STS-Name** einen Namen für die Konfiguration ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für InsideView** den Wert von **Vom Dienstanbieter initiierter Endpunkt**, und fügen Sie ihn in das Textfeld **SamlP/WS-Fed Unsolicated EndPoint** ein.
   3. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.      

     >[!TIP]
     >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache). 
     >
   4. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **STS-Zertifikat** ein.
   5. Führen Sie folgende Schritte aus:
    * Geben Sie in das Textfeld **Crm User Id Mapping** (Crm User ID-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    * Geben Sie in das Textfeld **Crm Email Mapping** (Crm E-Mail-Adressen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    * Geben Sie in das Textfeld **Crm First Name Mapping** (Crm Vornamen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    * Geben Sie in das Textfeld **Crm lastName Mapping** (Crm Nachnamen-Zuordnung) Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
   9. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-insideview-tutorial/IC794137.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei InsideView anmelden können, müssen sie in InsideView bereitgestellt werden. Im Fall von InsideView ist die Bereitstellung eine manuelle Aufgabe.

Wenden Sie sich an den Kundenservicemanager, oder senden Sie eine E-Mail an **support@insideview.com**

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von InsideView-Benutzerkonten oder mithilfe der von InsideView bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in InsideView folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **InsideView** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-insideview-tutorial/IC794138.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-insideview-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


