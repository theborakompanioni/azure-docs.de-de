<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Amazon Web Service (AWS) | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie Amazon Web Services (AWS) mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
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
	ms.date="06/06/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit Amazon Web Service (AWS)

Dieses Tutorial soll Ihnen zeigen, wie Sie Amazon Web Service (AWS) in Azure Active Directory (Azure AD) integrieren können. Die Integration von Amazon Web Service (AWS) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Amazon Web Service (AWS) Zugriff hat. 
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Service (AWS) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen 

Um die Azure AD-Integration mit Amazon Web Service (AWS) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement.
- Ein Abonnement, bei dem einmaliges Anmelden für Amazon Web Service (AWS) aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern. 

 
## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus drei großen Bausteinen:

1. Hinzufügen von Amazon Web Service (AWS) aus dem Katalog 
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Amazon Web Service (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Service (AWS) in Azure AD müssen Sie Amazon Web Service (AWS) aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

### Um Amazon Web Service (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
   
	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Amazon Web Service (AWS)** ein.
   
	![Anwendungen][5]

7. Wählen Sie im Ergebnisbereich **Amazon Web Service (AWS)** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Anwendungen][6]



##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens "Britta Simon" das einmalige Anmelden von Azure AD in Amazon Web Service (AWS) konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in Amazon Web Service (AWS) zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Service (AWS) muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert zum **Benutzernamen** in Amazon Web Service (AWS) zuweisen.
 
Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Service (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Testbenutzers für Amazon Web Service (AWS)](#creating-a-halogen-software-test-user)**, um eine Entsprechung von Britta Simon in Amazon Web Service (AWS) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** – um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren der einmaligen Anmeldung in Azure AD

Das Ziel dieses Abschnitts ist es, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Azure in Ihrer Amazon Web Service (AWS)-Anwendung zu konfigurieren. Die Amazon Web Service (AWS)-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:


![Einmaliges Anmelden konfigurieren][27]

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Service (AWS) die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Service (AWS)** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][7]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Amazon Web Service (AWS) anmelden** **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren][8]

3. Klicken Sie auf der Seite **App-Einstellungen konfigurieren** auf „Weiter“.

	![Konfigurieren von App-Einstellungen][9]
 
4. Klicken Sie auf der Seite **Einmaliges Anmelden für Amazon Web Service (AWS) konfigurieren** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.

	![Einmaliges Anmelden konfigurieren][10]

5. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Service (AWS) als Administrator an.

6. Klicken Sie auf die **Startseite der Konsole**.

	![Einmaliges Anmelden konfigurieren][11]

7. Klicken Sie auf **Identitäts- und Zugriffsverwaltung**.

	![Einmaliges Anmelden konfigurieren][12]

8. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**.

	![Einmaliges Anmelden konfigurieren][13]

9. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][14]

     a. Wählen Sie für **Anbietertyp** die Option **SAML** aus.

     b. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (z. B.: *WAAD*).

     c. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.

     d. Klicken Sie auf **Nächster Schritt**.


10. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**.

	![Einmaliges Anmelden konfigurieren][15]

11. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Neue Rolle erstellen**.

	![Einmaliges Anmelden konfigurieren][16]

12. Führen Sie im Dialogfeld **Rollennamen festlegen** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][17]

	a. Geben Sie im Textfeld **Rollenname** einen Rollennamen ein (z. B.: *TestUser*).

	b. Klicken Sie auf **Nächster Schritt**.

13. Führen Sie im Dialogfeld **Rollentyp auswählen** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][18]

    a. Wählen Sie **Rolle für Identitätsanbieterzugriff** aus.

    b. Klicken Sie im Abschnitt **Zugriff mit einmaliger Webanmeldung für SAML-Anbieter gewähren** auf **Auswählen**.


14. Führen Sie im Dialogfeld **Vertrauensstellung herstellen** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][19]
     
     a. Wählen Sie als SAML-Anbieter den zuvor erstellten SAML-Anbieter aus (z. B.: *WAAD*).

     b. Klicken Sie auf **Nächster Schritt**.


15. Klicken Sie im Dialogfeld **Vertrauenswürdigkeit der Rolle überprüfen** auf **Nächster Schritt**.

	![Einmaliges Anmelden konfigurieren][32]


