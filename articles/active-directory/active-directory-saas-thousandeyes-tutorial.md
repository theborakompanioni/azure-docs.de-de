---
title: 'Tutorial: Azure Active Directory-Integration mit ThousandEyes | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ThousandEyes konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: 392add7d5f0a55598b8b90760f5c3f2d1e67ac02
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Tutorial: Azure Active Directory-Integration mit ThousandEyes

In diesem Tutorial erfahren Sie, wie Sie ThousandEyes in Azure Active Directory (Azure AD) integrieren.

Die Integration von ThousandEyes in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ThousandEyes hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ThousandEyes anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ThousandEyes konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ThousandEyes-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ThousandEyes aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-thousandeyes-from-the-gallery"></a>Hinzufügen von ThousandEyes aus dem Katalog
Zum Konfigurieren der Integration von ThousandEyes in Azure AD müssen Sie ThousandEyes aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um ThousandEyes aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Namen **ThousandEyes** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_search.png)

5. Wählen Sie im Ergebnisbereich **ThousandEyes** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ThousandEyes mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ThousandEyes als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ThousandEyes muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in ThousandEyes den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei ThousandEyes müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ThousandEyes-Testbenutzers](#creating-a-thousandeyes-test-user)**, um ein Pendant von Britta Simon in ThousandEyes zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer ThousandEyes-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in ThousandEyes die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **ThousandEyes** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für ThousandEyes** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://app.thousandeyes.com/login/sso`.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **ThousandEyes-Konfiguration** auf **ThousandEyes konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der **ThousandEyes** -Unternehmenswebsite als Administrator an.

8. Klicken Sie im oberen Menü auf das Symbol **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-thousandeyes-tutorial/ic790066.png "Einstellungen")

9. Klicken Sie unten auf der Seite auf **Konto**
   
    ![Konto](./media/active-directory-saas-thousandeyes-tutorial/ic790067.png "Konto")

10. Klicken Sie auf die Registerkarte **Sicherheit und Authentifizierung**.
   
    ![Sicherheit und Authentifizierung](./media/active-directory-saas-thousandeyes-tutorial/ic790068.png "Sicherheit und Authentifizierung")

11. Führen Sie im Abschnitt **Einmaliges Anmelden einrichten** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden einrichten](./media/active-directory-saas-thousandeyes-tutorial/ic790069.png "Einmaliges Anmelden einrichten")
  
    a. Wählen Sie **Einmaliges Anmelden aktivieren**aus.
  
    b. Fügen Sie in das Textfeld **Anmeldeseiten-URL** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
  
    c. Fügen Sie im Textfeld mit der **URL der Abmeldeseite** die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.
  
    d. Fügen Sie in das Textfeld **Aussteller des Identitätsanbieters** die **SAML-Entitäts-ID** ein, die Sie aus dem Azure-Portal kopiert haben.
  
    e. Klicken Sie unter **Verifizierungszertifikat** auf **Datei auswählen**, und laden Sie dann das Zertifikat hoch, das Sie aus dem Azure-Portal heruntergeladen haben.
  
    f. Klicken Sie auf **Speichern**.
 
> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thousandeyes-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-thousandeyes-test-user"></a>Erstellen eines ThousandEyes-Testbenutzers

Damit sich Azure AD-Benutzer bei ThousandEyes anmelden können, müssen sie in ThousandEyes bereitgestellt werden.  
Im Fall von ThousandEyes ist die Bereitstellung eine manuelle Aufgabe.

>[!NOTE]
>Sie können Azure Active Directory-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von ThousandEyes-Benutzerkonten oder mithilfe der von ThousandEyes bereitgestellten APIs erstellen.

**Führen Sie zum Bereitstellen eines Benutzerkontos in ThousandEyes die folgenden Schritte aus:**

1. Melden Sie sich bei der ThousandEyes-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Einstellungen")

3. Klicken Sie auf **Account**.
   
    ![Konto](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Konto")

4. Klicken Sie auf Registerkarte **Konten und Benutzer**.
   
    ![Konten und Benutzer](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Konten und Benutzer")

5. Führen Sie im Abschnitt **Benutzer und Konten hinzufügen** die folgenden Schritte aus:
   
    ![Benutzerkonten hinzufügen](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Benutzerkonten hinzufügen")   
  
    a. Geben Sie im Textfeld **Name** den Namen des Benutzers ein, z.B. **Britta Simon**.

    b. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.
   
    b. Klicken Sie auf **Neuen Benutzer zum Konto hinzufügen**.
      
     >[!NOTE]
     >Der Besitzer des Azure Active Directory-Kontos erhält eine E-Mail mit einem Link zur Bestätigung und Aktivierung des Kontos.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ThousandEyes gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon ThousandEyes zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **ThousandEyes** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ThousandEyes“ klicken, sollten Sie automatisch bei Ihrer ThousandEyes-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thousandeyes-tutorial/tutorial_general_203.png


