---
title: 'Tutorial: Azure Active Directory-Integration mit ScaleX Enterprise | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ScaleX Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: be747170a0d8a7a6defd790a3f8a122c4d397671
ms.openlocfilehash: 0ebed0c2605862426384c0e219e52c9d626b6246
ms.contentlocale: de-de
ms.lasthandoff: 05/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-scalex-enterprise"></a>Tutorial: Azure Active Directory-Integration mit ScaleX Enterprise

In diesem Tutorial erfahren Sie, wie Sie ScaleX Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von ScaleX Enterprise in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ScaleX Enterprise hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ScaleX Enterprise anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zum Integrieren von SaaS-Apps in Azure AD finden Sie unter Was bedeuten Anwendungszugriff und einmaliges Anmelden mit [Azure Active Directory](active-directory-appssoaccess-whatis.md)?

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ScaleX Enterprise konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ScaleX Enterprise-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ScaleX Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Hinzufügen von ScaleX Enterprise aus dem Katalog
Zum Konfigurieren der Integration von ScaleX Enterprise in Azure AD müssen Sie ScaleX Enterprise aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ScaleX Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **ScaleX Enterprise** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_search.png)

5. Wählen Sie im Ergebnisbereich die Option **ScaleX Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ScaleX Enterprise basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ScaleX Enterprise als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ScaleX Enterprise muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in ScaleX Enterprise zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ScaleX Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ScaleX Enterprise-Testbenutzers](#creating-a-scalex-enterprise-test-user)**, um eine Entsprechung von Britta Simon in ScaleX Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ScaleX Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in ScaleX Enterprise die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ScaleX Enterprise** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für ScaleX Enterprise** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** einen Wert nach folgendem Muster ein: `https://platform.rescale.com/saml2/<company id>/`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://platform.rescale.com/saml2/<company id>/acs/`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://platform.rescale.com/saml2/<company id>/sso/`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die tatsächliche Antwort-URL bzw. die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von ScaleX Enterprise Client](http://info.rescale.com/contact_sales), um diese Werte zu erhalten. 

5. Die ScaleX-Anwendung erwartet die SAML-Assertions in einem bestimmten Format. Daher müssen Sie die Zuordnungen benutzerdefinierter Attribute in Ihrer SAML-Tokenattributkonfiguration bearbeiten. Klicken Sie auf das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, um die Einstellungen für benutzerdefinierte Attribute zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_attributes.png)
    
    a. Klicken Sie mit der rechten Maustaste auf das Attribut **name**, und klicken Sie auf „Löschen“.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/delete_attribute_name.png)

    b. Klicken Sie auf das Attribut **emailaddress**, um das Fenster „Attribut bearbeiten“ zu öffnen. Ändern Sie den Wert von **user.mail** zu **user.userprincipalname**, und klicken Sie auf „OK“.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/edit_email_attribute.png)    
    
5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **ScaleX Enterprise-Konfiguration** auf **ScaleX Enterprise konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_configure.png) 

8. Um das einmalige Anmelden auf der Seite von **ScaleX Enterprise** zu konfigurieren, melden Sie sich bei der ScaleX Enterprise-Unternehmenswebsite als Administrator an.

9. Klicken Sie auf das Menü oben rechts, und wählen Sie **Contoso Administration** aus.

    > [!NOTE] 
    > „Contoso“ ist nur ein Beispiel. Auf der Seite steht der tatsächliche Name Ihres Unternehmens. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/Test_Admin.png) 

10. Wählen Sie aus dem oberen Menü die Option **Integrations** aus, und wählen Sie dann **Single Sign-On** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/admin_sso.png) 

11. Füllen Sie das Formular wie folgt aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/scalex_admin_save.png) 
    
    a. Wählen Sie **Create any user who can authenticate with SSO** aus.

    b. **Service Provider saml**: Fügen Sie den Wert ***urn:oasis:names:tc:SAML:2.0:nameid-format:persistent*** ein.

    c. **Name of Identity Provider email field in ACS response**: Fügen Sie den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.

    d. **Identity Provider EntityDescriptor Entity ID**: Fügen Sie den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.

    e. **Identity Provider SingleSignOnService URL**: Fügen Sie den Wert der **URL für den SAML-SSO-Dienst** aus dem Azure-Portal ein.

    f. **Identity Provider public X509 certificate:** Öffnen Sie das X509-Zertifikat, das Sie aus Azure heruntergeladen haben, in Editor, und fügen Sie den Inhalt in dieses Feld ein. Stellen Sie sicher, dass das Zertifikat keine Zeilenumbrüche enthält.
    
    g. Aktivieren Sie die folgenden Kontrollkästchen: **Enabled, Encrypt NameID und Sign AuthnRequests**.

    h. Klicken Sie auf **Update SSO Settings**, um die Einstellungen zu speichern.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden** und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** im unteren Bereich auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-scalexenterprise-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-scalex-enterprise-test-user"></a>Erstellen eines ScaleX Enterprise-Testbenutzers

Damit sich Azure AD-Benutzer bei ScaleX Enterprise anmelden können, müssen sie in ScaleX Enterprise bereitgestellt werden. Bei ScaleX Enterprise erfolgt die Bereitstellung automatisch, sodass keine manuellen Schritte erforderlich sind. Alle Benutzer, die sich erfolgreich mit Anmeldeinformationen für das einmalige Anmelden authentifizieren können, werden automatisch auf der Seite von ScaleX bereitgestellt.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ScaleX Enterprise gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu ScaleX Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ScaleX Enterprise**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-scalexenterprise-tutorial/tutorial_scalexenterprise_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Klicken Sie im Zugriffsbereich auf die Kachel „ScaleX Enterprise“ – Sie werden automatisch bei Ihrer ScaleX Enterprise-Anwendung angemeldet. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-scalexenterprise-tutorial/tutorial_general_203.png


