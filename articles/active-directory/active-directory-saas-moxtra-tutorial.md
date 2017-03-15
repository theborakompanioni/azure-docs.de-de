---
title: 'Tutorial: Azure Active Directory-Integration mit Moxtra | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Moxtra konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 2aed2d4b-1dcd-4839-8fed-9419d107c61c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4a07f946d396a9263d5b00202cd5229ddc86d1be
ms.openlocfilehash: cbb93424c6315ec7605bb238fd40dc62ccbb4e17
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-moxtra"></a>Tutorial: Azure Active Directory-Integration mit Moxtra
Dieses Tutorial soll Ihnen zeigen, wie Sie Moxtra in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Moxtra in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer Zugriff auf Moxtra hat. 
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Moxtra anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Moxtra konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Moxtra-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern. 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Moxtra aus dem Katalog 
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="adding-moxtra-from-the-gallery"></a>Hinzufügen von Moxtra aus dem Katalog
Zum Konfigurieren der Integration von Moxtra in Azure AD müssen Sie Moxtra aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Moxtra aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **Moxtra**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_01.png)
7. Wählen Sie im Ergebnisbereich **Moxtra** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_02.png)

## <a name="configuring-and-testing-azure-ad-sso"></a>Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Moxtra konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Moxtra als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Moxtra muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in Moxtra zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Moxtra müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Moxtra-Testbenutzers](#creating-a-moxtra-test-user)** , um eine Entsprechung von Britta Simon in Moxtra zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Moxtra-Anwendung zu konfigurieren. 

Die Moxtra-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_09.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Moxtra die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Moxtra** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Moxtra anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_04.png) 
  1. Geben Sie im Textfeld **Anmelde-URL** die folgende URL ein: **https://www.moxtra.com/service/#login**. 
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Moxtra** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_05.png) 
  1. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Moxtra-Unternehmenswebsite als Administrator an.
6. Klicken Sie in der Symbolleiste auf der linken Seite auf **Administratorkonsole > Einmalige SAML-Anmeldung** und dann auf **Neu**.
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_06.png) 
7. Führen Sie auf der Seite **SAML** die folgenden Schritte aus:
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_08.png)   
  1. Geben Sie im Textfeld **Name** einen Namen für Ihre Konfiguration ein (z.B. *SAML*). 
  2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Moxtra** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **IdP Entity ID** ein. 
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Moxtra** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein. 
  4. Geben Sie in das Textfeld **AuthnContextClassRef** die Zeichenkette **urn:oasis:names:tc:SAML:2.0:ac:classes:Password** ein. 
  5. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Moxtra** den Wert für **Namensbezeichnerformat**, und fügen Sie ihn in das Textfeld **Namensbezeichnerformat** ein. 
  6. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **Zertifikat** ein.    
  7. Geben Sie im Textfeld für die SAML-E-Mail-Domäne Ihre SAML-E-Mail-Domäne ein.    
   
   >[!NOTE]
   >Klicken Sie unten auf „**i**“, um die Schritte zum Überprüfen der Domäne anzuzeigen.
   >  
  8. Klicken Sie auf **Aktualisieren**.
8. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
   
  ![Azure AD – einmaliges Anmelden][10]
9. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
  
  ![Azure AD – einmaliges Anmelden][11]
19. Klicken Sie zum Hinzufügen von benutzerdefinierten Attributzuordnungen zur Konfiguration der SAML-Tokenattribute im Menü oben auf **Attribute**, um das Dialogfeld **SAML-Tokenattribute** zu öffnen. 
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_general_80.png) 
11. Führen Sie für jede Datenzeile in der Tabelle unten die folgenden Schritte aus:
   
     | Attributname | Attributwert |
     | --- | --- |
     | firstname |givenname |
     | lastname |surname |
     | idpid |*\<Wert für die **Entitäts-ID** aus dem Dialogfeld **Einmaliges Anmelden konfigurieren für Moxtra** im klassischen Azure-Portal\>* |
  1. Klicken Sie auf „Benutzerattribut hinzufügen“. 

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_general_81.png) 
  2. Geben Sie im Dialogfeld **Benutzerattribut hinzufügen** den Attributnamen und den Attributwert ein, der für die jeweilige Zeile in der Tabelle angezeigt wird. 

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_general_82.png) 
  3. Klicken Sie auf **Fertig stellen**.
12. Klicken Sie auf **Änderungen übernehmen**. 
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_general_84.png) 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_09.png)  
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_05.png)  
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-moxtra-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-moxtra-test-user"></a>Erstellen eines Moxtra-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Moxtra.

**Um einen Benutzer namens Britta Simon in Moxtra zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der Moxtra-Unternehmenswebsite als Administrator an.
2. Klicken Sie auf der Symbolleiste auf der linken Seite auf **Administratorkonsole > Benutzerverwaltung** und dann auf **Benutzer hinzufügen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_10.png) 
3. Führen Sie im Dialogfeld **Add User** die folgenden Schritte aus:
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse von Britta Simon im klassischen Azure-Portal ein.
  4. Geben Sie im Textfeld **Division** den Text **Dev** ein.
  5. Geben Sie im Textfeld **Abteilung** den Text **IT** ein.
  6. Wählen Sie **Administrator**aus.
  7. Klicken Sie auf **Hinzufügen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Moxtra erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Moxtra zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der Verzeichnisansicht im oberen Menü auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Moxtra**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moxtra-tutorial/tutorial_moxtra_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel „Moxtra“ klicken, sollten Sie automatisch bei Ihrer Moxtra-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-moxtra-tutorial/tutorial_general_205.png







