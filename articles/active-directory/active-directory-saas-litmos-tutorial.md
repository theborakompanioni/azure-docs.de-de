---
title: 'Tutorial: Azure Active Directory-Integration mit Litmos | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Litmos konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: cfaae4bb-e8e5-41d1-ac88-8cc369653036
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 479425fe3e19b843fed2aeae94dcf1fd7e9a0a77


---
# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Tutorial: Azure Active Directory-Integration mit Litmos
Dieses Tutorial soll Ihnen zeigen, wie Sie Litmos in Azure Active Directory (Azure AD) integrieren können.  
Die Integration von Litmos in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer Zugriff auf Litmos hat. 
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Litmos anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – in Azure Active Directory. 

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Litmos konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Litmos-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern. 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.  
Das in diesem Tutorial beschriebene Szenario besteht aus drei großen Bausteinen:

1. Hinzufügen von Litmos aus dem Katalog 
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-litmos-from-the-gallery"></a>Hinzufügen von Litmos aus dem Katalog
Zum Konfigurieren der Integration von Litmos in Azure AD müssen Sie Litmos aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Litmos aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Litmos**ein.
   
    ![Anwendungen][5]
7. Wählen Sie im Ergebnisbereich **Litmos** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Anwendungen][500]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Litmos konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Litmos als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Litmos muss eine Linkbeziehung eingerichtet werden.  
Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Litmos zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Litmos müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Litmos-Testbenutzers](#creating-a-halogen-software-test-user)** , um eine Entsprechung von Britta Simon in Litmos zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure AD-Portal zu aktivieren und in Ihrer Litmos-Anwendung zu konfigurieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache) weitere Informationen.

Im Rahmen der Konfiguration müssen Sie die **SAML-Tokenattribute** für Ihre Litmos-Anwendung anpassen.  

![Azure AD – einmaliges Anmelden][17] 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Litmos die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure AD-Portal auf der Anwendungsintegrationsseite für **Litmos** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Litmos anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][7] 
3. Melden Sie sich bei Ihrer Litmos-Unternehmenswebsite (z.B.: *https://azureapptest.litmos.com/account/Login*) als Administrator an.
   
    ![Azure AD – einmaliges Anmelden][21] 
4. Klicken Sie auf der Navigationsleiste links auf **Accounts**.
   
    ![Azure AD – einmaliges Anmelden][22] 
5. Klicken Sie auf die Registerkarte **Integrations** .
   
    ![Azure AD – einmaliges Anmelden][23] 
6. Scrollen Sie auf der Registerkarte **Integrationen** nach unten zu **Drittanbieterintegrationen**, und klicken Sie dann auf die Registerkarte **SAML 2.0**.
   
    ![Azure AD – einmaliges Anmelden][24] 
7. Kopieren Sie den Wert unter **The SAML endpoint for Litmos is:**.
   
    ![Azure AD – einmaliges Anmelden][26] 
8. Führen Sie im klassischen Azure-Portal auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][8] 
   
    a. Geben Sie im Textfeld **Bezeichner** die von Ihren Benutzern für die Anmeldung bei Ihrer Litmos-Anwendung verwendete URL ein (z.B. *https://azureapptest.litmos.com/account/Login*).
   
    b. Fügen Sie im Textfeld **Antwort-URL** den Wert ein, den Sie im vorherigen Schritt in der Litmos-Anwendung kopiert haben.
   
    c. Klicken Sie auf **Weiter**.
9. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Litmos** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][2] 
   
    a. Klicken Sie auf "Zertifikat herunterladen", und speichern Sie das Zertifikat auf Ihrem Computer.
10. Führen Sie in der **Litmos** -Anwendung die folgenden Schritte aus:
    
     ![Azure AD – einmaliges Anmelden][25] 
    
     a. Aktivieren Sie **Enable SAML**.
    
     b. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
    
    > [!TIP]
    > Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
    
     c. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **SAML-X.509-Zertifikat** ein.
    
     d. Klicken Sie auf **Änderungen speichern**.
11. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
    
     ![Azure AD – einmaliges Anmelden][10]
12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
     ![Azure AD – einmaliges Anmelden][11]
13. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen. 
    
    ![Einmaliges Anmelden konfigurieren][12]
14. Führen Sie im Dialogfeld **Benutzerattribut hinzufügen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren][14]
    
    | Attributname | Attributwert |
    | --- | --- |
    | E-Mail |user.mail |
    | Vorname |user.givenname |
    | Lastname |user.surname |
    
    Führen Sie für jede Datenzeile in der Tabelle oben die folgenden Schritte aus:
    
    a. Klicken Sie auf **Benutzerattribut hinzufügen**. 
    
    ![Einmaliges Anmelden konfigurieren][15]

    a. Geben Sie im Textfeld **Attributname** den für die Zeile angezeigten **Attributnamen** ein.

    b. Wählen Sie den für die Zeile angezeigten **Attributwert** aus.

    c. Klicken Sie auf **Abschließen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.


1. Klicken Sie auf **Änderungen übernehmen**. 
   
   ![Einmaliges Anmelden konfigurieren][16]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  
   
    a. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-litmos-test-user"></a>Erstellen eines Litmos-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Litmos.  
Die Litmos-Anwendung unterstützt die Just-in-Time-Bereitstellung. Das bedeutet, dass ggf. automatisch ein Benutzerkonto erstellt wird, wenn über den Zugriffsbereich auf die Anwendung zugegriffen wird.

**Um einen Benutzer namens Britta Simon in Litmos zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer Litmos-Unternehmenswebsite (z.B.: *https://azureapptest.litmos.com/account/Login*) als Administrator an.
   
    ![Azure AD – einmaliges Anmelden][21] 
2. Klicken Sie auf der Navigationsleiste links auf **Accounts**.
   
    ![Azure AD – einmaliges Anmelden][22] 
3. Klicken Sie auf die Registerkarte **Integrations** .
   
    ![Azure AD – einmaliges Anmelden][23] 
4. Scrollen Sie auf der Registerkarte **Integrationen** nach unten zu **Drittanbieterintegrationen**, und klicken Sie dann auf die Registerkarte **SAML 2.0**.
   
    ![Azure AD – einmaliges Anmelden][24] 
5. Wählen Sie **Autogenerate Users:**aus.
   
    ![Azure AD – einmaliges Anmelden][27] 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf Litmos erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Litmos zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Litmos**aus.
   
    ![Benutzer zuweisen][202] 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.  
Wenn Sie im Zugriffsbereich auf die Kachel „Litmos“ klicken, sollten Sie automatisch bei Ihrer Litmos-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png








<!--HONumber=Nov16_HO3-->


