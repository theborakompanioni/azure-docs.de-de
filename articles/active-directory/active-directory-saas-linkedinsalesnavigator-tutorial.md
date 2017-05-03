---
title: 'Tutorial: Integration von Azure Active Directory in LinkedInSalesNavigator | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedInSalesNavigator konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 69143ccdbfbf50701d44e0342e8eb4ebe68b22da
ms.lasthandoff: 04/21/2017


---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>Tutorial: Integration von Azure Active Directory in LinkedIn Sales Navigator

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Sales Navigator in Azure Active Directory (Azure AD) integrieren.

Die Integration von LinkedIn Sales Navigator in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Sales Navigator hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Sales Navigator anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Integration von Azure AD in LinkedIn Sales Navigator konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LinkedIn Sales Navigator-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von LinkedIn Sales Navigator aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Hinzufügen von LinkedIn Sales Navigator aus dem Katalog
Zum Konfigurieren der Integration von LinkedIn Sales Navigator in Azure AD müssen Sie LinkedIn Sales Navigator aus dem Katalog zu Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Sales Navigator aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld **LinkedIn Sales Navigator** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

5. Wählen Sie im Ergebnisbereich die Option **LinkedIn Sales Navigator** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Sales Navigator mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LinkedIn Sales Navigator als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Sales Navigator muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in LinkedIn Sales Navigator zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei LinkedIn Sales Navigator über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines LinkedIn Sales Navigator-Testbenutzers](#creating-a-linkedin-sales-navigator-test-user)**, um eine Entsprechung von Britta Simon in LinkedIn Sales Navigator zu erhalten, die mit der Darstellung des Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LinkedIn Sales Navigator-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei LinkedIn Sales Navigator über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Sales Navigator** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

3. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrer **LinkedIn Sales Navigator**-Website als Administrator an.

4. Klicken Sie im **Kontocenter** unter **Einstellungen** auf **Globale Einstellungen**. Wählen Sie außerdem **Sales Navigator** aus der Dropdownliste aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

5. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, und kopieren Sie die **Entitäts-ID** und **Assertion Consumer Access(ACS)-URL**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

6. Führen Sie im Azure-Portal unter **Domäne und URLs für LinkedIn Sales Navigator** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie im Textfeld **Antwort-URL** die **Assertion Consumer Access(ACS)-URL** ein, die Sie aus dem LinkedIn-Portal kopiert haben.

7. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`.

8. Die **LinkedIn Sales Navigator**-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Im folgenden Screenshot wird ein Beispiel gezeigt. Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, LinkedIn Sales Navigator erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste oder den entsprechenden Attributwert gemäß Ihrer Organisationskonfiguration verwenden. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/updateusermail.png)
    
9. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest. Sie müssen einen weiteren Anspruch namens **department** hinzufügen, dessen Wert **user.department** zugeordnet werden muss.

    | Attributname | Attributwert |
    | --- | --- |    
    | department| user.department |

       ![Creating an Azure AD test user](./media/active-directory-saas-linkedinsalesnavigator-tutorial/userattribute.png)

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld zum Attribut zu öffnen.

       ![Creating an Azure AD test user](./media/active-directory-saas-linkedinsalesnavigator-tutorial/adduserattribute.png)
   
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

10. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

11. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_400.png)

12. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Klicken Sie auf **XML-Datei hochladen**, um die Metadaten-XML-Datei hochzuladen, die Sie vom Azure-Portal heruntergeladen haben.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

13. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden** und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** im unteren Bereich auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das **Azure Active Directory**-Symbol.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

2. Klicken Sie auf **Benutzer und Gruppen** und **Alle Benutzer**.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>Erstellen eines LinkedIn Sales Navigator-Testbenutzers

Die LinkedIn Sales Navigator-Anwendung unterstützt Just-in-Time(JIT)-Benutzerbereitstellungen. Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie **Lizenzen automatisch zuweisen**, um dem Benutzer eine Lizenz zuzuweisen.
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LinkedIn Sales Navigator gewähren.

![Benutzer zuweisen][200] 

**Um LinkedIn Sales Navigator Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **LinkedIn Sales Navigator** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie auf die Kachel „LinkedIn Sales Navigator“ im Zugriffsbereich klicken, sollten Sie zur Seite der Organisation umgeleitet werden, auf der Sie Details Ihres persönlichen LinkedIn-Kontos angeben müssen. Hierdurch wird Ihr persönliches Konto mit Ihrem LinkedIn-Geschäftskonto verknüpft. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-linkedinsalesnavigator-tutorial/tutorial_general_203.png


