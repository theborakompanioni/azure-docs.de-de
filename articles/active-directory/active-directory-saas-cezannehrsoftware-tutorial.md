<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Cezanne HR Software | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cezanne HR Software konfigurieren."
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


# Tutorial: Azure Active Directory-Integration mit Cezanne HR Software

Dieses Tutorial soll Ihnen zeigen, wie Sie Cezanne HR Software in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Cezanne HR Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Cezanne HR Software Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Cezanne HR Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Cezanne HR Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cezanne HR Software-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Cezanne HR Software aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Cezanne HR Software aus dem Katalog
Zum Konfigurieren der Integration von Cezanne HR Software in Azure AD müssen Sie Cezanne HR Software aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Cezanne HR Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
	
	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
	
	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie in das Suchfeld den Suchbegriff **Cezanne HR Software** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Wählen Sie im Ergebnisbereich **Cezanne HR Software** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Auswählen der App im Katalog](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in Cezanne HR Software konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Cezanne HR Software zu einem Benutzer in Azure AD ist. Anders ausgedrückt muss zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cezanne HR Software eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in Cezanne HR Software zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Cezanne HR Software müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Cezanne HR Software-Testbenutzers](#creating-a-cezanne-hr-software-test-user)**, um eine Entsprechung von Britta Simon in Cezanne HR Software zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer Cezanne HR Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Cezanne HR Software die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Cezanne HR Software** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
	 
	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Cezanne HR Software anmelden** die Option **Azure AD einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

	a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `https://w3.cezanneondemand.com/CezanneOnDemand/`.

	c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

	d. Klicken Sie auf **Weiter**.

	> [AZURE.NOTE] Beachten Sie Folgendes: Sie müssen diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL ersetzen. Wenden Sie sich unter <mailto:info@cezannehr.com> an das Supportteam von Cezanne HR Software, um diese Werte zu erhalten.

4. Führen Sie auf der Seite **Einmaliges Anmelden bei Cezanne HR Software konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Next**.

5. Melden Sie sich in einem anderen Webbrowserfenster an Ihrem Cezanne HR Software-Mandanten als Administrator an.

6. Klicken Sie im linken Navigationsbereich auf **System Setup** (Systeminstallation). Wechseln Sie zu **Sicherheitseinstellungen**. Navigieren Sie anschließend zu **Single Sign-On Configuration** (Konfiguration des einmaligen Anmeldens).

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. Aktivieren Sie im Bereich **Allow users to log in using the following Single Sign-On (SSO) Service** (Benutzeranmeldung per SSO-Dienst (Single Sign-On) zulassen) das Kontrollkästchen **SAML 2.0**, und wählen Sie daneben die Option **Erweiterte Konfiguration** aus.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Klicken Sie auf die Schaltfläche **Neu hinzufügen**.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. Führen Sie im Abschnitt **SAML 2.0 IDENTITY PROVIDERS** (SAML 2.0-IDENTITÄTSANBIETER) die folgenden Schritte aus:

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

	a. Geben Sie den Namen Ihres Identitätsanbieters als **Anzeigename** ein.

	b. Geben Sie im Textfeld **Entitätsbezeichner** den Wert für die **Entitäts-ID** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	c. Ändern Sie **SAML-Bindung** in „POST“.

	d. Geben Sie im Textfeld **Security Token Service Endpoint** (Sicherheitstoken-Dienstendpunkt) den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	e. Geben Sie „http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name“ unter **Benutzer-ID-Attributname** ein.

	f. Klicken Sie auf das Symbol **Hochladen**, um das heruntergeladene Zertifikat aus Azure AD hochzuladen.

	g. Klicken Sie auf die Schaltfläche **OK**.

10. Klicken Sie auf die Schaltfläche **Save**.

	![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
	![Azure AD – einmaliges Anmelden][10]

12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
    
	![Azure AD – einmaliges Anmelden][11]



### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
    
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Cezanne HR Software-Testbenutzers

Damit sich Azure AD-Benutzer bei Cezanne HR Software anmelden können, müssen sie in Cezanne HR Software bereitgestellt werden. Bei Cezanne HR Software ist die Bereitstellung ein manueller Vorgang.

####Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1.  Melden Sie sich als Administrator auf Ihrer Cezanne HR Software-Unternehmenswebsite an.

2.  Klicken Sie im linken Navigationsbereich auf **System Setup** (Systeminstallation). Navigieren Sie zu **Benutzer verwalten**. Navigieren Sie anschließend zu **Neuen Benutzer hinzufügen**.

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Neuer Benutzer")

3.  Führen Sie im Abschnitt **Person Details** (Angaben zur Person) die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Neuer Benutzer")

	a. Legen Sie **Interner Benutzer** auf „AUS“ fest.

	b. Geben Sie in das Textfeld **Vorname** **Britta** ein.

    c. Geben Sie in das Textfeld **Nachname** **Simon** ein.

	d. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

4.  Führen Sie unter **Kontoinformationen** die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Neuer Benutzer")

	a. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse von Britta Simon ein.

	b. Geben Sie im Textfeld **Kennwort** das Kennwort des Kontos von Britta Simon ein.

	c. Wählen Sie unter **Sicherheitsrolle** die Option **HR Professional** (HR-Mitarbeiter).

	d. Klicken Sie auf **OK**.

5. Navigieren Sie zur Registerkarte **Einmalige Anmeldung**, und wählen Sie im Bereich **SAML 2.0 Identifiers** (SAML 2.0-Bezeichner) die Option **Neu hinzufügen**.

	![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Benutzer")

6. Wählen Sie unter **Identitätsanbieter** Ihren Identitätsanbieter aus, und geben Sie im Textfeld von **Benutzer-ID** die E-Mail-Adresse des Kontos von Britta Simon ein.

	![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Benutzer")
	
7. Klicken Sie auf die Schaltfläche **Save**.

	![Benutzer](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Benutzer")


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist es, Britta Simon für das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf Cezanne HR Software erhält.
	
![Benutzer zuweisen][200]

**Um Britta Simon Cezanne HR Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
    
	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **Cezanne HR Software** aus.
    
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.
    
	![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
	![Benutzer zuweisen][205]

### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
 
Wenn Sie im Zugriffsbereich auf die Kachel „Cezanne HR Software“ klicken, sollten Sie automatisch an Ihrer Cezanne HR Software-Anwendung angemeldet werden.

## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->