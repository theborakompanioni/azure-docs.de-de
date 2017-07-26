---
title: 'Tutorial: Azure Active Directory-Integration mit BlueJeans| Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und BlueJeans konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 03bf65852b8d3cf14aebf155891a028db86e78d0
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Tutorial: Azure Active Directory-Integration mit BlueJeans

In diesem Tutorial erfahren Sie, wie Sie BlueJeans in Azure Active Directory (Azure AD) integrieren.

Die Integration von BlueJeans in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf BlueJeans hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei BlueJeans anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit BlueJeans konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein BlueJeans-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von BlueJeans aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-bluejeans-from-the-gallery"></a>Hinzufügen von BlueJeans aus dem Katalog
Zum Konfigurieren der Integration von BlueJeans in Azure AD müssen Sie BlueJeans aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um BlueJeans aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **BlueJeans** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_search.png)

5. Wählen Sie im Ergebnisbereich die Option **BlueJeans** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei BlueJeans basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in BlueJeans als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in BlueJeans muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in BlueJeans den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei BlueJeans müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines BlueJeans-Testbenutzers](#creating-a-bluejeans-test-user)**, um ein Pendant von Britta Simon in BlueJeans zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer BlueJeans-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei BlueJeans die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **BlueJeans** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für BlueJeans** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.BlueJeans.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<companyname>.BlueJeans.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam von BlueJeans](https://support.bluejeans.com/contact), um diese Werte zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **BlueJeans-Konfiguration** auf **BlueJeans konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie **Abmelde-URL, URL für Kennwortänderung und SAML-Dienst-URL für einmalige Anmeldung** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der **BlueJeans**-Unternehmenswebsite als Administrator an.

8. Wechseln Sie zu **ADMIN (Administrator) \> Group Settings (Gruppeneinstellungen) \> Security (Sicherheit)**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Admin")

9. Führen Sie im Abschnitt **Sicherheit** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden für SAML](./media/active-directory-saas-bluejeans-tutorial/IC785869.png "Einmaliges Anmelden für SAML")   
   
   a. Wählen Sie **SAML Single Sign On**aus.
  
   b. Wählen Sie **Enable automatic provisioning**aus.

10. Führen Sie die folgenden Schritte aus:

    ![Zertifikatpfad](./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Zertifikatpfad")
    
    a. Klicken Sie auf **Datei auswählen**, und laden Sie dann das heruntergeladene Zertifikat hoch.
   
    b. Fügen Sie die **SAML-Dienst-URL für einmalige Anmeldung** in das Textfeld **Anmelde-URL** ein.
   
    c. Fügen Sie die **URL für Kennwortänderung** in das Textfeld **URL für Kennwortänderung** ein.
   
    d. Fügen Sie die **Abmelde-URL** in das Textfeld **Abmelde-URL** ein.

11. Führen Sie die folgenden Schritte aus:
    
    ![Änderungen speichern](./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Änderungen speichern")
    
    a. Geben Sie in das Textfeld **Benutzer-ID** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
   
    b. Geben Sie in das Textfeld **E-Mail** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
   
    c. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bluejeans-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-bluejeans-test-user"></a>Erstellen eines BlueJeans-Testbenutzers

Damit sich Azure AD-Benutzer bei BlueJeans anmelden können, müssen sie in BlueJeans bereitgestellt werden.  

Im Fall von BlueJeans ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der **BlueJeans** -Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **ADMIN (Administrator) \> Manage Users (Benutzer verwalten) \> Add User (Benutzer hinzufügen)**.
   
   ![Admin](./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Admin")
   
   >[!IMPORTANT]
   >Die Registerkarte **Add User** ist nur verfügbar, wenn auf der Registerkarte **Security** (Sicherheit) die Option **Enable automatic provisioning** (Automatische Bereitstellung aktivieren) deaktiviert ist. 
   
3. Führen Sie im Abschnitt **Add User** die folgenden Schritte aus:

    ![Benutzer hinzufügen](./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Benutzer hinzufügen")
    
    a. Geben Sie für die Bereitstellung einen **BlueJeans-Benutzernamen**, eine **E-Mail-Adresse**, eine **BlueJeans-Meeting-ID**, eine **Moderatorenkennung**, den **vollständigen Namen** und das **Unternehmen** eines gültigen AAD-Kontos in die entsprechenden Textfelder ein.
    
    b. Klicken Sie auf **Benutzer hinzufügen**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von BlueJeans-Benutzerkonten oder mithilfe von APIs erstellen, die von BlueJeans zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf BlueJeans gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon BlueJeans zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **BlueJeans** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „BlueJeans“ klicken, sollte die Anmeldeseite der BlueJeans-Anwendung angezeigt werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bluejeans-tutorial/tutorial_general_203.png


