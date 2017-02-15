---
title: 'Tutorial: Azure Active Directory-Integration mit Pacific Timesheet | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Pacific Timesheet konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: e546e8ba-821a-4942-9545-c84b0670beab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/06/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3e6982023a40d0794ab1285302f48047e68c499d


---
# <a name="tutorial-azure-active-directory-integration-with-pacific-timesheet"></a>Tutorial: Azure Active Directory-Integration mit Pacific Timesheet
In diesem Tutorial erfahren Sie, wie Sie Pacific Timesheet in Azure Active Directory (Azure AD) integrieren.

Die Integration von Pacific Timesheet in Azure AD bietet Ihnen folgende Vorteile:

* Sie können in Azure AD steuern, wer auf Pacific Timesheet Zugriff hat
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Pacific Timesheet anzumelden (einmaliges Anmelden)
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Pacific Timesheet konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein **Pacific Timesheet-Abonnement**, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Pacific Timesheet aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-pacific-timesheet-from-the-gallery"></a>Hinzufügen von Pacific Timesheet aus dem Katalog
Zum Konfigurieren der Integration von Pacific Timesheet in Azure AD müssen Sie Pacific Timesheet aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Pacific Timesheet aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie **Pacific Timesheet** in das Suchfeld ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_01.png)
7. Wählen Sie im Ergebnisbereich die Option **Pacific Timesheet** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Pacific Timesheet.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Pacific Timesheet als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Pacific Timesheet muss eine Linkbeziehung eingerichtet werden.
Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD dem **Benutzernamen** in Pacific Timesheet zuweisen. Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Pacific Timesheet müssen Sie die folgenden Schritte durchführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Pacific Timesheet-Testbenutzers](#creating-a-pacific-timesheet-test-user)**, um eine Entsprechung von Britta Simon in Pacific Timesheet zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Pacific Timesheet-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Pacific Timesheet die folgenden Schritte aus:**

1. Klicken Sie im oberen Menü auf **Schnellstart**.
   
    ![Einmaliges Anmelden konfigurieren][6]
2. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Pacific Timesheet** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][7] 
3. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Pacific Timesheet anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_06.png)
4. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, um die Anwendung im **IDP-initiierten Modus** zu konfigurieren:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_07.png)

    a. Geben Sie im Textfeld Bezeichner eine URL nach folgendem Muster ein: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`

    b. Geben Sie im Textfeld für die Antwort-URL eine URL nach folgendem Muster ein: `https://<InstanceID>.pacifictimesheet.com/timesheet/home.do`.

    b. Klicken Sie auf **Weiter**.

1. Auf der Seite **Einmaliges Anmelden an Pacific Timesheet konfigurieren**. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_09.png)
2. Wenden Sie sich an das Supportteam von Pacific Timesheet, um SSO für Ihre Anwendung konfigurieren zu lassen. Bitte beachten Sie, dass Sie eine E-Mail mit der Aussteller-URL und der SAML SSO-URL von der Seite **Configure single sign-on at Pacific Timesheet** (Einmaliges Anmelden an Pacific Timesheet konfigurieren) senden und ein heruntergeladenes Zertifikat anhängen müssen.
3. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
4. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pacific-timesheet-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-pacific-timesheet-test-user"></a>Erstellen eines Pacific Timesheet-Testbenutzers
In diesem Abschnitt erstellen Sie in Pacific Timesheet einen Benutzer mit dem Namen „Britta Simon“. Bitte wenden Sie sich an das Pacific Timesheet-Supportteam, um einen Benutzer in der Anwendung zu erstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Pacific Timesheet gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Pacific Timesheet zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Pacific Timesheet **aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_pacific_timesheet_10.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Liste „Alle Benutzer“ den Eintrag **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel Pacific Timesheet klicken, sollten Sie automatisch bei Ihrer Pacific Timesheet-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pacific-timesheet-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


