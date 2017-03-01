---
title: 'Tutorial: Azure Active Directory-Integration mit TiViTz | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TiViTz konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b97ed88f-7888-4185-be22-41d1f62cbbf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 67c859ca7cb88c4b03b605330391856a150323dd
ms.openlocfilehash: 6f60c803ae0f6d32f79611aced40bef3ea87173b
ms.lasthandoff: 12/06/2016


---
# <a name="tutorial-azure-active-directory-integration-with-tivitz"></a>Tutorial: Azure Active Directory-Integration mit TiViTz

In diesem Tutorial erfahren Sie, wie Sie TiViTz in Azure Active Directory (Azure AD) integrieren.

Diese Integration bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TiViTz haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei TiViTz anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit TiViTz konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges TiViTz-Abonnement


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von TiViTz über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-tivitz-from-the-gallery"></a>Hinzufügen von TiViTz über den Katalog
Zum Konfigurieren der Integration von TiViTz in Azure AD müssen Sie TiViTz über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um TiViTz über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld die Zeichenfolge **TiViTz** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_001.png)

7. Wählen Sie im Ergebnisbereich die Option **TiViTz** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei TiViTz mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TiViTz als Gegenpart zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TiViTz muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den** **Benutzernamen in Azure AD dem** **Benutzernamen in TiViTz zuweisen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit TiViTz zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines TiViTz-Testbenutzers](#creating-a-tivitz-test-user)**, um in TiViTz einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer TiViTz-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit TiViTz die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **TiViTz** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei TiViTz anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_02.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_03.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.o365.tivitz.com/`

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<company name>.o365.tivitz.com/`

    c. Klicken Sie auf **Weiter**.

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Wenden Sie sich an das [Supportteam von TiViTz](emaiLto:info@tivitz.com), um diese Werte zu erhalten.

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für TiViTz** auf **Metadaten herunterladen**, und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_04.png) 

5. Wenden Sie sich an das [Supportteam von TiViTz](emaiLto:info@tivitz.com), um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie die heruntergeladenen **Metadaten** bereit.

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
  
    ![Azure AD – einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_06.png) 

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tivitz-tutorial/create_aaduser_08.png) 

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-a-tivitz-test-user"></a>Erstellen eines TiViTz-Testbenutzers

In diesem Abschnitt wird in TiViTz ein Benutzer namens Britta Simon erstellt. TiViTz unterstützt die standardmäßig aktivierte Just-in-Time-Bereitstellung.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Sollte noch kein Benutzer vorhanden sein, wird beim Zugreifen auf TiViTz ein neuer Benutzer erstellt.

> [!NOTE]
> Setzen Sie sich mit dem [Supportteam von TiViTz](emaiLto:info@tivitz.com) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TiViTz gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu TiViTz durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **TiViTz** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tivitz-tutorial/tutorial_tivitz_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „TiViTz“ klicken, sollten Sie automatisch bei Ihrer TiViTz-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tivitz-tutorial/tutorial_general_205.png

