---
title: 'Tutorial: Azure Active Directory-Integration mit Recognize | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Recognize konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: a2ce8c3a6aad76ca491ca17beab8555ad77a1142
ms.openlocfilehash: 3bfb340639674406a0a983106b56ca4a993aaf14
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Tutorial: Azure Active Directory-Integration mit Recognize
In diesem Tutorial erfahren Sie, wie Sie Recognize in Azure Active Directory (Azure AD) integrieren.

Die Integration von Recognize in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Recognize hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Recognize anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Recognize konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Recognize-Abonnement, für das einmaliges Anmelden aktiviert ist

 >[!NOTE]
 >Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
 > 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Recognize aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="adding-recognize-from-the-gallery"></a>Hinzufügen von Recognize aus dem Katalog
Zum Konfigurieren der Integration von Workrite in Azure AD müssen Sie Recognize aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Recognize aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **Recognize** ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_01.png)
7. Wählen Sie im Ergebnisbereich **Recognize** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_0001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Recognize konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Recognize als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Recognize muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Recognize zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Recognize müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Recognize-Testbenutzers](#creating-a-recognize-test-user)**, um eine Entsprechung von Britta Simon in Recognize zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Recognize-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Recognize die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Recognize** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Recognize anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_03.png)
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_04.png)
  1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://recognizeapp.com/<your-domain>/saml/sso`. 
  2. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://recognizeapp.com/<your-domain>/saml/metadata`.
  3. Klicken Sie auf **Weiter**.
  
    >[!NOTE]
    >Wenn Sie diese URLs nicht kennen, geben Sie Beispiel-URLs im Beispielformat ein. Informationen zum Abrufen dieser Werte finden Sie in Schritt 9. Sie können sich auch über <mailto:support@recognizeapp.com> mit dem Recognize-Supportteam in Verbindung setzen.
    >

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Recognize** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_05.png)
5. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Recognize-Mandanten als Administrator an.
6. Klicken Sie auf der rechten oberen Ecke auf **Menü**. Wechseln Sie zu **Company Admin** (Unternehmensadministrator).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)
7. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)
8. Führen Sie im Abschnitt **SSO Settings** (SSO-Einstellungen) die folgenden Schritte aus:
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
 1. Wählen Sie für **Enable SSO** (SSO aktivieren) die Option **ON** (EIN) aus.
 2. Geben Sie im Textfeld **IDP Entity ID** (IdP-Entitäts-ID) den Wert der **Aussteller-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein. 
 3. Geben Sie im Textfeld **SSO Target URL** (SSO-Ziel-URL) den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein. 
 4. Geben Sie im Textfeld **SSO Target URL** (SSO-Ziel-URL) den Wert für die **Dienst-URL für einmaliges Anmelden** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein. 
 5. Öffnen Sie das heruntergeladene Zertifikat in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat** ein.  
 6. Klicken Sie auf die Schaltfläche **Save settings** (Einstellungen speichern). 
9. Kopieren Sie neben dem Abschnitt **SSO Settings** (SSO-Einstellungen) die URL unter **Service Provider Metadata url** (Metadaten-URL des Dienstanbieters).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)
10. Öffnen Sie den **Metadaten-URL-Link** in einem leeren Browserfenster, um das Metadatendokument herunterzuladen. Verwenden Sie den von Recognize bereitgestellten EntityDescriptor-Wert im Dialogfeld **App-Einstellungen konfigurieren** für **Bezeichner**.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)
11. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]
12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_05.png)
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.  
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_06.png) 
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_08.png) 
 1. Notieren Sie den Wert von **Neues Kennwort**. 
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-recognize-test-user"></a>Erstellen eines Recognize-Testbenutzers
Damit sich Azure AD-Benutzer bei Recognize anmelden können, müssen sie in Recognize bereitgestellt werden. Im Fall von Recognize muss die Bereitstellung manuell durchgeführt werden.

Diese App unterstützt keine SCIM-Bereitstellung. Sie verfügt aber über eine Benutzersynchronisierung, die Benutzer bereitstellt. 

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Recognize-Unternehmenswebsite als Administrator an.
2. Klicken Sie auf der rechten oberen Ecke auf **Menü**. Wechseln Sie zu **Company Admin** (Unternehmensadministrator).
3. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.
4. Führen Sie im Abschnitt **User Sync** (Benutzersynchronisierung) die folgenden Schritte aus.
   
   ![Neuer Benutzer](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Neuer Benutzer")   
 1. Wählen Sie für **Sync Enabled** (Synchronisierung aktiviert) die Option **ON** (EIN) aus. 
 2. Wählen Sie für **Choose sync provider** (Synchronisierungsanbieter auswählen) die Option **Microsoft/Office 365** aus. 
 3. Klicken Sie auf **Run User Sync** (Benutzersynchronisierung ausführen).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist, für Britta Simon das einmalige Anmelden bei Azure zu aktivieren, indem sie Zugriff auf Recognize erhält.

![Benutzer zuweisen][200]

**Um Britta Simon zu Recognize zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste den Eintrag **Recognize**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Recognize“ klicken, sollten Sie automatisch in Ihrer Recognize-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_205.png

