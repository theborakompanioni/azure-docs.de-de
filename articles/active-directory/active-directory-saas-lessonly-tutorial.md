---
title: 'Tutorial: Azure Active Directory-Integration mit Lesson.ly | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Lesson.ly konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 8c9dc6e6-5d85-4553-8a35-c7137064b928
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 33f4c13930017bdcbaf899b16d32ba84bdfbf9ee
ms.openlocfilehash: e51deb2cb890669c4800c232e2b781c5f27117f2


---
# <a name="tutorial-azure-active-directory-integration-with-lessonly"></a>Tutorial: Azure Active Directory-Integration mit Lesson.ly
Dieses Tutorial soll Ihnen zeigen, wie Sie Lesson.ly in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Lesson.ly in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Lesson.ly hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Lesson.ly anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Lesson.ly konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Lesson.ly-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Lesson.ly aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-lessonly-from-the-gallery"></a>Hinzufügen von Lesson.ly aus dem Katalog
Zum Konfigurieren der Integration von Lesson.ly in Azure AD müssen Sie Lesson.ly aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Lesson.ly aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Lesson.ly**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_01.png)
7. Wählen Sie im Ergebnisbereich **Lesson.ly** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Lesson.ly konfiguriert und getestet werden kann.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Lesson.ly als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Das heißt, zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Lesson.ly muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Lesson.ly zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Lesson.ly müssen Sie die folgenden Schritte durchführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Lesson.ly-Testbenutzers](#creating-a-Lessonly-test-user)** , um eine Entsprechung von Britta Simon in Lesson.ly zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Lesson.ly zu authentifizieren.

Die Lesson.ly-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.
Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_06.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Lesson.ly die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Lesson.ly-Anwendungsintegrationsseite im Menü oben auf **Attribute**, um das Dialogfeld **SAML-Tokenattribute** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_07.png) 
2. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_08.png) 
   
    a. Klicken Sie für jede Datenzeile in der obigen Tabelle auf **Benutzerattribut hinzufügen**.
   
    b. Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
   
    c. Wählen Sie in der Liste **Attributwert** den für die Zeile angezeigten Attributwert aus.
   
    d. Klicken Sie unten auf der Seite auf **Abschließen**
3. Klicken Sie auf **Änderungen übernehmen**.
4. Klicken Sie in Ihrem Browser auf **Zurück**, um erneut das Dialogfeld "Schnellstart" zu öffnen.
5. Klicken Sie auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
6. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Lesson.ly anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_03.png) 
7. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_04.png) 

    a. Geben Sie im Textfeld „Anmelde-URL“ die URL, die Benutzer zum Anmelden bei der Lessonly-Anwendung verwenden, im folgenden Format ein: **„https://companyname.lesson.ly/signin“**. Beim Verweisen auf einen generischen Namen muss **companyname** durch einen tatsächlichen Namen ersetzt werden.


1. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Lesson.ly** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_05.png) 
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Weiter**.
2. Wenden Sie sich über dev@lessonly.com. Hängen Sie die heruntergeladene Zertifikatsdatei an Ihre E-Mail-Nachricht an und teilen Sie dem Lesson.ly-Team die Metadaten-URLs (Entitäts-ID, Anmelde-URL und Abmelde-URL für SSO) zum Einrichten von SSO auf ihrer Seite mit.
3. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
   ![Azure AD – einmaliges Anmelden][10]
4. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
   ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_06.png) 
   
    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lessonly-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-lessonly-test-user"></a>Erstellen eines Lesson.ly-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Benutzer namens Britta Simon in Lesson.ly zu erstellen. Lesson.ly unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf Lesson.ly ein neuer Benutzer erstellt. [Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on).

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von Lesson.ly in Verbindung.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf Lesson.ly erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Lesson.ly zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Lesson.ly**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lessonly-tutorial/tutorial_lessonly_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.

Wenn Sie im Zugriffsbereich auf die Kachel „Lesson.ly“ klicken, sollten Sie automatisch bei Ihrer Lesson.ly-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lessonly-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO4-->


