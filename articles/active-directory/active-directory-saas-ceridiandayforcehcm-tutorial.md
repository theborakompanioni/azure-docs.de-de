---
title: 'Tutorial: Azure Active Directory-Integration mit Ceridian Dayforce HCM | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Ceridian Dayforce HCM konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7adf1eb3-d063-45d6-96a8-fd53b329b3f3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 26c07d30f9166e0e52cb396cdd0576530939e442
ms.openlocfilehash: b2ea3d92f233dab5bd6814e4875f881117eac8e3
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Tutorial: Azure Active Directory-Integration mit Ceridian Dayforce HCM

In diesem Tutorial erfahren Sie, wie Sie Ceridian Dayforce HCM in Azure Active Directory (Azure AD) integrieren.

Die Integration von Ceridian Dayforce HCM in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Ceridian Dayforce HCM hat.
- Sie können es Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Ceridian Dayforce HCM anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Ceridian Dayforce HCM konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Ceridian Dayforce HCM-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Ceridian Dayforce HCM aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Hinzufügen von Ceridian Dayforce HCM aus dem Katalog
Zum Konfigurieren der Integration von Ceridian Dayforce HCM in Azure AD müssen Sie Ceridian Dayforce HCM aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie zum Hinzufügen von Ceridian Dayforce HCM aus dem Katalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **Ceridian Dayforce HCM** ein, und klicken Sie auf **Ceridian Dayforce HCM** in der Ergebnisliste. Klicken Sie dann auf die Schaltfläche **Add** (Hinzufügen), um die Anwendung hinzuzufügen.

    ![Ceridian Dayforce HCM in der Ergebnisliste](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens „Britta Simon“, wie das einmalige Anmelden von Azure AD in Ceridian Dayforce HCM konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Ceridian Dayforce HCM als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Ceridian Dayforce HCM muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Ceridian Dayforce HCM den Wert des **user name** (Benutzernamens) in Azure AD als Wert für **Username** (Benutzername) zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Ceridian Dayforce HCM müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Create a Ceridian Dayforce HCM test user (Erstellen Sie einen Ceridian Dayforce HCM-Testbenutzer)](#create-a-ceridian-dayforce-hcm-test-user)**, um eine Entsprechung von Britta Simon in Ceridian Dayforce HCM zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren es in Ihrer Ceridian Dayforce HCM-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Ceridian Dayforce HCM die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Ceridian Dayforce HCM** auf **Single sign-on** (Einmaliges Anmelden konfigurieren).

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Ceridian Dayforce HCM Domain and URLs (Domänen und URLs für Ceridian Dayforce HCM)** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_url.png)
    
    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die Benutzer zum Anmelden bei Ihrer Ceridian Dayforce HCM-Anwendung verwenden.
    
    | Environment | URL |
    | :-- | :-- |
    | Für die Bereitstellung | `https://sso.dayforcehcm.com/<DayforcehcmNamespace>` |
    | Zu Testzwecken | `https://ssotest.dayforcehcm.com/<DayforcehcmNamespace>` |
    
    b. Geben Sie im Textfeld **Identifier** (Bezeichner) eine URL nach folgendem Muster ein:
    
    | Environment | URL |
    | :-- | :-- |
    | Für die Bereitstellung | `https://ncpingfederate.dayforcehcm.com/sp` |
    | Zu Testzwecken | `https://fs-test.dayforcehcm.com/sp` |
    
    c. Geben Sie im Textfeld **Antwort-URL** die URL ein, die von Azure AD zum Senden der Antwort verwendet wird.
    
    | Environment | URL |
    | :-- | :-- |
    | Für die Bereitstellung | `https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2` |
    | Zu Testzwecken | `https://fs-test.dayforcehcm.com/sp/ACS.saml2` |
    
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Wenden Sie sich an das [Ceridian Dayforce HCM Client support team (Ceridian Dayforce HCM Client-Supportteam)](https://www.ceridian.com/contact-us/index.html), um diese Werte abzurufen.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_certificate.png) 

5. Die Ceridian Dayforce HCM-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Wenden Sie sich zunächst an das [Ceridian Dayforce HCM support team (Ceridian Dayforce HCM Client-Supportteam)](https://www.ceridian.com/contact-us/index.html), um die korrekte Benutzer-ID zu identifizieren. Microsoft empfiehlt die Verwendung des Attributs **name** als Benutzer-ID. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:  

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname  | Attributwert |
    | --------------- | -------------------- |    
    | name  | user.extensionattribute2 |    

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Wählen Sie aus der Liste **Value**(Wert) das Benutzerattribut aus, das Sie für Ihre Implementierung verwenden möchten.
    Wenn Sie z.B. den Wert „EmployeeID“ als eindeutige Benutzer-ID verwenden möchten und den Attributwert in „ExtensionAttribute2“ gespeichert haben, wählen Sie **user.extensionattribute2** aus.
    
    d. Klicken Sie auf **OK**.

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_400.png)
    
8. Klicken Sie im Abschnitt **Ceridian Dayforce HCM Configuration** (Konfiguration von Ceridian Dayforce HCM) auf **Configure Ceridian Dayforce HCM** (Ceridian Dayforce HCM konfigurieren), um das Fenster **Configure sign-in** (Anmeldung konfigurieren) zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Konfiguration von Ceridian Dayforce HCM](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_configure.png) 

9. Um einmaliges Anmelden auf der Seite **Ceridian Dayforce HCM** zu konfigurieren, müssen sie die heruntergeladene **Metadaten-XML** und die **Abmelde-URL, SAML-Entitäts-ID und SAML-SSO-Dienst-URL** an das [Supportteam von Ceridian Dayforce HCM](https://www.ceridian.com/contact-us/index.html) senden.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Users and groups** (Benutzer und Gruppen), und klicken Sie dann auf **All users** (Alle Benutzer), um die Benutzerliste anzuzeigen.

    ![Die Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **All Users** (Alle Benutzer) auf **Add** (Hinzufügen), um das Dialogfeld **User** (Benutzer) zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Show Password** (Kennwort anzeigen), und notieren Sie sich den Wert, der im Feld **Password** (Kennwort) angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-ceridian-dayforce-hcm-test-user"></a>Erstellen einen Ceridian Dayforce HCM-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Ceridian Dayforce HCM. Wenden Sie sich an das [Ceridian Dayforce HCM support team (Ceridian Dayforce HCM Client-Supportteam)](https://www.ceridian.com/contact-us/index.html), um Benutzer zur Ceridian Dayforce HCM-Anwendung hinzuzufügen. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Ceridian Dayforce HCM gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Ceridian Dayforce HCM zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Ceridian Dayforce HCM**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Ceridian Dayforce HCM gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Ceridian Dayforce HCM zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Ceridian Dayforce HCM**aus.

    ![Der Link „Ceridian Dayforce HCM“ in der Anwendungsliste](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.  
Wenn Sie im Zugriffsbereich auf die Kachel „Ceridian Dayforce HCM“ klicken, sollten Sie automatisch bei Ihrer Ceridian Dayforce HCM-Anwendung angemeldet werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png


