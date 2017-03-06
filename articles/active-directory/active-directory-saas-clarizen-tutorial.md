---
title: 'Tutorial: Azure Active Directory-Integration mit Clarizen | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Clarizen konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 3908c354a8551d7b16c759a48ef3e5316bfa11fa
ms.openlocfilehash: 574c6877bddac8be7d6d541bfabbdc10f6be3101
ms.lasthandoff: 02/21/2017


---
# <a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Lernprogramm: Azure Active Directory-Integration mit Clarizen

In diesem Tutorial erfahren Sie, wie Sie Clarizen in Azure Active Directory (Azure AD) integrieren. Diese Integration bietet Ihnen die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Clarizen zugreifen kann.
- Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Clarizen anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort, im Azure-Portal, verwalten.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptaufgaben:

1. Hinzufügen von Clarizen aus dem Katalog
2. Konfigurieren und testen Sie das einmalige Anmelden in Azure AD.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Clarizen konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Clarizen-Abonnement, das für einmaliges Anmelden aktiviert ist

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Verwenden Sie keine Produktionsumgebung, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="add-clarizen-from-the-gallery"></a>Hinzufügen von Clarizen aus dem Katalog
Zum Konfigurieren der Integration von Clarizen in Azure AD fügen Sie Clarizen über den Katalog der Liste mit den verwalteten SaaS-Apps hin.

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf das Symbol **Azure Active Directory**.

    ![Azure Active Directory-Symbol][1]

2. Klicken Sie auf **Unternehmensanwendungen**. Klicken Sie dann auf **Alle Anwendungen**.

    ![Klicken auf „Unternehmensanwendungen“ und „Alle Anwendungen“][2]

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**.

    ![Schaltfläche „Hinzufügen“][3]

4. Geben Sie im Suchfeld als Suchbegriff **Clarizen** ein.

    ![Eingeben von „Clarizen“ in das Suchfeld](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_000.png)

