<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Marketo | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Marketo konfigurieren."
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
	ms.date="09/07/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Marketo

In diesem Tutorial erfahren Sie, wie Sie Marketo in Azure Active Directory (Azure AD) integrieren.

Die Integration von Marketo in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Marketo hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Marketo anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Marketo konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Marketo-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Marketo aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Marketo aus dem Katalog
Zum Konfigurieren der Integration von Marketo in Azure AD müssen Sie Marketo aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Marketo aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld **Marketo** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_01.png)

7. Wählen Sie im Ergebnisbereich **Marketo** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Marketo.

Für das einmalige Anmelden muss Azure AD wissen, welcher Benutzer in Marketo als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Marketo muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in Marketo zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Marketo müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Marketo-Testbenutzers](#creating-a-predictix-price-reporting-test-user)**, um eine Entsprechung von Britta Simon in Marketo zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Marketo-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Marketo die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Marketo** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Marketo anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_04.png)

    a. Geben Sie im Textfeld **Bezeichner** die URL nach folgendem Muster ein: `https://saml.marketo.com/sp`
	
	b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://login.marketo.com/saml/assertion/<munchkinid>`

	c. Klicken Sie auf **Weiter**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Marketo** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.


5. Um die Munchkin-ID Ihrer Anwendung zu erhalten, melden Sie sich mit Ihren Administratoranmeldeinformationen bei Marketo an und führen Sie folgende Aktionen aus:

	a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-Anwendung an.

	b. Klicken Sie auf die Schaltfläche „Admin“ im oberen Navigationsbereich.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png)

	c. Navigieren Sie zum Menü „Integration“, und klicken Sie auf den Munchkin-Link.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_11.png)

	d. Kopieren Sie die Munchkin-ID, die auf dem Bildschirm angezeigt wird, und schließen Sie Ihre Antwort-URL im Azure AD-Konfigurations-Assistenten ab.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_12.png)

6.	Führen Sie die unten beschriebenen Schritte aus, um SSO in der Anwendung zu konfigurieren:

	a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-Anwendung an.

	b. Klicken Sie auf die Schaltfläche „Admin“ im oberen Navigationsbereich.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png)

	c. Navigieren Sie zum Menü „Integration“, und klicken Sie auf „Einmaliges Anmelden“.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_07.png)

	d. Klicken zum Aktivieren der SAML-Einstellungen auf die Schaltfläche „Bearbeiten“.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_08.png)

	e. **Aktivieren** Sie die Einstellungen für einmaliges Anmelden.

	f. Geben Sie die Aussteller-ID ein, die Sie aus dem Azure AD-Konfigurations-Assistenten kopiert haben.

	g. Geben Sie die URL als **http://saml.marketo.com/sp** in das Textfeld „Entitäts-ID“ ein.

	h. Wählen Sie den Speicherort der Benutzer-ID als **Namensbezeichnerelement**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_09.png)

	> [AZURE.NOTE] Wenn Ihre Benutzer-ID kein UPN-Wert ist, ändern Sie den Wert in der Registerkarte „Attribut“.
	 
	i. Laden Sie das aus Azure AD-Zertifikat hoch, das Sie aus dem Azure AD-Konfigurations-Assistenten heruntergeladen haben. Speichern Sie die Einstellungen.

	j. Bearbeiten Sie die Einstellungen für Seitenumleitungen.

	k. Kopieren Sie die Anmelde-URL aus dem Azure AD-Konfigurations-Assistenten in das Textfeld **Anmelde-URL**.

	l. Kopieren Sie die Abmelde-URL aus dem Azure AD-Konfigurations-Assistenten in das Textfeld **Abmelde-URL**.

	m. Kopieren Sie in der Fehler-URL die URL Ihrer Marketo-Instanz, und klicken Sie auf die Schaltfläche „Speichern“, um die Einstellungen zu speichern.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_10.png)

7. Um SSO für Benutzer zu aktivieren, führen Sie die folgenden Aktionen aus:

	a. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-Anwendung an.

	b. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png)

	c. Navigieren Sie zum Menü **Sicherheit**, und klicken Sie auf **Anmeldeeinstellungen**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_13.png)

	d. Aktivieren Sie die Option **SSO erfordern**, und speichern Sie die Einstellungen.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_14.png)

8. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
	
	![Azure AD – einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
 
	![Azure AD – einmaliges Anmelden][11]


### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-marketo-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Marketo-Testbenutzers

In diesem Abschnitt erstellen Sie in Marketo einen Benutzer namens Britta Simon. Befolgen Sie diese Schritte, um einen Benutzer in der Marketo-Plattform zu erstellen.

1. Melden Sie sich mit Ihren Administratoranmeldeinformationen bei der Marketo-Anwendung an.

2. Klicken Sie auf die Schaltfläche **Admin** im oberen Navigationsbereich.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_06.png)

3. Navigieren Sie zum Menü **Sicherheit**, und klicken Sie auf **Users & Roles** (Benutzer und Rollen).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_19.png)

4. Klicken Sie auf den Link **Invite New User** (Neuen Benutzer einladen) auf der Registerkarte „Benutzer“.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_15.png)

5. Geben Sie die folgenden Informationen im Assistenten zum Einladen neuer Benutzer ein.

	a. Geben Sie die **E-Mail**-Adresse des Benutzers in das Textfeld ein.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_16.png)

	b. Geben Sie den **Vornamen** in das Textfeld ein.
	
	c. Geben Sie den **Nachnamen** in das Textfeld ein.

	d. Klicken Sie auf „Weiter“.

6. Wählen Sie auf der Registerkarte **Berechtigungen** die Benutzerrollen aus, und klicken Sie auf „Weiter“.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_17.png)

7. Klicken Sie auf die Schaltfläche „Senden“, um die Benutzer-Einladung zu senden.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_18.png)

8. Der Benutzer erhält die e-Mail-Benachrichtigung und muss auf den Link klicken sowie das Kennwort ändern, um das Konto zu aktivieren.


### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Marketo gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Marketo zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Marketo** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-marketo-tutorial/tutorial_marketo_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Marketo“ klicken, sollten Sie automatisch bei Ihrer Marketo-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-marketo-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0914_2016-->