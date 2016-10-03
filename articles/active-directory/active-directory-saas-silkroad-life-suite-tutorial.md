<properties
	pageTitle="Tutorial: Azure Active Directory-Integration in SilkRoad Life Suite | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und der SilkRoad Life Suite konfigurieren."
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


# Tutorial: Azure Active Directory-Integration in die SilkRoad Life Suite

In diesem Tutorial erfahren Sie, wie Sie die SilkRoad Life Suite in Azure Active Directory (Azure AD) integrieren. Die Integration der SilkRoad Life Suite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf die SilkRoad Life Suite zugreifen kann.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei der SilkRoad Life Suite anzumelden (einmaliges Anmelden).

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen 

Um die Azure AD-Integration in die SilkRoad Life Suite konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SilkRoad Life Suite-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

 
## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen der SilkRoad Life Suite aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen der SilkRoad Life Suite aus dem Katalog
Zum Konfigurieren der Integration der SilkRoad Life Suite in Azure AD müssen Sie die SilkRoad Life Suite aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um die SilkRoad Life Suite aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie in das Suchfeld **SilkRoad Life Suite** ein.

	![Anwendungen][5]

7. Wählen Sie im Ergebnisbereich **SilkRoad Life Suite** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Anwendungen][50]


##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in der SilkRoad Life Suite konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in der SilkRoad Life Suite einem Benutzer in Azure AD entspricht. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in der SilkRoad Life Suite muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen in Azure AD dem Benutzernamen in der SilkRoad Life Suite zuweisen.
 
Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei der SilkRoad Life Suite müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines SilkRoad Life Suite-Testbenutzers](#creating-a-silkroad-life-suite-test-user)**, um eine Entsprechung von Britta Simon in der SilkRoad Life Suite zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in der SilkRoad Life Suite-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei der SilkRoad Life Suite die folgenden Schritte aus:**

5. Melden Sie sich bei Ihrer SilkRoad Life Suite-Unternehmenswebsite als Administrator an.


    > [AZURE.NOTE] Um Zugriff auf die Authentifizierungsanwendung der Anwendung SilkRoad Life Suite zu erhalten und den Verbund mit Microsoft Azure AD zu konfigurieren, wenden Sie sich an den SilkRoad-Support oder Ihren Vertriebsmitarbeiter für SilkRoad-Dienste.


6. Wechseln Sie zu **Dienstanbieter**, und klicken Sie dann auf **Verbunddetails**.

	![Azure AD – einmaliges Anmelden][10]


1. Klicken Sie auf **Download Federation Metadata** (Verbundmetadaten herunterladen), und speichern Sie die Metadatendatei dann auf Ihrem Computer.

	![Azure AD – einmaliges Anmelden][11]

3. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SilkRoad Life Suite** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SilkRoad Life Suite anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][7]

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Azure AD – einmaliges Anmelden][8]
 
    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei der SilkRoad Life Suite-Website verwenden (beispielsweise *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).

    b. Öffnen Sie die heruntergeladene **Silkroad**-Metadatendatei.

    c. Suchen Sie das Tag **AssertionConsumerService**, und kopieren Sie dann das Attribut **Location**.

	![Azure AD – einmaliges Anmelden][21]
   
    d. Fügen Sie den Wert im Textfeld **Antwort-URL** ein.
 
    e. Klicken Sie auf **Next**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für SilkRoad Life Suite** die folgenden Schritte aus:

	![Azure AD – einmaliges Anmelden][9]

    a. Klicken Sie auf "Zertifikat herunterladen", und speichern Sie das Zertifikat auf Ihrem Computer.

    b. Klicken Sie auf **Next**.




1. Klicken Sie in der Anwendung **SilkRoad** auf **Authentication Sources** (Authentifizierungsquellen).

	![Azure AD – einmaliges Anmelden][12]



1. Klicken Sie auf **Add Authentication Source** (Authentifizierungsquelle hinzufügen).

	![Azure AD – einmaliges Anmelden][13]



1. Führen Sie im Abschnitt **Add Authentication Source** (Authentifizierungsquelle hinzufügen) die folgenden Schritte aus:

	![Azure AD – einmaliges Anmelden][14]

    a. Klicken Sie unter **Option 2 - Metadata File** (Option 2 – Metadatendatei) auf **Browse** (Durchsuchen), um die heruntergeladene Metadatendatei hochzuladen.

    b. Klicken Sie auf **Create Identity Provider using File Data** (Identitätsanbieter mithilfe von Dateidaten erstellen).



1. Klicken Sie im Abschnitt **Authentication Sources** (Authentifizierungsquellen) auf **Edit** (Bearbeiten).

	![Azure AD – einmaliges Anmelden][15]


1. Führen Sie im Dialogfeld **Edit Authentication Source** (Authentifizierungsquelle bearbeiten) die folgenden Schritte aus:

	![Azure AD – einmaliges Anmelden][16]

    a. Wählen Sie für **Enabled** (Aktiviert) die Option **Yes** (Ja) aus.

    b. Geben Sie im Textfeld **IdP Description** (IdP-Beschreibung) eine Beschreibung für die Konfiguration ein (etwa *Azure AD SSO*).

    c. Geben Sie im Textfeld **IdP Name** (IdP-Name) einen für Ihre Konfiguration spezifischen Namen ein (etwa *Azure SP*).

    d. Klicken Sie auf **Speichern**.


6. Deaktivieren Sie alle anderen Authentifizierungsquellen.

	![Azure AD – einmaliges Anmelden][17]

7. Klicken Sie im klassischen Azure-Portal auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Weiter**.

	![Azure AD – einmaliges Anmelden][18]

1. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Azure AD – einmaliges Anmelden][19]


### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png)
 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)
 
    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Rolle **Benutzer** aus. e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png)
 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)
  
    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.

  
 
### Erstellen eines SilkRoad Life Suite-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in der SilkRoad Life Suite. Britta benötigt eine SSO-ID (auch *AuthParam* genannt), die Brittas **E-Mail-Adresse** in Azure AD entspricht.

**Führen Sie die folgenden Schritte aus, um eine Benutzerin namens Britta Simon in der SilkRoad Life Suite zu erstellen:**

1. Bitten Sie das SilkRoad Life Suite-Supportteam, einen Benutzer zu erstellen, dessen Attribut für die **SSO-ID** der **E-Mail-Adresse** von Britta Simon in Azure AD entspricht.



### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem ihr Zugriff auf die SilkRoad Life Suite gewährt wird.

![Benutzer zuweisen][200]

**Führen Sie die folgenden Schritte aus, um Britta Simon der SilkRoad Life Suite zuzuweisen:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **SilkRoad Life Suite** aus.

	![Benutzer zuweisen][202]

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel für die SilkRoad Life Suite klicken, sollten Sie automatisch an Ihrer SilkRoad Life Suite-Anwendung angemeldet werden.


## Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->