---
title: 'Tutorial: Azure Active Directory-Integration mit EthicsPoint Incident Management (EPIM) | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und EthicsPoint Incident Management (EPIM) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: 8cb31a4c-9309-469b-93ac-daf0d3c7a3e6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 33a5e085722fd9accf70793a580b5c8a5dc124c4
ms.openlocfilehash: 83cfbd6a4297577dc5284982fcc5d0c1318fbf5c


---
# <a name="tutorial-azure-active-directory-integration-with-ethicspoint-incident-management-epim"></a>Tutorial: Azure Active Directory-Integration mit EthicsPoint Incident Management (EPIM)

In diesem Tutorial erfahren Sie, wie Sie EthicsPoint Incident Management (EPIM) in Azure Active Directory (Azure AD) integrieren.

Die Integration von EthicsPoint Incident Management (EPIM) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf EthicsPoint Incident Management (EPIM) haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei EthicsPoint Incident Management (EPIM) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit EthicsPoint Incident Management (EPIM) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Abonnement für EthicsPoint Incident Management (EPIM)


>[!NOTE] 
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von EthicsPoint Incident Management (EPIM) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-ethicspoint-incident-management-epim-from-the-gallery"></a>Hinzufügen von EthicsPoint Incident Management (EPIM) aus dem Katalog
Zum Konfigurieren der Integration von EthicsPoint Incident Management (EPIM) in Azure AD müssen Sie EthicsPoint Incident Management (EPIM) aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um EthicsPoint Incident Management (EPIM) aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie in das Suchfeld **EthicsPoint Incident Management** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_01.png)

7. Wählen Sie im Ergebnisbereich die Option **EthicsPoint Incident Management** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit EthicsPoint Incident Management (EPIM).

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in EthicsPoint Incident Management (EPIM) als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in EthicsPoint Incident Management (EPIM) muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird eingerichtet, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in EthicsPoint Incident Management (EPIM) zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit EthicsPoint Incident Management (EPIM) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für EthicsPoint Incident Management (EPIM)](#creating-a-ethicspoint-incident-management-test-user)**, um eine Entsprechung von Britta Simon in EthicsPoint Incident Management (EPIM) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer EthicsPoint Incident Management-Anwendung (EPIM).


**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit EthicsPoint Incident Management (EPIM) zu konfigurieren:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **EthicsPoint Incident Management (EPIM)** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei EthicsPoint Incident Management (EPIM) anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_04.png) 

    a. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei EthicsPoint Incident Management (EPIM) verwendet wird, nach folgendem Muster ein: `https://<companyname>.navexglobal.com` oder `https://<companyname>.ethicspointvp.com`.
    
    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<servername>.navexglobal.com/adfs/ls/`.

    c. Klicken Sie auf **Weiter**
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für EthicsPoint Incident Management (EPIM)** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_05.png)

    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Wenden Sie sich an das Supportteam von EthicsPoint Incident Management (EPIM), um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie die heruntergeladenen **Metadatendatei** bereit.

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
 
    ![Azure AD – einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.


![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_06.png) 

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ethicspoint-incident-management-tutorial/create_aaduser_08.png) 

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-an-ethicspoint-incident-management-epim-test-user"></a>Erstellen eines Testbenutzers für EthicsPoint Incident Management (EPIM)

In diesem Abschnitt erstellen Sie in EthicsPoint Incident Management (EPIM) einen Benutzer namens Britta Simon. Arbeiten Sie mit dem Supportteam von EthicsPoint Incident Management (EPIM) zusammen, um die Benutzer zur Plattform von EthicsPoint Incident Management (EPIM) hinzuzufügen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf EthicsPoint Incident Management (EPIM) gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon zu EthicsPoint Incident Management (EPIM) hinzuzufügen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **EthicsPoint Incident Management (EPIM)** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_ethicspointincidentmanagement_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

    ![Benutzer zuweisen][205]


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „EthicsPoint Incident Management (EPIM)“ klicken, sollten Sie automatisch bei Ihrer EthicsPoint Incident Management (EPIM)-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ethicspoint-incident-management-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


