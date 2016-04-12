<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Expensify | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Expensify konfigurieren."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/16/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Expensify

In diesem Tutorial erfahren Sie, wie Sie Expensify in Azure Active Directory (Azure AD) integrieren.

Die Integration von Expensify in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Expensify hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Expensify anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Expensify konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Expensify-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. <br> Das in diesem Tutorial beschriebene Szenario besteht aus zwei großen Bausteinen:

1. Hinzufügen von Expensify aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Expensify aus dem Katalog
Zum Konfigurieren der Integration von Expensify in Azure AD müssen Sie Expensify aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Expensify aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. <br><br> ![Active Directory][1]<br>

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.<br><br> ![Anwendungen][2]<br>
4. Klicken Sie unten auf der Seite auf **Hinzufügen**.<br><br> ![Anwendungen][3]<br>
5. Klicken Sie im Dialogfeld **What do you want to do** auf **Anwendung aus dem Katalog hinzufügen**.<br><br> ![Anwendungen][4]<br>
6. Geben Sie im Suchfeld das Wort **Expensify** ein.<br><br> ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_01.png)<br>
7. Wählen Sie im Ergebnisbereich **Expensify** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.<br><br>

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden für Azure AD mit Expensify basierend auf einem Testbenutzer mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Expensify als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Expensify muss eine Linkbeziehung eingerichtet werden.<br> Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in Expensify zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Expensify müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Expensify-Testbenutzers](#creating-an-expensify-test-user)**, um eine Entsprechung von Britta Simon in Expensify zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden für Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Expensify-Anmeldung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Expensify die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Expensify** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen. <br><br> ![Einmaliges Anmelden konfigurieren][6] <br>

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Expensify anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**. <br><br> ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_03.png) <br>

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** folgende Schritte aus: <br><br>![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_04.png) <br>


    a. Geben Sie im Textfeld „Anmelde-URL“ die URL ein, die von Ihren Benutzern nach folgendem Muster zur Anmeldung bei der Expensify-Anwendung verwendet wird: **„https://www.expensify.com/authentication/saml/login“**.


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Expensify** die folgenden Schritte aus: <br><br>![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_05.png) <br>

    a. Klicken Sie auf **Metadaten herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Zum Aktivieren von SSO in Expensify müssen Sie in der Anwendung zuerst die „Domain Control“ aktivieren. Sie können die „Domain Control“ in der Anwendung aktivieren. Die erforderlichen Schritte sind [hier](http://help.expensify.com/domain-control) angegeben. Weitere Hilfe erhalten Sie, wenn Sie sich unter [help@expensify.com](mailto:help@expensify.com) an das Unternehmen wenden. Führen Sie nach dem Aktivieren der „Domain Control“ die unten angegebenen Schritte aus.<br>
   1. Melden Sie sich mit Administratorrechten am Expensify-Konto an.<br>
   2. Klicken Sie oben auf die Registerkarte „Admin“.<br>
   3. Klicken Sie links auf „Domain Control“.<br>
   4. Klicken Sie auf Ihren bestätigten Domänennamen.<br>
   5. Klicken Sie nach dem Klicken auf den bestätigten Domänennamen links auf „SAML“, und aktivieren Sie SAML.<br>
   6. Öffnen Sie die aus Azure AD heruntergeladenen Verbundmetadaten, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld „Identity Provider Metadata“ ein.<br>
   7. Die Änderungen der SAML-Einstellungen werden automatisch gespeichert.<br> <br><br>![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_51.png) <br><br>
  
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**. <br><br>![Azure AD – einmaliges Anmelden][10]<br>

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**. <br><br>![Azure AD – einmaliges Anmelden][11]




### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen „Britta Simon“.<br> Wählen Sie in der Benutzerliste **Britta Simon** aus.<br><br>![Azure AD-Benutzer erstellen][20]<br>

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. <br><br>![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_09.png)<br>

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**. <br><br> ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_03.png) <br>

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. <br><br> ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_04.png) <br>

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: <br><br> ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_05.png) <br>

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: <br><br>![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_06.png) <br>

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**. <br><br> ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_07.png) <br>

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus: <br><br>![Erstellen einen Azure AD-Testbenutzers](./media/active-directory-saas-expensify-tutorial/create_aaduser_08.png) <br>

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Expensify-Testbenutzers

In diesem Abschnitt erstellen Sie in Expensify einen Benutzer mit dem Namen „Britta Simon“. Expensify unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ein neuer Benutzer wird erstellt, wenn Sie auf Expensify zugreifen, falls er noch nicht vorhanden ist. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von Expensify in Verbindung.


### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden für Azure, indem Sie ihr Zugriff auf Expensify gewähren. <br><br>![Benutzer zuweisen][200] <br>

**Um Britta Simon Expensify zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**. <br><br>![Benutzer zuweisen][201] <br>

2. Wählen Sie in der Anwendungsliste **Expensify** aus. <br><br>![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-expensify-tutorial/tutorial_expensify_50.png) <br>

1. Klicken Sie im oberen Menü auf **Benutzer**. <br><br>![Benutzer zuweisen][203] <br>

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**. <br><br>![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mithilfe des Zugriffsbereichs.<br> Wenn Sie im Zugriffsbereich auf die Kachel „Expensify“ klicken, sollten Sie automatisch bei Ihrer Expensify-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-expensify-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0330_2016-->