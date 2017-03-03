---
title: 'Tutorial: Azure Active Directory-Integration mit SkyDesk Email | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SkyDesk Email konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 04a045f41965b093aab71e59cd9b5f328b44de84
ms.openlocfilehash: fcea07a412de7b35931ff95b01fbe1276302f1ea
ms.lasthandoff: 02/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Tutorial: Azure Active Directory-Integration mit SkyDesk Email
Dieses Tutorial soll Ihnen zeigen, wie Sie SkyDesk Email in Azure Active Directory (Azure AD) integrieren können.

Die Integration von SkyDesk Email in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf SkyDesk Email zugreifen kann.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SkyDesk Email anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten: im klassischen Azure Active Directory-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit SkyDesk Email konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SkyDesk Email-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SkyDesk Email aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-skydesk-email-from-the-gallery"></a>Hinzufügen von SkyDesk Email aus dem Katalog
Zum Konfigurieren der Integration von SkyDesk Email in Azure AD müssen Sie SkyDesk Email aus dem Katalog zu Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um SkyDesk Email aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **SkyDesk Email**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)
7. Wählen Sie im Ergebnisbereich **SkyDesk Email** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens „Britta Simon“ veranschaulicht werden, wie das einmalige Anmelden von Azure AD in SkyDesk Email konfiguriert und getestet werden kann.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SkyDesk Email als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SkyDesk Email muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in SkyDesk Email zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit SkyDesk Email sind die folgenden Bausteine erforderlich:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SkyDesk Email-Testbenutzers](#creating-a-Skydesk-Email-test-user)**, um ein Gegenstück zu Britta Simon in SkyDesk Email zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer SkyDesk Email-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SkyDesk Email die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SkyDesk Email** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SkyDesk Email anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 

   1. Geben Sie im Textfeld „Anmelde-URL“ die von Ihren Benutzern für die Anmeldung bei Ihrer SkyDesk Email-Anwendung verwendete URL in folgendem Format ein: **https://mail.skydesk.jp/portal/\<Unternehmensname\>**.
   2. Klicken Sie auf **Weiter**.

4. Führen Sie auf der Seite **Einmaliges Anmelden bei SkyDesk Email konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
    b. Klicken Sie auf **Weiter**.
5. Führen Sie die folgenden Schritte aus, um das einmalige Anmelden in **SkyDesk Email** zu aktivieren:
   1. Melden Sie sich als Administrator bei Ihrem SkyDesk Email-Konto an.
   2. Klicken Sie im oberen Menü auf „Setup“, und wählen Sie „Org“ aus. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)  
   3. Klicken Sie im linken Bereich auf „Domains“.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)
   4. Klicken Sie auf „Add Domain“.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)
   5. Geben Sie den Namen Ihrer Domäne ein, und überprüfen Sie die Domäne dann.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)
   6. Klicken Sie im linken Bereich auf **SAML Authentication** .
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)
6. Führen Sie auf der Dialogseite **SAML Authentication** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
   > [!NOTE]
   > Um die SAML-basierte Authentifizierung zu verwenden, sollten Sie entweder eine **überprüfte Domäne** oder eine **Portal-URL** eingerichtet haben. Sie können die Portal-URL mit dem eindeutigen Namen einrichten.
   > 
   > 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

   1. Kopieren Sie im klassischen Azure AD-Portal den Wert **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   2. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Dienst-URL für einmalige Abmeldung**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
   3. **Change Password URL** ist optional, lassen Sie dieses Feld leer.
   4. Klicken Sie auf **Schlüssel aus Datei abrufen**, um das heruntergeladene SkyDesk Email-Zertifikat auszuwählen, und klicken Sie dann auf **Öffnen**, um das Zertifikat hochzuladen.
   5. Wählen Sie als **Algorithmus** den Wert **RSA** aus.
   6. Klicken Sie zum Speichern der Änderungen auf **OK** .

7. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 
   
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein. 
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.  
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 
   
   1. Notieren Sie den Wert von **Neues Kennwort**.
   2. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-skydesk-email-test-user"></a>Erstellen eines SkyDesk Email-Testbenutzers
In diesem Abschnitt erstellen Sie in SkyDesk Email einen Benutzer mit dem Namen Britta Simon.

1. Klicken Sie in SkyDesk Email im linken Bereich auf **User Access**, und geben Sie Ihren Benutzernamen ein. 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Wenn Sie mehrere Benutzer erstellen müssen, setzen Sie sich mit dem Supportteam von SkyDesk Email in Verbindung.
>


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf SkyDesk Email erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon SkyDesk Email zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **SkyDesk Email** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SkyDesk Email“ klicken, sollten Sie automatisch bei Ihrer SkyDesk Email-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png

