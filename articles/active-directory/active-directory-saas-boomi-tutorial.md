---
title: 'Tutorial: Azure Active Directory-Integration mit Boomi | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Boomi konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 1121d22beddf73fd2109a4b410422f76dd37478e
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Tutorial: Azure Active Directory-Integration mit Boomi

In diesem Tutorial erfahren Sie, wie Sie Boomi in Azure Active Directory (Azure AD) integrieren.

Die Integration von Boomi in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Boomi hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Boomi anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Boomi benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Boomi-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Boomi über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-boomi-from-the-gallery"></a>Hinzufügen von Boomi über den Katalog
Zum Konfigurieren der Integration von Boomi in Azure AD müssen Sie Boomi aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Boomi aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Boomi** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Boomi** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Boomi.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Boomi als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Boomi muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Boomi den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Boomi müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Boomi-Testbenutzers](#creating-a-boomi-test-user)**, um eine Entsprechung von Britta Simon in Boomi zu erhalten, die mit der Darstellung dieses Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Boomi-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Boomi zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Boomi** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Boomi** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://platform.boomi.com/sso/<accountname>/saml`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://platform.boomi.com/sso/<accountname>/saml`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam von Boomi](https://boomi.com/company/contact/), um diesen Wert zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_certificate.png)

4. Die Boomi-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute.png)

5. Führen Sie im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** für jede in der folgenden Tabelle aufgeführte Zeile die folgenden Schritte aus:

    | Attributname | Attributwert |
    | -------------- | --------------- |
    | FEDERATION_ID | user.mail |
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_04.png)
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_attribute_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **Boomi-Konfiguration** auf **Boomi konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster bei der Boomi-Unternehmenswebsite als Administrator an. 

9. Navigieren Sie zu **Firmenname**, und wechseln Sie zu **Einrichten**.

10. Klicken Sie auf die Registerkarte **SSO-Optionen**, und führen Sie folgende Schritte aus.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_11.png)

    a. Aktivieren Sie das Kontrollkästchen **Enable SAML Single Sign-On**.

    b. Klicken Sie auf **Importieren**, um das heruntergeladene Zertifikat aus Azure AD in das **Identitätsanbieterzertifikat** hochzuladen.
    
    c. Geben Sie im Textfeld **Anmelde-URL des Identitätsanbieters:** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    d. Wählen Sie als **Federation Id Location** das Optionsfeld **Federation Id is in FEDERATION_ID Attribute element** aus. 

    e. Klicken Sie auf die Schaltfläche **Save** .

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-boomi-test-user"></a>Erstellen eines Boomi-Testbenutzers

Damit sich Azure AD-Benutzer bei Boomi anmelden können, müssen sie in Boomi bereitgestellt werden. Im Fall von Boomi ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1. Melden Sie sich bei der Boomi-Unternehmenswebsite als Administrator an.

2. Navigieren Sie nach der Anmeldung zu **Benutzerverwaltung** und wechseln Sie zu **Benutzer**.

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Benutzer")

3. Klicken Sie auf das Symbol **+**. Das Dialogfeld **Benutzerrollen hinzufügen/verwalten** wird geöffnet.

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Benutzer")

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Benutzer")

    a. Geben Sie im Textfeld **E-Mail-Adresse des Benutzers** die E-Mail-Adresse des Benutzers, z.B. BrittaSimon@contoso.com, ein.
    
    b. Geben Sie im Textfeld **Vorname** den Vornamen des Benutzers ein, z.B. „Britta“.

    c. Geben Sie im Textfeld **Nachname** den Vornamen des Benutzers ein, z.B. „Simon“.
    
    d. Geben Sie die **Verbund-ID.** des Benutzers ein. Jeder Benutzer muss über eine Verbund-ID verfügen, die den Benutzer im Konto eindeutig identifiziert.
    
    e. Weisen Sie dem Benutzer die Rolle **Standardbenutzer** zu. Weisen Sie ihm nicht die Rolle „Administrator“ zu, da er dadurch normalen Zugriff auf Atmosphere sowie Zugriff über einmaliges Anmelden erhalten würde.
    
    f. Klicken Sie auf **OK**.
    
    > [!NOTE]
    > Der Benutzer erhält keine Begrüßungs-E-Mail mit einem Kennwort, das zum Anmelden beim AtomSphere-Konto verwendet werden kann, da sein Kennwort über den Identitätsanbieter verwaltet wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Boomi-Benutzerkonten oder mithilfe der von Boomi bereitgestellten APIs erstellen. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Boomi gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Boomi zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Boomi** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Boomi“ klicken, sollten Sie automatisch bei Ihrer Boomi-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png


