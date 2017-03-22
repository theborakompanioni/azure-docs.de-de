---
title: 'Tutorial: Azure Active Directory-Integration mit 23 Video | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und 23 Video konfigurieren."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: a59a0782176f5221bb8b2f590faeee660f4d1101
ms.openlocfilehash: 2ebc4571cb7ff763449f192f5140c79a65d69833
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Tutorial: Azure Active Directory-Integration mit 23 Video
Dieses Tutorial soll Ihnen zeigen, wie Sie 23 Video in Azure Active Directory (Azure AD) integrieren können.

Die Integration von 23 Video in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer Zugriff auf 23 Video hat. 
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei 23 Video mit einmaligem Anmelden (Single Sign-On, SSO) anzumelden.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit 23 Video konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein 23 Video-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von 23 Video aus dem Katalog 
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="adding-23-video-from-the-gallery"></a>Hinzufügen von 23 Video aus dem Katalog
Zum Konfigurieren der Integration von 23 Video in Azure AD müssen Sie 23 Video aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um 23 Video aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **23 Video**ein.
   
    ![Anwendungen][5]
7. Wählen Sie im Ergebnisbereich **23 Video** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Anwendungen][25]

## <a name="configuring-and-testing-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in 23 Video konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in 23 Video als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in 23 Video muss eine Linkbeziehung eingerichtet werden.  

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in 23 Video zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei 23 Video müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines 23 Video-Testbenutzers](#creating-a-23-video-test-user)** , um eine Entsprechung von Britta Simon in 23 Video zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer 23 Video-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in 23 Video die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **23 Video** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei 23 Video anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][7] 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][8] 
  1. Geben Sie im Textfeld **Antwort-URL** die URL ein, die die Benutzer für die Anmeldung bei der 23 Video-Website verwenden (z.B. *https://britta-simon.23Video.com/saml/login*).
   
    >[!NOTE]
    >Die Active Directory-Integration mithilfe von SAML 2.0 ist für alle 23 Video-Benutzer verfügbar. Wenn Sie zugehörige Metadaten benötigen, wenden Sie sich unter [support@23company.com](mailto:support@23company.com) an den Support. 
    > 
 
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für 23 Video** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][9] 
 1. Klicken Sie auf "Zertifikat herunterladen", und speichern Sie das Zertifikat auf Ihrem Computer.
 2. Wenden Sie sich unter [support@23company.com](mailto:support@23company.com) an das 23 Video-Supportteam. Geben Sie das heruntergeladene Zertifikat, die **Aussteller-URL**, die **Service-URL für einmaliges Anmelden** und die **URL für einmaliges Abmelden** an, und bitten Sie das Team, für Ihre 23 Video-App SSO einzurichten.    
 3. Klicken Sie auf **Weiter**.
5. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
   
    ![Azure AD – einmaliges Anmelden][10]
6. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus. 
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png)  
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png)  
 1. Notieren Sie den Wert von **Neues Kennwort**. 
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-23-video-test-user"></a>Erstellen eines 23 Video-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in 23 Video.

**Um einen Benutzer namens Britta Simon in 23 Video zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer 23 Video-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Einstellungen**.
3. Klicken Sie im Abschnitt **Benutzer** auf **Konfigurieren**.
   
    ![Benutzer zuweisen][400]
4. Klicken Sie auf **Neuen Benutzer hinzufügen**. 
   
    ![Benutzer zuweisen][401]
5. Führen Sie im Abschnitt **Personen zu dieser Website einladen** die folgenden Schritte aus:
   
    ![Benutzer zuweisen][402]
 1. Geben Sie im Textfeld **E-Mail-Adressen** die E-Mail-Adresse von Britta Simon in Azure AD ein.  
 2. Klicken Sie auf **Benutzer hinzufügen**.   

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf 23 Video erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon 23 Video zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **23 Video**aus.
   
    ![Benutzer zuweisen][202] 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „23 Video“ klicken, sollten Sie automatisch bei Ihrer 23 Video-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png





