---
title: 'Tutorial: Azure Active Directory-Integration mit Adobe Creative Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Adobe Creative Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9ba1171e-56b1-4475-b308-58637d35e5a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 3d13608612c77236346b0e98551d7fc427d602e1
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-azure-active-directory-integration-with-adobe-creative-cloud"></a>Tutorial: Azure Active Directory-Integration mit Adobe Creative Cloud

In diesem Tutorial erfahren Sie, wie Sie Adobe Creative Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Adobe Creative Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Adobe Creative Cloud Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Adobe Creative Cloud anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Adobe Creative Cloud konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Adobe Creative Cloud-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Adobe Creative Cloud aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-adobe-creative-cloud-from-the-gallery"></a>Hinzufügen von Adobe Creative Cloud aus dem Katalog
Zum Konfigurieren der Integration von Adobe Creative Cloud in Azure AD müssen Sie Adobe Creative Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Adobe Creative Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Adobe Creative Cloud** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_000.png)

5. Wählen Sie im Ergebnisbereich die Option **Adobe Creative Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD bei Adobe Creative Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Adobe Creative Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Adobe Creative Cloud muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in Adobe Creative Cloud zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Adobe Creative Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Adobe Creative Cloud-Testbenutzers](#creating-an-adobe-creative-cloud-test-user)**, um eine Entsprechung von Britta Simon in Adobe Creative Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Adobe Creative Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Adobe Creative Cloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Adobe Creative Cloud** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_01.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Adobe Creative Cloud** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url1.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `https://www.okta.com/saml2/service-provider/<token>` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<company name>.okta.com/auth/saml20/accauthlinktest`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Setzen Sie sich mit dem Supportteam von Adobe Creative Cloud in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

4. Führen Sie im Abschnitt **Domäne und URLs für Adobe Creative Cloud** die folgenden Schritte aus, wenn Sie die Anwendung im **SP**-initiierten Modus konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_url2.png)

    a. Klicken Sie auf die Option **Erweiterte URL-Einstellungen anzeigen**.

    b. Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `https://adobe.com`

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_05.png) 

6. Klicken Sie im Abschnitt **Adobe Creative Cloud-Konfiguration** auf **Adobe Creative Cloud konfigurieren**, um das Fenster **Anmelden konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** und die **SAML-SSO-Dienst-URL** aus dem Abschnitt mit der Kurzübersicht.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_06.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Adobe Creative Cloud-Mandanten als Administrator an.

8.  Wechseln Sie im linken Navigationsbereich zu **Identität**, und klicken Sie auf Ihre Domäne. Führen Sie im Abschnitt **SSO-Konfiguration erforderlich** die folgenden Schritte aus.

    ![Einstellungen](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_001.png "Einstellungen")

9. Klicken Sie auf **Durchsuchen**, um das aus Azure AD heruntergeladene Zertifikat in das **IDP-Zertifikat** hochzuladen.

10. Fügen Sie im Textfeld **IDP-Aussteller** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Abschnitt **Anmelden konfigurieren** im Azure-Portal kopiert haben.

11. Fügen Sie im Textfeld **IDP-Anmelde-URL** den Wert der **SAML-SSO-Dienst-URL** ein, den Sie aus dem Abschnitt **Anmelden konfigurieren** im Azure-Portal kopiert haben.

12. Wählen Sie **HTTP-Umleitung** als **IDP-Bindung** aus.

13. Wählen Sie **E-Mail-Adresse** als **Einstellung für die Benutzeranmeldung** aus.
 
14. Klicken Sie auf die Schaltfläche **Save** .

15. Im Dashboard wird jetzt die XML-Datei **Metadaten herunterladen** angezeigt. Sie enthält die EntityDescriptor-URL und die AssertionConsumerService-URL von Adobe. Öffnen Sie die Datei, und konfigurieren sie diese in der Azure AD-Anwendung.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_002.png)

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_003.png)

    a. Verwenden Sie den von Adobe bereitgestellten EntityDescriptor-Wert für **Bezeichner** im Dialogfeld **App-Einstellungen konfigurieren**.

    b. Verwenden Sie den von Adobe bereitgestellten AssertionConsumerService-Wert für **Antwort-URL** im Dialogfeld **App-Einstellungen konfigurieren**.
 
### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 

### <a name="creating-an-adobe-creative-cloud-test-user"></a>Erstellen eines Adobe Creative Cloud-Testbenutzers

Damit sich Azure AD-Benutzer bei Adobe Creative Cloud anmelden können, müssen sie in Adobe Creative Cloud bereitgestellt werden.  
Im Fall von Adobe Creative Cloud ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der Adobe Creative Cloud-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **People**.

    ![Personen](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_001.png "Personen")

3. Klicken Sie auf **Benutzer einladen**.

    ![Benutzer einladen](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_002.png "Benutzer einladen")

4. Führen Sie auf der Dialogfeldseite **Personen einladen** die folgenden Schritte aus:

    ![Personen einladen](./media/active-directory-saas-adobe-creative-cloud-tutorial/create_aaduser_003.png "Personen einladen")

    a. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.
    
    b. Klicken Sie auf **Invite**.

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Adobe Creative Cloud gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Adobe Creative Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Adobe Creative Cloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_adobe-creative-cloud_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Adobe Creative Cloud“ klicken, werden Sie automatisch bei Ihrer Adobe Creative Cloud-Anwendung angemeldet.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-adobe-creative-cloud-tutorial/tutorial_general_203.png
