---
title: 'Tutorial: Azure Active Directory-Integration mit LinkedIn Learning | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedIn Learning konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d5857070-bf79-4bd3-9a2a-4c1919a74946
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 6ad28cb3adaa63ddc3d3769a650d26ca6a7e2695
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-learning"></a>Tutorial: Azure Active Directory-Integration mit LinkedIn Learning

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Learning in Azure Active Directory (Azure AD) integrieren.

Die Integration von LinkedIn Learning in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Learning hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Learning anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LinkedIn Learning konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LinkedIn Learning-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von LinkedIn Learning aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-linkedin-learning-from-the-gallery"></a>Hinzufügen von LinkedIn Learning aus dem Katalog
Zum Konfigurieren der Integration von LinkedIn Learning in Azure AD müssen Sie LinkedIn Learning aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Learning aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie **LinkedIn Learning** im Suchfeld ein. Wählen Sie im Ergebnisbereich die Option **LinkedIn Learning** aus, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_000.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Learning mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LinkedIn Learning als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Learning muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in LinkedIn Learning zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei LinkedIn Learning über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines LinkedIn Learning-Testbenutzers](#creating-a-linkedin-learning-test-user)** – um das einmalige Anmelden über Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LinkedIn Learning-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei LinkedIn Learning über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Learning** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedin_01.png)

3. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem LinkedIn Learning-Mandanten als Administrator an.

4. Klicken Sie im **Account Center** unter **Settings** auf **Global Settings**. Wählen Sie außerdem aus der Dropdownliste die Option **Learning - Default** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_01.png)

5. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, und kopieren Sie die **Entitäts-ID** und **Assertion Consumer Access(ACS)-URL**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_03.png)

6. Führen Sie im Azure-Portal unter **Domäne und URLs für LinkedIn Learning** die folgenden Schritte aus, wenn Sie das einmalige Anmelden im Modus **IdP-initiiert** konfigurieren möchten.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_01.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie im Feld **Antwort-URL** den Wert für **Assertion Consumer Access (ACS) Url** ein, den Sie aus dem LinkedIn-Portal kopiert haben.

7. Wenn Sie das einmalige Anmelden im Modus **SP-initiiert** konfigurieren möchten, klicken Sie im Konfigurationsabschnitt auf die Option „Erweiterte URL-Einstellungen anzeigen“, und konfigurieren Sie die Anmelde-URL nach folgendem Muster:

    `https://www.linkedin.com/checkpoint/enterprise/login/<AccountId>?application=learning&applicationInstanceId=<InstanceId>`

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_signon_02.png)   
    
8. Die LinkedIn Learning-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, LinkedIn Learning erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/updateusermail.png)
    
9. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest. Der Benutzer muss vier Ansprüche mit den Namen **email**, **department**, **firstname** und **lastname** hinzufügen, und der Wert muss mit **user.mail**, **user.department**, **user.givenname** bzw. **user.surname** zugeordnet werden.

    | Attributname | Attributwert |
    | --- | --- |
    | E-Mail| user.mail |    
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/userattribute.png)
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld zum Attribut zu öffnen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_04.png)

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinLearning-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

10. Führen Sie für das Attribut **Name** die folgenden Schritte aus:

    a. Klicken Sie auf das Attribut, um das Fenster **Attribut bearbeiten** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinLearning-tutorial/url_update.png)

    b. Löschen Sie den URL-Wert aus dem **Namespace**.
    
    c. Klicken Sie auf **OK**, um die Einstellung zu speichern.

11. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_certificate.png) 

12. Klicken Sie auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_400.png)

13. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Laden Sie die XML-Datei hoch, die Sie gerade aus dem Azure-Portal heruntergeladen haben, indem Sie auf die Option „XML-Datei hochladen“ klicken.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_metadata_03.png)

14. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial_linkedin_admin_05.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinlearning-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 

### <a name="creating-a-linkedin-learning-test-user"></a>Erstellen eines LinkedIn Learning-Testbenutzers

Die LinkedIn Learning-Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Schalten Sie im LinkedIn Learning-Portal auf der Seite mit den Administratoreinstellungen die Option **Automatically Assign licenses** ein, um die Just-in-Time-Bereitstellung zu aktivieren. Dadurch wird dem Benutzer auch eine Lizenz zugewiesen.
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinLearning-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf LinkedIn Learning gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu LinkedIn Learning zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **LinkedIn Learning** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinlearning-tutorial/tutorial-linkedinlearning_0001.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LinkedIn Learning“ klicken, sollte die Azure-Anmeldeseite geöffnet werden. Nach der erfolgreichen Anmeldung auf diese Seite sollten Sie Ihre LinkedIn Learning-Anwendung öffnen können.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinlearning-tutorial/tutorial_general_203.png
