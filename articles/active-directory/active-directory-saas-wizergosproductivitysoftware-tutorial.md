<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Wizergos Productivity Software | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Wizergos Productivity Software konfigurieren."
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
	ms.date="09/15/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Wizergos Productivity Software 

In diesem Tutorial erfahren Sie, wie Sie Wizergos Productivity Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von Wizergos Productivity Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Wizergos Productivity Software hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Wizergos Productivity Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Wizergos Productivity Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Wizergos Productivity Software-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Wizergos Productivity Software aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Wizergos Productivity Software aus dem Katalog
Zum Konfigurieren der Integration von Wizergos Productivity Software in Azure AD müssen Sie Wizergos Productivity Software über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Wizergos Productivity Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
	
	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
	
	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **Wizergos Productivity Software** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. Wählen Sie im Ergebnisbereich **Wizergos Productivity Software** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Auswählen der App im Katalog](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in Wizergos Productivity Software konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Wizergos Productivity Software zu einem Benutzer in Azure AD ist. Anders ausgedrückt muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Wizergos Productivity Software eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in Wizergos Productivity Software zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei der Wizergos Productivity Software müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Wizergos Productivity Software-Testbenutzers](#creating-a-wizergos-productivity-software-test-user)**, um eine Entsprechung von Britta Simon in Wizergos Productivity Software zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Wizergos Productivity Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Wizergos Productivity Software die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Wizergos Productivity Software** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Wizergos Productivity Software anmelden** die Option **Azure AD einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

	a. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `https://www.wizergos.net`.

    b. Geben Sie im Textfeld **Antwort-URL** Folgendes ein: `https://www.wizergos.net/register_users/saml2/`

	c. Klicken Sie auf **Weiter**.

4. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

	a. Geben Sie im Textfeld **Anmelde-URL** Folgendes ein: `https://www.wizergos.net`.

	b. Klicken Sie auf **Weiter**.

5. Führen Sie auf der Seite **Einmaliges Anmelden bei Wizergos Productivity Software konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_06.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.

6. Melden Sie sich in einem anderen Webbrowserfenster an Ihrem Wizergos Productivity Software-Mandanten als Administrator an.

7. Wählen Sie im Hamburger-Menü die Option **Admin**.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

8. Wählen Sie auf der Seite „Admin“ links im Menü die Option **AUTHENTIFIZIERUNG**, und klicken Sie auf **Azure AD**.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

9. Führen Sie im Abschnitt für die **Authentifizierung** die folgenden Schritte aus:

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

	a. Öffnen Sie das heruntergeladene Zertifikat von Azure, und kopieren Sie den Fingerabdruck des Zertifikats. Fügen Sie ihn in das Textfeld von **Zertifikatfingerabdruck** ein.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_004.png)

	b. Geben Sie im Textfeld **Aussteller-URL** den Wert der **Aussteller-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	c. Geben Sie im Textfeld für die **Single Sign-on Service URL** (URL für einmaliges Anmelden) den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	d. Geben Sie im Textfeld für die **Single Sign-out Service URL** (URL für einmaliges Abmelden) den Wert für die **Dienst-URL für einmaliges Abmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	e. Klicken Sie auf die Schaltfläche **Save**.

	> [AZURE.NOTE] Beachten Sie, dass der Zertifikatfingerabdruck keine Leerzeichen enthalten sollte.

7. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
	![Azure AD – einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
    
	![Azure AD – einmaliges Anmelden][11]



### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
    
	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Wizergos Productivity Software-Testbenutzers

In diesem Abschnitt erstellen Sie in der Wizergos Productivity Software einen Benutzer namens Britta Simon. Wenden Sie sich unter [support@wizergos.com](emailTo:support@wizergos.com) an das Supportteam von Wizergos Productivity Software, um die Benutzer der Wizergos Productivity Software-Plattform hinzufügen zu lassen.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist es, Britta Simon für das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf Wizergos Productivity Software erhält.
	
   ![Benutzer zuweisen][200]

**Um Britta Simon Wizergos Productivity Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
    
	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Wizergos Productivity Software** aus.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.
    
	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
 
Wenn Sie im Zugriffsbereich auf die Kachel „Wizergos Productivity Software“ klicken, sollten Sie automatisch bei Ihrer Wizergos Productivity Software-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->