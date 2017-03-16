---
title: 'Tutorial: Azure Active Directory-Integration mit Yonyx Interactive Guides | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Yonyx Interactive Guides konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 07db4e01-319b-4cb6-9b93-4577bffd3cbc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4a07f946d396a9263d5b00202cd5229ddc86d1be
ms.openlocfilehash: 0d2ac97c7f8e42f1e6b847e338d0676592054d93
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Tutorial: Azure Active Directory-Integration mit Yonyx Interactive Guides
In diesem Tutorial erfahren Sie, wie Sie Yonyx Interactive Guides in Azure Active Directory (Azure AD) integrieren.

Die Integration von Yonyx Interactive Guides in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf Yonyx Interactive Guides Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Yonyx Interactive Guides anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Yonyx Interactive Guides konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Yonyx Interactive Guides-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Yonyx Interactive Guides aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Hinzufügen von Yonyx Interactive Guides aus dem Katalog
Zum Konfigurieren der Integration von Yonyx Interactive Guides in Azure AD müssen Sie Yonyx Interactive Guides aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Yonyx Interactive Guides aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie in das Suchfeld den Suchbegriff **Yonyx Interactive Guides**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_01.png)
7. Wählen Sie im Ergebnisbereich die Option **Yonyx Interactive Guides** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_0001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Yonyx Interactive Guides konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Yonyx Interactive Guides als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Yonyx Interactive Guides muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Yonyx Interactive Guides zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Yonyx Interactive Guides müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Yonyx Interactive Guides-Testbenutzers](#creating-a-yonyx-interactive-guides-test-user)**, um eine Entsprechung von Britta Simon in Yonyx Interactive Guides zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Yonyx Interactive Guides-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Yonyx Interactive Guides die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Yonyx Interactive Guides** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Yonyx Interactive Guides anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_03.png)
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_04.png)
  1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`. 
  2. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<company name>.yonyx.com`.
  3. Klicken Sie auf **Weiter**.
   
    >[!NOTE]
    > Hinweis: Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Um diese Werte zu erhalten, wenden Sie sich unter <mailto:support@yonyx.com> an das Yonyx Interactive Guides-Supportteam. 
    > 
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Yonyx Interactive Guides** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_05.png)
5. Wenden Sie sich unter <mailto:support@yonyx.com> an das Supportteam von Yonyx Interactive Guides, um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie dazu Folgendes bereit:
  * Das heruntergeladene **Zertifikat**
  * Die **Aussteller-URL**
  * Die **Dienst-URL für einmaliges Anmelden**
  * Die **Dienst-URL für einmaliges Abmelden**
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_05.png) 
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.  
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_06.png) 
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-yonyx-tutorial/create_aaduser_08.png) 
 1. Notieren Sie den Wert von **Neues Kennwort**.
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-yonyx-interactive-guides-test-user"></a>Erstellen eines Yonyx Interactive Guides-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Yonyx Interactive Guides. Yonyx Interactive Guides unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Yonyx Interactive Guides ein neuer Benutzer erstellt.

>[!NOTE]
>Setzen Sie sich über <mailto:support@yonyx.com> mit dem Supportteam von Yonyx Interactive Guides in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Yonyx Interactive Guides erhält.

![Benutzer zuweisen][200]

**Um Britta Simon zu Yonyx Interactive Guides zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste **Yonyx Interactive Guides** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Yonyx Interactive Guides“ klicken, sollten Sie automatisch bei Ihrer Yonyx Interactive Guides-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_205.png