16. Klicken Sie im Dialogfeld **Richtlinie anhängen** auf **Nächster Schritt**.

	![Einmaliges Anmelden konfigurieren][33]


17. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][34]

     a. Kopieren Sie den Wert **Rollen-ARN**.

     b. Kopieren Sie den ARN-Wert **Vertrauenswürdige Entitäten**.

     c. Klicken Sie auf **Rolle erstellen**.

18. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

	![Was ist Azure AD Connect?][20]

19. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.

	![Was ist Azure AD Connect?][22]


20. Klicken Sie oben im Menü auf **Attribute**, um das Dialogfeld **SAML-Tokenattribute** zu öffnen.

	![Einmaliges Anmelden konfigurieren][21]

21. Klicken Sie auf **Benutzerattribut hinzufügen**.

	![Einmaliges Anmelden konfigurieren][23]

22. Führen Sie im Dialogfeld „Benutzerattribut hinzufügen“ die folgenden Schritte aus.

	![Einmaliges Anmelden konfigurieren][24]

    a. Geben Sie im Textfeld **Attributname** die Zeichenfolge **https://aws.amazon.com/SAML/Attributes/Role** ein.

    b. Geben Sie im Textfeld **Attributwert** **[den Rollen-ARN-Wert],[den ARN-Wert für die vertrauenswürdige Entität]** ein.

    >[AZURE.TIP] Dies sind die Werte, die Sie bei der Erstellung der Rolle im Dialogfeld "Überprüfung" kopiert haben.

    c. Klicken Sie auf **Abschließen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.

23. Klicken Sie auf **Benutzerattribut hinzufügen**.

	![Einmaliges Anmelden konfigurieren][23]


24. Führen Sie im Dialogfeld „Benutzerattribut hinzufügen“ die folgenden Schritte aus.

	![Einmaliges Anmelden konfigurieren][25]


     a. Geben Sie im Textfeld **Attributname** die Zeichenfolge **https://aws.amazon.com/SAML/Attributes/RoleSessionName** ein.

     b. Geben Sie im Textfeld **Attributwert** den Wert **userprincipalname** ein.

     c. Klicken Sie auf **Abschließen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.


25. Klicken Sie auf **Änderungen übernehmen**.

	![Einmaliges Anmelden konfigurieren][26]





### Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Erstellen einen Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png)
 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png)

  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.
  3. Klicken Sie auf Weiter.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png)

	a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

	b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  
	c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

	d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  
	e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png)
 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png)

	a. Notieren Sie den Wert von **Neues Kennwort**.
  
	b. Klicken Sie auf **Fertig stellen**.
  
 
### Erstellen eines Testbenutzers für Amazon Web Service (AWS)

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Amazon Web Service (AWS).

### Um einen Benutzer namens Britta Simon in Amazon Web Service (AWS) zu erstellen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei der Unternehmenswebsite von **Amazon Web Service (AWS)** als Administrator an.

2. Klicken Sie auf das Symbol für die **Startseite der Konsole**.

	![Einmaliges Anmelden konfigurieren][11]

3. Klicken Sie auf „Identitäts- und Zugriffsverwaltung“.

	![Einmaliges Anmelden konfigurieren][28]

4. Klicken Sie im Dashboard auf „Benutzer“ und dann auf „Neue Benutzer erstellen“.

	![Einmaliges Anmelden konfigurieren][29]

5. Führen Sie im Dialogfeld „Benutzer erstellen“ die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren][30]

     a. Geben Sie in die Textfelder unter **Benutzernamen eingeben** den Benutzernamen („userprincipalname“) von Britta Simon in Azure AD ein.

     b. Klicken Sie auf **Erstellen**.




### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist es, Britta Simon für das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf Amazon Web Service (AWS) erhält.

![Benutzer zuweisen][31]

**Um Britta Simon CloudPassage zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][26]

2. Wählen Sie in der Anwendungsliste **Amazon Web Service (AWS)** aus.

	![Benutzer zuweisen][27]

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][25]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][29]

### Testen der einmaligen Anmeldung

In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden. Wenn Sie im Zugriffsbereich auf die Kachel "Amazon Web Service (AWS)" klicken, sollten Sie automatisch in Ihrer Anwendung Amazon Web Service (AWS) angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png

<!---HONumber=AcomDC_0608_2016-->