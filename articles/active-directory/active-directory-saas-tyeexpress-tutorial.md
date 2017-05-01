---
title: 'Tutorial: Azure Active Directory-Integration mit T&E Express | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und T&E Express konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: B42374E5-2559-4309-8EF2-820BEE7EBB0C
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 869e5284c71904fcc817ceee0f39d94fab1bc6f3
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-azure-active-directory-integration-with-te-express"></a>Tutorial: Azure Active Directory-Integration mit T&E Express

In diesem Tutorial erfahren Sie, wie Sie T&E Express in Azure Active Directory (Azure AD) integrieren.

Die Integration von T&E Express in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf T&E Express hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei T&E Express anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit T&E Express konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein T&E Express-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von T&E Express aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-te-express-from-the-gallery"></a>Hinzufügen von T&E Express aus dem Katalog
Zum Konfigurieren der Integration von T&E Express in Azure AD müssen Sie T&E Express aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um T&E Express aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **T&E Express** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_search.png)

5. Wählen Sie im Ergebnisbereich **T&E Express** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei T&E Express mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in T&E Express als Gegenpart zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in T&E Express muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in T&E Express zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei T&E Express müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines T&E Express-Testbenutzers](#creating-a-te-express-test-user)**, um in T&E Express eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer T&E Express-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei T&E Express die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **T&E Express** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für T&E Express** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_url.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `https://<domain>.tyeexpress.com` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<domain>.tyeexpress.com/authorize/samlConsume.aspx`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von T&E Express](http://www.tyeexpress.com/contacto.aspx), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_400.png)

8. Um das einmalige Anmelden auf der **T&E Express**-Seite zu konfigurieren, melden Sie sich mit Administratoranmeldeinformationen ohne SAML-SSO bei der T&E Express-Anwendung an.

9. Klicken Sie auf der Registerkarte **Administrator** auf **SAML-Domäne**, um die Seite mit den SAML-Einstellungen zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tye-SAML.png)

10. Ändern Sie die Option **Activar** (Aktivieren) von **No** (Nein) in **Sí** (Ja). Fügen Sie in das Textfeld mit den **Identitätsanbietermetadaten** die XML-Metadaten ein, die Sie aus dem Azure-Portal heruntergeladen haben.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tyeAdmin.png)

11. Klicken Sie auf die Schaltfläche **Guardar** (Speichern), um die Änderungen zu speichern.    


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tyeexpress-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-te-express-test-user"></a>Erstellen eines T&E Express-Testbenutzers

Damit sich Azure AD-Benutzer bei T&E Express anmelden können, müssen sie in T&E Express bereitgestellt werden.  
Im Fall von T&E Express ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der T&E Express-Unternehmenswebsite als Administrator an.

2. Klicken Sie unter dem Admin-Tag auf „Benutzer“ um die Masterseite für Benutzer zu öffnen.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-tyeexpress-tutorial/tye-adminusers.png)

3. Klicken Sie auf der Startseite auf **+**, um die Benutzer hinzuzufügen.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-tyeexpress-tutorial/tye-usershome.png)

4. Geben Sie alle obligatorischen Details ein, die im Formular abgefragt werden, und klicken Sie auf die Schaltfläche zum Speichern der Details.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-tyeexpress-tutorial/tye-usersadd.png)

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-tyeexpress-tutorial/tye-userssave.png)


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf T&E Express gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon T&E Express zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **T&E Express**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tyeexpress-tutorial/tutorial_tyeexpress_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „T&E Express“ klicken, sollten Sie automatisch bei Ihrer T&E Express-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tyeexpress-tutorial/tutorial_general_203.png


