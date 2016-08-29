<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Predictix Ordering | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Predictix Ordering konfigurieren."
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
	ms.date="08/12/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Predictix Ordering

In diesem Tutorial erfahren Sie, wie Sie Predictix Ordering in Azure Active Directory (Azure AD) integrieren.

Die Integration von Predictix Ordering in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Predictix Ordering hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Predictix Ordering anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Predictix Ordering konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Predictix Ordering-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Predictix Ordering aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Predictix Ordering aus dem Katalog
Zum Konfigurieren der Integration von Predictix Ordering in Azure AD müssen Sie Predictix Ordering aus dem Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Predictix Ordering aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **Predictix Ordering** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_01.png)

7. Wählen Sie im Ergebnisbereich **Predictix Ordering** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_02.png)


##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Predictix Ordering basierend auf einem Testbenutzer mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Predictix Ordering als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Predictix Ordering muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Predictix Ordering zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Predictix Ordering sind die folgenden Bausteine erforderlich:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Predictix Ordering-Testbenutzers](#creating-a-predictix-price-reporting-test-user)**, um eine Entsprechung von Britta Simon in Predictix Ordering zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Predictix Ordering-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Predictix Ordering die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Predictix Ordering** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Predictix Ordering anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_04.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der Predictix Ordering-Anwendung verwendet wird, nach folgendem Muster ein: **https://\<Unternehmensname-Preise>.ordering.predictix.com/sso/request**.
	
	b. Klicken Sie auf **Weiter**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Predictix Ordering** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.


5. Wenden Sie sich an das Supportteam von Predictix Ordering, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:

	• Das heruntergeladene Zertifikat

	• Die **Entitäts-ID**

	• Die **SAML-SSO-URL**

	• Die **Dienst-URL für einmaliges Abmelden**

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
	
	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
 
	![Azure AD – einmaliges Anmelden][11]


### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-predictix-ordering-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Predictix Ordering-Testbenutzers

In diesem Abschnitt erstellen Sie in Predictix Ordering einen Benutzer mit dem Namen Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer ggf. vom Predictix Ordering-Supportteam unterstützen.


### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf Predictix Ordering gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Predictix Ordering zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Predictix Ordering** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-predictix-ordering-tutorial/tutorial_predictixordering_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Predictix Ordering“ klicken, sollten Sie automatisch bei Ihrer Predictix Ordering-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictix-ordering-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0817_2016-->