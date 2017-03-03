---
title: 'Tutorial: Azure Active Directory-Integration mit AirWatch | Microsoft Docs'
description: "Erfahren Sie, wie Sie AirWatch mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 8bb5433fb8d81635185d8bfc765ea240461e8a33
ms.openlocfilehash: 4659531575e21823441eb75905b79d79592fa695
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Lernprogramm: Azure Active Directory-Integration mit AirWatch
In diesem Lernprogramm wird die Integration von Azure und AirWatch erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein AirWatch-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die AirWatch zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer AirWatch-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Sie können aber auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für AirWatch
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")

## <a name="enable-the-application-integration-for-airwatch"></a>Aktivieren der Anwendungsintegration für AirWatch
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für AirWatch aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für AirWatch die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-airwatch-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-airwatch-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-airwatch-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **AirWatch** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-airwatch-tutorial/IC791914.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **AirWatch** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![AirWatch](./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei AirWatch zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei erstellen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **AirWatch** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/IC791916.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei AirWatch anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/IC791917.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **AirWatch-Anmelde-URL** die von Ihren Benutzern zur Anmeldung bei der AirWatch-Anwendung verwendete URL ein (z.B. *https:// unternehmenscode.awmdm.com/AirWatch/Login?gid=unternehmenscode*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-airwatch-tutorial/IC791918.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren um AirWatch** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/IC791919.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der AirWatch-Unternehmenswebsite als Administrator an.
6. Klicken Sie im linken Navigationsbereich auf **Konten**, und klicken Sie dann auf **Administratoren**.
   
   ![Administratoren](./media/active-directory-saas-airwatch-tutorial/IC791920.png "Administratoren")
7. Erweitern Sie das Menü **Einstellungen**, und klicken Sie dann auf **Verzeichnisdienste**.
   
   ![Einstellungen](./media/active-directory-saas-airwatch-tutorial/IC791921.png "Einstellungen")
8. Klicken Sie auf die Registerkarte **Benutzer**, geben Sie im Textfeld **Basis-DN** Ihren Domänennamen ein, und klicken Sie dann auf **Speichern**.
   
   ![Benutzer](./media/active-directory-saas-airwatch-tutorial/IC791922.png "Benutzer")
9. Klicken Sie auf die Registerkarte **Server** .
   
   ![Server](./media/active-directory-saas-airwatch-tutorial/IC791923.png "Server")
10. Führen Sie die folgenden Schritte aus:
    
   ![Hochladen](./media/active-directory-saas-airwatch-tutorial/IC791924.png "Hochladen")    
    1. Wählen Sie unter **Verzeichnistyp** die Option **Keiner** aus.
    2. Aktivieren Sie **Use SAML For Authentication**.
    3. Klicken Sie auf **Upload**, um das heruntergeladene Zertifikat hochzuladen.
11. Führen Sie im Abschnitt **Request** die folgenden Schritte aus:
    
    ![Anforderung](./media/active-directory-saas-airwatch-tutorial/IC791925.png "Anforderung")  
    1. Wählen Sie als **Anforderungsbindungstyp** die Option **POST** aus.
    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Airwatch** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **SSO-URL des Identitätsanbieters** ein.
    3. Wählen Sie als **NameID-Format** die Option **E-Mail-Adresse** aus.
    4. Klicken Sie auf **Speichern**.
12. Klicken Sie erneut auf die Registerkarte **User** .
    
    ![Benutzer](./media/active-directory-saas-airwatch-tutorial/IC791926.png "Benutzer")
13. Führen Sie im Abschnitt **Attribute** die folgenden Schritte aus:
    
    ![Attribut](./media/active-directory-saas-airwatch-tutorial/IC791927.png "Attribut")
    1. Geben Sie im Textfeld **Objektbezeichner** die URL **http://schemas.microsoft.com/identity/claims/objectidentifier** ein.
    2. Geben Sie in das Textfeld **Benutzername** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3. Geben Sie in das Textfeld **Anzeigename** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4. Geben Sie in das Textfeld **Vorname** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5. Geben Sie in das Textfeld **Nachname** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.
    6. Geben Sie in das Textfeld **E-Mail** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7. Klicken Sie auf **Speichern**.
14. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/IC791928.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei AirWatch anmelden können, müssen sie in AirWatch bereitgestellt werden.

* Im Fall von AirWatch ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der **AirWatch** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im Navigationsbereich links auf **Konten**, und klicken Sie dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-airwatch-tutorial/IC791929.png "Benutzer")
3. Klicken Sie im Menü **Benutzer** auf **Listenansicht**, und klicken Sie dann auf **Hinzufügen \> Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-airwatch-tutorial/IC791930.png "Benutzer hinzufügen")
4. Führen Sie im Dialogfeld **Add / Edit User** die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-airwatch-tutorial/IC791931.png "Benutzer hinzufügen")   
   1. Geben Sie in die Textfelder **Benutzername**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname** und **E-Mail-Adresse** die Informationen eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von AirWatch-Benutzerkonten oder mithilfe der von AirWatch bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in AirWatch folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **AirWatch** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-airwatch-tutorial/IC791932.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-airwatch-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


