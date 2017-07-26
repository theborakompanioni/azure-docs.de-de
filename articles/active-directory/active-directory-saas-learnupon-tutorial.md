---
title: 'Tutorial: Azure Active Directory-Integration mit LearnUpon | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LearnUpon konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b11c6315-c79d-4f34-9610-bd17070ab7c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c69630688e4bcd68ab3b4ee6d9fdb0e0c46d04b
ms.openlocfilehash: b6ac8acc244e9029be01ede5e0865c280171217d
ms.contentlocale: de-de
ms.lasthandoff: 06/24/2017


---
# <a name="tutorial-azure-active-directory-integration-with-learnupon"></a>Tutorial: Azure Active Directory-Integration mit LearnUpon

In diesem Tutorial erfahren Sie, wie Sie LearnUpon in Azure Active Directory (Azure AD) integrieren.

Die Integration von LearnUpon in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LearnUpon hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LearnUpon anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit LearnUpon konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LearnUpon-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von LearnUpon aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-learnupon-from-the-gallery"></a>Hinzufügen von LearnUpon aus dem Katalog
Zum Konfigurieren der Integration von LearnUpon in Azure AD müssen Sie LearnUpon aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um LearnUpon aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **LearnUpon**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_search.png)

5. Wählen Sie im Ergebnisbereich **LearnUpon** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei LearnUpon mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LearnUpon als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LearnUpon muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in LearnUpon den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei LearnUpon müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines LearnUpon-Testbenutzers](#creating-a-learnupon-test-user)**, um eine Entsprechung von Britta Simon in LearnUpon zu erhalten, die mit ihrer Entsprechung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer LearnUpon-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in LearnUpon die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **LearnUpon** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für LearnUpon** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_url.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<companyname>.learnupon.com/saml/consumer`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um einen Beispielwert. Sie müssen den Wert mit der richtigen Antwort-URL aktualisieren. Wenden Sie sich an das [Supportteam von LearnUpon](https://www.learnupon.com/features/support/), um diesen Wert zu erhalten.



4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Rohdaten)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **LearnUpon-Konfiguration** auf **LearnUpon konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_configure.png) 

7. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich mit einem Administratorkonto bei LearnUpon an. 

8. Klicken Sie auf die Registerkarte **Einstellungen** .
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)

9. Klicken Sie auf **Einmaliges Anmelden – SAML** und anschließend auf **Allgemeine Einstellungen**, um SAML-Einstellungen zu konfigurieren.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png) 

10. Führen Sie im Abschnitt **Allgemeine Einstellungen** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)  
  
    a. Wählen Sie **Aktiviert**.

    b. Wählen Sie für **Version** den Wert **2.0** aus.

    c. Wählen Sie für **Bedingungen überspringen** die Option **Nein** aus.

    d. Geben Sie im Textfeld für den **POST-Parametername für SAML-Token** den Namen des POST-Parameters zur oben angegebenen SAML-Verbraucher-URL mit der zu überprüfenden und authentifizierenden SAML-Assertion ein. Beispiel: **SAMLResponse**.

    e. Geben Sie im Textfeld **Namensbezeichnerformat** den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich die Benutzer-ID (E-Mail-Adresse) befindet. Beispiel: **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.
  
    f. Geben Sie im Textfeld **Identify Provider Location** (Anbieterstandort identifizieren) den Wert ein, der angibt, an welche Adresse die Benutzer weitergeleitet werden, wenn sie auf dem Anmeldebildschirm des Azure-Portals auf das von Ihnen hochgeladene Symbol klicken.
  
    g. Fügen Sie in das Textfeld **Abmelde-URL** die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.
    
    h. Klicken Sie auf **Manage finger prints**(Fingerabdrücke verwalten), und laden Sie dann den Fingerabdruck des heruntergeladenen Zertifikats hoch.

11. Klicken Sie auf **Benutzereinstellungen**, und führen Sie die folgenden Schritte aus:
   
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)  
 
    a. Geben Sie im Textfeld **Format der Vornamen-ID** den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Vorname des Benutzers befindet, z.B. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
  
    b. Geben Sie im Textfeld **Format der Nachnamen-ID** den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Nachname des Benutzers befindet, z.B. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-learnupon-test-user"></a>Erstellen eines LearnUpon-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in LearnUpon. LearnUpon unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf LearnUpon ein neuer Benutzer erstellt. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on).

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem [Supportteam von LearnUpon](https://www.learnupon.com/features/support/) in Verbindung. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf LearnUpon gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon LearnUpon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **LearnUpon**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „LearnUpon“ klicken, sollten Sie automatisch bei Ihrer LearnUpon-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png


