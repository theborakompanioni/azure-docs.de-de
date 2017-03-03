---
title: 'Tutorial: Azure Active Directory-Integration mit OpsGenie | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OpsGenie konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7cc133d6289bffbc3b7fc591104bc51ebfc67ddd
ms.openlocfilehash: bf6cf459b258e14e9dd36b6ecc999f455d31f52f
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Tutorial: Azure Active Directory-Integration mit OpsGenie
Dieses Tutorial soll Ihnen zeigen, wie Sie OpsGenie in Azure Active Directory (Azure AD) integrieren können.

Die Integration von OpsGenie in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf OpsGenie hat.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei OpsGenie anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit OpsGenie konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein OpsGenie-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>  

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von OpsGenie aus dem Katalog
* Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-opsgenie-from-the-gallery"></a>Hinzufügen von OpsGenie aus dem Katalog
Zum Konfigurieren der Integration von OpsGenie in Azure AD müssen Sie OpsGenie aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um OpsGenie aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **OpsGenie**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)
7. Wählen Sie im Ergebnisbereich **OpsGenie** aus und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden (SSO) von Azure AD in OpsGenie konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in OpsGenie als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OpsGenie muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in OpsGenie zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei OpsGenie müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines OpsGenie-Testbenutzers](#creating-a-opsgenie-test-user)** , um eine Entsprechung von Britta Simon in OpsGenie zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden (SSO) in Ihrer OpsGenie-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei OpsGenie die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **OpsGenie** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei OpsGenie anmelden?** die Option **Azure AD – einmaliges Anmelden** aus und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png)
  1. Geben Sie im Textfeld „Anmelde-URL“ die URL ein, die von Ihren Benutzern nach folgendem Muster zur Anmeldung bei der OpsGenie-Anwendung verwendet wird: **„https://app.opsgenie.com/auth/login“**.

    >[!NOTE] 
    >Wenn Sie Ihre Anmelde-URL benötigen, wenden Sie sich an das [OpsGenie-Supportteam](mailto:support@opsgenie.com) .
    >

  2. Klicken Sie auf **Weiter**.

4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für OpsGenie** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png)   
  1. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer. Dieses Zertifikat und die Metadaten-URLs (Entitäts-ID, Anmelde-URL für SSO und Abmelde-URL für SSO) werden benötigt, um SSO (Single Sign-On, einmaliges Anmelden) aufseiten von OpsGenie einzurichten.
  2. Klicken Sie auf **Weiter**.
5. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich bei OpsGenie als Administrator an.
6. Klicken Sie auf **Einstellungen** und dann auf die Registerkarte **Einmaliges Anmelden**.
   
    ![Einmaliges Anmelden bei OpsGenie](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 
7. Um SSO (Single Sign-On, einmaliges Anmelden) zu aktivieren, wählen Sie **Enabled**aus.
   
    ![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
8. Klicken Sie im Abschnitt **Anbieter** auf die Registerkarte **Azure Active Directory**.
   
    ![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
9. Führen Sie auf der Dialogfeldseite "Azure Active Directory" die folgenden Schritte aus:
   
    ![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)  
  1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für OpsGenie** den Wert für **Dienst-URL für einmalige Anmeldung**, und fügen Sie ihn ins Textfeld **SAML 2.0 Endpoint** (SAML 2.0-Endpunkt) ein.
  2. Erstellen Sie eine Base64-codierte Datei aus dem heruntergeladenen Zertifikat.      
   
    >[!NOTE]
    >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be)(in englischer Sprache).
    >

  3. Öffnen Sie das Base64-codierte Zertifikat in Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.500 Certificate** ein.
  4. Klicken Sie auf **Änderungen speichern**.
10. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
11. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.  
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.    
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 
   1. Notieren Sie den Wert von **Neues Kennwort**.
   2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-opsgenie-test-user"></a>Erstellen eines OpsGenie-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in OpsGenie. 

1. Melden Sie sich in einem Webbrowserfenster bei Ihrem OpsGenie-Mandanten als Administrator an.
2. Navigieren Sie zur Liste der Benutzer, indem Sie im linken Bereich auf **User** klicken.
   
   ![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 
3. Klicken Sie auf**Add User**.
4. Führen Sie im Dialogfeld **Add User** die folgenden Schritte aus:
   
   ![OpsGenie-Einstellungen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 
  1. Geben Sie im Textfeld **Email** die E-Mail-Adresse von Britta Simon in Azure Active Directory ein.
  2. Geben Sie im Textfeld **Vollständiger Name** den Namen **Britta Simon** ein.
  3. Klicken Sie auf **Speichern**. 

>[!NOTE]
>Daraufhin erhält Britta Simon eine E-Mail mit Anweisungen zum Einrichten ihres Profils.
>

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) bei Azure zu ermöglichen, indem sie Zugriff auf OpsGenie erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon OpsGenie zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **OpsGenie**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „OpsGenie“ klicken, sollten Sie automatisch bei Ihrer OpsGenie-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png

