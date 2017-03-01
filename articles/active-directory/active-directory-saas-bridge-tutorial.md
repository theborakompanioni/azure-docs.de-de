---
title: 'Tutorial: Azure Active Directory-Integration mit Bridge | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden zwischen Azure Active Directory und Bridge konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dbb6499-80c1-4d00-a0b4-e0ad5522cf0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1b01c8a16c217350e82db1bc7a6cc54b84888b9e
ms.openlocfilehash: 17898ff5a2a98383a9956346d7932c31587386a7
ms.lasthandoff: 12/13/2016


---
# <a name="tutorial-azure-active-directory-integration-with-bridge"></a>Tutorial: Azure Active Directory-Integration mit Bridge

In diesem Tutorial erfahren Sie, wie Sie Bridge in Azure Active Directory (Azure AD) integrieren.

Die Integration von Bridge in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Bridge hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Bridge anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Bridge benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Bridge-Abonnement


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Bridge aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-bridge-from-the-gallery"></a>Hinzufügen von Bridge aus dem Katalog
Zum Konfigurieren der Integration von Bridge in Azure AD müssen Sie Bridge aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Bridge aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie **Bridge** in das Suchfeld ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_001.png)

7. Wählen Sie im Ergebnisbereich den Eintrag **Bridge** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Bridge mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Bridge einem Benutzer in Azure AD entspricht. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Bridge muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Bridge zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Bridge müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Bridge-Testbenutzers](#creating-a-bridge-test-user)**, um in Bridge einen Gegenpart für Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer Bridge-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Bridge die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Bridge** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bridge anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_02.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_03.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.bridgeapp.com`

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<company name>.bridgeapp.com`

    c. Klicken Sie auf **Weiter**.

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Sie können <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">hier</a> ein Supportticket bei Bridge erstellen, um diese Werte zu erhalten.

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Bridge** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_04.png) 

5. Sie können <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">hier</a> ein Supportticket bei Bridge erstellen und die folgenden Informationen angeben, um einmaliges Anmelden für Ihre Anwendung konfigurieren zu lassen: 

    •  Die heruntergeladene **Zertifikatsdatei**

    •  Die **Entitäts-ID**

    •  Die **Dienst-URL für einmalige Anmeldung**

    •  Die **Dienst-URL für einmalige Abmeldung**

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
  
    ![Azure AD – einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_06.png) 

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bridge-tutorial/create_aaduser_08.png) 

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-a-bridge-test-user"></a>Erstellen eines Bridge-Testbenutzers

In diesem Abschnitt erstellen Sie in Bridge einen Benutzer namens Britta Simon. Wenden Sie sich an das Supportteam von Bridge, um einen Benutzer in der Plattform zu erstellen. Sie können <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">hier</a> ein Supportticket bei Bridge erstellen, um die Benutzer in der Bridge-Plattform hinzuzufügen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Bridge gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Bridge durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Bridge**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Bridge“ klicken, sollten Sie automatisch bei Ihrer Bridge-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_205.png

