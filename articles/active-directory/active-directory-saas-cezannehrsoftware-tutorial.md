---
title: 'Tutorial: Azure Active Directory-Integration mit Cezanne HR Software | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Cezanne HR Software konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: fb8f95bf-c3c1-4e1f-b2b3-3b67526be72d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 623c438edfce5f98c2d32d8bb25a97d86aa77909
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="tutorial-integrate-azure-active-directory-with-cezanne-hr-software"></a>Tutorial: Integrieren von Azure Active Directory mit Cezanne HR Software

In diesem Tutorial erfahren Sie, wie Sie Cezanne HR Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von Cezanne HR Software in Azure AD bietet die folgenden Vorteile. Ihre Möglichkeiten:

- Steuern Sie in Azure AD, wer auf Cezanne HR Software Zugriff hat.
- Ermöglichen Sie Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Cezanne HR Software anzumelden (einmaliges Anmelden, SSO).
- Verwalten Sie Ihre Konten zentral im Azure-Portal.

Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Cezanne HR Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Cezanne HR Software-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

> [!NOTE]
> Sie sollten zum Testen der Schritte in diesem Tutorial keine Produktionsumgebung verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von Cezanne HR Software aus dem Katalog
* Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-cezanne-hr-software-from-the-gallery"></a>Hinzufügen von Cezanne HR Software aus dem Katalog
Zum Konfigurieren der Integration von Cezanne HR Software in Azure AD fügen Sie Cezanne HR Software aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzu.

Um Cezanne HR Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Wählen Sie im linken Bereich des **[Azure-Portals](https://portal.azure.com)** die Schaltfläche **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Link „Alle Anwendungen“][2]
    
3. Um eine neue Anwendung hinzuzufügen, wählen Sie oben im Dialogfeld **Alle Anwendungen** die Schaltfläche **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld den Suchbegriff **Cezanne HR Software**ein.

    ![Das Suchfeld](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_search.png)

5. Wählen Sie in der Ergebnisliste **Cezanne HR Software** und dann die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Die Ergebnisliste](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD bei Cezanne HR Software basierend auf einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Cezanne HR Software als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt, müssen Sie zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in der Cezanne HR Software eine Linkbeziehung einrichten.

Um die Linkbeziehung herzustellen, weisen Sie den Wert für **Benutzername** in der Cezanne HR Software in Azure AD als Wert für **Benutzername** zu.

Zum Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD bei der Cezanne HR Software sind die folgenden Bausteine erforderlich.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt aktivieren Sie wie folgt das einmalige Anmelden von Azure AD im Azure-Portal und führen die entsprechende Konfiguration in Ihrer Cezanne HR Softwareanwendung durch:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cezanne HR Software** die Option **Einmaliges Anmelden**.

    ![Der Befehl „Einmaliges Anmelden“][4]

2. Um das einmalige Anmelden zu aktivieren, wählen Sie im Dialogfeld **Einmaliges Anmelden** im Feld **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Das Feld „Modus“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_samlbase.png)

3. Gehen Sie unter **Domäne und URLs für Cezanne HR Software** folgendermaßen vor:

    ![Der Abschnitt „Domäne und URLs für Cezanne HR Software“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_url.png)

    a. Geben Sie im Feld **URL für Anmeldung** eine URL mit folgender Syntax ein: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Geben Sie im Feld **Antwort-URL** eine URL mit folgender Syntax ein: `https://w3.cezanneondemand.com:443/<tenantid>`.    
     
    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie diese Werte mit der tatsächlichen Antwort-URL und Anmelde-URL. Um diese Werte zu erhalten, wenden Sie sich an das [Kundensupportteam von Cezanne HR Software](mailto:info@cezannehr.com).

4. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Der Abschnitt „SAML-Signaturzertifikat“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_certificate.png) 

5. Wählen Sie **Speichern** aus.

    ![Die Schaltfläche „Speichern“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_400.png)
    
