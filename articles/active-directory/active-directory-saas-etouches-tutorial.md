---
title: 'Tutorial: Azure Active Directory-Integration mit eTouches | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und eTouches konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 76cccaa8-859c-4c16-9d1d-8a6496fc7520
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2d8d925f80830a0d7047e9567fdd413af2e8c5c3
ms.openlocfilehash: e5706f1c33e5fb9305090c6c4444cf0adb5737c2
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Tutorial: Azure Active Directory-Integration mit eTouches
In diesem Tutorial erfahren Sie, wie Sie eTouches in Azure Active Directory (Azure AD) integrieren.

Die Integration von eTouches in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf eTouches Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei eTouches anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit eTouches konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein eTouches-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von eTouches aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="adding-etouches-from-the-gallery"></a>Hinzufügen von eTouches aus dem Katalog
Zum Konfigurieren der Integration von eTouches in Azure AD müssen Sie eTouches aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um eTouches aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **eTouches**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)
7. Wählen Sie im Ergebnisbereich **eTouches** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit eTouches basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in eTouches als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in eTouches muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in eTouches zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei eTouches müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines eTouches-Testbenutzers](#creating-a-predictix-price-reporting-test-user)**, um eine Entsprechung von Britta Simon in eTouches zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer eTouches-Anwendung.

Die eTouches-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute auf der Registerkarte **Attribut** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in eTouches die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **eTouches** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png) 
2. Führen Sie im Dialogfeld **SAML-Tokenattribute** für jede Zeile in der folgenden Tabelle die folgenden Schritte aus:
   
   | Attributname | Attributwert |
   | --- | --- |
   | E-Mail |user.mail |
   
 1. Klicken Sie auf **Benutzerattribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png) 
  2. Geben Sie im Textfeld **Attributname** den für die Zeile angezeigten Attributnamen ein.
  3. Wählen Sie in der Liste **Attributwert** den für die Zeile angezeigten Attributwert aus.
  4. Klicken Sie auf **Fertig stellen**.    
3. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **eTouches** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
4. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei eTouches anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png) 
5. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png)   
  1. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der eTouches-Anwendung verwendet wird, nach folgenden Muster ein: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<accountid\>**.
  2. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für eTouches** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)  
  1. Klicken Sie auf **Metadaten herunterladen**, und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
7. Führen Sie zum Konfigurieren von SSO für Ihre Anwendung in der eTouches-Anwendung die folgenden Schritte aus:  
  1. Melden Sie sich mit Administratorrechten bei der Anwendung **eTouches** an. 
  2. Navigieren Sie zur **SAML**-Konfiguration.
  3. Fügen Sie im Abschnitt **Allgemeine Einstellungen** den Azure AD-Verbundmetadaten-Inhalt in das Textfeld ein.
  4. Klicken Sie auf die Schaltfläche **Save & Stay** (Speichern und bleiben).
  5. Klicken Sie im Abschnitt mit den SAML-Metadaten auf die Schaltfläche **Update Metadata** (Metadaten aktualisieren). 
  6. Die Seite wird geöffnet, und das einmalige Anmelden wird durchgeführt. Wenn das einmalige Anmelden funktioniert, können Sie den Benutzernamen einrichten.
  7. Wählen Sie im Feld **Benutzername** wie unten dargestellt **emailaddress** (E-Mail-Adresse) aus. 
  8. Kopieren Sie den Wert von **SSO URL/ACS** (SSO-URL/ACS), und fügen Sie ihn in das Textfeld „Anmelde-URL“ des Konfigurations-Assistenten der Azure AD-Anwendung ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)
8. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
9. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  

    ![Azure AD – einmaliges Anmelden][11]


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

 ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

 ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png)  
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png)   
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-an-etouches-test-user"></a>Erstellen eines eTouches-Testbenutzers
In diesem Abschnitt erstellen Sie in eTouches einen Benutzer mit dem Namen „Britta Simon“. Wenden Sie sich an das Supportteam von eTouches, um die Benutzer im eTouches-System hinzufügen zu lassen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf eTouches gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon eTouches zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **eTouches**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „eTouches“ klicken, sollten Sie automatisch bei Ihrer eTouches-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png

