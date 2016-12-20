---
title: 'Tutorial: Azure Active Directory-Integration mit Tangoe Command Premium Mobile | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tangoe Command Premium Mobile konfigurieren."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 560feea958c6cb3021395ca88dfe07fd1ead2842


---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Tutorial: Azure Active Directory-Integration mit Tangoe Command Premium Mobile
In diesem Tutorial erfahren Sie, wie Sie Tangoe Command Premium Mobile in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tangoe Command Premium Mobile in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Tangoe Command Premium Mobile hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tangoe Command Premium Mobile anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Tangoe Command Premium Mobile konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement
* Ein Tangoe Command Premium Mobile-Abonnement, für das einmaliges Anmelden aktiviert ist

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

1. Hinzufügen von Tangoe Command Premium Mobile aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Hinzufügen von Tangoe Command Premium Mobile aus dem Katalog
Zum Konfigurieren der Integration von Tangoe Command Premium Mobile in Azure AD müssen Sie Tangoe Command Premium Mobile aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Tangoe Command Premium Mobile aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld **Tangoe Command Premium Mobile**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)
7. Wählen Sie im Ergebnisbereich **Tangoe Command Premium Mobile** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Tangoe Command Premium Mobile basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tangoe Command Premium Mobile als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tangoe Command Premium Mobile muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert von **Username** in Tangoe Command Premium Mobile zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tangoe Command Premium Mobile müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Tangoe Command Premium Mobile-Testbenutzers](#creating-an-tangoe-test-user)** , um eine Entsprechung von Britta Simon in Tangoe Command Premium Mobile zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Tangoe Command Premium Mobile-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tangoe Command Premium Mobile die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Tangoe Command Premium Mobile** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6]
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Tangoe Command Premium Mobile anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 

    a. Geben Sie in das Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern für die Anmeldung bei Ihrer Tangoe Command Premium Mobile-Anwendung verwendet wird. Verwenden Sie dabei das folgende Muster: **„https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<Mandantenaussteller\>&Ziel=\<URL der Zielseite\>”**.

    b. Geben Sie im Textfeld **Antwort-URL** die URL nach folgendem Muster ein: **https://sso.tangoe.com/sp/ACS.saml2**

    > [AZURE.NOTE]  Wenn Sie die richtigen Werte für die URLs nicht kennen, können Sie die oben stehenden Werte als Platzhalter verwenden und die richtigen Werte vom Tangoe-Kundensupport anfordern.


1. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Tangoe Command Premium Mobile** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 
   
    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Weiter**.
2. Wenden Sie sich an den Tangoe-Kundensupport, und stellen Sie Folgendes bereit, um das einmalige Anmelden für Ihre Anwendung konfigurieren zu lassen:

    - Die heruntergeladene Metadatendatei
    - Die **Aussteller-URL**
    - Die **SAML-SSO-URL**
    - Die **Dienst-URL für einmaliges Abmelden**



1. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
2. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Erstellen eines Tangoe Command Premium Mobile-Testbenutzers
In diesem Abschnitt erstellen Sie in Tangoe Command Premium Mobile einen Benutzer mit dem Namen Britta Simon. In der Tangoe Command Premium Mobile-Anwendung müssen alle Benutzer bereitgestellt werden, bevor das einmalige Anmelden verwendet werden kann. Wenden Sie sich daher an den Tangoe-Kundensupport, um sämtliche Benutzer in der Anwendung bereitzustellen. 

> [!NOTE]
> Wenn Sie einen Benutzer manuell oder eine Gruppe von Benutzern erstellen müssen, wenden Sie sich an das Tangoe Command Premium Mobile-Supportteam.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf Tangoe Command Premium Mobile gewähren.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu Tangoe Command Premium Mobile führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

![Benutzer zuweisen][201] 

1. Wählen Sie in der Anwendungsliste **Tangoe Command Premium Mobile**aus.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. Klicken Sie im oberen Menü auf **Benutzer**.

![Benutzer zuweisen][203] 

1. Wählen Sie in der Benutzerliste **Britta Simon**aus.
2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tangoe Command Premium Mobile“ klicken, sollten Sie automatisch bei Ihrer Tangoe Command Premium Mobile-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


