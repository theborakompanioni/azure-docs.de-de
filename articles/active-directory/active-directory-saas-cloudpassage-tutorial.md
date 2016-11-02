<properties
    pageTitle="Tutorial: Azure Active Directory-Integration mit CloudPassage | Microsoft Azure"
    description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und CloudPassage konfigurieren."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>



# <a name="tutorial:-azure-active-directory-integration-with-cloudpassage"></a>Tutorial: Azure Active Directory-Integration mit CloudPassage

In diesem Tutorial erfahren Sie, wie Sie CloudPassage in Azure Active Directory (Azure AD) integrieren.  
Diese Integration bietet folgende Vorteile: 

- Sie können in Azure AD steuern, wer auf CloudPassage Zugriff hat. 
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei CloudPassage anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – in Azure Active Directory. 


Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen 

Um die Azure AD-Integration mit CloudPassage konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement.
- Ein CloudPassage-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie nicht über eine Azure AD-Testumgebung verfügen, erhalten Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine kostenlose, einmonatige Azure-Testversion. 

 
## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.  
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von CloudPassage aus dem Katalog 
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-cloudpassage-from-the-gallery"></a>Hinzufügen von CloudPassage aus dem Katalog
Zum Konfigurieren der Integration von CloudPassage in Azure AD müssen Sie CloudPassage aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

### <a name="to-add-cloudpassage-from-the-gallery,-perform-the-following-steps:"></a>Um CloudPassage aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
 
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld die Zeichenfolge **CloudPassage**ein.

    ![Anwendungen][5]

7. Wählen Sie im Ergebnisbereich die Option **CloudPassage** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Anwendungen][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens "Britta Simon" das einmalige Anmelden von Azure AD in CloudPassage konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in CloudPassage zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in CloudPassage muss eine Linkbeziehung eingerichtet werden.  
Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in CloudPassage zuweisen.
 
Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei CloudPassage müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren der einmaligen Anmeldung in Azure AD](#configuring-azure-ad-single-single-sign-on)** – um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines CloudPassage-Testbenutzers](#creating-a-halogen-software-test-user)** – um eine Entsprechung von Britta Simon in CloudPassage zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden in Azure AD im klassischen Azure AD-Portal zu aktivieren und es in Ihrer CloudPassage-Anwendung zu konfigurieren.  
Die CloudPassage-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden konfigurieren][21]

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in CloudPassage die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure AD-Portal auf der Anwendungsintegrationsseite für **CloudPassage** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren][7]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei CloudPassage anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren][8]

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus: 

    ![App-Einstellungen konfigurieren][9]
 
    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern zum Anmelden an der CloudPassage-App verwendet wird (z.B. *https://portal.cloudpassage.com/saml/init/accountid*). 

    b. Geben Sie im Textfeld **Antwort-URL** Ihre AssertionConsumerService-URL ein (z.B. *https://portal.cloudpassage.com/saml/consume/accountid*). Sie können Ihren Wert für dieses Attribut abrufen, indem Sie auf die **Dokumentation der SSO-Einrichtung** im Abschnitt mit den **Einstellungen für einmaliges Anmelden** Ihres CloudPassage-Portals klicken.  
    ![Einmaliges Anmelden konfigurieren][10]

    C. Klicken Sie auf **Weiter**.



4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für CloudPassage** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer. 

    ![Einmaliges Anmelden konfigurieren][11]

5. Melden Sie sich in einem anderen Browserfenster bei der CloudPassage-Unternehmenswebsite als Administrator an.

6. Klicken Sie im Menü am oberen Rand auf **Einstellungen** und dann auf **Websiteverwaltung**. 

    ![Einmaliges Anmelden konfigurieren][12]

7. Klicken Sie auf die Registerkarte **Authentifizierungseinstellungen** . 

    ![Einmaliges Anmelden konfigurieren][13]


8. Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus: 

    ![Einmaliges Anmelden konfigurieren][14]


    a. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für CloudPassage** den Wert der **Aussteller-URL**, und fügen Sie ihn in das Textfeld **URL des SAML-Ausstellers** ein.

    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für CloudPassage** den Wert von **Vom Dienstanbieter initiierter Endpunkt**, und fügen Sie ihn in das Textfeld **URL des SAML-Endpunkts** ein.

    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für CloudPassage** den Wert für **Abmelde-URL**, und fügen Sie ihn in das Textfeld **Zielseite der Abmeldung** ein.

    d. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat. 
          
    >[AZURE.TIP] Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).

    e. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.

    f. Klicken Sie auf **Speichern**.


9. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 

    ![Einmaliges Anmelden konfigurieren][15]


10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**. 

    ![Einmaliges Anmelden konfigurieren][16]



11. Klicken Sie im Menü am oberen Rand auf **Attribute**, um das Dialogfeld **SAML-Tokenattribute** zu öffnen. 

    ![Einmaliges Anmelden konfigurieren][17]

12. Führen Sie für jede Zeile in der folgenden Tabelle die folgenden Schritte aus, um die erforderlichen Benutzerattribute hinzuzufügen: 

  	| Attributname | Attributwert |
  	| --- | --- |
  	| firstname | user.givenname |
  	| lastname | user.surname |
  	| email | user.mail |

 

    a. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][18]

    b. Geben Sie im Textfeld **Attributname** den für die entsprechende Zeile angezeigten Attributnamen ein, und wählen Sie als **Attributwert** den für diese Zeile angezeigten Attributwert aus. 

    ![Einmaliges Anmelden konfigurieren][19]
     
    c. Klicken Sie auf **Fertig stellen**.


