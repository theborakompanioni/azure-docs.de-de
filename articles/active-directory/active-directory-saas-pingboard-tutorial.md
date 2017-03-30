---
title: 'Tutorial: Azure Active Directory-Integration mit PingBoard | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und PingBoard konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: d9daa59b1f20a76dfe18390ffdab008245387332
ms.lasthandoff: 03/22/2017


---
# <a name="tutorial-azure-active-directory-integration-with-pingboard"></a>Tutorial: Azure Active Directory-Integration mit PingBoard

In diesem Tutorial erfahren Sie, wie Sie PingBoard in Azure Active Directory (Azure AD) integrieren.

Die Integration von PingBoard in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf PingBoard haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei PingBoard anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit PingBoard konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges PingBoard-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von PingBoard aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-pingboard-from-the-gallery"></a>Hinzufügen von PingBoard aus dem Katalog
Zum Konfigurieren der Integration von PingBoard in Azure AD müssen Sie PingBoard aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um PingBoard aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **PingBoard** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/tutorial_PingBoard_search.png)

5. Wählen Sie im Ergebnisbereich die Option **PingBoard** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/tutorial_PingBoard_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei PingBoard mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in PingBoard als Gegenpart zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in PingBoard muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in PingBoard zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei PingBoard müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines PingBoard-Testbenutzers](#creating-a-pingboard-test-user)**, um eine Entsprechung von Britta Simon in PingBoard zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer PingBoard-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in PingBoard die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **PingBoard** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_PingBoard_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für PingBoard** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_PingBoard_url.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `http://<entity-id>.pingboard.com/sp` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<entity-id>.pingboard.com/auth/saml/consume`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von PingBoard](https://support.pingboard.com/), um diese Werte zu erhalten. 

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_pingboard_sp_initiated01.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `http://<sub-domain>.pingboard.com/sign_in`
     
5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_pingboard_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_general_400.png)

7. Öffnen Sie zum Konfigurieren des einmaligem Anmeldens auf PingBoard-Seite ein neues Browserfenster, und melden Sie sich bei Ihrem PingBoard-Konto an. Zum Einrichten des einmaligen Anmeldens müssen Sie ein PingBoard-Administrator sein.

8. Wählen Sie auf der oberen Menüleiste **Apps > Integrations** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/pingboard_integration.png)

9.    Wechseln Sie auf der Seite **Integrations** zur Kachel **Azure Active Directory**, und klicken Sie darauf.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/pingboard_aad.png)

10. Klicken Sie im geöffneten modalen Fenster auf **Configure**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/pingboard_configure.png)

11. Auf der folgenden Seite steht „Azure SSO Integration is enabled“. Öffnen Sie die heruntergeladene Metadaten-XML-Datei in Editor, und fügen Sie den Inhalt **IDP Metadata** hinzu.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/pingboard_sso_configure.png)

12. Die Datei wird überprüft. Wenn alles in Ordnung ist, ist einmaliges Anmelden nun aktiviert.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-PingBoard-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-pingboard-test-user"></a>Erstellen eines PingBoard-Testbenutzers

Damit sich Azure AD-Benutzer bei PingBoard anmelden können, müssen sie in PingBoard bereitgestellt werden.  
Im Fall von PingBoard ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der Unternehmenswebsite von PingBoard als Administrator an.

2. Klicken Sie auf der Seite **Directory** auf die Schaltfläche **Add Employee**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-PingBoard-tutorial/create_testuser_add.png)

3. Führen Sie auf der Dialogfeldseite **Add Employee** die folgenden Schritte aus.

    ![Invite People](./media/active-directory-saas-PingBoard-tutorial/create_testuser_name.png)

    a. Geben Sie im Textfeld **Full Name** den vollständigen Namen von Britta Simon ein.

    b. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    c. Geben Sie im Textfeld **Job Title** die Position von Britta Simon ein.

    d. Wählen Sie in der Dropdownliste **Location** den Standort von Britta Simon aus.
    
    e. Klicken Sie auf **Add**.    

4. Ein Bestätigungsfenster wird eingeblendet, in dem das Hinzufügen der Benutzerin bestätigt wird.
    
    ![Bestätigen](./media/active-directory-saas-PingBoard-tutorial/create_testuser_confirm.png)
        
    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf PingBoard gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu PingBoard durchzuführen:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **PingBoard**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-PingBoard-tutorial/tutorial_pingboard_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „PingBoard“ klicken, sollten Sie automatisch bei Ihrer PingBoard-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-PingBoard-tutorial/tutorial_general_203.png

