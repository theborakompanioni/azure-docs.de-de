---
title: 'Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS) | Microsoft-Dokumentation'
description: "Hier erfahren Sie, wie Sie Amazon Web Services (AWS) mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 2cbd477776329ef84ef615b6c28c56cfee12fc99
ms.lasthandoff: 03/31/2017


---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS)
Dieses Tutorial soll Ihnen zeigen, wie Sie Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren können.  

Die Integration von Amazon Web Services (AWS) in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat. 
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Amazon Web Services (AWS) konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein AWS-Abonnement (Amazon Web Services), für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.  

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Amazon Web Services (AWS) aus dem Katalog 
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Amazon Web Services (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1] 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** . 
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** . 
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**. 
   
    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Amazon Web Services (AWS)** ein.
   
    ![Anwendungen][5]

7. Wählen Sie im Ergebnisbereich **Amazon Web Services (AWS)** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Anwendungen][6]

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt soll veranschaulicht werden, wie basierend auf einem Testbenutzer namens „Britta Simon“ das einmalige Anmelden von Azure AD in Amazon Web Services (AWS) konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Amazon Web Services (AWS) als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Services (AWS) muss eine Linkbeziehung eingerichtet werden.  

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in Amazon Web Services (AWS) zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für Amazon Web Services (AWS)](#creating-a-halogen-software-test-user)**, um eine Entsprechung von Britta Simon in Amazon Web Services (AWS) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer AWS-Anwendung (Amazon Web Services) zu konfigurieren.  

Die AWS-Anwendung (Amazon Web Services) erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen. 

Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden konfigurieren][27]

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Services (AWS) die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][7]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Amazon Web Services (AWS) anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren][8]

3. Klicken Sie auf der Seite **App-Einstellungen konfigurieren** auf **Weiter**. 
   
    ![App-Einstellungen konfigurieren][9]

4. Klicken Sie auf der Seite **Einmaliges Anmelden für Amazon Web Services (AWS) konfigurieren** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren][10]

5. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

6. Klicken Sie auf die **Startseite der Konsole**.
   
    ![Einmaliges Anmelden konfigurieren][11]

7. Klicken Sie auf **Identitäts- und Zugriffsverwaltung**. 
   
    ![Einmaliges Anmelden konfigurieren][12]

8. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**. 
   
    ![Einmaliges Anmelden konfigurieren][13]

9. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren][14]   
  1. Wählen Sie für **Anbietertyp** die Option **SAML** aus.
  2. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (z.B.: *WAAD*).
  3. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.
  4. Klicken Sie auf **Nächster Schritt**.

10. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren][15]

11. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Neue Rolle erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren][16]

12. Führen Sie im Dialogfeld **Rollennamen festlegen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren][17] 
  1. Geben Sie im Textfeld **Rollenname** einen Rollennamen ein (z.B.: *TestUser*). 
  2. Klicken Sie auf **Nächster Schritt**.

13. Führen Sie im Dialogfeld **Rollentyp auswählen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren][18] 
  1. Wählen Sie **Rolle für Identitätsanbieterzugriff**aus. 
  2. Klicken Sie im Abschnitt **Zugriff mit einmaliger Webanmeldung für SAML-Anbieter gewähren** auf **Auswählen**.

14. Führen Sie im Dialogfeld **Vertrauensstellung herstellen** die folgenden Schritte aus:  
    
    ![Einmaliges Anmelden konfigurieren][19] 
  1. Wählen Sie als SAML-Anbieter den zuvor erstellten SAML-Anbieter aus (z.B.: *WAAD*).   
  2. Klicken Sie auf **Nächster Schritt**.

15. Klicken Sie im Dialogfeld **Vertrauenswürdigkeit der Rolle überprüfen** auf **Nächster Schritt**. 
    
    ![Einmaliges Anmelden konfigurieren][32]

16. Klicken Sie im Dialogfeld **Richtlinie anhängen** auf **Nächster Schritt**.  
    
    ![Einmaliges Anmelden konfigurieren][33]

17. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:   
    
    ![Einmaliges Anmelden konfigurieren][34] 
  1. Kopieren Sie den Wert **Rollen-ARN** .  
  2. Kopieren Sie den ARN-Wert **Vertrauenswürdige Entitäten** . 
  3. Klicken Sie auf **Rolle erstellen**. 

18. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
    ![Was ist Azure AD Connect?][20]

19. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Was ist Azure AD Connect?][22]

20. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen. 
    
    ![Einmaliges Anmelden konfigurieren][21]

21. Klicken Sie auf **Benutzerattribut hinzufügen**. 
    
    ![Einmaliges Anmelden konfigurieren][23]

22. Führen Sie im Dialogfeld „Benutzerattribut hinzufügen“ die folgenden Schritte aus. 
    
    ![Einmaliges Anmelden konfigurieren][24]  
  1. Geben Sie im Textfeld **Attributname** die URL **https://aws.amazon.com/SAML/Attributes/Role** ein.  
  2. Geben Sie im Textfeld **Attributwert** die Zeichenfolge **[Rollen-ARN-Wert],[ARN-Wert für die vertrauenswürdige Entität]** ein.
    
     >[!TIP]
     >Dies sind die Werte, die Sie bei der Erstellung der Rolle im Dialogfeld "Überprüfung" kopiert haben. 
     > 
     
  3. Klicken Sie auf **Abschließen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.

23. Klicken Sie auf **Benutzerattribut hinzufügen**. 
    
    ![Einmaliges Anmelden konfigurieren][23]

24. Führen Sie im Dialogfeld „Benutzerattribut hinzufügen“ die folgenden Schritte aus, und klicken Sie dann auf **Änderungen übernehmen**. 
    
    ![Einmaliges Anmelden konfigurieren][25]
 1. Geben Sie im Textfeld **Attributname** die URL **https://aws.amazon.com/SAML/Attributes/RoleSessionName** ein.
 2. Geben Sie **user.userprincipalname** in das Textfeld **Attributwert** ein, oder wählen Sie diesen Eintrag in der Dropdownliste aus.

     ![Einmaliges Anmelden konfigurieren][35]
 3. Klicken Sie auf **Abschließen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu schließen.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_05.png) 
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
 3. Klicken Sie auf Weiter.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_06.png)  
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_08.png)  
 1. Notieren Sie den Wert von **Neues Kennwort**.  
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-amazon-web-services-aws-test-user"></a>Erstellen eines Testbenutzers für Amazon Web Services (AWS)
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Amazon Web Services (AWS).

**Um einen Benutzer namens Britta Simon in Amazon Web Services (AWS) zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der Unternehmenswebsite von **Amazon Web Services (AWS)** als Administrator an.

2. Klicken Sie auf das Symbol für die **Startseite der Konsole** . 
   
    ![Einmaliges Anmelden konfigurieren][11]

3. Klicken Sie auf „Identitäts- und Zugriffsverwaltung“. 
   
    ![Einmaliges Anmelden konfigurieren][28]

4. Klicken Sie im Dashboard auf „Benutzer“ und dann auf „Neue Benutzer erstellen“. 
   
    ![Einmaliges Anmelden konfigurieren][29]

5. Führen Sie im Dialogfeld „Benutzer erstellen“ die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren][30]   
 1. Geben Sie in die Textfelder unter **Benutzernamen eingeben** den Benutzernamen („userprincipalname“) von Britta Simon in Azure AD ein. 
 2. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Amazon Web Services (AWS) erhält.

![Benutzer zuweisen][31]

**Um Britta Simon Amazon Web Services (AWS) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][26]

2. Wählen Sie in der Anwendungsliste **Amazon Web Services (AWS)** aus.
   
    ![Benutzer zuweisen][27]

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][25]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][29]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel „Amazon Web Services (AWS)“ klicken, sollten Sie automatisch in Ihrer Anwendung Amazon Web Services (AWS) angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service-tutorial/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service-tutorial/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/user_attributes_01.png























