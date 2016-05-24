<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit OpsGenie | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OpsGenie konfigurieren."
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
	ms.date="04/26/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit OpsGenie

Dieses Tutorial soll Ihnen zeigen, wie Sie OpsGenie in Azure Active Directory (Azure AD) integrieren können.

Die Integration von OpsGenie in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf OpsGenie hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei OpsGenie anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit OpsGenie konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein OpsGenie-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von OpsGenie aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von OpsGenie aus dem Katalog
Zum Konfigurieren der Integration von OpsGenie in Azure AD müssen Sie OpsGenie aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um OpsGenie aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **OpsGenie** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Wählen Sie im Ergebnisbereich **OpsGenie** aus und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens „Britta Simon“ veranschaulicht werden, wie das einmalige Anmelden von Azure AD in OpsGenie konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in OpsGenie als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OpsGenie muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in OpsGenie zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei OpsGenie müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines OpsGenie-Testbenutzers](#creating-a-opsgenie-test-user)**, um eine Entsprechung von Britta Simon in OpsGenie zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer OpsGenie-Anwendung zu konfigurieren.



**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei OpsGenie die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **OpsGenie** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei OpsGenie anmelden?** die Option **Azure AD – einmaliges Anmelden** aus und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png)


    a. Geben Sie im Textfeld „Anmelde-URL“ die URL ein, die von Ihren Benutzern nach folgendem Muster zur Anmeldung bei der OpsGenie-Anwendung verwendet wird: **„https://app.opsgenie.com/auth/login“**.

    > [AZURE.NOTE] Wenn Sie Ihre Anmelde-URL benötigen, wenden Sie sich an das [OpsGenie-Supportteam](mailto:support@opsgenie.com).

    b. Klicken Sie auf **Weiter**.


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für OpsGenie** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer. Dieses Zertifikat und die Metadaten-URLs (Entitäts-ID, Anmelde-URL für SSO und Abmelde-URL für SSO) werden benötigt, um SSO (Single Sign-On, einmaliges Anmelden) aufseiten von OpsGenie einzurichten.

    b. Klicken Sie auf **Weiter**.


5. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich bei OpsGenie als Administrator an.

6. Klicken Sie auf **Settings** und dann auf die Registerkarte **Single Sign On**.
 
	![Einmaliges Anmelden bei OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

7. Um SSO (Single Sign-On, einmaliges Anmelden) zu aktivieren, wählen Sie **Enabled** aus.

	![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png)
   
8. Klicken Sie im Abschnitt **Provider** auf die Registerkarte **Azure Active Directory**.

	![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png)
    
9. Führen Sie auf der Dialogfeldseite "Azure Active Directory" die folgenden Schritte aus:
 
	![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)

    a. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für OpsGenie** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn ins Textfeld **SAML 2.0 Endpoint** ein.

    b. Erstellen Sie eine Base64-codierte Datei aus dem heruntergeladenen Zertifikat.
    
    > [AZURE.NOTE] Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be) (in englischer Sprache).

    c. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.500 Certificate** ein.

    d. Klicken Sie auf **Änderungen speichern**.


6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Azure AD – einmaliges Anmelden][11]




### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.



![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines OpsGenie-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in OpsGenie.

1.	Melden Sie sich in einem Webbrowserfenster bei Ihrem OpsGenie-Mandanten als Administrator an.

2.	Navigieren Sie zur Liste der Benutzer, indem Sie im linken Bereich auf **User** klicken.
   
	![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png)

3.	Klicken Sie auf **Add User**.

3.	Führen Sie im Dialogfeld **Add User** die folgenden Schritte aus:

	![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)

    a. Geben Sie im Textfeld **Email** die E-Mail-Adresse von Britta Simon in Azure Active Directory ein.

    b. Geben Sie im Textfeld **Full Name** den Namen **Britta Simon** ein.

    c. Klicken Sie auf **Speichern**.

Daraufhin erhält Britta Simon eine E-Mail mit Anweisungen zum Einrichten ihres Profils.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf OpsGenie erhält.

![Benutzer zuweisen][200]

**Um Britta Simon OpsGenie zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
 
	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **OpsGenie** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.

Wenn Sie im Zugriffsbereich auf die Kachel „OpsGenie“ klicken, sollten Sie automatisch bei Ihrer OpsGenie-Anwendung angemeldet werden.


## Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0511_2016-->