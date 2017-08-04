---
title: 'Tutorial: Azure Active Directory-Integration mit SkyDesk Email | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SkyDesk Email konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 0ffcca4161fc836192fc9c9871a905f36ea76b32
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Tutorial: Azure Active Directory-Integration mit SkyDesk Email

In diesem Tutorial erfahren Sie, wie Sie SkyDesk Email in Azure Active Directory (Azure AD) integrieren.

Die Integration von SkyDesk Email in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf SkyDesk Email zugreifen kann.
- Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SkyDesk Email anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SkyDesk Email konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Skydesk Email-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SkyDesk Email aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-skydesk-email-from-the-gallery"></a>Hinzufügen von SkyDesk Email aus dem Katalog
Zum Konfigurieren der Integration von SkyDesk Email in Azure AD müssen Sie SkyDesk Email aus dem Katalog zu Ihrer Liste der verwalteten SaaS-Apps hinzufügen.

**Um SkyDesk Email aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **SkyDesk Email**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. Wählen Sie im Ergebnisbereich die Option **SkyDesk Email** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SkyDesk Email mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SkyDesk Email als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SkyDesk Email muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SkyDesk Email den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit SkyDesk Email sind die folgenden Bausteine erforderlich:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Skydesk Email-Testbenutzers](#creating-a-skydesk-email-test-user)**, um ein Pendant zu Britta Simon in Skydesk Email zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SkyDesk Email-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SkyDesk Email die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SkyDesk Email** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für SkyDesk Email** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://mail.skydesk.jp/portal/<companyname>`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam für den SkyDesk Email-Client](https://www.skydesk.sg/support/), um den Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **SkyDesk Email-Konfiguration** auf **SkyDesk Email konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. Führen Sie die folgenden Schritte aus, um das einmalige Anmelden in **SkyDesk Email** zu aktivieren:

    a. Melden Sie sich als Administrator bei Ihrem SkyDesk Email-Konto an.

    b. Klicken Sie im oberen Menü auf **Setup**, und wählen Sie **Org** aus. 
    
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    c. Klicken Sie im linken Bereich auf **Domains** (Domänen).
    
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Klicken Sie auf **Add Domain** (Domäne hinzufügen).
    
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Geben Sie den Namen Ihrer Domäne ein, und überprüfen Sie die Domäne dann.
    
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Klicken Sie im linken Bereich auf **SAML Authentication** .
    
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. Führen Sie auf der Dialogseite **SAML Authentication** die folgenden Schritte aus:
   
      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    >Um die SAML-basierte Authentifizierung zu verwenden, sollten Sie entweder eine **überprüfte Domäne** oder eine **Portal-URL** eingerichtet haben. Sie können die Portal-URL mit dem eindeutigen Namen einrichten.
    > 
    > 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    a. Fügen Sie in das Textfeld **Anmelde-URL** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    b. Fügen Sie in das Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. **Change Password URL** ist optional, lassen Sie dieses Feld leer.

    d. Klicken Sie auf **Schlüssel aus Datei abrufen**, um das heruntergeladene Zertifikat aus dem Azure-Portal auszuwählen, und klicken Sie dann auf **Öffnen**, um das Zertifikat hochzuladen.

    e. Wählen Sie als **Algorithmus** den Wert **RSA** aus.

    f. Klicken Sie zum Speichern der Änderungen auf **OK** .

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-skydesk-email-test-user"></a>Erstellen eines SkyDesk Email-Testbenutzers

In diesem Abschnitt erstellen Sie in SkyDesk Email einen Benutzer mit dem Namen Britta Simon.

1. Klicken Sie in SkyDesk Email im linken Bereich auf **User Access**, und geben Sie Ihren Benutzernamen ein. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
>Wenn Sie mehrere Benutzer erstellen müssen, setzen Sie sich mit dem [Supportteam für den SkyDesk Email-Client](https://www.skydesk.sg/support/) in Verbindung.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SkyDesk Email gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SkyDesk Email zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SkyDesk Email** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SkyDesk Email“ klicken, sollten Sie automatisch bei Ihrer SkyDesk Email-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png


