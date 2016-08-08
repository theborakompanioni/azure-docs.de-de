<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit eDigitalResearch | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und eDigitalResearch konfigurieren."
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


# Tutorial: Azure Active Directory-Integration mit eDigitalResearch

In diesem Tutorial erfahren Sie, wie Sie eDigitalResearch in Azure Active Directory (Azure AD) integrieren.

Die Integration von eDigitalResearch in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf eDigitalResearch hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei eDigitalResearch anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit eDigitalResearch konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein eDigitalResearch-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von eDigitalResearch aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von eDigitalResearch aus dem Katalog
Zum Konfigurieren der Integration von eDigitalResearch in Azure AD müssen Sie eDigitalResearch über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um eDigitalResearch aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
	
	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie in das Suchfeld **eDigitalResearch** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_01.png)
7. Wählen Sie im Ergebnisbereich **eDigitalResearch** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Auswählen der App im Katalog](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_0001.png)


##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in eDigitalResearch konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in eDigitalResearch als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in eDigitalResearch muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in eDigitalResearch zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei eDigitalResearch müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines eDigitalResearch-Testbenutzers](#creating-a-edigitalresearch-test-user)**, um eine Entsprechung von Britta Simon in eDigitalResearch zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer eDigitalResearch-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens in Azure AD mit eDigitalResearch die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **eDigitalResearch** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei eDigitalResearch anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_03.png)

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_04.png)

    a. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein:

	`https://<company name>.edigitalresearch.com/login/consume`

	b. Klicken Sie auf **Weiter**.

	> [AZURE.NOTE] Der Wert der Antwort-URL ist in diesem Tutorial nur ein Platzhalter. Den tatsächlichen Wert für Ihre Umgebung können Sie bei eDigitalResearch erfragen.

4. Führen Sie auf der Seite **Einmaliges Anmelden bei eDigitalResearch konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.

5. Wenden Sie sich an das Supportteam von eDigitalResearch, um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:

	  - Die **heruntergeladene Zertifikatdatei**
	
	  - Die **Entitäts-ID**
	
	  - Die **Dienst-URL für einmaliges Abmelden**

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Azure AD – einmaliges Anmelden][11]



### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.
	
![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
	
	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-edigitalresearch-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines eDigitalResearch-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in eDigitalResearch.

Arbeiten Sie mit dem Supportteam von eDigitalResearch zusammen, um Benutzer zu erstellen.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist, für Britta Simon das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf eDigitalResearch erhält.
	
![Benutzer zuweisen][200]

**Um Britta Simon eDigitalResearch zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **eDigitalResearch** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-edigitalresearch-tutorial/tutorial_edigitalresearch_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.
	
	![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „eDigitalResearch“ klicken, sollten Sie automatisch bei Ihrer eDigitalResearch-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-edigitalresearch-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0727_2016-->