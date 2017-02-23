---
title: 'Tutorial: Azure Active Directory-Integration mit Tableau Server | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Server konfigurieren."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0849c77c66ac8617e217a69696b5c404be3b5eb4
ms.openlocfilehash: 3aefb8188880fac5c1fcbe237a1e133584089e6a

---

# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Azure Active Directory-Integration mit Tableau Server
Dieses Tutorial soll Ihnen zeigen, wie Sie Tableau Server in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tableau Server in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Tableau Server hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Server anzumelden (einmaliges Anmelden).
* Mit dem klassischen Azure-Portal können Sie Ihre Konten an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Tableau Server konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Tableau Server-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Tableau Server aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-tableau-server-from-the-gallery"></a>Hinzufügen von Tableau Server aus dem Katalog
Zum Konfigurieren der Integration von Tableau Server in Azure AD müssen Sie Tableau Server aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Tableau Server aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld **Tableau Server**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)
7. Wählen Sie im Ergebnisbereich **Tableau Server** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Tableau Server konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tableau Server als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Server muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für **Username** in Tableau Server zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Server müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Tableau Server-Testbenutzers](#creating-a-tableauserver-test-user)** , um eine Entsprechung von Britta Simon in Tableau Server zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Tableau Server-Anwendung zu konfigurieren.

Die Tableau Server-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tableau Server die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **Tableau Server** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 
2. Führen Sie im Dialogfeld **SAML-Tokenattribute** die folgenden Schritte aus:

   1. Klicken Sie auf **Benutzerattribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 
   2. Geben Sie im Textfeld **Attributname** die Zeichenfolge **username** ein.
   3. Wählen Sie in der Liste **Attributwert** den Wert **user.displayname** aus.
   4. Klicken Sie auf **Fertig stellen**.    

3. Klicken Sie im oberen Menü auf **Schnellstart**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  
4. Klicken Sie auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
5. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Tableau Server anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 
6. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 

   1. Geben Sie im Textfeld **Anmelde-URL** die URL Ihrer Tableau Server-Instanz ein. 
   2. Kopieren Sie die URL in das Feld **Bezeichner**.
   3. Klicken Sie auf **Weiter**.
 h.
7. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Tableau Server** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 

   1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   2. Klicken Sie auf **Weiter**.

8. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem Tableau Server-Mandanten anmelden.
   
   1. Klicken Sie in der Tableau Server-Konfiguration auf die Registerkarte **SAML** .
  
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
   2. Aktivieren Sie das Kontrollkästchen **Use SAML for single sign-on**.
   3. Suchen Sie nach der Datei mit Ihren Verbundmetadaten, die Sie aus dem klassischen Azure-Portal heruntergeladen haben, und laden Sie sie in die **SAML Idp metadata file**hoch.
   4. „Tableau Server return URL“: Die URL, auf die Tableau Server-Benutzer zugreifen, z.B. http://tableau_server. Es wird nicht empfohlen, http://localhost zu verwenden. Die Verwendung einer URL mit einem nachstehenden Schrägstrich (z.B. http://tableau_server/) wird nicht unterstützt. Kopieren Sie den Wert von **Tableau Server return URL**, und fügen Sie ihn in das Azure AD-Textfeld **Anmelde-URL** ein, wie in Schritt 3 gezeigt
   5. „SAML entity ID“: Die Entitäts-ID zur eindeutigen Identifizierung Ihrer Tableau Server-Installation durch den IdP. Sie können in dieses Feld erneut Ihre Tableau Server-URL eingeben, es muss jedoch nicht die Tableau Server-URL verwendet werden. Kopieren Sie den Wert von **SAML entity ID**, und fügen Sie ihn in das Azure AD-Textfeld **BEZEICHNER** ein, wie in Schritt 3 gezeigt.
   6. Klicken Sie auf **Export Metadata File** , und öffnen Sie die Datei im Text-Editor. Suchen Sie nach „Assertion Consumer Service URL“ mit HTTP Post und Index 0, und kopieren Sie die URL. Fügen Sie sie dann in das Azure AD-Textfeld **Antwort-URL** ein, wie in Schritt 3 gezeigt. 
   7. Klicken Sie unten auf der Tableau Server Configuration-Seite auf **OK**.
   
    >[!NOTE] 
    >Wenn Sie Hilfe bei der Konfiguration von SAML für Tableau Server benötigen, finden Sie weitere Informationen im Artikel [Konfigurieren von SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >

9. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**. 
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

* Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 
   
   1. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.
   2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 
   
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 
   
   1. Notieren Sie den Wert von **Neues Kennwort**.
   2. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-tableau-server-test-user"></a>Erstellen eines Tableau Server-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Tableau Server. Sie müssen alle Benutzer in Tableau Server bereitstellen. Beachten Sie außerdem, dass der username-Wert des Benutzers dem Wert entsprechen sollte, den Sie im benutzerdefinierten Attribut **username**in Azure AD konfiguriert haben. Bei ordnungsgemäßer Zuordnung sollte die Integration funktionieren: [Konfigurieren des einmaligen Anmeldens in Azure AD](#configuring-azure-ad-single-single-sign-on)

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an den Tableau Server-Administrator in Ihrer Organisation.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf Tableau Server erhält.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu Tableau Server führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Tableau Server**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Server“ klicken, sollten Sie automatisch bei Ihrer Tableau Server-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png



<!--HONumber=Feb17_HO1-->


