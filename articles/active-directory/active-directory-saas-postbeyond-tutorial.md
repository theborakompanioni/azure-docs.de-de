---
title: 'Tutorial: Azure Active Directory-Integration mit PostBeyond | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und PostBeyond konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 09992f08-ec50-4472-997f-ccbe719039e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 6a635e6b767d976ad8b52df36c42ca4056c297e1
ms.openlocfilehash: 2424e3f8f43d4c5a1fe4c05913c6d80cda15a5d7
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a>Tutorial: Azure Active Directory-Integration mit PostBeyond
In diesem Tutorial erfahren Sie, wie Sie PostBeyond in Azure Active Directory (Azure AD) integrieren.

Die Integration von PostBeyond in Azure AD bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf PostBeyond haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei PostBeyond anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit PostBeyond konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein **PostBeyond**-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von PostBeyond über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-postbeyond-from-the-gallery"></a>Hinzufügen von PostBeyond aus dem Katalog
Zum Konfigurieren der Integration von PostBeyond in Azure AD müssen Sie PostBeyond über den Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um PostBeyond über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals**auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld die Zeichenfolge **PostBeyond**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_01.png)
7. Wählen Sie im Ergebnisbereich die Option **PostBeyond** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit PostBeyond.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in PostBeyond als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in PostBeyond muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD dem Benutzernamen**** in PostBeyond zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit PostBeyond müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines PostBeyond-Testbenutzers](#creating-a-PostBeyond-test-user)** , um in PostBeyond einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer PostBeyond-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in PostBeyond die folgenden Schritte aus:**

1. Klicken Sie im oberen Menü auf **Schnellstart**.
   
    ![Einmaliges Anmelden konfigurieren][6]
2. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **PostBeyond** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][7] 
3. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei PostBeyond anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_06.png)
4. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_07.png)
 1. Geben Sie im Textfeld für die Anmelde-URL eine URL im folgenden Format ein: `https://app.postbeyond.com`. 
 2. Klicken Sie auf **Weiter**.

5. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für PostBeyond** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer. Kopieren Sie auch die Aussteller-URL, die Dienst-URL für einmaliges Anmelden und die Dienst-URL für einmaliges Abmelden. Der PostBeyond-Support benötigt diese Angaben, um das einmalige Anmelden konfigurieren zu können.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_08.png)
6. Wenden Sie sich unter <mailto:sso@postbeyond.com> an das Supportteam von PostBeyond, um SSO für Ihre Anwendung konfigurieren zu lassen. Dort erhalten Sie Informationen zum richtigen Kanal für die SSO-Konfiguration. Dazu muss Folgendes bereitgestellt werden: 
   
   * Das heruntergeladene Zertifikat
   * Die **Aussteller-URL**
   * Die **SAML-SSO-URL**
   * Die **Dienst-URL für einmaliges Abmelden**
7. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_05.png) 
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus. 
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_06.png)  
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_08.png)  
 1. Notieren Sie den Wert von **Neues Kennwort**. 
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-postbeyond-test-user"></a>Erstellen eines PostBeyond-Testbenutzers
In diesem Abschnitt erstellen Sie in PostBeyond einen Benutzer namens Britta Simon. Falls Sie nicht wissen, wie Sie Britta Simon in PostBeyond hinzufügen sollen, können Sie sich beim Hinzufügen des Testbenutzers und beim Aktivieren von SSO vom PostBeyond-Supportteam unterstützen lassen. Verwenden Sie hierfür die E-Mail-Adresse <mailto:sso@postbeyond.com>.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf PostBeyond gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu PostBeyond durchzuführen:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste die Option **PostBeyond**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_09.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Liste „Alle Benutzer“ den Eintrag **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „PostBeyond“ klicken, sollte die Anmeldeseite von PostBeyond erscheinen. Klicken Sie auf **Sign in with Office 365**(Mit Office 365 anmelden), und geben Sie Ihre Azure AD-Anmeldeinformationen ein. Anschließend sollten Sie bei PostBeyond angemeldet sein.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_205.png

