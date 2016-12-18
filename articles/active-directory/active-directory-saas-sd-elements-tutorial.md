---
title: 'Tutorial: Azure Active Directory-Integration mit SD Elements | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SD Elements konfigurieren."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/18/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 53aa0a84a7f22c8cda5144eb6e1b82f38b72acb8


---
# <a name="tutorial-azure-active-directory-integration-with-sd-elements"></a>Tutorial: Azure Active Directory-Integration in SD Elements
Dieses Tutorial soll Ihnen zeigen, wie Sie SD Elements in Azure Active Directory (Azure AD) integrieren können.  
Die Integration von SD Elements in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD den Zugriff auf SD Elements steuern.
* Benutzer können sich mit ihren Azure AD-Konten automatisch bei SD Elements anmelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – in Azure Active Directory. 

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration in SD Elements konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SD Elements-Abonnement, für das einmaliges Anmelden aktiviert ist

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

1. Hinzufügen von SD Elements aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sd-elements-from-the-gallery"></a>Hinzufügen von SD Elements aus dem Katalog
Zum Konfigurieren der Integration von SD Elements in Azure AD müssen Sie SD Elements aus dem Katalog zu Ihrer Liste verwalteter SaaS-Apps hinzufügen.

**Führen Sie folgende Schritte aus, um SD Elements aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **SD Elements**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_01.png)

7. Wählen Sie im Ergebnisbereich **SD Elements** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt wird anhand einer Testbenutzerin namens Britta Simon veranschaulicht, wie das einmalige Anmelden von Azure AD in SD Elements konfiguriert und getestet wird.

Zur Verwendung des einmaligen Anmeldens muss Azure AD den entsprechenden Gegenbenutzer in SD Elements kennen. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SD Elements muss eine Linkbeziehung eingerichtet werden.  
Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in SD Elements zuweisen.

Um das einmalige Anmelden von Azure AD mit SD Elements konfigurieren und testen zu können, ist Folgendes erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SD Elements-Testbenutzers](#creating-a-sd-elements-test-user)** , um eine Entsprechung von Britta Simon in SD Elements zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt wird das einmalige Anmelden von Azure AD im klassischen Azure-Portal aktiviert und das einmalige Anmelden in Ihrer SD Elements-Anwendung konfiguriert.

Die SD Elements-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diesen Vorgang:

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_14.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SD Elements folgende Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SD Elements** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SD Elements anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_04.png) 

    a. Geben Sie in das Textfeld **Aussteller** die Aussteller-URL Ihres Mandanten in folgendem Format ein: *https://\<Mandantenname\>.sdelements.com/sso/saml2/metadata*.

    b. Geben Sie im Textfeld **Antwort-URL** die URL in folgendem Format ein: *https://\<Mandantenname\>.sdelements.com/sso/saml2/acs*.       

    > [!NOTE] 
    > Die tatsächliche Aussteller-URL und Antwort-URL für Ihren Mandanten erhalten Sie vom [SD Elements-Supportteam](mailto:support@sdelements.com).

    c. Klicken Sie auf **Next**.


1. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für SD Elements** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_05.png) 
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Next**.
2. Wenden Sie sich zur Aktivierung des einmaligen Anmeldens mit der heruntergeladenen Zertifikatdatei an das [SD Elements-Supportteam](mailto:support@sdelements.com) .
3. Melden Sie sich in einem anderen Browserfenster bei Ihrem SD Elements-Mandanten als Administrator an.
4. Klicken Sie im oberen Menü auf „System“ und anschließend auf „Single Sign-on“. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_09.png) 
5. Führen Sie im Dialogfeld **Single Sign-On Settings** folgende Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_10.png) 
   
    a. Wählen Sie für **SSO-Typ** die Option **SAML** aus.
   
    b. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für SD Elements** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Entitäts-ID des Identitätsanbieters** ein
   
    c. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für SD Elements** den Wert für **Dienst-URL für einmalige Anmeldung**, und fügen Sie ihn in das Textfeld für den **SSO-Dienst des Identitätsanbieters** ein.
   
    d. Klicken Sie auf **Speichern**.
6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]
8. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen. 
   
    ![Einmaliges Anmelden konfigurieren][21]
9. Führen Sie für jede Zeile der folgenden Tabelle die folgenden Schritte aus:
   
   | Attributname | Attributwert |
   | --- | --- |
   | E-Mail |user.mail |
   | firstname |user.givenname |
   | lastname |user.surname |

    a. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][23]

    b. Geben Sie im Textfeld **Attributname** den **Attributnamen** ein, und wählen Sie als **Attributwert** den für diese Zeile angezeigten Attributwert aus.

    ![Einmaliges Anmelden konfigurieren][22]

    c. Klicken Sie auf **Benutzerattribut hinzufügen**. 

    ![Einmaliges Anmelden konfigurieren][23]

1. Klicken Sie auf **Änderungen übernehmen**. 
   
    ![Einmaliges Anmelden konfigurieren][24]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-sd-elements-test-user"></a>Erstellen eines SD Elements-Testbenutzers
In diesem Abschnitt wird in SD Elements eine Benutzerin namens Britta Simon erstellt. Im Falle von in SD Elements müssen Benutzer manuell erstellt werden.

**Führen Sie die folgenden Schritte aus, um in SD Elements eine Benutzerin namens Britta Simon zu erstellen:**

1. Melden Sie sich in einem Webbrowserfenster bei der SD Elements-Unternehmenswebsite als Administrator an.
2. Klicken Sie im oberen Menü auf „Benutzerverwaltung“ und dann auf „Benutzer“.
   
   ![Erstellen eines SD Elements-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_11.png) 
3. Klicken Sie auf „Neuen Benutzer hinzufügen“.
   
   ![Erstellen eines SD Elements-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_12.png) 
4. Führen Sie im Dialogfeld „Add New User“ folgende Schritte aus:
   
   ![Erstellen eines SD Elements-Testbenutzers](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_13.png) 
   
   a. Geben Sie in das Textfeld **E-Mail** die E-Mail-Adresse von Britta Simon aus Azure AD ein.
   
   b. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.
   
   c. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   d. Wählen Sie für **Rolle** die Option **Benutzer** aus. 
   
   e. Klicken Sie auf **Benutzer erstellen**.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens von Azure zu ermöglichen, indem sie Zugriff auf SD Elements erhält.

![Benutzer zuweisen][200] 

**Führen Sie folgende Schritte aus, um Britta Simon SD Elements zuzuweisen:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **SD Elements**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sd-elements-tutorial/tutorial_sd-elements_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der **Benutzerliste** den Eintrag **Britta Simon** aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.  
Wenn Sie im Zugriffsbereich auf die Kachel „SD Elements“ klicken, werden Sie automatisch in Ihrer SD Elements-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_100.png

[21]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_82.png
[23]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_83.png


[200]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sd-elements-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