13. Klicken Sie dann auf der Symbolleiste am unteren Rand auf **Änderungen übernehmen**. 

    ![Einmaliges Anmelden konfigurieren][20]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.  

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_01.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_02.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_03.png) 
 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  
    c. Klicken Sie auf Weiter.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_06.png) 
  
    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  
    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  
    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  
    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_07.png) 
 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-cloudpassage-tutorial/create_aaduser_08.png) 
  
    a. Notieren Sie den Wert von **Neues Kennwort**.
  
    b. Klicken Sie auf **Fertig stellen**.   


  
 
### <a name="creating-a-cloudpassage-test-user"></a>Erstellen eines CloudPassage-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in CloudPassage.

#### <a name="to-create-a-user-called-britta-simon-in-cloudpassage,-perform-the-following-steps:"></a>Um einen Benutzer namens Britta Simon in CloudPassage zu erstellen, führen Sie die folgenden Schritte aus:

1.  Melden Sie sich bei der **CloudPassage** -Unternehmenswebsite als Administrator an. 

2.  Klicken Sie auf der Symbolleiste am oberen Rand auf **Einstellungen** und dann auf **Websiteverwaltung**. 

    ![Erstellen eines CloudPassage-Testbenutzers][22] 

3.  Klicken Sie auf die Registerkarte **Benutzer** und anschließend auf **Neuen Benutzer hinzufügen**. 

    ![Erstellen eines CloudPassage-Testbenutzers][23]
    
4.  Führen Sie im Abschnitt **Neuen Benutzer hinzufügen** die folgenden Schritte aus: 

    ![Erstellen eines CloudPassage-Testbenutzers][24]

    a. Geben Sie in das Textfeld **Vorname** den Namen Britta ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen Simon ein.

    c. Geben Sie in die Textfelder **Benutzername**, **E-Mail** und **E-Mail erneut eingeben** Brittas Benutzernamen in Azure AD ein.

    d. Wählen Sie als **Zugriffstyp** die Option **Halo-Portalzugriff aktivieren** aus.

    e. Klicken Sie auf **Hinzufügen**.










### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf CloudPassage erhält.

![Benutzer zuweisen][30]

**Um Britta Simon CloudPassage zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Azure-Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][26]

2. Wählen Sie in der Anwendungsliste die Option **CloudPassage**aus.

    ![Benutzer zuweisen][27]

1. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][25]

1. Wählen Sie in der Benutzerliste **Britta Simon**aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

    ![Benutzer zuweisen][29]



### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.  
Wenn Sie im Zugriffsbereich auf die Kachel "CloudPassage" klicken, sollten Sie automatisch in Ihrer CloudPassage-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_01.png
[6]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_02.png
[7]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_05.png
[8]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_03.png
[9]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_04.png
[10]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_05.png
[11]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_06.png
[12]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_07.png
[13]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_08.png
[14]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_09.png
[15]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_10.png
[16]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_11.png
[17]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_10.png
[18]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_11.png
[19]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_12.png
[20]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_14.png
[21]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_14.png
[22]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[23]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_16.png
[24]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_17.png
[25]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_12.png
[27]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_13.png
[28]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_cloudpassage_15.png
[29]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-cloudpassage-tutorial/tutorial_general_17.png
























<!--HONumber=Oct16_HO2-->


