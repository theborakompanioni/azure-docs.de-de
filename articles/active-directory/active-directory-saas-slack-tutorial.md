---
title: 'Tutorial: Azure Active Directory-Integration mit Slack | Microsoft-Dokumentation'
description: "Hier erfahren Sie, wie Sie Slack mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: c7a0b761-75b7-4e6b-9980-71d645643a78
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/21/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4d5144c6a690c2338dec45b27dcb308328b6fecb
ms.openlocfilehash: 37440a8ba397c4dc227a448dfa574cebd14be49c


---

# <a name="tutorial-azure-active-directory-integration-with-slack"></a>Tutorial: Azure Active Directory-Integration mit Slack

Dieses Tutorial soll Ihnen zeigen, wie Sie Slack in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Slack in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Slack hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Slack anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Slack konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Slack-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
>  Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Slack aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-slack-from-the-gallery"></a>Hinzufügen von Slack aus dem Katalog
Um die Integration von Slack in Azure AD zu konfigurieren, müssen Sie Slack über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Slack über den Katalog hinzuzufügen:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
    
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
    
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie in das Suchfeld den Suchbegriff **Slack** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/tutorial_slack_01.png)

7. Wählen Sie im Ergebnisbereich **Slack** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Auswählen der App im Katalog](./media/active-directory-saas-slack-tutorial/tutorial_slack_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in Slack konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Slack als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Slack muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Slack zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Slack müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Slack-Testbenutzers](#creating-a-slack-test-user)**, um eine Entsprechung von Britta Simon in Slack zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Slack-Anwendung zu konfigurieren.

Die Slack-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute auf der Registerkarte**Attribute**der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_09.png)

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Slack die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **Slack** im Menü oben auf **Attribute**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_03.png)

2. Führen Sie im Dialogfeld **SAML-Tokenattribute** für jede Zeile in der folgenden Tabelle die folgenden Schritte aus:

    | Attributname | Attributwert |
    | --- | --- |    
    | User.Email | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Username | extractmailprefix([Benutzerprinzipalname]) |

    a. Klicken Sie auf **Benutzerattribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_04.png)
    
    b. Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Attributwert** den für die Zeile angezeigten Attributwert ein.
    
    d. Klicken Sie unten auf der Seite auf **Abschließen**

3. Klicken Sie im oberen Menü auf **Schnellstart**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_05.png) 

4. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Slack anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_06.png)

5. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_07.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.slack.com`.

    b. Klicken Sie auf **Weiter**.

    > [!NOTE] 
    > Beachten Sie hierbei, dass Sie den Wert mit der richtigen Anmelde-URL aktualisieren müssen. Wenden Sie sich an das Supportteam von Slack, um diesen Wert zu erhalten.

6. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Slack** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_08.png)

7.  Melden Sie sich in einem anderen Webbrowserfenster bei der Slack-Unternehmenswebsite als Administrator an.

8.  Navigieren Sie zu **Microsoft Azure AD** und dann zu **Teameinstellungen**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

9.  Klicken Sie im Abschnitt **Teameinstellungen** auf die Registerkarte **Authentifizierung**, und klicken Sie dann auf **Einstellungen ändern**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

10. Führen Sie im Dialogfeld **SAML-Authentifizierungseinstellungen** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a.  Geben Sie im Textfeld **SAML 2.0 Endpoint (HTTP)** den Wert der **SAML-SSO-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

    b.  Geben Sie im Textfeld **Aussteller des Identitätsanbieters** den Wert für die **Aussteller-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

    c.  Öffnen Sie das heruntergeladene Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Öffentliches Zertifikat** ein.

    d.  Deaktivieren Sie **Benutzern gestatten, ihre E-Mail-Adresse zu ändern**.

    e.  Wählen Sie **Benutzern gestatten, ihren eigenen Benutzernamen zu wählen**aus.

    f.  Wählen Sie für **Authentifizierung für Ihr Team muss verwendet werden von** die Option **Ist optional** aus.

    g.  Klicken Sie auf **Konfiguration speichern**.

11. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]

12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-slack-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-a-slack-test-user"></a>Erstellen eines Slack-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Slack. Slack unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Slack ein neuer Benutzer erstellt.

> [!NOTE] 
> Setzen Sie sich mit dem Supportteam von Slack in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem ihr Zugriff auf Slack gewährt wird.
    
![Benutzer zuweisen][200]

**Um Britta Simon Slack zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
    
    ![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste die Option **Slack** aus.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_50.png)

3. Klicken Sie im oberen Menü auf **Benutzer**.
    
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
 
Wenn Sie im Zugriffsbereich auf die Kachel „Slack“ klicken, sollten Sie automatisch bei Ihrer Slack-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-slack-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-slack-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-slack-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-slack-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-slack-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO4-->