5. Wählen Sie im Ergebnisbereich **Clarizen** aus, und klicken Sie dann auf **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Wählen von Clarizen im Ergebnisbereich](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In den folgenden Abschnitten konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Clarizen mithilfe der Testbenutzerin Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Clarizen als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Clarizen muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD dem **Benutzernamen** in Clarizen zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Clarizen müssen Sie die folgenden Aufgaben ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit der Testbenutzerin Britta Simon zu testen.
3. **[Erstellen eines Clarizen-Testbenutzers](#create-a-clarizen-test-user)**, um eine Entsprechung von Britta Simon in Clarizen zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
Aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal, und konfigurieren Sie das einmalige Anmelden in Ihrer Clarizen-Anwendung.

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Clarizen** auf **Einmaliges Anmelden**.

    ![Klicken auf „Einmaliges Anmelden“][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswählen von „SAML-basierte Anmeldung“](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_01.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Clarizen** die folgenden Schritte aus:

    ![Felder für Bezeichner und Antwort-URL](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_02.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert **Clarizen** ein.

    b. Geben Sie in das Feld **Antwort-URL** eine URL mit dem folgenden Muster ein: **https://<company name>.clarizen.com/Clarizen/Pages/Integrations/SAML/SamlResponse.aspx**

    > [!NOTE]
    > Hierbei handelt es sich um Beispielwerte. Sie müssen die tatsächlichen Werte für Bezeichner und Antwort-URL verwenden. Hier empfehlen wir Ihnen, den eindeutigen Wert einer Zeichenfolge als Bezeichner zu verwenden. Um die tatsächlichen Werte zu erhalten, wenden Sie sich an das [Clarizen-Supportteam](https://success.clarizen.com/hc/en-us/requests/new).

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Klicken auf „Neues Zertifikat erstellen“](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_03.png)     

5. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein Ablaufdatum aus. Klicken Sie anschließend auf **Speichern**.

    ![Auswählen und Speichern eines Ablaufdatums](./media/active-directory-saas-clarizen-tutorial/tutorial_general_300.png)

6. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Neues Zertifikat aktivieren**, und klicken Sie dann auf **Speichern**.

    ![Aktivieren des Kontrollkästchens zum Aktivieren des neuen Zertifikats](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_04.png)

7. Klicken Sie im Dialogfeld **Rolloverzertifikat** auf **OK**.

    ![Klicken auf „OK“, um sicherzustellen, dass das Zertifikat aktivieren möchten](./media/active-directory-saas-clarizen-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Klicken auf „Zertifikat (Base64)“, um den Download zu starten](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_05.png)

9. Klicken Sie im Abschnitt **Clarizen-Konfiguration** auf **Clarizen konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Klicken auf „Clarizen konfigurieren“](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_06.png)

    ![Fenster „Anmelden konfigurieren“ einschließlich Dateien und URLs](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_07.png)

10. Melden Sie sich in einem anderen Webbrowserfenster bei der Clarizen-Unternehmenswebsite als Administrator an.

11. Klicken Sie auf Ihren Benutzernamen und dann auf **Settings**.

    ![Klicken auf „Settings“ unter Ihrem Benutzernamen](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_001.png "Settings")

12. Klicken Sie auf die Registerkarte **Global Settings**. Klicken Sie anschließend neben **Federated Authentication** auf **edit**.

    ![Registerkarte „Global Settings“](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_002.png "Global Settings")

13. Führen Sie im Dialogfeld **Federated Authentication** die folgenden Schritte aus:

    ![Dialogfeld „Federated Authentication“](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_003.png "Federated Authentication")

    a. Wählen Sie **Enable Federated Authentication** (Verbundauthentifizierung aktivieren).

    b. Klicken Sie auf **Upload** , um das heruntergeladene Zertifikat hochzuladen.

    c. Geben Sie im Feld **Anmelde-URL** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    d. Geben Sie im Feld für die **Abmelde-URL** den Wert für die **Abmelde-URL** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    e. Aktivieren Sie **Use POST**.

    f. Klicken Sie auf **Speichern**.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen Britta Simon.

![Name und E-Mail-Adresse des Azure AD-Testbenutzers][100]

1. Klicken Sie im linken Bereich des Azure-Portals auf das Symbol **Azure Active Directory**.

    ![Azure Active Directory-Symbol](./media/active-directory-saas-clarizen-tutorial/create_aaduser_01.png)

2. Klicken Sie auf **Benutzer und Gruppen** und dann auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.

    ![Klicken auf „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-clarizen-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-clarizen-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“, ausgefüllt mit Name, E-Mail-Adresse und Kennwort](./media/active-directory-saas-clarizen-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die E-Mail-Adresse des Kontos von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.

### <a name="create-a-clarizen-test-user"></a>Erstellen eines Clarizen-Testbenutzers
Um für Azure AD-Benutzer das Anmelden bei Clarizen zu aktivieren, müssen Sie Benutzerkonten bereitstellen. Im Fall von Clarizen ist die Bereitstellung eine manuelle Aufgabe.

1. Melden Sie sich bei der Clarizen-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **People**.

    ![Klicken Sie auf „People“](./media/active-directory-saas-clarizen-tutorial/create_aaduser_001.png "Personen")

3. Klicken Sie auf **Invite User**.

    ![Schaltfläche „Invite User“](./media/active-directory-saas-clarizen-tutorial/create_aaduser_002.png "Benutzer einladen")

4. Führen Sie auf der Dialogfeldseite **Invite People** die folgenden Schritte aus:

    ![Dialogfeld „Invite People“](./media/active-directory-saas-clarizen-tutorial/create_aaduser_003.png "Personen einladen")

    a. Geben Sie im Feld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    b. Klicken Sie auf **Invite**.

    > [!NOTE]
    > Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail und folgt einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Clarizen gewähren.

![Zugewiesener Testbenutzer][200]

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht, klicken Sie auf **Unternehmensanwendungen** und dann auf **Alle Anwendungen**.

    ![Klicken auf „Unternehmensanwendungen“ und „Alle Anwendungen“][201]

2. Wählen Sie in der Anwendungsliste **Clarizen** aus.

    ![Wählen von Clarizen in der Liste](./media/active-directory-saas-clarizen-tutorial/tutorial_clarizen_50.png)

3. Klicken Sie auf der linken Seite auf **Benutzer und Gruppen**.

    ![Klicken auf „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Die Schaltfläche „Hinzufügen“ und das Dialogfeld „Zuordnung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Testen Sie Ihre Azure AD-Konfiguration für das einmalige Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Clarizen“ klicken, sollten Sie automatisch bei Ihrer Clarizen-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-clarizen-tutorial/tutorial_general_203.png

