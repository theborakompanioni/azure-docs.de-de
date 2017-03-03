---
title: 'Tutorial: Azure Active Directory-Integration mit Lifesize Cloud | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Lifesize Cloud konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7cc133d6289bffbc3b7fc591104bc51ebfc67ddd
ms.openlocfilehash: 8edbee8e554a7818b97669a4bb64b31ada67b1a5
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Tutorial: Azure Active Directory-Integration mit Lifesize Cloud
In diesem Tutorial erfahren Sie, wie Sie Lifesize Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Lifesize Cloud in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf Lifesize Cloud Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Lifesize Cloud anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Lifesize Cloud konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Lifesize Cloud-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von Lifesize Cloud aus dem Katalog
* Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-lifesize-cloud-from-the-gallery"></a>Hinzufügen von Lifesize Cloud aus dem Katalog
Zum Konfigurieren der Integration von Lifesize Cloud in Azure AD müssen Sie Lifesize Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Lifesize Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Lifesize Cloud** ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)
7. Wählen Sie im Ergebnisbereich die Option **Lifesize Cloud** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Lifesize Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Lifesize Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Lifesize Cloud muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in Lifesize Cloud zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Lifesize Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Lifesize Cloud-Testbenutzers](#creating-a-lifesize-cloud-test-user)**, um eine Entsprechung von Britta Simon in Lifesize Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Lifesize Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Lifesize Cloud die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Lifesize Cloud** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Lifesize Cloud anmelden** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png)   
  1. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von Ihren Benutzern nach folgendem Muster zur Anmeldung bei der Lifesize Cloud-Anwendung verwendet wird: **https://login.lifesizecloud.com/ls/?acs**.
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Lifesize Cloud** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)
   1. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   2. Klicken Sie auf **Weiter**.
5. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung bei der Lifesize Cloud-Anwendung mit Administratorrechten an.
6. Klicken Sie in der rechten oberen Ecke auf Ihren Namen und dann auf **Erweiterte Einstellungen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)
7. Klicken Sie in den erweiterten Einstellungen nun auf den Link **SSO-Konfiguration**. Dies öffnet die SSO-Konfigurationsseite für Ihre Instanz.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)
8. Konfigurieren Sie jetzt die folgenden Werte in der SSO-Konfigurationsbenutzeroberfläche.    
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
  1. Kopieren Sie den Wert der Aussteller-URL von Azure AD, und fügen Sie diesen in das Textfeld für den **Aussteller des Identitätsanbieters** ein. 
  2. Kopieren Sie den Wert der Remoteanmelde-URL von Azure AD, und fügen Sie diese in das Textfeld **Anmelde-URL** ein.   
  3. Öffnen Sie das heruntergeladene Zertifikat in Editor, und kopieren Sie den Inhalt des Zertifikats mit Ausnahme der Zeilen „Begin Certificate“ und „End Certificate“ in das Textfeld **X.509-Zertifikat**.
  4. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld **Vorname** den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname** ein.
  5. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld **Nachname** den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** ein.
  6. Geben Sie in der Zuordnung des SAML-Attributs für das Textfeld **E-Mail** den Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ein.
9. Um die Konfiguration zu überprüfen, klicken Sie auf die Schaltfläche **Testen**.
   
   >[!NOTE]
   >Zum erfolgreichen Testen müssen Sie den Konfigurations-Assistenten in Azure AD abschließen und den Benutzer oder Gruppen, die den Test durchführen können, Zugriff gewähren.
   >  
10. Aktivieren Sie SSO, indem Sie auf die Schaltfläche **SSO aktivieren** klicken.
11. Klicken Sie nun auf die Schaltfläche **Update**, damit alle Einstellungen gespeichert werden. Dadurch wird den Wert RelayState generiert. Kopieren Sie den Wert RelayState, der im Textfeld generiert wurde. Wir benötigen diesen Wert in den nächsten Schritten.
12. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
   ![Azure AD – einmaliges Anmelden][10]
13. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]
    
**So melden Sie sich beim Azure-Verwaltungsportal an**

1. Melden Sie sich mit Administratoranmeldeinformationen unter **https://portal.azure.com** an.
2. Klicken Sie im linken Navigationsbereich auf den Link **Weitere Dienste**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)
3. Suchen Sie nach Azure Active Directory, und klicken Sie auf den Link **Azure Active Directory**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)
4. Sie finden alle SaaS-Anwendungen über die Schaltfläche **Unternehmensanwendungen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)
5. Klicken Sie auf dem nächsten Blatt auf den Link **Alle Anwendungen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)
6. Suchen Sie nach der Lifesize-Anwendung, für die Sie RelayState einrichten möchten. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)
7. Klicken Sie nun auf dem Blatt auf den Link **Einmaliges Anmelden**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)
8. Das Kontrollkästchen **Erweiterte URL-Einstellungen anzeigen** wird angezeigt. Aktivieren Sie das Kontrollkästchen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
9. Konfigurieren Sie jetzt RelayState für die Anwendung, die auf der SSO-Konfigurationsseite der Lifesize-Anwendung angezeigt wird. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)
10. Speichern Sie die Einstellungen.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 
   1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 
    1. Notieren Sie den Wert von **Neues Kennwort**.
    2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-an-lifesize-cloud-test-user"></a>Erstellen eines Lifesize Cloud-Testbenutzers
In diesem Abschnitt erstellen Sie in Lifesize Cloud einen Benutzer namens Britta Simon. Lifesize Cloud unterstützt die automatische Benutzerbereitstellung. Nach erfolgreicher Authentifizierung bei Azure AD wird der Benutzer automatisch in der Anwendung bereitgestellt. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Lifesize Cloud gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Lifesize Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Lifesize Cloud** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Lifesize Cloud“ klicken, sollten Sie automatisch bei Ihrer Lifesize Cloud-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png

