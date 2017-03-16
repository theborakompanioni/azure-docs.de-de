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
ms.sourcegitcommit: 9c027decf5d982519bc9f41aa9588fa431ef3975
ms.openlocfilehash: 17ad8938cbee539e74f87210077a12c72e777edc
ms.lasthandoff: 02/24/2017


---
# <a name="tutorial-azure-ad-integration-with-slack"></a>Tutorial: Azure AD-Integration mit Slack

In diesem Tutorial erfahren Sie, wie Sie Slack in Azure Active Directory (Azure AD) integrieren.

Bei der Integration von Slack in Azure AD haben Sie folgende Möglichkeiten:

* Sie können in Azure AD steuern, wer Zugriff auf Slack hat.
* Ermöglichen Sie Benutzern das automatische einmalige Anmelden (SSO) an Slack mit ihren Azure AD-Konten.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Slack konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SSO-fähiges Slack-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

* Verwenden Sie Ihre Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. Das zu befolgende Szenario besteht aus zwei Hauptbausteinen:

* Hinzufügen von Slack aus dem Katalog
* Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD

## <a name="add-slack-from-the-gallery"></a>Hinzufügen von Slack aus dem Katalog
Um die Integration von Slack in Azure AD zu konfigurieren, fügen Sie Slack wie folgt über den Katalog Ihrer Liste mit den verwalteten SaaS-Apps hinzu:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im linken Bereich auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“][1]

3. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen**.

    ![Schaltfläche „Alle Anwendungen“ auf dem Blatt „Unternehmensanwendungen“][2]

4. Klicken Sie oben im Dialogfeld **Alle Anwendungen** auf **Hinzufügen**.

    ![Schaltfläche „Hinzufügen“ im Dialogfeld „Alle Anwendungen“][3]

5. Geben Sie in das Suchfeld den Suchbegriff **Slack** ein.

    ![Suchfeld „Eine Anwendung hinzufügen“](./media/active-directory-saas-slack-tutorial/tutorial_slack_000.png)

6. Wählen Sie im Ergebnisbereich die Option **Slack**, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Auswählen von „Slack“ im Ergebnisbereich](./media/active-directory-saas-slack-tutorial/tutorial_slack_0001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Slack basierend auf einem Testbenutzer namens Britta Simon.

Damit SSO funktioniert, muss von Azure AD eine Linkbeziehung zwischen dem Azure AD-Benutzer und seiner Entsprechung in Slack hergestellt werden. Sie stellen die Linkbeziehung her, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Slack zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Slack müssen Sie die folgenden Bausteine ausführen:

1. [Konfigurieren Sie das einmalige Anmelden von Azure AD](#configuring-azure-ad-single-sign-on), damit der Benutzer dieses Feature nutzen kann.
2. [Erstellen Sie einen Azure AD-Testbenutzer](#creating-an-azure-ad-test-user), um das einmalige Anmelden für Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. [Erstellen Sie einen Slack-Testbenutzer](#creating-a-slack-test-user), um für die Azure AD-Benutzerin Britta Simon eine Entsprechung in Slack einzurichten.
4. [Weisen Sie den Azure AD-Testbenutzer zu](#assigning-the-azure-ad-test-user), um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. [Testen Sie das einmalige Anmelden](#testing-single-sign-on), um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD

In diesem Abschnitt aktivieren Sie wie folgt das einmalige Anmelden von Azure AD im Azure-Portal und führen die entsprechende Konfiguration in Ihrer Slack-Anwendung durch:

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Slack** auf **Einmaliges Anmelden**.

    ![Seite für die Integration der Slack-Anwendung][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** in der Liste **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.

    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-slack-tutorial/tutorial_slack_01.png)

3. Führen Sie unter **Domäne und URLs für Slack** die folgenden Schritte aus:

    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-slack-tutorial/tutorial_slack_02.png)

    a. Geben Sie im Feld **Anmelde-URL** eine URL mit der Benennungskonvention *https://<company name>.slack.com* ein.

    b. Geben Sie im Feld **Bezeichner** die URL **https://slack.com** ein.

    > [!NOTE]
    > Die vorangehenden Werte sind keine echten Werte. Hier empfehlen wir Ihnen, für die URL und den Bezeichner eindeutige Werte zu verwenden. Später aktualisieren Sie die Werte mit der tatsächlichen URL und dem Bezeichner. Wenden Sie sich an das [Supportteam von Slack](https://slack.com/help/contact), um diese Werte zu erhalten.

4. Die Slack-Anwendung erwartet, dass die SAML-Assertionen (Security Assertion Markup Language) in einem bestimmten Format angezeigt werden. Konfigurieren Sie die Ansprüche, und verwalten Sie die Werte der Attribute im Abschnitt **Benutzerattribute** der Seite für die Integration der Slack-Anwendung. Dies ist im folgenden Screenshot dargestellt:

    ![Konfigurieren von Ansprüchen im Abschnitt „Benutzerattribute“](./media/active-directory-saas-slack-tutorial/tutorial_slack_03.png)

5. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** unter **Benutzer-ID** die Option **user.mail**. Führen Sie für jede Zeile der Tabelle die folgenden Schritte aus:

    | Attributname | Attributwert |
    | --- | --- |    
    | User.Email | user.userprincipalname |
    | first_name | user.givenname |
    | last_name | user.surname |
    | User.Username | extractmailprefix([Benutzerprinzipalname]) |

    a. Klicken Sie auf die Schaltfläche **Attribut hinzufügen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_04.png)

    b. Geben Sie im Dialogfeld **Attribut hinzufügen** im Feld **Name** den ersten Namen aus der Spalte **Attributname** der Tabelle ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-slack-tutorial/tutorial_slack_05.png)

    c. Geben Sie im Feld **Wert** den ersten Wert aus der Spalte **Attributwert** der Tabelle ein.

    d. Klicken Sie auf **OK**.

    e. Wiederholen Sie die Schritte „a“ bis „d“ für die nächsten drei Tabellenzeilen.

6. Klicken Sie unter **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Erstellen eines Zertifikats](./media/active-directory-saas-slack-tutorial/tutorial_slack_06.png)     

7. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf die Schaltfläche **Kalender**, wählen Sie ein Ablaufdatum aus, und klicken Sie dann auf **Speichern**.

    ![Auswählen eines Ablaufdatums für das Zertifikat](./media/active-directory-saas-slack-tutorial/tutorial_general_300.png)

8. Aktivieren Sie unter **SAML-Signaturzertifikat** das Kontrollkästchen **Neues Zertifikat aktivieren**, und klicken Sie dann auf **Speichern**.

    ![Aktivieren des SAML-Signaturzertifikats](./media/active-directory-saas-slack-tutorial/tutorial_slack_07.png)

9. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Popupfenster „Rolloverzertifikat“](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

10. Klicken Sie unter **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem lokalen Datenträger.

    ![Speichern des Zertifikats auf Ihrem lokalen Datenträger](./media/active-directory-saas-slack-tutorial/tutorial_slack_08.png)

11. Klicken Sie unter **Slack-Konfiguration** auf **Slack konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Klicken auf „Slack konfigurieren“ zum Öffnen des Fensters „Anmeldung konfigurieren“](./media/active-directory-saas-slack-tutorial/tutorial_slack_09.png)

    ![Fenster „Anmeldung konfigurieren“](./media/active-directory-saas-slack-tutorial/tutorial_slack_10.png)

