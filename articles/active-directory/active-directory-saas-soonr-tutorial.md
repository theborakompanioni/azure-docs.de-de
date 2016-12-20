---
title: 'Tutorial: Azure Active Directory-Integration mit Soonr Workplace | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Soonr Workplace konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 88319106-8dc8-4814-a3d4-89200cc9f72a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 816da28df83d220074f1d5a183c1c378d2313761


---
# <a name="tutorial-azure-active-directory-integration-with-soonr-workplace"></a>Tutorial: Azure Active Directory-Integration mit Soonr Workplace
Dieses Tutorial soll Ihnen zeigen, wie Sie Soonr Workplace in Azure Active Directory (Azure AD) integrieren können.  
Die Integration von Soonr Workplace in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD den Zugriff auf Soonr Workplace steuern.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Soonr Workplace anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Soonr Workplace konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Soonr Workplace-Abonnement, für das einmaliges Anmelden aktiviert ist

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

1. Hinzufügen von Soonr Workplace aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-soonr-workplace-from-the-gallery"></a>Hinzufügen von Soonr Workplace aus dem Katalog
Zum Konfigurieren der Integration von Soonr Workplace in Azure AD müssen Sie Soonr Workplace aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Soonr Workplace aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Soonr Workplace**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_01.png)
7. Wählen Sie im Ergebnisbereich **Soonr Workplace** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Soonr Workplace konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Soonr Workplace als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Soonr Workplace muss eine Linkbeziehung eingerichtet werden.  
Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Soonr Workplace zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Soonr Workplace müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Soonr Workplace-Testbenutzers](#creating-a-soonr-workplace-test-user)** – um ein Gegenstück zu Britta Simon in Soonr Workplace zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in der Soonr Workplace-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Soonr Workplace die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Soonr Workplace** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Soonr Workplace anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_04.png) 

    a. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei Ihrer Anwendung verwendet wird, nach folgendem Muster ein: `https://<server-name>.soonr.com/singlesignon/saml/SSO`.

    b. Klicken Sie auf **Next**.

1. Führen Sie auf der Seite **Einmaliges Anmelden bei Soonr Workplace konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_05.png) 
   
    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Die **Aussteller-URL**
   
    C. Die **SAML-SSO-URL**
   
    D. Die **Dienst-URL für einmalige Abmeldung**
2. Wenden Sie sich <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">hier</a> an das Soonr-Team, um einmaliges Anmelden für Soonr Workplace konfigurieren zu lassen.
3. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
4. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-soonr-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-soonr-workplace-test-user"></a>Erstellen eines Soonr Workplace-Testbenutzers
 In diesem Abschnitt wird in Soonr Workplace eine Benutzerin namens Britta Simon erstellt. Wenden Sie sich an das Supportteam von Soonr, um einen Benutzer in der Plattform zu erstellen. Das Supportticket für Soonr kann <a href="https://na01.safelinks.protection.outlook.com/?url=http%3A%2F%2Fsoonr.com%2FAWPHelp%2FContent%2F0_HOME%2FSupport_for_End_Clients.htm&data=01%7C01%7Cv-saikra%40microsoft.com%7Ccbb4367ab09b4dacaac408d3eebe3f42%7C72f988bf86f141af91ab2d7cd011db47%7C1&sdata=FB92qtE6m%2Fd8yox7AnL2f1h%2FGXwSkma9x9H8Pz0955M%3D&reserved=0/">hier</a> erstellt werden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf Soonr Workplace erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Soonr Workplace zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Soonr Workplace**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-soonr-tutorial/tutorial_soonr_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.  
Wenn Sie im Zugriffsbereich auf die Kachel „Soonr Workplace“ klicken, sollten Sie automatisch bei Ihrer Soonr Workplace-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-soonr-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


