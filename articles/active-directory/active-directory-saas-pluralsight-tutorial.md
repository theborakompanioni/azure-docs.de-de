---
title: 'Tutorial: Azure Active Directory-Integration mit Pluralsight | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Pluralsight konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2d8d925f80830a0d7047e9567fdd413af2e8c5c3
ms.openlocfilehash: f77f459219a5c9a0e218de924c0c7578647a3594
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Tutorial: Azure Active Directory-Integration mit Pluralsight
Dieses Tutorial soll Ihnen zeigen, wie Sie Pluralsight in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Pluralsight in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Pluralsight hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Pluralsight anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Pluralsight konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure-Abonnement
* Ein Pluralsight-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Pluralsight aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-pluralsight-from-the-gallery"></a>Hinzufügen von Pluralsight aus dem Katalog
Zum Konfigurieren der Integration von Pluralsight in Azure AD müssen Sie Pluralsight aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Pluralsight aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld das Wort **Pluralsight**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)
7. Wählen Sie im Ergebnisbereich **Pluralsight** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Pluralsight konfiguriert und getestet werden kann.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Pluralsight müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Pluralsight-Testbenutzers](#creating-a-pluralsight-test-user)** , um eine Entsprechung von Britta Simon in Pluralsight zu erstellen, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Pluralsight-Anwendung zu konfigurieren.

Die Pluralsight-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

Sie können auch das Attribut **Unique ID** mit einem Wert wie beispielsweise „EmployeeID“ oder einem anderen für Ihre Organisation geeigneten Wert verwenden. Beachten Sie, dass dieses Attribut nicht erforderlich ist. Sie können es jedoch zum eindeutigen Identifizieren des Benutzers hinzufügen. 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Pluralsight die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **Pluralsight** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 
2. Um die redundanten **SAML-Tokenattribute**zu entfernen, führen Sie die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/2829.png) 
  * Zeigen Sie in der Tabelle auf jedes der oben im rot umrandeten Feld aufgeführten Benutzerattribute, und klicken Sie auf „Löschen“. 
3. Um die erforderlichen **SAML-Tokenattribute**hinzuzufügen, führen Sie für jede Zeile in der folgenden Tabelle die folgenden Schritte aus:
   
   | Attributname | Attributwert |
   | --- | --- |
   | Vorname |user.givenname |
   | Nachname |user.surname |
   | E-Mail |user.mail |
4. Klicken Sie auf **Benutzerattribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png)
  1. Geben Sie im Textfeld **Attributname** den für die Zeile angezeigten Attributnamen ein.
  2. Wählen Sie in der Liste **Attributwert** den für die Zeile angezeigten Attributwert aus.
  3. Klicken Sie auf **Fertig stellen**.    
  4. Klicken Sie auf **Änderungen übernehmen**.
 
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/3232.png) 
    
5. Klicken Sie im oberen Menü auf **Schnellstart**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  
6. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Pluralsight** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
7. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Pluralsight anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 
8. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 
  1. Geben Sie im Textfeld „Anmelde-URL“ die URL ein, die von Ihren Benutzern zur Anmeldung bei der Pluralsight-Anwendung genutzt wird, und verwenden Sie dabei folgendes Muster: `https://<instance name>.pluralsight.com/sso/<comapny name>`
  2. Klicken Sie auf **Weiter**.
9. Führen Sie auf der Seite **Einmaliges Anmelden bei Pluralsight konfigurieren** die folgenden Schritte aus:

  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png)   
  1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
10. Wenden Sie sich an das [Professional Services](mailTo:professionalservices@pluralsight.com) -Team von Pluralsight, um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie die heruntergeladene Metadatendatei bereit.
11. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

* Wählen Sie in der Benutzerliste **Britta Simon**aus.

  ![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png)  
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-pluralsight-test-user"></a>Erstellen eines Pluralsight-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Pluralsight. Arbeiten Sie mit dem Pluralsight-Supportteam zusammen, um Benutzer im Pluralsight-Konto hinzuzufügen. 

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von Pluralsight in Verbindung. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Pluralsight erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon Pluralsight zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **Pluralsight**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Pluralsight“ klicken, sollten Sie automatisch bei Ihrer Pluralsight-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png

