---
title: 'Tutorial: Azure Active Directory-Integration mit MCM | Microsoft Docs'
description: Erfahren Sie, wie Sie MCM mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/30/2016
ms.author: jeedes

---
# Tutorial: Azure Active Directory-Integration mit MCM
In diesem Tutorial erfahren Sie, wie Sie MCM in Azure Active Directory (Azure AD) integrieren.

Die Integration von MCM in Azure AD bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf MCM haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei MCM anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen
Um die Azure AD-Integration mit MCM konfigurieren zu können, benötigen Sie Folgendes:

* Ein gültiges Azure-Abonnement
* Ein SSO-fähiges MCM-Abonnement

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von MCM über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## Hinzufügen von MCM über den Katalog
Um die Integration von MCM in Azure AD zu konfigurieren, müssen Sie MCM über den Katalog Ihrer Liste mit verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um MCM über den Katalog hinzuzufügen:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
   ![Anwendungen](./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
   
   ![Anwendung hinzufügen](./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **MCM** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich die Option **MCM** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![MCM](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

## Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in MCM konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in MCM als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in MCM muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen in Azure AD dem Benutzernamen in MCM zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei MCM müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines MCM-Testbenutzers](#creating-a-mcm-test-user)**, um in MCM eine Entsprechung von Britta Simon zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer MCM-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit MCM die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **MCM** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Configure single sign-on](./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei MCM anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD – einmaliges Anmelden")
3. Führen Sie auf der Dialogfeldseite zum Konfigurieren der App-Einstellungen die folgenden Schritte aus:
   
   ![App-URL konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "App-URL konfigurieren")
   
   a. Geben Sie im Textfeld **Anmelde-URL** die URL `https://myaba.co.uk/client-access/<company name>/saml.php` ein.
   
   b. Klicken Sie auf **Weiter**.
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für MCM** auf **Metadaten herunterladen**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Einmaliges Anmelden konfigurieren")
5. Wenden Sie sich an das Supportteam von MCM, um SSO für Ihre Anwendung konfigurieren zu lassen. Geben Sie die heruntergeladene Metadatendatei an das MCM-Team weiter, damit es SSO bei sich einrichten kann.
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Einmaliges Anmelden konfigurieren")
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Einmaliges Anmelden konfigurieren")

### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.

### Erstellen eines MCM-Testbenutzers
In diesem Abschnitt erstellen Sie in MCM einen Benutzer namens Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer ggf. vom MCM-Supportteam unterstützen.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von MCM-Benutzerkonten oder mithilfe der von MCM bereitgestellten APIs erstellen.
> 
> 

### Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem ihr Zugriff auf MCM gewährt wird.

![Benutzer zuweisen](./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Benutzer zuweisen")

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu MCM durchzuführen:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Benutzer zuweisen")
2. Wählen Sie in der Anwendungsliste die Option **MCM** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Benutzer zuweisen")
4. Wählen Sie in der Benutzerliste **Britta Simon** aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen](./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Benutzer zuweisen")

### Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „MCM“ klicken, sollten Sie automatisch bei Ihrer MCM-Anwendung angemeldet werden.

## Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!---HONumber=AcomDC_0907_2016-->