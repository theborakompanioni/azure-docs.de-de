---
title: 'Tutorial: Azure Active Directory-Integration mit TOPdesk - Public | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TOPdesk - Public konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: f21fe0b363776974108ff460060e4c15a51a58a3
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Tutorial: Azure Active Directory-Integration mit TOPdesk - Public

In diesem Tutorial erfahren Sie, wie Sie TOPdesk - Public in Azure Active Directory (Azure AD) integrieren.

Die Integration von TOPdesk - Public in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TOPdesk - Public hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei TOPdesk - Public anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in TOPdesk - Public konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein TOPdesk - Public-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von TOPdesk - Public aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-topdesk---public-from-the-gallery"></a>Hinzufügen von TOPdesk - Public aus dem Katalog
Zum Konfigurieren der Integration von TOPdesk - Public in Azure AD müssen Sie TOPdesk - Public aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um TOPdesk - Public aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **TOPdesk - Public** ein, wählen Sie im Ergebnisbereich **TOPdesk - Public** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![TOPdesk - Public in der Ergebnisliste](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei TOPdesk - Public mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TOPdesk - Public als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TOPdesk - Public muss eine Linkbeziehung eingerichtet werden.

Weisen Sie den Wert für **Benutzername** aus Azure AD in TOPdesk - Public als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei TOPdesk - Public müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines TOPdesk - Public-Testbenutzers](#create-a-topdesk---public-test-user)**, um ein Pendant von Britta Simon in TOPdesk - Public zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer TOPdesk - Public-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei TOPdesk - Public die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **TOPdesk - Public** auf **Einmaliges Anmelden**.

    ![Link „Einmaliges Anmelden konfigurieren“][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für TOPdesk - Public** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für TOPdesk - Public](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.topdesk.net`.
    
    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<companyname>.topdesk.net/tas/public/login/verify`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<companyname>.topdesk.net/tas/public/login/saml`
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Die Antwort-URL wird weiter unten in diesem Tutorial erläutert. Wenden Sie sich an den [Supportteam für den TOPdesk - Public-Client](https://help.topdesk.com/saas/enterprise/user/), um diese Werte zu erhalten.  

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. Klicken Sie im Abschnitt **TOPdesk - Public-Konfiguration** auf **TOPdesk - Public konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![TOPdesk - Public-Konfiguration](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. Melden Sie sich bei der **TOPdesk - Public** -Unternehmenswebsite als Administrator an.

8. Klicken Sie im Menü **TOPdesk** auf **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Einstellungen")

9. Klicken Sie auf **Anmeldeeinstellungen**.
   
    ![Anmeldeeinstellungen](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Anmeldeeinstellungen")

10. Erweitern Sie das Menü **Anmeldeeinstellungen**, und klicken Sie dann auf **Allgemein**.
   
    ![Allgemein](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "Allgemein")

11. Führen Sie im Abschnitt **Öffentlich** des Konfigurationsabschnitts **SAML-Anmeldung** die folgenden Schritte aus:
   
    ![Technische Einstellungen](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technische Einstellungen")
   
    a. Klicken Sie auf **Herunterladen** , um die  öffentliche Metadatendatei herunterzuladen und sie dann lokal auf Ihrem Computer zu speichern.
   
    b. Öffnen Sie die heruntergeladene Metadatendatei, und suchen Sie den Knoten **AssertionConsumerService**.

    ![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")
   
    c. Kopieren Sie den Wert **AssertionConsumerService**, und fügen Sie ihn im Abschnitt **Domäne und URLs für TOPdesk - Public** in das Textfeld **Antwort-URL** ein.      
   
12. Zum Erstellen einer Zertifikatsdatei führen Sie die folgenden Schritte durch:
    
    ![Zertifikat](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Zertifikat")
    
    a. Öffnen Sie die aus dem Azure-Portal heruntergeladene Metadatendatei.
    
    b. Erweitern Sie den Knoten **RoleDescriptor**, der über einen **xsi:type** von **fed:ApplicationServiceType** verfügt.
    
    c. Kopieren Sie den Wert des Knotens **X509Certificate** .
    
    d. Speichern Sie den kopierten **X509Certificate** -Wert lokal in einer Datei auf Ihrem Computer.

13. Klicken Sie im Abschnitt **Öffentlich** auf **Hinzufügen**.
    
    ![SAML-Anmeldung](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-Anmeldung")

14. Führen Sie auf der Seite **SAML Configuration Assistant** die folgenden Schritte aus:
    
    ![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")
    
    a. Klicken Sie zum Hochladen der aus dem Azure-Portal heruntergeladenen Metadatendatei unter **Verbundmetadaten** auf **Durchsuchen**.

    b. Klicken Sie zum Hochladen Ihrer Zertifikatsdatei unter **Zertifikat (RSA)** auf **Durchsuchen**.

    c. Klicken Sie zum Hochladen der Logodatei, die Sie vom TOPdesk-Supportteam erhalten haben, unter **Logosymbol** auf **Durchsuchen**.

    d. Geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` in das Textfeld **Benutzernamenattribut** ein.

    e. Geben Sie in das Textfeld **Anzeigenname** einen Namen für die Konfiguration ein.

    f. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-topdesk---public-test-user"></a>Erstellen eines TOPdesk - Public-Testbenutzers

Damit sich Azure AD-Benutzer bei TOPdesk - Public anmelden können, müssen sie in TOPdesk - Public bereitgestellt werden.  
Im Fall von TOPdesk - Public ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei Ihrem **TOPdesk - Public** -Unternehmenswebsite als Administrator an.

2. Klicken Sie im Menü oben auf **TOPdesk \> Neu \> Support-Dateien \> Person**.
   
    ![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")

3. Führen Sie im Dialogfeld „Neue Person“ die folgenden Schritte aus:
   
    ![Neue Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "Neue Person")
   
    a. Klicken Sie auf die Registerkarte „Allgemein“.

    b. Geben Sie im Textfeld **Nachname** den Nachnamen des Benutzers ein, z.B. „Simon“.
 
    c. Wählen Sie einen **Speicherort** für das Konto aus.
 
    d. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von TOPdesk - Public-Benutzerkonten oder mithilfe der von TOPdesk - Public bereitgestellten APIs erstellen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TOPdesk - Public gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu TOPdesk - Public zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **TOPdesk - Public** aus.

    ![TOPdesk - Public-Link in der Anwendungsliste](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die TOPdesk - Public-Kachel klicken, sollten Sie automatisch bei Ihrer TOPdesk - Public-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png


