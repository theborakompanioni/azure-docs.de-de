---
title: 'Tutorial: Azure Active Directory-Integration mit Keylight | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Keylight konfigurieren."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 234a32f1-9f56-4650-9e31-7b38ad734b1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2318fd7676e0d2cf2a897024ef1bcb28e7511bae


---
# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Tutorial: Azure Active Directory-Integration mit Keylight
In diesem Tutorial erfahren Sie, wie Sie Keylight in Azure Active Directory (Azure AD) integrieren.

Die Integration von Keylight in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Keylight hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Keylight anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Keylight konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement
* Ein Keylight-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Keylight aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-keylight-from-the-gallery"></a>Hinzufügen von Keylight aus dem Katalog
Zum Konfigurieren der Integration von Keylight in Azure AD müssen Sie Keylight aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Keylight aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Keylight**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)
7. Wählen Sie im Ergebnisbereich **Keylight** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Keylight basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Keylight müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Keylight-Testbenutzers](#creating-a-keylight-test-user)** , um ein Gegenstück zu Britta Simon in Keylight zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Keylight-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Keylight die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Keylight** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Keylight anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 

    a. Geben Sie im Textfeld „Anmelde-URL“ die von Ihren Benutzern für die Anmeldung bei Ihrer Keylight-Anwendung verwendete URL in folgendem Format ein: **https://\<Unternehmensname\>.keylightgrc.com/Login.aspx?saml=1**.


1. Führen Sie auf der Seite **Einmaliges Anmelden bei Keylight konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Weiter**.
2. Führen Sie die folgenden Schritte aus, um das einmalige Anmelden in Keylight zu aktivieren:
   
    a. Melden Sie sich als Administrator bei Ihrem Keylight-Konto an.
   
    b. Klicken Sie im Menü oben auf **Person**, und wählen Sie **Keylight Setup** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/401.png) 
   
    c. Klicken Sie in der Strukturansicht links auf **SAML**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/402.png) 
   
    d. Klicken Sie im Dialogfeld **SAML-Einstellungen** auf **Bearbeiten**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/404.png) 
3. Führen Sie auf der Dialogfeldseite **Edit SAML Settings** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/405.png) 
   
    a. Legen Sie für **SAML-Authentifizierung** die Einstellung **Aktiv** fest.

    b. Kopieren Sie im klassischen Azure AD-Portal den Wert **SAML-SSO-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.

    c. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Dienst-URL für einmalige Abmeldung**, und fügen Sie ihn in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.

    d. Klicken Sie auf **Datei auswählen**, um das heruntergeladene Keylight-Zertifikat auszuwählen, und klicken Sie dann auf **Öffnen**, um das Zertifikat hochzuladen.


    e. Legen Sie für **Speicherort der SAML-Benutzer-ID** die Einstellung **NameIdentifier-Element der subject-Anweisung** fest.

    f. Geben Sie den **Keylight-Dienstanbieter in folgendem Format ein: **https://&lt;Unternehmensname&gt;.keylightgrc.com**.

    g. Legen Sie für **Benutzer automatisch bereitstellen** die Einstellung **Aktiv** fest.

    h. Legen Sie für **Kontotyp automatisch bereitstellen** die Einstellung **Vollständiger Benutzer** fest.

    i. Wählen Sie für **Sicherheitsrolle automatisch bereitstellen** die Option **Standardbenutzer mit SAML** aus.

    j. Legen Sie für **Sicherheitskonfiguration automatisch bereitstellen** die Einstellung **Standardbenutzerkonfiguration** fest.

    k. Geben Sie in das Textfeld „E-Mail“ Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    l. Geben Sie in das Textfeld für das **Vornamenattribute** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Geben Sie in das Textfeld für das **Nachnamenattribute** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    n. Klicken Sie auf **Speichern**.




1. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
2. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Azure-Portal einen Testbenutzer mit dem Namen Britta Simon.

Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-keylight-test-user"></a>Erstellen eines Keylight-Testbenutzers
In diesem Abschnitt erstellen Sie in Keylight einen Benutzer mit dem Namen Britta Simon. Keylight unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls der Benutzer noch nicht vorhanden ist, wird er erstellt, wenn Sie auf Keylight zugreifen. 

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von Keylight in Verbindung.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf Keylight gewähren.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu Keylight führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Keylight**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Keylight“ klicken, sollten Sie automatisch bei Ihrer Keylight-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


