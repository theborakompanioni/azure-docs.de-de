<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit People | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und People konfigurieren."
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
	ms.date="04/11/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit People

Dieses Tutorial soll Ihnen zeigen, wie Sie People in Azure Active Directory (Azure AD) integrieren können.

Die Integration von People in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf People hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei People anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit People konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure-Abonnement
- Ein People-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von People aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von People aus dem Katalog
Zum Konfigurieren der Integration von People in Azure AD müssen Sie People aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um People aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
 
	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld **People** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. Wählen Sie im Ergebnisbereich **People** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens Britta Simon das einmalige Anmelden von Azure AD in People konfiguriert und getestet werden kann.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei People müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines People-Testbenutzers](#creating-a-people-test-user)**, um eine Entsprechung von Britta Simon in People zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer People-Anwendung zu konfigurieren.



**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in People die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **People** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	[Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei People anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_03.png)

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
 
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_04.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern zur Anmeldung bei der People-Anwendung verwendet wird. Verwenden Sie dabei das folgende Muster: **https://<Firmenname>.peoplehr.com/**

    b. Wenn Sie die Mandanten-URL nicht kennen, wenden Sie sich über [customerservices@peoplehr.com](mailto:customerservices@peoplehr.com) an das People-Supportteam, um diese URL zu erfragen.

	c. Geben Sie im Textfeld **Bezeichner** die Mandanten-URL ein.

    d. Geben Sie im Textfeld **Antwort-URL** die unter Verwendung des folgenden Musters die URL ein: ****https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**"

	e. Klicken Sie auf **Weiter**.


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für People** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_05.png)

    a. Klicken Sie auf **Metadaten herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem People-Mandanten anmelden.
	
	a. Klicken Sie im Menü links auf **Settings**.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_001.png)

	b. Klicken Sie auf **Company**.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_002.png)

	c. Klicken Sie neben **Upload 'Single Sign On' SAML meta-data file** auf **Browse**, um die heruntergeladene Metadatendatei hochzuladen.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
 
	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Azure AD – einmaliges Anmelden][11]




### Erstellen einen Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt. Wählen Sie in der Benutzerliste **Britta Simon** aus.

![Azure AD-Benutzer erstellen][20]


**Um einen People-Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
 
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einen Azure AD-Testbenutzers](./media/active-directory-saas-people-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines People-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in People. People bietet keine Unterstützung für die Just-in-Time-Bereitstellung. Wenden Sie sich an das People-Supportteam, um manuell einen Benutzer zu erstellen.




### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf People erhält.

![Benutzer zuweisen][200]

**Zum Zuweisen von Britta Simon zu People führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **People** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-people-tutorial/tutorial_people_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel „People“ klicken, sollten Sie automatisch bei Ihrer People-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0413_2016-->