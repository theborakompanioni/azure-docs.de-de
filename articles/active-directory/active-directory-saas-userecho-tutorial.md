---
title: 'Tutorial: Azure Active Directory-Integration mit UserEcho | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und UserEcho konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 2d824d8d5eb8a25db128397b908a126bd87213ea
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Tutorial: Azure Active Directory-Integration mit UserEcho

In diesem Tutorial erfahren Sie, wie Sie UserEcho in Azure Active Directory (Azure AD) integrieren.

Die Integration von UserEcho in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf UserEcho Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei UserEcho anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit UserEcho konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein UserEcho-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von UserEcho aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-userecho-from-the-gallery"></a>Hinzufügen von UserEcho aus dem Katalog
Zum Konfigurieren der Integration von UserEcho in Azure AD müssen Sie UserEcho aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um UserEcho aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **UserEcho**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_search.png)

5. Wählen Sie im Ergebnisbereich die Option **UserEcho** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit UserEcho.

Für das einmalige Anmelden muss Azure AD wissen, welcher Benutzer in UserEcho als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in UserEcho muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in UserEcho den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei UserEcho müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines UserEcho-Testbenutzers](#creating-a-userecho-test-user)**, um ein Pendant von Britta Simon in UserEcho zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer UserEcho-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in UserEcho die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **UserEcho** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für UserEcho** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.userecho.com/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den UserEcho-Client](https://feedback.userecho.com/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **UserEcho-Konfiguration** auf **UserEcho konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der UserEcho-Unternehmenswebsite als Administrator an.

8. Klicken Sie auf der Symbolleiste oben auf Ihren Benutzernamen, um das Menü zu erweitern, und klicken Sie dann auf **Setup**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 

9. Klicken Sie auf **Integrations**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 

10. Klicken Sie auf **Website** und dann auf **Single sign-on (SAML2)** (Einmaliges Anmelden (SAML2)).
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 

11. Führen Sie auf der Seite **Single sign-on (SAML)** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png)
    
    a. Wählen Sie für **SAML-aktiviert** die Option **Ja**.
    
    b. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **SAML-SSO-URL** ein.
    
    c. Fügen Sie die **Abmelde-URL**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Remoteabmelde-URL** ein.
    
    d. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.
    
    e. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-userecho-test-user"></a>Erstellen eines UserEcho-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in UserEcho.

**Um einen Benutzer namens Britta Simon in UserEcho zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der UserEcho-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der Symbolleiste oben auf Ihren Benutzernamen, um das Menü zu erweitern, und klicken Sie dann auf **Setup**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png)

3. Klicken Sie auf **Benutzer**, um den Abschnitt **Benutzer** zu erweitern.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png)

4. Klicken Sie auf **Users**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png)

5. Klicken Sie auf **Invite a new user**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)

6. Führen Sie im Dialogfeld **Invite a new user** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png)

    a. Geben Sie im Textfeld **Name** den Namen des Benutzers, z.B. „Britta Simon“, ein.
    
    b.  Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers, z.B. Brittasimon@contoso.com, ein.
    
    c. Klicken Sie auf **Invite**.

Eine Einladung wird an Britta gesendet, die ihr Zugriff auf UserEcho gewährt. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf UserEcho gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon UserEcho zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **UserEcho**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel „UserEcho“ klicken, sollten Sie automatisch in Ihrer UserEcho-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png


