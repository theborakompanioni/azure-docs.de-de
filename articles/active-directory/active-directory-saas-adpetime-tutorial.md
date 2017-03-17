---
title: 'Tutorial: Azure Active Directory-Integration mit ADP eTime | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ADP eTime konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: a3e9f0be-19ed-4b99-a180-619e7624fc26
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2d8d925f80830a0d7047e9567fdd413af2e8c5c3
ms.openlocfilehash: 84c23a43b87c7357ed99f33c275717c68cf7c8bb
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Tutorial: Azure Active Directory-Integration mit ADP eTime
Dieses Tutorial soll Ihnen zeigen, wie Sie ADP eTime in Azure Active Directory (Azure AD) integrieren können.

Die Integration von ADP eTime in Azure AD bietet die folgenden Vorteile:

* Sie können in ADP eTime steuern, wer Zugriff auf ADP eTime hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ADP eTime anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit ADP eTime konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein ADP eTime-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ADP eTime aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-adp-etime-from-the-gallery"></a>Hinzufügen von ADP eTime aus dem Katalog
Zum Konfigurieren der Integration von ADP eTime in Azure AD müssen Sie ADP eTime aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ADP eTime aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals**auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **ADP eTime**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)
7. Wählen Sie im Ergebnisbereich **ADP eTime** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden (SSO) von Azure AD in ADP eTime konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in ADP eTime als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ADP eTime muss eine Linkbeziehung eingerichtet werden.  

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamen** in ADP eTime zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ADP eTime müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ADP eTime-Testbenutzers](#creating-a-adpetime-test-user)** , um eine Entsprechung von Britta Simon in ADP eTime zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in der ADP eTime-Anwendung zu konfigurieren.

Die ADP eTime-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Anspruchsname ist immer eine Kombination aus **PersonImmutableID** und dem Wert, den Sie ExtensionAttribute2 zugewiesen haben. Dieses Attribut enthält die Mitarbeiter-ID (EmployeeID) des Benutzers. 

Hier erfolgt die Benutzerzuordnung von Azure AD zu ADP eTime auf Grundlage der Mitarbeiter-ID. Je nach Anwendungseinstellungen ist jedoch auch eine Zuordnung zu einem anderen Wert möglich. Wenden Sie sich also zunächst an das ADP eTime-Team, um den korrekten Bezeichner eines Benutzers zu verwenden, und ordnen Sie diesen Wert dann dem Anspruch **PersonImmutableID** zu.  

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Bevor Sie die SAML-Assertion konfigurieren können, müssen Sie sich an Ihr ADP eTime-Supportteam wenden und für Ihren Mandanten den Wert des Attributs für den eindeutigen Bezeichner anfordern. Sie benötigen diesen Wert, um den benutzerdefinierten Anspruch für Ihre Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens in Azure AD bei ADP eTime die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ADP eTime** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ADP eTime anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 
  1. Geben Sie im Textfeld **Antwort-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der ADP eTime-Anwendung verwendet wird, nach folgendem Muster ein: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für ADP eTime** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png)  
  1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer. 
  2. Klicken Sie auf **Weiter**.
2. Um SSO für Ihre Anwendung konfigurieren zu lassen, wenden Sie sich an das Supportteam von ADP eTime, und senden Sie zum Konfigurieren für die SSO-Integration die heruntergeladene Metadatendatei als E-Mail-Anhang.
   
   >[!NOTE]
   >Nachdem das **ADP eTime**-Team die Instanz konfiguriert hat, erfragen Sie den Wert **RelayState**. Führen Sie die folgenden Schritte aus, um ihn zu konfigurieren. Nach der Konfiguration können Sie die Integration testen. Bedenken Sie also, dass diese Konfiguration wichtig ist, damit die Anwendungsintegration funktioniert.
   >  
6. Führen Sie die folgenden Schritte aus, um den RelayState-Wert in Azure AD zu konfigurieren: 
  1. Melden Sie sich beim [Azure-Verwaltungsportal](https://portal.azure.com) als Administrator an.
  2. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**.  

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png) 
 3. Geben Sie im Textfeld **Suche** **Azure Active Directory** ein, und klicken Sie dann auf den entsprechenden Link. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)   
 4. Klicken Sie auf **Unternehmensanwendungen**. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)  
 5. Klicken Sie im Bereich **Verwalten** auf **Alle Anwendungen**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png) 
 6. Geben Sie im Textfeld **Suche** **ADP eTime** ein, und klicken Sie dann auf den entsprechenden Link. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)  
 7. Klicken Sie im Bereich **Verwalten** auf **Einmaliges Anmelden**. 
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png) 
 8. Wählen Sie **Erweiterte URL-Einstellungen anzeigen**.
 
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png) 
 9. Geben Sie im Textfeld **Relayzustand** einen Wert nach den folgenden Mustern ein, und speichern Sie die Einstellungen: 
 
    * Produktionsumgebung: `https://fed.adp.com/saml/fedlanding.html?<id>` 
    * Stagingumgebung: `https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`
     
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png) 

4. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
 
 ![Azure AD – einmaliges Anmelden][10]
5. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
 ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.  

* Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png)  
 1. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png)  
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png)  
 1. Notieren Sie den Wert von **Neues Kennwort**.
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-adp-etime-test-user"></a>Erstellen eines ADP eTime-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in ADP eTime. Wenden Sie sich an das Supportteam von ADP eTime, um die Benutzer unter dem ADP eTime-Konto hinzufügen zu lassen. 

>[!NOTE]
>Setzen Sie sich mit dem Supportteam von ADP eTime in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf ADP eTime erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon ADP eTime zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **ADP eTime**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ADP eTime“ klicken, sollten Sie automatisch bei Ihrer ADP eTime-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png

