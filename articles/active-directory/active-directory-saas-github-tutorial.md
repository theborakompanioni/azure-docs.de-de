---
title: 'Tutorial: Azure Active Directory-Integration mit GitHub | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und GitHub konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4395bd95-05de-4deb-87a5-dc3bc8ac4d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 8b0417d131bc25225e777285b9e4bfbaa97aad8f
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017

---
# <a name="tutorial-azure-active-directory-integration-with-github"></a>Tutorial: Azure Active Directory-Integration mit GitHub

In diesem Tutorial erfahren Sie, wie Sie GitHub in Azure Active Directory (Azure AD) integrieren.

Die Integration von GitHub in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf GitHub hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei GitHub anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit GitHub konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein GitHub-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von GitHub aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-github-from-the-gallery"></a>Hinzufügen von GitHub aus dem Katalog
Zum Konfigurieren der Integration von GitHub in Azure AD müssen Sie GitHub aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um GitHub aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **GitHub.com** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/tutorial_github_search02.png)

5. Wählen Sie im Ergebnisbereich die Option **GitHub** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/tutorial_github_search_result02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei GitHub mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in GitHub als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in GitHub muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in GitHub zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei GitHub müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines GitHub-Testbenutzers](#creating-a-GitHub-test-user)**, um eine Entsprechung von Britta Simon in GitHub zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer GitHub-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in GitHub die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **GitHub** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für GitHub** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_saml011.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `https://github.com/orgs/<entity-id>/sso`

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://github.com/orgs/<entity-id>`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Navigieren Sie zum Abschnitt „GitHub Admin“ (GitHub-Verwaltung), um diese Werte abzurufen. 

4. Wählen Sie im Abschnitt **Benutzerattribute** für **Benutzer-ID** die Option „user.mail“.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_attribute_new01.png)
    
5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_03.png)

6. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein **Ablaufdatum** aus. Klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_general_300.png)

7. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Make new certificate active** (Neues Zertifikat zum aktiven Zertifikat machen), und klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_04.png)

8. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_general_400.png)

9. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_05.png) 

10. Klicken Sie im Abschnitt **GitHub-Konfiguration** auf **GitHub konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_06.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_07.png)

11. Melden Sie sich in einem anderen Webbrowserfenster bei der GitHub-Organisationswebsite als Administrator an.

12. Navigieren Sie zu **Einstellungen**, und klicken Sie auf **Sicherheit**.

    ![Einstellungen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_03.png)

13. Aktivieren Sie das Kontrollkästchen **SAML-Authentifizierung aktivieren**, um die Felder für die Konfiguration des einmaligen Anmeldens anzuzeigen. Verwenden Sie anschließend den Wert für die SSO-URL, um die URL für das einmalige Anmelden in der Azure AD-Konfiguration zu aktualisieren.

    ![Einstellungen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_13.png)

14. Konfigurieren Sie die folgenden Felder:

    a. **Anmelde-URL**: Geben Sie die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **GitHub konfigurieren** in Azure AD ein.

    b. **Aussteller**: Geben Sie die **SAML-Entitäts-ID** aus dem Abschnitt **GitHub konfigurieren** in Azure AD ein.

    c. **Öffentliches Zertifikat**: Öffnen Sie das heruntergeladene Zertifikat aus Azure AD in einem Editor, und kopieren Sie den Inhalt, einschließlich „BEGIN CERTIFICATE“ und „END CERTIFICATE“.

    ![Einstellungen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_051.png)

15. Klicken Sie auf **Test SAML configuration** (SAML-Konfiguration testen), um sich zu vergewissern, dass beim einmaligen Anmelden keine Validierungsfehler oder anderen Fehler auftreten.

    ![Einstellungen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_06.png)

16. Klicken Sie unten auf der Seite auf **Speichern**

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-github-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 


### <a name="creating-a-github-test-user"></a>Erstellen eines GitHub-Testbenutzers

Damit sich Azure AD-Benutzer bei GitHub anmelden können, müssen sie in GitHub bereitgestellt werden.  
Im Fall von GitHub wird die Bereitstellung manuell durchgeführt.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der GitHub-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **People**.

    ![Personen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_08.png "Personen")

3. Klicken Sie auf **Invite member** (Mitglied einladen).

    ![Benutzer einladen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_09.png "Benutzer einladen")

4. Führen Sie auf der Dialogfeldseite **Invite member** (Mitglied einladen) die folgenden Schritte aus:

    a. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    ![Personen einladen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_10.png "Personen einladen")
    
    b. Klicken Sie auf **Einladung senden**.

    ![Personen einladen](./media/active-directory-saas-github-tutorial/tutorial_github_config_github_11.png "Personen einladen")

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf GitHub gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu GitHub durchzuführen:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **GitHub.com** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-github-tutorial/tutorial_github_search_result021.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die GitHub-Kachel klicken, sollten Sie bei Ihrer GitHub-Anwendung angemeldet werden. Sie melden sich mit einem Organisationskonto an, aber anschließend müssen Sie sich mit Ihrem persönlichen Konto anmelden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-github-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-github-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-github-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-github-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-github-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-github-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-github-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-github-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-github-tutorial/tutorial_general_203.png