6. Wählen Sie im Abschnitt mit der **Cezanne HR Software-Konfiguration** die Option **Cezanne HR Software konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** und die URL für den **SAML-SSO-Dienst** aus dem **Abschnitt Kurzübersicht**.

    ![Der Abschnitt „Cezanne HR Software-Konfiguration“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Cezanne HR Software-Mandanten als Administrator an.

8. Wählen Sie im linken Bereich **System Setup**. Wählen Sie **Security Settings** > **Single Sign-On Configuration**.

    ![Der „Single Sign-On Configuration“-Link](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

9. Aktivieren Sie im Bereich **Allow users to log in using the following Single Sign-On (SSO) services** das Kontrollkästchen **SAML 2.0**, und wählen Sie die Option **Advanced Configuration** aus.

    ![„Single Sign-On services“-Optionen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

10. Wählen Sie **Add New** aus.

    ![Die Schaltfläche „Add New“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

11. Gehen Sie unter **SAML 2.0 Identity Providers** folgendermaßen vor:

    ![Der Abschnitt „SAML 2.0 Identity Providers“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)
    
    a. Geben Sie im Feld **Display Name** den Namen Ihres Identitätsanbieters ein.

    b. Fügen Sie im Feld **Entity Identifier** die **SAML-Entitäts-ID** ein, die Sie aus dem Azure-Portal kopiert haben. 

    c. Wählen Sie im Listenfeld **SAML Binding** die Option **POST**.

    d. Fügen Sie im Feld **Security Token Service Endpoint** die **SAML Single Sign-On Service**-URL ein, die Sie aus dem Azure-Portal kopiert haben. 
    
    e. Geben Sie im Feld **User ID Attribute Name** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
    
    f. Um das heruntergeladene Zertifikat aus Azure AD hochzuladen, wählen Sie die Schaltfläche **Upload**.
    
    g. Klicken Sie auf **OK**. 

12. Wählen Sie **Speichern** aus.

    ![Die Schaltfläche „Speichern“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

> [!TIP]
> Wie Sie die App einrichten, können Sie eine kürzere Version der vorherigen Anweisungen im [Azure-Portal](https://portal.azure.com) lesen. Nachdem Sie diese App aus dem Bereich **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus. Öffnen Sie dann die eingebettete Dokumentation vom Abschnitt **Konfiguration** aus. 

Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im Azure-Portal eine Testbenutzerin mit dem Namen Britta Simon.

![Die Testbenutzerin Britta Simon][100]

Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:

1. Wählen Sie im linken Bereich des **Azure-Portals** die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** > **Alle Benutzer**.
    
    ![Der Link „Alle Benutzer“](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_02.png) 
    
    Das Dialogfeld **Alle Benutzer** wird geöffnet.

3. Zum Öffnen des Dialogfelds **Benutzer** wählen Sie **Hinzufügen**.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** folgende Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die **E-Mail-Adresse** der Benutzerin Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** generiert wurde.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-cezanne-hr-software-test-user"></a>Erstellen eines Cezanne HR Software-Testbenutzers

Damit sich Azure AD-Benutzer bei Cezanne HR Software anmelden können, müssen sie in Cezanne HR Software bereitgestellt werden. Bei Cezanne HR Software ist die Bereitstellung ein manueller Vorgang.

Stellen Sie ein Benutzerkonto folgendermaßen bereit:

1.  Melden Sie sich auf Ihrer Cezanne HR Software-Unternehmenswebsite als Administrator an.

2.  Wählen Sie im linken Bereich **System Setup** > **Manage Users** > **Add New User**.

    ![Der Link „Add New User“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "New User")

3.  Gehen Sie unter **Person Details** folgendermaßen vor:

    ![Der Abschnitt „Person Details“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "New User")
    
    a. Legen Sie **Internal User** auf **OFF** fest.
    
    b. Geben Sie im Feld **First Name** den Vornamen des Benutzers ein, z.B. **Britta**.  
 
    c. Geben Sie im Feld **Last Name** den Nachnamen des Benutzers ein, z.B. **Simon**.
    
    d. Geben Sie im Feld **E-mail** die E-Mail-Adresse des Benutzers ein, z.B. Brittasimon@contoso.com.

4.  Gehen Sie unter **Account Information** folgendermaßen vor:

    ![Der Abschnitt „Account Information“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "New User")
    
    a. Geben Sie im Feld **Username** die E-Mail-Adresse des Benutzers ein, z.B. Brittasimon@contoso.com.
    
    b. Geben Sie im Feld **Password** das Kennwort des Benutzers ein.
    
    c. Wählen Sie im Feld **Security Role** die Option **HR Professional**.
    
    d. Klicken Sie auf **OK**.

5. Wählen Sie auf der Registerkarte **Single sign-on** im Abschnitt **SAML 2.0 Identifiers** die Option **Add New**.

    ![Die Schaltfläche „Add New“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "User")

6. Wählen Sie im Listenfeld **Identity Provider** Ihren Identitätsanbieter aus. Geben Sie im Feld **User Identifier** die E-Mail-Adresse für das Konto der Testbenutzerin Britta Simon ein.

    ![Die Felder „Identity Provider“ und „User Identifier“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "User")
    
7. Wählen Sie **Speichern** aus.

    ![Die Schaltfläche „Save“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "User")

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Testbenutzerin Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure, indem Sie ihr Zugriff auf Cezanne HR Software gewähren.

![Testbenutzerzugriff][200] 

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Wählen Sie **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Der Link „Alle Anwendungen“][201] 

2. Wählen Sie in der Anwendungsliste **Cezanne HR Software**aus.

    ![Die Liste „Anwendungen“](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_app.png) 

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][202] 

4. Wählen Sie **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Wählen Sie im Dialogfeld **Benutzer und Gruppen** die Option **Auswählen**.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Cezanne HR Software“ auswählen, sollten Sie automatisch bei Ihrer Cezanne HR Software-Anwendung angemeldet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png


