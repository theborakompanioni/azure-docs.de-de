---
title: 'Tutorial: Azure Active Directory-Integration mit LinkedIn Lookup | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LinkedIn Lookup konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2757a39-1ead-4a3e-91e4-270be3055683
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: f343d8dbbd80de1a50b625e949377aa7c48d0cae
ms.contentlocale: de-de
ms.lasthandoff: 04/29/2017


---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-lookup"></a>Tutorial: Azure Active Directory-Integration mit LinkedIn Lookup

In diesem Tutorial erfahren Sie, wie Sie LinkedIn Lookup in Azure Active Directory (Azure AD) integrieren.

Die Integration von LinkedIn Lookup in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LinkedIn Lookup hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LinkedIn Lookup anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LinkedIn Lookup konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LinkedIn Lookup-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von LinkedIn Lookup aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-linkedin-lookup-from-the-gallery"></a>Hinzufügen von LinkedIn Lookup aus dem Katalog
Zum Konfigurieren der Integration von LinkedIn Lookup in Azure AD müssen Sie LinkedIn Lookup aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um LinkedIn Lookup aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie **LinkedIn Lookup** im Suchfeld ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_search.png)

5. Wählen Sie im Ergebnisbereich die Option **LinkedIn Lookup** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei LinkedIn Lookup mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LinkedIn Lookup als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LinkedIn Lookup muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in LinkedIn Lookup zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei LinkedIn Lookup müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen einen LinkedIn Lookup-Testbenutzers](#creating-an-linkedin-lookup-test-user)**, um eine Entsprechung von Britta Simon in LinkedIn Lookup zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LinkedIn Lookup-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei LinkedIn Lookup über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LinkedIn Lookup** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_samlbase.png)

3. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrer **LinkedIn Lookup**-Website als Administrator an.

4. Klicken Sie im **Kontocenter** unter **Einstellungen** auf **Globale Einstellungen**. Wählen Sie außerdem in der Dropdownliste die Option **Lookup** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_011.png)

5. Klicken Sie auf **ODER klicken Sie hier, um einzelne Felder aus dem Formular zu laden und zu kopieren**, und kopieren Sie die **Entitäts-ID** und **Assertion Consumer Access(ACS)-URL**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_LinkedIn_admin_032.png)

6. Führen Sie im Azure-Portal unter **Domäne und URLs für LinkedIn Lookup** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die **Entitäts-ID** ein, die Sie aus dem LinkedIn-Portal kopiert haben. 

    b. Geben Sie im Textfeld **Antwort-URL** die **Assertion Consumer Access(ACS)-URL** ein, die Sie aus dem LinkedIn-Portal kopiert haben.

7. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_url1.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://www.linkedIn.com/checkpoint/enterprise/login/<AccountId>?application=lookup`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um einen Beispielwert. Die Werte müssen vom Benutzer durch die tatsächliche Anmelde-URL ersetzt werden. Wenden Sie sich an das [Kundensupportteam von LinkedIn Lookup](https://business.LinkedIn.com/lookup), um diesen Wert zu erhalten.

8. Ihre **LinkedIn Lookup**-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Der Benutzer muss der Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Im folgenden Screenshot wird ein Beispiel gezeigt. Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, LinkedIn Lookup erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste oder den entsprechenden Attributwert gemäß Ihrer Organisationskonfiguration verwenden. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/updateusermail.png)
    
9. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest. Sie müssen einen weiteren Anspruch namens **department** hinzufügen, dessen Wert **user.department** zugeordnet werden muss.

    | Attributname | Attributwert |
    | --- | --- |    
    | department| user.department |

       ![Creating an Azure AD test user](./media/active-directory-saas-LinkedInlookup-tutorial/userattribute.png)

       a. Click **Add attribute** to open the attribute details page add the department attribute as shown below-

       ![Creating an Azure AD test user](./media/active-directory-saas-LinkedInlookup-tutorial/adduserattribute.png)
   
       b. Click **Ok** to save the attribute.

10. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_certificate.png) 

11. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_400.png)

12. Wechseln Sie zum Abschnitt **LinkedIn-Administratoreinstellungen**. Laden Sie die XML-Datei hoch, die Sie gerade aus dem Azure-Portal heruntergeladen haben, indem Sie auf die Option **XML-Datei hochladen** klicken.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_metadata_03.png)

13. Klicken Sie auf **Ein**, um das einmalige Anmelden zu aktivieren. Der Status des einmaligen Anmeldens wechselt von **Nicht verbunden** zu **Verbunden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedIn_admin_05.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden** und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** im unteren Bereich auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_01.png) 

2. Klicken Sie auf **Benutzer und Gruppen** und **Alle Benutzer**.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_02.png) 

3. Klicken Sie auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **Britta Simon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-linkedin-lookup-test-user"></a>Erstellen eines LinkedIn Lookup-Testbenutzers

Die LinkedIn Lookup-Anwendung unterstützt Just-in-Time-Benutzerbereitstellungen (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. Aktivieren Sie **Lizenzen automatisch zuweisen**, um dem Benutzer eine Lizenz zuzuweisen.
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedin_admin_license.png)

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf LinkedIn Lookup gewähren.

![Benutzer zuweisen][200] 

**Um LinkedIn Lookup Britta Simon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **LinkedIn Lookup** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_linkedInlookup_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie auf die Kachel „LinkedIn Lookup“ im Zugriffsbereich klicken, sollten Sie zur Seite der Organisation umgeleitet werden, auf der Sie Details Ihres persönlichen LinkedIn-Kontos angeben müssen. Hierdurch wird Ihr persönliches Konto mit Ihrem LinkedIn-Geschäftskonto verknüpft. 

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-LinkedInlookup-tutorial/tutorial_general_203.png


