---
title: 'Tutorial: Azure Active Directory-Integration mit Citrix ShareFile | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Citrix ShareFile konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ddf6c69b-4a76-4b42-8619-6f2a22800a23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 9dfbf4bcdcd580773a368b90c869bf9c4fefbd77
ms.lasthandoff: 03/25/2017


---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Tutorial: Azure Active Directory-Integration mit Citrix ShareFile

In diesem Tutorial erfahren Sie, wie Sie Citrix ShareFile in Azure Active Directory (Azure AD) integrieren.

Die Integration von Citrix ShareFile in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Citrix ShareFile Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Citrix ShareFile anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Citrix ShareFile konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Citrix ShareFile-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Citrix ShareFile aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-citrix-sharefile-from-the-gallery"></a>Hinzufügen von Citrix ShareFile aus dem Katalog
Zum Konfigurieren der Integration von Citrix ShareFile in Azure AD müssen Sie Citrix ShareFile aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Citrix ShareFile aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld **Citrix ShareFile** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_001.png)

5. Wählen Sie im Ergebnisbereich die Option **Citrix ShareFile** aus und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Citrix ShareFile mithilfe eines Testbenutzers namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Citrix ShareFile als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Citrix ShareFile muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in Citrix ShareFile zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Citrix ShareFile müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Citrix ShareFile-Testbenutzers](#creating-a-citrix-sharefile-test-user)**, um eine Entsprechung von Britta Simon in Citrix ShareFile zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Citrix ShareFile-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Citrix ShareFile die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Citrix ShareFile** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Citrix ShareFile** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_02.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenant-name>.sharefile.com/saml/login`    

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Wenn Sie diese URLs nicht kennen, geben Sie Beispiel-URLs mit Beispielmustern ein. Sie erhalten dann die tatsächlichen URLs, nachdem Sie Schritt 13 durchgeführt haben.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_05.png) 

5. Klicken Sie im Abschnitt **Citrix ShareFile-Konfiguration** auf **Citrix ShareFile konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_06.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_07.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der Citrix ShareFile-Unternehmenswebsite als Administrator an.

7. Klicken Sie oben auf der Symbolleiste auf **Admin**.

8. Klicken Sie im linken Navigationsbereich auf **Configure Single Sign-On**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_002.png)

9. Gehen Sie auf der Dialogseite **SSO-/SAML 2.0-Konfiguration** unter **Grundeinstellungen** folgendermaßen vor:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-sharefile-tutorial/sso_configuration_2.png)

    a. Aktivieren Sie **Enable SAML**.

    b. Geben Sie im Textfeld **IdP-Aussteller/Entitäts-ID** den Wert der **SAML-Entitäts-ID** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c. Klicken Sie auf **Ändern** neben dem Feld **X.509-Zertifikat** und laden Sie dann die heruntergeladene Zertifikatsdatei hoch. 

    d. Geben Sie im Textfeld **Anmelde-URL** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    e. Geben Sie im Textfeld **Abmelde-URL** den Wert für die **Abmelde-URL** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

10. Klicken Sie im Citrix ShareFile-Verwaltungsportal auf **Save** .
    
 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 



### <a name="creating-a-citrix-sharefile-test-user"></a>Erstellen eines Citrix ShareFile-Testbenutzers

Damit sich Azure AD-Benutzer bei Citrix ShareFile anmelden können, müssen sie in Citrix ShareFile bereitgestellt werden. Im Fall von Citrix ShareFile ist die Bereitstellung eine manuelle Aufgabe.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1. Melden Sie sich bei der Citrix ShareFile-Unternehmenswebsite als Administrator an.

2. Klicken Sie oben in der Symbolleiste auf **Benutzer verwalten**. Klicken Sie dann auf **Benutzerbasis verwalten** und auf **Mitarbeiter erstellen**.

    ![Neuer Benutzer](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_010.png "Neuer Benutzer")

3. Führen Sie unter **Grundlegende Informationen** die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_011.png "Neuer Benutzer")

    a. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Kontos von Britta Simon ein.  

    b. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    c. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

4. Klicken Sie auf **Benutzer hinzufügen**.

    > [!NOTE]
    > Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Citrix ShareFile-Benutzerkonten oder mithilfe der von Citrix ShareFile bereitgestellten APIs erstellen.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Citrix ShareFile gewähren.

![Benutzer zuweisen][200] 

**Führen Sie zum Zuweisen von Britta Simon in Citrix ShareFile folgende Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Citrix ShareFile** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Citrix ShareFile“ klicken, sollten Sie automatisch bei Ihrer Citrix ShareFile-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png
