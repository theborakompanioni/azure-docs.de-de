---
title: 'Tutorial: Azure Active Directory-Integration mit Tableau Online | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Online konfigurieren."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d4b1149-ba3b-4f4e-8bce-9791316b730d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 443fab1198a91a4d5749e6421f7b8603fc75a81e
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017


---
# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Tutorial: Azure Active Directory-Integration mit Tableau Online

In diesem Tutorial erfahren Sie, wie Sie Tableau Online in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tableau Online in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Tableau Online hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Online anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Tableau Online konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Tableau Online-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Tableau Online aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-tableau-online-from-the-gallery"></a>Hinzufügen von Tableau Online aus dem Katalog
Zum Konfigurieren der Integration von Tableau Online in Azure AD müssen Sie Tableau Online aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Tableau Online aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **Tableau Online**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_search.png)

5. Wählen Sie im Ergebnisbereich **Tableau Online** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Tableau Online basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tableau Online als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Online muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Tableau Online den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Online müssen Sie die folgenden Schritte ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Tableau Online-Testbenutzers](#creating-a-tableau-online-test-user)** , um eine Entsprechung von Britta Simon in Tableau Online zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Tableau Online-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tableau Online die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Tableau Online** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Tableau Online** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_url.png)
    
    a. Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://sso.online.tableau.com`

    b. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://sso.online.tableau.com/public/sp/<instancename>`

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Browserfenster in Ihrer Tableau Online-Anwendung als Administrator an. Wechseln Sie zu **Einstellungen** und dann zu **Authentifizierung**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)
    
7. Gehen Sie im Abschnitt **Authentifizierungstypen** zum Aktivieren von SAML wie folgt vor. Aktivieren Sie das Kontrollkästchen **Single sign-on with SAML** (Einmaliges Anmelden mit SAML).
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

8. Scrollen Sie nach unten bis zum Abschnitt **Import metadata file into Tableau Online** (Metadatendatei in Tableau Online importieren).  Klicken Sie auf „Durchsuchen“, und importieren Sie die Metadatendatei, die Sie aus Azure AD heruntergeladen haben. Klicken Sie anschließend auf **Übernehmen**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

9. Fügen Sie im Abschnitt **Match assertions** (Assertionen abgleichen) den entsprechenden Identitätsanbieter-Assertionsnamen für **E-Mail-Adresse**, **Vorname** und **Nachname** ein. Gehen Sie wie folgt vor, um diese Informationen aus Azure AD abzurufen: 
  
    a. Wechseln Sie im Azure-Portal zur Anwendungsintegrationsseite von **Tableau Online**.
    
    b. Aktivieren Sie im Abschnitt „Attribute“ das Kontrollkästchen **Alle weiteren Benutzerattribute anzeigen und bearbeiten**. 
    
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/attributesection.png)
      
    c. Kopieren Sie den Namespacewert für die Attribute „givenname“, „email“ und „surname“ mithilfe der folgenden Schritte:

   ![Azure AD – einmaliges Anmelden](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)
    
    d. Klicken Sie auf den Wert **user.givenname**. 
    
    e. Kopieren Sie den Wert aus dem Textfeld **Namespace**.

   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/attributesection2.png)

    f. Befolgen Sie vorherigen Anweisungen, um die Namespacewerte für „email“ und „surname“ zu kopieren.

    g. Wechseln Sie zur Tableau Online-Anwendung, und legen Sie den Abschnitt **Tableau Online Attributes** (Tableau Online-Attribute) dann wie folgt fest:
     * E-Mail: **mail** oder **userprincipalname**
     * Vorname: **givenname**
     * Nachname: **surname**
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-tableau-online-test-user"></a>Erstellen eines Tableau Online-Testbenutzers

In diesem Abschnitt erstellen Sie in Tableau Online einen Benutzer namens Britta Simon.

1. Klicken Sie in **Tableau Online** auf **Settings** (Einstellungen) und dann auf den Abschnitt **Authentication** (Authentifizierung). Scrollen Sie nach unten zum Abschnitt **Select Users** (Benutzer auswählen). Klicken Sie auf **Add Users** (Benutzer hinzufügen) und dann auf **Enter Email Addresses** (E-Mail-Adressen eingeben).
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Wählen Sie **Add users for single sign-on (SSO) authentication**(Benutzer für SSO-Authentifizierung (einmaliges Anmelden) hinzufügen). Fügen Sie im Textfeld **Enter Email Addresses** (E-Mail-Adressen eingeben) den Eintrag britta.simon@contoso.com hinzu.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)
3. Klicken Sie auf **Erstellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Tableau Online gewähren.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu Tableau Online führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Tableau Online**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Online“ klicken, sollten Sie automatisch bei Ihrer Tableau Online-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png


