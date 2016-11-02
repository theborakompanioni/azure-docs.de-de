<properties
	pageTitle="Tutorial: Azure Active Directory-Integration von ImageRelay | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ImageRelay konfigurieren."
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
	ms.date="08/15/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration von ImageRelay

Dieses Tutorial soll Ihnen zeigen, wie Sie ImageRelay in Azure Active Directory (Azure AD) integrieren können. Diese Integration von ImageRelay bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf ImageRelay Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ImageRelay anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration von ImageRelay konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ImageRelay-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ImageRelay aus dem Katalog

2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von ImageRelay aus dem Katalog
Zum Konfigurieren der Integration von ImageRelay in Azure AD müssen Sie ImageRelay aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um ImageRelay aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld **ImageRelay** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. Wählen Sie im Ergebnisbereich **ImageRelay** aus, und klicken Sie dann auf **Fertig stellen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand einer Testbenutzerin namens „Britta Simon“ veranschaulicht werden, wie das einmalige Anmelden von Azure AD bei ImageRelay konfiguriert und getestet werden kann.

Damit das einmalige Anmelden funktioniert, benötigt Azure AD ein Benutzerkonto, das für einen entsprechenden Benutzer in ImageRelay steht. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ImageRelay muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Wert **Benutzername** aus Azure AD dem Wert **Benutzername** in ImageRelay zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD bei ImageRelay müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden von Azure AD mit der Testbenutzerin Britta Simon zu testen.
4. **[Erstellen eines ImageRelay-Testbenutzers](#creating-a-userecho-test-user)**, um eine Entsprechung von Britta Simon in ImageRelay zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure AD-Portal zu aktivieren und das einmalige Anmelden in der ImageRelay-Anwendung zu konfigurieren.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei ImageRelay die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ImageRelay** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

     ![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ImageRelay anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei der ImageRelay-Anwendung verwenden (z.B. *https://fabrikam.ImageRelay.com/*).

    b. Klicken Sie auf **Next**.

4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für ImageRelay** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.

5. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrer ImageRelay-Unternehmenswebsite als Administrator an.

1. Klicken Sie auf der Symbolleiste oben auf die Workload **Users & Permissions** (Benutzer und Berechtigungen).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png)

1. Klicken Sie auf **Create New Permission** (Neue Berechtigung erstellen).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png)

1. Aktivieren Sie in der Workload **Single Sign On Settings** (Einstellungen für einmaliges Anmelden) das Kontrollkästchen **This Group can only sign-in via Single Sign On** (Diese Gruppe kann sich nur per einmaligem Anmelden anmelden), und klicken Sie dann auf **Speichern**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png)

1. Wechseln Sie zu **Kontoeinstellungen**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png)

1. Klicken Sie auf die Workload **Single Sign On Settings** (Einstellungen für einmaliges Anmelden).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Führen Sie im Dialogfeld **SAML Settings** (SAML-Einstellungen) die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

	a. Kopieren Sie im klassischen Azure-Portal die **Dienst-URL für einmaliges Anmelden**, und fügen Sie sie in das Textfeld **Anmelde-URL** ein.


	b. Kopieren Sie im klassischen Azure-Portal die **Dienst-URL für einmalige Abmeldung**, und fügen Sie sie in das Textfeld **Abmelde-URL** ein.

	c. Wählen Sie unter **Name Id Format** (Format der Namens-ID) die Option **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress** aus.

	
    d. Wählen Sie unter **Binding Options for Requests from the Service Provider (Image Relay)** (Bindungsoptionen für Anforderungen vom Dienstanbieter (Image Relay)) die Option **POST Binding** (POST-Bindung) aus.
   

	e. Klicken Sie unter **x.509-Zertifikat** auf **Zertifikat aktualisieren**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld „x.509-Zertifikat“ ein.
  
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. Aktivieren Sie im Abschnitt **Just-In-Time User Provisioning** (Just-In-Time-Benutzerbereitstellung) das Kontrollkästchen **Enable Just-In-Time User Provisioning** (Just-In-Time-Benutzerbereitstellung aktivieren).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Wählen Sie die Berechtigungsgruppe aus (z.B. **SSO Basic** (SSO allgemein)), bei der die Anmeldung nur per einmaligem Anmelden erfolgen soll.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. Klicken Sie auf **Speichern**.

6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

    ![Azure AD – einmaliges Anmelden][11]


### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines ImageRelay-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in ImageRelay.

**Um eine Benutzerin namens Britta Simon in ImageRelay zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer ImageRelay-Unternehmenswebsite als Administrator an.

1. Wechseln Sie zu **Users & Permissions** (Benutzer und Berechtigungen), und klicken Sie auf **Create SSO User** (SSO-Benutzer erstellen).

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png)

1. Geben Sie die E-Mail-Adresse, den Vornamen, den Nachnamen und das Unternehmen des bereitzustellenden Benutzers an. Wählen Sie außerdem die Berechtigungsgruppe aus (z.B. „SSO Basic“ (SSO allgemein)). Dies ist die Gruppe, die sich nur per einmaligem Anmelden anmelden kann.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png)

1. Klicken Sie auf **Create**.

### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf ImageRelay erhält.

![Benutzer zuweisen][200]

**Um Britta Simon zu ImageRelay zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der Verzeichnisansicht im oberen Menü auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **ImageRelay** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel „ImageRelay“ klicken, sollten Sie automatisch bei Ihrer ImageRelay-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0817_2016-->