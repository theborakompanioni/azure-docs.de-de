<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Kronos | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Kronos konfigurieren."
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
	ms.date="09/19/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Kronos

In diesem Tutorial erfahren Sie, wie Sie Kronos in Azure Active Directory (Azure AD) integrieren.

Die Integration von Kronos in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Kronos hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Kronos anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Kronos konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein **Kronos Workforce Central-Abonnement**, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Kronos aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Kronos aus dem Katalog
Zum Konfigurieren der Integration von Kronos in Azure AD müssen Sie Kronos aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Kronos aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Kronos** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_01.png)

7. Wählen Sie im Ergebnisbereich **Kronos** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_06.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Kronos basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Kronos als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Kronos muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Kronos zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Kronos müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Kronos-Testbenutzers](#creating-an-kronos-test-user)**, um ein Gegenstück zu Britta Simon in Kronos zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Kronos-Anwendung.

Ihre Kronos-Anwendung erwartet die SAML-Assertions in einem bestimmten Format. Wenden Sie sich zunächst an das Kronos-Team, um die richtige Benutzer-ID zu ermitteln, die in der Anwendung zugeordnet wird. Halten Sie sich zudem in Bezug auf das Attribut, das für diese Zuordnung verwendet werden soll, an die Anweisungen des Kronos-Teams. Microsoft empfiehlt die Verwendung des Attributs **NameIdentifier** als Benutzer-ID. Sie können den Wert dieses Attributs auf der Registerkarte **Attribut** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Hier haben wir dem Anspruch „nameidentifier“ das Attribut **userprincipalname** zusammen mit der Funktion **ExtractMailPrefix** zugeordnet, das eine eindeutige Benutzer-ID bereitstellt. Diese wird in jeder erfolgreichen SAML-Antwort an die Kronos-Anwendung gesendet.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_07.png)


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Kronos die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Kronos** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	 ![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Kronos anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_04.png)


    a. Geben Sie im Textfeld „Bezeichner“ die URL, die von Ihren Benutzern zur Anmeldung bei der Kronos-Anwendung genutzt wird, nach folgendem Muster ein: `https://<company name>.kronos.net/`

	b. Geben Sie im Textfeld „Antwort-URL“ die URL nach folgendem Muster ein: `https://<company name>.kronos.net/wfc/navigator/logonWithUID`


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Kronos** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_05.png)

    a. Klicken Sie auf **Metadaten herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.


5.  Kontaktieren Sie Ihren Kronos-Account Manager, um Informationen zum richtigen Kanal für die SSO-Konfiguration für Ihre Anwendung zu erhalten. Bitte beachten Sie, dass Sie eine E-Mail senden und die heruntergeladene Metadatendatei anfügen müssen.
  
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
	
	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
  	
	![Azure AD – einmaliges Anmelden][11]



### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-kronos-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Kronos-Testbenutzers

In diesem Abschnitt erstellen Sie in Kronos einen Benutzer mit dem Namen Britta Simon. In der Kronos-Anwendung müssen alle Benutzer bereitgestellt werden, bevor das einmalige Anmelden verwendet werden kann. Wenden Sie sich daher an den Kronos-Kundensupport, um sämtliche Benutzer in der Anwendung bereitzustellen.


> [AZURE.NOTE] Wenn Sie einen Benutzer manuell oder eine Gruppe von Benutzern erstellen müssen, wenden Sie sich an das Kronos-Supportteam.


### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Kronos gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon zu Kronos zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **Kronos** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kronos-tutorial/tutorial_kronos_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel „Kronos“ klicken, sollten Sie automatisch bei Ihrer Kronos-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kronos-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->