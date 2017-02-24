---
title: 'Tutorial: Azure Active Directory-Integration mit Slack | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Slack konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 14972d3e1212fb0cf6653bd4a693470425294d2c
ms.openlocfilehash: c975231ea18c7c1853d9b20fc49542c10ef9abcc


---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Tutorial: Azure Active Directory-Integration mit Slack

In diesem Tutorial erfahren Sie, wie Sie Slack in Azure Active Directory (Azure AD) integrieren.

Die Integration von Slack in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Slack hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Slack anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Slack konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Slack-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Slack aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-slack-from-the-gallery"></a>Hinzufügen von Slack aus dem Katalog
Um die Integration von Slack in Azure AD zu konfigurieren, müssen Sie Slack über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Slack über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld den Suchbegriff **Slack** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/tutorial_slack_000.png)

5. Wählen Sie im Ergebnisbereich die Option **Slack** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/tutorial_slack_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Slack.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Slack als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Slack muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Slack zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Slack müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Slack-Testbenutzers](#creating-a-slack-test-user)**, um eine Entsprechung von Britta Simon in Slack zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Slack-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Slack die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Slack** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Slack** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_02.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.slack.com`

    b. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `https://slack.com`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Hier empfehlen wir Ihnen, den eindeutigen Wert der URL im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von Slack](https://slack.com/help/contact), um diese Werte zu erhalten. 

4. Die Slack-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_03.png)

5. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** den Eintrag **user.mail** als **Benutzerbezeichner** aus, und führen Sie für jede in der folgenden Tabelle aufgeführte Zeile die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | --- | --- |    
    | User.Email | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Username | extractmailprefix([Benutzerprinzipalname]) |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_06.png)     

7. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein **Ablaufdatum** aus. Klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_general_300.png)

8. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Neues Zertifikat aktivieren**, und klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_07.png)

9. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

10. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_08.png) 

11. Klicken Sie im Abschnitt **Slack-Konfiguration** auf **Slack konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_09.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_10.png)

12.  Melden Sie sich in einem anderen Webbrowserfenster bei der Slack-Unternehmenswebsite als Administrator an.

13.  Navigieren Sie zu **Microsoft Azure AD** und dann zu **Teameinstellungen**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

14.  Klicken Sie im Abschnitt **Teameinstellungen** auf die Registerkarte **Authentifizierung**, und klicken Sie dann auf **Einstellungen ändern**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

15. Führen Sie im Dialogfeld **SAML-Authentifizierungseinstellungen** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  Geben Sie im Textfeld **SAML 2.0-Endpunkt (HTTP)** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    b.  Geben Sie im Textfeld **Aussteller des Identitätsanbieters** den Wert für die **SAML-Entitäts-ID** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c.  Öffnen Sie das heruntergeladene Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Öffentliches Zertifikat** ein.

    d. Konfigurieren Sie die oben genannten drei Einstellungen gemäß den Anforderungen Ihres Slack-Teams. Weitere Informationen zu den Einstellungen finden Sie hier im **SSO-Konfigurationshandbuch für Slack**. `https://get.slack.help/hc/en-us/articles/220403548-Guide-to-single-sign-on-with-Slack`

    e.  Klicken Sie auf **Konfiguration speichern**.
     
    <!-- Deaktivieren Sie **Benutzern gestatten, ihre E-Mail-Adresse zu ändern**.

    e.  Wählen Sie **Benutzern gestatten, ihren eigenen Benutzernamen zu wählen**aus.

    f.  Wählen Sie für **Authentifizierung für Ihr Team muss verwendet werden von** die Option **Ist optional** aus. -->
  

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 



### <a name="creating-a-slack-test-user"></a>Erstellen eines Slack-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Slack. Slack unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Slack ein neuer Benutzer erstellt.

> [!NOTE]
> Setzen Sie sich mit dem [Supportteam von Slack](https://slack.com/help/contact) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Slack gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Slack zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie dann zur Verzeichnisansicht, wechseln Sie zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Slack** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Slack“ klicken, sollten Sie automatisch bei Ihrer Slack-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png


<!--HONumber=Feb17_HO1-->


