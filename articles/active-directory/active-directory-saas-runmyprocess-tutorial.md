---
title: 'Tutorial: Azure Active Directory-Integration mit RunMyProcess | Microsoft Docs'
description: Es wird beschrieben, wie Sie das einmalige Anmelden zwischen Azure Active Directory und RunMyProcess konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/21/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 223a0abfcd1ae2997e4d2facfd065cf8dc721668


---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Tutorial: Azure Active Directory-Integration mit RunMyProcess
Dieses Tutorial soll Ihnen zeigen, wie Sie RunMyProcess in Azure Active Directory (Azure AD) integrieren.

Die Integration von RunMyProcess in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf RunMyProcess hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei RunMyProcess anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit RunMyProcess konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein RunMyProcess-Abonnement, für das einmaliges Anmelden aktiviert ist

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

1. Hinzufügen von RunMyProcess aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-runmyprocess-from-the-gallery"></a>Hinzufügen von RunMyProcess aus dem Katalog
Zum Konfigurieren der Integration von RunMyProcess in Azure AD müssen Sie RunMyProcess aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um RunMyProcess aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **RunMyProcess** ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)
7. Wählen Sie im Ergebnisbereich die Option **RunMyProcess** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in RunMyProcess konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in RunMyProcess als Gegenpart für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in RunMyProcess muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in RunMyProcess zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei RunMyProcess müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines RunMyProcess-Testbenutzers](#creating-a-runmyprocess-test-user)**, um eine Entsprechung von Britta Simon in RunMyProcess zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer RunMyProcess-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in RunMyProcess die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **RunMyProcess** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei RunMyProcess anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 
   
    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://live.runmyprocess.com/live/<tenant id>`.
   
    b. click **Weiter**
   
   > [!NOTE]
   > Beachten Sie hierbei, dass Sie den Wert mit der richtigen Anmelde-URL aktualisieren müssen. Wenden Sie sich unter <mailto:support@runmyprocess.com> an das Supportteam von RunMyProcess, um diesen Wert zu erhalten.
   > 
   > 
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für RunMyProcess** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)
5. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem RunMyProcess-Mandanten als Administrator an.
6. Klicken Sie links im Navigationsbereich auf **Konto**, und wählen Sie **Konfiguration**.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)
7. Navigieren Sie zum Abschnitt **Authentifizierungsmethode**, und führen Sie die folgenden Schritte aus:
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)
   
    a. Wählen unter **Methode** die Option **SSO mit Samlv2** aus.
   
    b. Geben Sie im Textfeld **SSO redirect** (SSO-Umleitung) den Wert der **SAML-SSO-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
   
    c. Geben Sie im Textfeld **Logout redirect** (Abmeldungsumleitung) den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
   
    d. Geben Sie im Textfeld **Name Id Format** (ID-Format des Namens) den Wert von **Namensbezeichnerformat** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
   
    e. Kopieren Sie den Inhalt der heruntergeladenen Zertifikatdatei, und fügen Sie ihn in das Textfeld **Zertifikat** ein. 
   
    f. Klicken Sie auf das Symbol **Speichern**.
8. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
9. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-runmyprocess-test-user"></a>Erstellen eines RunMyProcess-Testbenutzers
Damit sich Azure AD-Benutzer bei RunMyProcess anmelden können, müssen sie in RunMyProcess bereitgestellt werden. Im Fall von RunMyProcess ist die Bereitstellung eine manuelle Aufgabe.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei der RunMyProcess-Unternehmenswebsite als Administrator an.
2. Klicken Sie auf **Konto**, und wählen Sie links im Navigationsbereich die Option **Benutzer**. Klicken Sie anschließend auf **Neuer Benutzer**.
   
   ![Neuer Benutzer](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")
3. Führen Sie im Abschnitt **Benutzereinstellungen** die folgenden Schritte aus:
   
   ![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")
   
   a. Geben Sie **Name** und **E-Mail-Adresse** eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   
   b. Wählen Sie eine **IDE-Sprache**, eine **Sprache** und ein **Profil** aus.
   
   c. Wählen Sie **Kontoerstellungs-E-Mail an mich senden**aus.
   
   d. Klicken Sie auf **Speichern**.
   
   > [!NOTE]
   > Sie können Azure Active Directory-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von RunMyProcess-Benutzerkonten oder mithilfe der von RunMyProcess bereitgestellten APIs erstellen.
   > 
   > 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist, für Britta Simon das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf RunMyProcess erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon RunMyProcess zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste den Eintrag **RunMyProcess** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Symbolleiste am Seitenende auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.

Wenn Sie im Zugriffsbereich auf die Kachel „RunMyProcess“ klicken, sollten Sie automatisch bei Ihrer RunMyProcess-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


