---
title: 'Tutorial: Azure Active Directory-Integration mit MOVEit Transfer | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und MOVEit Transfer konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3207124143dbc148206ea33d6030a986226c4d66


---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer"></a>Tutorial: Azure Active Directory-Integration mit MOVEit Transfer
In diesem Tutorial erfahren Sie, wie Sie MOVEit Transfer in Azure Active Directory (Azure AD) integrieren.

Die Integration von MOVEit Transfer in Azure AD bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf MOVEit Transfer haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei MOVEit Transfer anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit MOVEit Transfer konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SSO-fähiges MOVEit Transfer-Abonnement

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

1. Hinzufügen von MOVEit Transfer über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-moveit-transfer-from-the-gallery"></a>Hinzufügen von MOVEit Transfer über den Katalog
Zum Konfigurieren der Integration von MOVEit Transfer in Azure AD müssen Sie MOVEit Transfer über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um MOVEit Transfer über den Katalog hinzuzufügen:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld die Zeichenfolge **MOVEit Transfer**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_01.png)
7. Wählen Sie im Ergebnisbereich die Option **MOVEit Transfer** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_0001.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD mit MOVEit Transfer konfiguriert und getestet wird.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in MOVEit Transfer als Gegenpart zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in MOVEit Transfer muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in MOVEit Transfer zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit MOVEit Transfer müssen die folgenden Schritte ausgeführt werden:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines MOVEit Transfer-Testbenutzers](#creating-a-moveit-transfer-test-user)** , um in MOVEit Transfer einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren es in Ihrer MOVEit Transfer-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit MOVEit Transfer zu konfigurieren:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **MOVEit Transfer** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei MOVEit Transfer anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_03.png)
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_04.png)
   
    a. Geben Sie im Textfeld **Anmelde-URL** die Anmelde-URL mit Ihrer eigenen Domäne ein.
   
    b. Geben Sie im Textfeld **Bezeichner** eine Entitäts-ID-URL ein.
   
    c. Geben Sie im Textfeld **Antwort-URL** eine aktivierte Assertions-URL für die Kundenschnittstelle ein.
   
    d. Klicken Sie auf **Weiter**
   
   > [!NOTE]
   > Hinweis: Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Informationen zum Abrufen dieser Werte finden Sie in Schritt 8. Alternativ können Sie sich auch mit [MOVEit Transfer](https://www.ipswitch.com/support/technical-support) in Verbindung setzen.
   > 
   > 
4. Führen Sie auf der Seite **Einmaliges Anmelden bei MOVEit Transfer konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_05.png)
   
    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Weiter**.
5. Melden Sie sich bei Ihrem MOVEit Transfer-Mandanten als Administrator an.
6. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)
7. Klicken Sie auf den Link **Einmaliges Anmelden** (unter **Sicherheitsrichtlinien > Benutzerauthentifizierung**).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)
8. Klicken Sie auf den Metadaten-URL-Link, um das Metadatendokument herunterzuladen.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
   
   * Vergewissern Sie sich, dass **entityID** dem **Bezeichner** aus Schritt 3 entspricht.
   * Vergewissern Sie sich, dass die Speicherort-URL unter **AssertionConsumerService** der **ANTWORT-URL** aus Schritt 3 entspricht.
     
     ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)
9. Klicken Sie auf die Schaltfläche **Add Identity Provider** (Identitätsanbieter hinzufügen), um einen neuen Verbundidentitätsanbieter hinzuzufügen.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)
10. Klicken Sie auf **Durchsuchen...**, um die Metadatendatei auszuwählen, die Sie in Schritt 4 heruntergeladen haben, und klicken Sie anschließend auf **Add Identity Provider** (Identitätsanbieter hinzufügen), um die heruntergeladene Datei hochzuladen. 
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)
11. Wählen Sie auf der Seite **Edit Federated Identity Provider Settings...** (Einstellungen für Verbundidentitätsanbieter bearbeiten...) unter **Aktiviert** die Option **Ja** aus, und klicken Sie anschließend auf **Speichern**.
    
     ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)
12. Führen Sie auf der Seite **Edit Federated Identity Provider User Settings** (Benutzereinstellungen für Verbundidentitätsanbieter) die folgenden Aktionen aus, und klicken Sie anschließend auf **Speichern**.
    
    a. Wählen Sie unter **Anmeldename** die Option **SAML NameID** (SAML-Namens-ID) aus.
    
    b. Wählen Sie unter **Vollständiger Name** die Option **Other** (Sonstiges) aus, und geben Sie im Textfeld **Attributname** den folgenden Wert ein: http://schemas.microsoft.com/identity/claims/displayname
    
    c. Wählen Sie unter **E-Mail** die Option **Other** (Sonstiges) aus, und geben Sie im Textfeld **Attributname** den folgenden Wert ein: http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress.
    
    d. Wählen Sie unter **Auto-create account on signon** (Konto bei der Anmeldung automatisch erstellen) die Option **Ja** aus.
    
    e. Klicken Sie auf die Schaltfläche **Save** .
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
13. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]
14. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_05.png)
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_06.png)
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_08.png)
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-moveit-transfer-test-user"></a>Erstellen eines MOVEit Transfer-Testbenutzers
In diesem Abschnitt wird in MOVEit Transfer ein Benutzer namens Britta Simon erstellt. MOVEit Transfer unterstützt die von Ihnen aktivierte Just-in-Time-Bereitstellung.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist noch kein Benutzer vorhanden, wird beim Aufrufen von MOVEit Transfer ein neuer Benutzer erstellt.

> [!NOTE]
> Setzen Sie sich mit dem Supportteam von MOVEit Transfer in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf MOVEit Transfer erhält.

![Benutzer zuweisen][200]

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu MOVEit Transfer durchzuführen:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste die Option **MOVEit Transfer**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „MOVEit Transfer“ klicken, sollten Sie automatisch bei Ihrer MOVEit Transfer-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


