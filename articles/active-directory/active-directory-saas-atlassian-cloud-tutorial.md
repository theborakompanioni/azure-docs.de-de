---
title: 'Tutorial: Azure Active Directory-Integration mit Atlassian Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Atlassian Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8928581d9636f571008f965185eeb61b414a16e7
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a>Tutorial: Azure Active Directory-Integration mit Atlassian Cloud

In diesem Tutorial erfahren Sie, wie Sie Atlassian Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Atlassian Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Atlassian Cloud Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Atlassian Cloud anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Atlassian Cloud benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Atlassian Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist


>[!NOTE] 
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Atlassian Cloud aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD


## <a name="add-atlassian-cloud-from-the-gallery"></a>Hinzufügen von Atlassian Cloud aus dem Katalog
Zum Konfigurieren der Integration von Atlassian Cloud in Azure AD müssen Sie Atlassian Cloud aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Atlassian Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Atlassian Cloud** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_01.png)

7. Wählen Sie im Ergebnisbereich die Option **Atlassian Cloud** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_02.png)

##  <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden in Azure AD mit Atlassian Cloud.

Damit das einmalige Anmelden (Single Sign-On, SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Atlassian Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Atlassian Cloud muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in Atlassian Cloud zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Atlassian Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Atlassian Cloud-Testbenutzers](#creating-Atlassian-cloud-test-user)**, um eine Entsprechung von Britta Simon in Atlassian Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden (Single Sign-On, SSO) in Ihrer Atlassian Cloud-Anwendung.


**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD mit Atlassian Cloud die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Atlassian Cloud** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Atlassian Cloud anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_04.png) 
 1. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der Atlassian Cloud-Anwendung verwendet wird, nach folgendem Muster ein: `https://<instancename>.atlassian.net`    
 2. Geben Sie im Textfeld **Bezeichner** die URL im folgenden Format ein: `https://id.atlassian.com/login`

    >[!NOTE] 
    >Den exakten Wert für **Bezeichner** können Sie auf dem Bildschirm für die SAML-Konfiguration für Atlassian Cloud abrufen.
    >

 3. Klicken Sie auf **Weiter**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Atlassian Cloud** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_05.png)
 1. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
 2. Klicken Sie auf **Weiter**.

5. Melden Sie sich zum Konfigurieren von SSO für Ihre Anwendung mit Administratorrechten beim Atlassian-Portal an.

6. Klicken Sie im linken Navigationsbereich im Authentifizierungsabschnitt auf **Domänen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)
 1. Geben Sie in das Textfeld Ihren Domänennamen ein, und klicken Sie dann auf **Domäne hinzufügen**.
        
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)
 2. Klicken Sie zum Überprüfen der Domäne auf **Überprüfen**. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png) 
  3. Laden Sie die HTML-Datei zur Domänenüberprüfung herunter, laden Sie sie in den Stammordner der Website Ihrer Domäne hoch, und klicken Sie auf **Domäne überprüfen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)
  4. Der Wert im Feld **Stauts** lautet nach der Überprüfung der Domäne **Überprüft**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

7. Klicken Sie in der linken Navigationsleiste auf **SAML**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

8. Erstellen Sie eine neue SAML-Konfiguration, und fügen Sie die Identitätsanbieterkonfiguration hinzu.
  1. Kopieren Sie den Wert der Entitäts-ID aus Azure AD, und fügen Sie ihn in das Feld für die Entitäts-ID des Identitätsanbieters ein.
  2. Kopieren Sie die SAML-SSO-URL, und fügen Sie sie in das Feld für die SSO-URL des Identitätsanbieters ein.
  3. Öffnen Sie das heruntergeladene Zertifikat von Azure AD in Editor, kopieren Sie die Werte ohne die Anfangs- und Endzeilen, und fügen Sie sie in das Feld für das öffentliche X509-Zertifikat ein.
  4. Speichern Sie die Einstellungen.

      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)
 
9. Aktualisieren Sie die Azure AD-Einstellungen, um sicherzustellen, dass Sie die richtige Bezeichner-URL eingerichtet haben.
  1. Kopieren Sie die SP-Identitäts-ID auf dem SAML-Bildschirm, und fügen Sie sie in Azure AD als Wert für **Bezeichner** ein.
  2. Die Anmelde-URL ist die Mandanten-URL von Atlassian Cloud.     

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
10. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
 
    ![Azure AD – einmaliges Anmelden][11]


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_06.png) 
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie unten auf der Seite auf **Fertigstellen**.   

### <a name="create-an-atlassian-cloud-test-user"></a>Erstellen eines Atlassian Cloud-Testbenutzers

In diesem Abschnitt erstellen Sie in Atlassian Cloud einen Benutzer namens Britta Simon. Es ist wichtig, dass dieser Benutzer in Atlassian Cloud vorhanden ist, bevor Sie einmaliges Anmelden (Single Sign-On, SSO) ausführen. 

Melden Sie sich mit Administratorrechten bei Ihrer Atlassian Cloud-Instanz an, und führen Sie die folgenden Schritte aus.

>[!NOTE] 
>Sie können die Benutzer auch erstellen, indem Sie im Abschnitt „Benutzer“ auf die Schaltfläche **Bulk Create** (Massenerstellung) klicken.

1. Klicken Sie im Abschnitt „Site administration“ (Websiteverwaltung) auf die Schaltfläche **Benutzer**.

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. Klicken Sie auf die Schaltfläche **Benutzer erstellen**, um einen Benutzer in Atlassian Cloud zu erstellen.

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. Geben Sie E-Mail-Adresse, Benutzername und vollständigen Namen des Benutzers ein, und weisen Sie den Anwendungszugriff zu. 

    ![Atlassian Cloud-Benutzer erstellen](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. Klicken Sie auf die Schaltfläche **Benutzer erstellen**. Dadurch wird die E-Mail-Einladung an den Benutzer gesendet, und nach der Annahme der Einladung ist der Benutzer im System aktiv. 

### <a name="assig-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Atlassian Cloud gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Atlassian Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Atlassian Cloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Atlassian Cloud“ klicken, sollten Sie automatisch bei Ihrer Atlassian Cloud-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_205.png

