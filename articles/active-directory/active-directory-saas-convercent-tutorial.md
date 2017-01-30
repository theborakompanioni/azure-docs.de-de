---
title: 'Tutorial: Azure Active Directory-Integration mit Convercent | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Convercent konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: f9c9d290-0e13-490b-b559-0be772d6a690
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: aec25285cd0fa2f09e21a629648e015dffbeb11d
ms.openlocfilehash: 9ea835787db25adabe9b8cb71551de49b330a6dd


---
# <a name="tutorial-azure-active-directory-integration-with-convercent"></a>Tutorial: Azure Active Directory-Integration mit Convercent

In diesem Tutorial erfahren Sie, wie Sie Convercent in Azure Active Directory (Azure AD) integrieren.

Die Integration von Convercent in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Convercent hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Convercent anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Convercent konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Convercent-Abonnement, für das einmaliges Anmelden aktiviert ist


>[!NOTE] 
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Convercent aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-convercent-from-the-gallery"></a>Hinzufügen von Convercent aus dem Katalog
Zum Konfigurieren der Integration von Convercent in Azure AD müssen Sie Convercent aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Convercent aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **Convercent** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_01.png)

7. Wählen Sie im Ergebnisbereich die Option **Convercent** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Convercent basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Convercent als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Convercent muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Convercent zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Convercent müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Convercent-Testbenutzers](#creating-a-convercent-test-user)**, um eine Entsprechung von Britta Simon in Convercent zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Convercent-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Convercent die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Convercent** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Convercent anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_03.png) 

3. Klicken Sie auf der Seite **App-Einstellungen konfigurieren** auf „Weiter“. Hiermit konfigurieren Sie die Anwendung im IdP-initiierten Modus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_04.png) 

    >[!NOTE] 
    >Da Sie diese Anwendung im IdP-initiierten Modus konfigurieren, müssen Sie RelayState für die Anwendung konfigurieren. Andernfalls funktioniert die SSO-Integration nicht. Führen Sie Schritt 5 für die RelayState-Konfiguration aus.

4. Wenn Sie die Anwendung im SP-initiierten Modus konfigurieren möchten, klicken Sie auf das Kontrollkästchen **Erweiterte Einstellungen anzeigen** und führen die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_05.png) 

    a. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern zur Anmeldung bei Ihrer Convercent-Anwendung verwendet wird. Verwenden Sie dabei das folgende Muster: `https://app.convercent.com/`.
    
    b. Klicken Sie auf **Weiter**.
 
5. Wenn Sie die Anwendung im IdP-initiierten Modus konfiguriert haben, konfigurieren Sie den RelayState-Wert für die Anwendung. Führen Sie die folgenden Schritte aus, um den RelayState-Wert in Azure AD zu konfigurieren: 
    
    a. Melden Sie sich beim [Azure-Verwaltungsportal](https://portal.azure.com) als Administrator an.

    b. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_07.png)

    c. Geben Sie im Textfeld **Suche** **Azure Active Directory** ein, und klicken Sie dann auf den entsprechenden Link.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_08.png)

    d. Klicken Sie auf **Unternehmensanwendungen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_09.png)

    e. Klicken Sie im Bereich **Verwalten** auf **Alle Anwendungen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_10.png)

    f. Geben Sie im Textfeld **Suche** **ADP eTime** ein, und klicken Sie dann auf den entsprechenden Link. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_11.png)

    g. Klicken Sie im Bereich **Verwalten** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_12.png)

    h. Wählen Sie **Erweiterte URL-Einstellungen anzeigen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_13.png)
    
    i. Geben Sie im Textfeld **Relayzustand** einen Wert nach den folgenden Mustern ein: `https://app.convercent.com/`.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_14.png)

    j. Speichern Sie die Einstellungen.

6. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Convercent** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_06.png)

    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


6. Wenden Sie sich an das [Supportteam](mailTo:support@convercent.com) von Convercent, um SSO (einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:

    a. Die heruntergeladenen **Metadaten**

7. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]

8. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
 
    ![Azure AD – einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_06.png) 

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-convercent-tutorial/create_aaduser_08.png) 

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-an-convercent-test-user"></a>Erstellen eines Convercent-Testbenutzers

In diesem Abschnitt erstellen Sie in Convercent einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Supportteam](mailTo:support@convercent.com) von Convercent, um die Benutzer der Convercent-Plattform hinzufügen zu lassen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Convercent gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Convercent zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Convercent** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-convercent-tutorial/tutorial_convercent_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Convercent“ klicken, sollten Sie automatisch in Ihrer Convercent-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-convercent-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO3-->


