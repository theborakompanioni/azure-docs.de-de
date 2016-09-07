<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Bynder | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Bynder konfigurieren."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/21/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Bynder

Dieses Tutorial soll Ihnen zeigen, wie Sie Bynder in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Bynder in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Bynder hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Bynder anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Bynder konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Bynder-Abonnement


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Microsoft Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Bynder aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Microsoft Azure AD


## Hinzufügen von Bynder aus dem Katalog
Zum Konfigurieren der Integration von Bynder in Azure AD müssen Sie Bynder aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Bynder aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
	
	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
	
	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Bynder** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)

7. Wählen Sie im Ergebnisbereich **Bynder** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Auswählen der App im Katalog](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Microsoft Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Microsoft Azure AD mit Bynder konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Bynder als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Bynder muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in Bynder zuweisen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Microsoft Azure AD mit Bynder zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Microsoft Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden von Microsoft Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Bynder-Testbenutzers](#creating-a-bynder-test-user)**, um eine Entsprechung von Britta Simon in Bynder zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon das einmalige Anmelden von Microsoft Azure AD zu ermöglichen.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Microsoft Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Microsoft Azure AD im klassischen Portal und konfigurieren es in Ihrer Bynder-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Microsoft Azure AD mit Bynder zu konfigurieren:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Bynder** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bynder anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)

    a. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<company name>.getbynder.com/sso/SAML/authenticate/`

	b. Klicken Sie auf **Weiter**.

4. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.



	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)

	a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.getbynder.com/login/`

	b. Klicken Sie auf **Weiter**.

    > [AZURE.NOTE] Der Wert der Anmelde-URL ist in diesem Tutorial nur ein Platzhalter. Den tatsächlichen Wert für Ihre Umgebung können Sie bei Bynder erfragen.

5. Führen Sie auf der Seite **Einmaliges Anmelden bei Bynder konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)

    a. Klicken Sie auf **Metadaten herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.

6. Wenden Sie sich an das Supportteam von Bynder, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen. Fügen Sie die heruntergeladene Metadatendatei an die E-Mail an, damit das Bynder-Team SSO auf seiner Seite einrichten kann.

7. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
	![Azure AD – einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
    
	![Azure AD – einmaliges Anmelden][11]



### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
    
	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen einen Bynder-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Bynder. Bynder unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Bynder ein neuer Benutzer erstellt.

> [AZURE.NOTE] Setzen Sie sich mit dem Supportteam von Bynder in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf Bynder erhält.
	
   ![Benutzer zuweisen][200]

**Um Britta Simon Bynder zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
    
	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **Bynder** aus.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.
    
	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

In diesem Abschnitt wird die Microsoft Azure AD-Konfiguration für einmaliges Anmelden mithilfe des Zugriffsbereichs getestet.
 
Wenn Sie im Zugriffsbereich auf die Kachel „Bynder“ klicken, sollten Sie automatisch bei Ihrer Bynder-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0824_2016-->