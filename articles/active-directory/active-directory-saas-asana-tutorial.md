---
title: 'Tutorial: Azure Active Directory-Integration mit Asana | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Asana konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: a2f0cecb93cc382bcfd710c44eb70f80ae67f9b6
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Tutorial: Azure Active Directory-Integration mit Asana

In diesem Tutorial erfahren Sie, wie Sie Asana in Azure Active Directory (Azure AD) integrieren.

Die Integration von Asana in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Asana haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Asana anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Asana konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Asana -Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Asana über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-asana-from-the-gallery"></a>Hinzufügen von Asana über den Katalog
Zum Konfigurieren der Integration von Asana in Azure AD müssen Sie Asana über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Asana über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Asana** ein, wählen Sie im Ergebnisbereich **Asana** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens „Britta Simon“ das einmalige Anmelden von Azure AD mit Asana.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Asana als Gegenpart für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Asana muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Asana den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Asana müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Asana-Testbenutzers](#create-an-asana-test-user)**, um in Asana einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Asana-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmeldens von Azure AD mit Asana zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Asana** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. Führen Sie die folgenden Schritte im Abschnitt **Domäne und URLs für Asana** durch:

    ![SSO-Informationen zur Domäne und zu den URLs für Asana](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://app.asana.com/`.

    b. Geben Sie im Textfeld **Bezeichner** folgenden Wert ein: `https://app.asana.com/`
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Asana-Konfiguration** auf **Asana konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Asana-Konfiguration](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. Melden Sie sich in einem anderen Browserfenster bei Ihrer Asana-Anwendung als Administrator an. Greifen Sie zum Konfigurieren von SSO in Asana auf die Arbeitsbereichseinstellungen zu, indem Sie in der rechten oben Bildschirmecke auf den Namen des Arbeitsbereichs klicken. Klicken Sie dann auf **Einstellungen für \<Name Ihres Arbeitsbereichs\>**. 
   
    ![Asana-SSO-Einstellungen](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. Klicken Sie im Fenster **Organisationseinstellungen** auf **Verwaltung**. Klicken Sie auf **Members must log in via SAML** (Mitglieder müssen sich über SAML anmelden), um die SSO-Konfiguration zu aktivieren. Führen Sie anschließend die folgenden Schritte aus:
   
    ![Konfigurieren der Organisationseinstellungen für einmaliges Anmelden](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     a. Fügen Sie in das Textfeld **Anmelde-URL** die **URL für den SAML-SSO-Dienst** ein.

     b. Klicken Sie mit der rechten Maustaste auf das Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, und öffnen Sie die Zertifikatsdatei mit Editor oder Ihrem bevorzugten Texteditor. Kopieren Sie den Inhalt zwischen den Überschriften für Beginn und Ende des Zertifikats, und fügen Sie ihn in das Textfeld **X.509-Zertifikat** ein.

9. Klicken Sie auf **Speichern**. Weitere Informationen finden Sie im [Asana-Handbuch zum Einrichten von SSO](https://asana.com/guide/help/premium/authentication#gl-saml) .

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-an-asana-test-user"></a>Erstellen eines Asana-Testbenutzers

In diesem Abschnitt erstellen Sie in Asana einen Benutzer namens Britta Simon.

1. Navigieren Sie in **Asana** im linken Bereich zum Abschnitt **Teams**. Klicken Sie auf die Pluszeichen-Schaltfläche. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Geben Sie die E-Mail-Adresse „ britta.simon@contoso.com “ in das Textfeld ein, und wählen **Invite**(Einladen) aus.

3. Klicken Sie auf **Send Invite**(Einladung senden). Eine E-Mail wird an das E-Mail-Konto des neuen Benutzers gesendet. Der Benutzer muss das Konto erstellen und bestätigen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Asana gewähren.

![Zuweisen der Benutzerrolle][200]

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Asana durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Asana**aus.

    ![Asana-Link in der Anwendungsliste](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt wird das einmalige Anmelden von Azure AD getestet.

Rufen Sie die Asana-Anmeldeseite auf. Geben Sie im Textfeld für die E-Mail-Adresse die E-Mail-Adresse britta.simon@contoso.com ein. Lassen Sie das Textfeld für das Kennwort leer, und klicken Sie dann auf **Anmelden**. Sie werden zur Azure AD-Anmeldeseite weitergeleitet. Geben Sie Ihre Azure AD-Anmeldeinformationen an. Sie sind nun bei Asana angemeldet.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png