12. Öffnen Sie ein neues Browserfenster, und melden Sie sich dann als Administrator auf Ihrer Slack-Unternehmenswebsite an.

13. Navigieren Sie zu **Microsoft Azure AD** und dann zu **Teameinstellungen**.

    ![Schaltfläche „Teameinstellungen“ von Microsoft Azure AD auf der Slack-Unternehmenswebsite](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

14. Klicken Sie unter **Teameinstellungen** auf die Registerkarte **Authentifizierung**, und klicken Sie dann auf **Einstellungen ändern**.

    ![Schaltfläche „Einstellungen ändern“ auf der Seite „Teameinstellungen“](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

15. Führen Sie im Dialogfeld **SAML-Authentifizierungseinstellungen** die folgenden Schritte aus:

    ![Dialogfeld „SAML-Authentifizierungseinstellungen“](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    a. Geben Sie im Feld **SAML 2.0 Endpoint (HTTP)** (SAML 2.0-Endpunkt (HTTP)) den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    b. Geben Sie im Feld **Aussteller des Identitätsanbieters** den Wert **SAML-Entitäts-ID** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c. Öffnen Sie die heruntergeladene Zertifikatsdatei im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Feld **Öffentliches Zertifikat** ein.

    d. Konfigurieren Sie die oben genannten drei Einstellungen gemäß den Anforderungen Ihres Slack-Teams. Weitere Informationen zu den Einstellungen finden Sie unter [Guide für Einmaliges Anmelden in Slack](https://get.slack.help/hc/en-us/articles/220403548-Guide-to-single-sign-on-with-Slack).

    e. Klicken Sie auf **Konfiguration speichern**.

    <!-- Deaktivieren Sie **Benutzern gestatten, ihre E-Mail-Adresse zu ändern**.

    e. Wählen Sie **Benutzern gestatten, ihren eigenen Benutzernamen zu wählen**aus.

    f. Wählen Sie für **Authentifizierung für Ihr Team muss verwendet werden von** die Option **Ist optional** aus. -->


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Azure AD-Schaltfläche „Alle Benutzer“](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**.

    ![Schaltfläche „Hinzufügen“ im Dialogfeld „Alle Benutzer“](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png)

4. Geben Sie im Dialogfeld **Benutzer** die folgenden Informationen ein:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-slack-test-user"></a>Erstellen eines Slack-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Slack erstellt. Slack unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Slack vorhanden ist, wird beim Versuch, auf Slack zuzugreifen, ein neuer Benutzer erstellt.

> [!NOTE]
> Setzen Sie sich mit dem [Supportteam von Slack](https://slack.com/help/contact) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie dem Benutzer Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Slack gewähren.

![Zuweisen eines Benutzers des einmaligen Anmeldens von Azure][200]

Führen Sie die folgenden Schritte aus, um Slack den Benutzer Britta Simon zuzuweisen.

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht und dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201]

2. Wählen Sie in der Liste **Anwendungen** die Option **Slack** aus.

    ![Anwendungsliste im Azure-Portal](./media/active-directory-saas-slack-tutorial/tutorial_slack_50.png)

3. Klicken Sie auf der linken Seite auf **Benutzer und Gruppen**.

    ![Schaltfläche „Benutzer und Gruppen“ im linken Bereich][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**, und wählen Sie dann auf dem Blatt **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Schaltfläche „Hinzufügen“ und Blatt „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Klicken Sie auf die Schaltfläche **Auswählen**.

7. Klicken Sie auf dem Blatt **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Navigieren Sie zum Testen der Konfiguration zum Zugriffsbereich, und klicken Sie dann auf die Kachel **Slack**. Der Benutzer sollte automatisch an der Slack-Anwendung angemeldet werden.


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

