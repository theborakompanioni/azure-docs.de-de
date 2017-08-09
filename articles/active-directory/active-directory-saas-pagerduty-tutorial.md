---
title: 'Tutorial: Azure Active Directory-Integration mit PagerDuty | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und PagerDuty konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0410456a-76f7-42a7-9bb5-f767de75a0e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: bf5263ce4d8fbc231029c101f167f4b55a921e60
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="tutorial-azure-active-directory-integration-with-pagerduty"></a>Tutorial: Azure Active Directory-Integration mit PagerDuty

In diesem Tutorial erfahren Sie, wie Sie PagerDuty in Azure Active Directory (Azure AD) integrieren.

Die Integration von PagerDuty in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf PagerDuty hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei PagerDuty anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in PagerDuty konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein PagerDuty-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von PagerDuty aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-pagerduty-from-the-gallery"></a>Hinzufügen von PagerDuty aus dem Katalog
Zum Konfigurieren der Integration von PagerDuty in Azure AD müssen Sie PagerDuty aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um PagerDuty aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **PagerDuty** ein, wählen Sie im Ergebnisbereich **PagerDuty** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei PagerDuty mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in PagerDuty als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in PagerDuty muss eine Linkbeziehung eingerichtet werden.

Weisen Sie den Wert für **Benutzername** aus Azure AD in PagerDuty als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei PagerDuty müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines PagerDuty-Testbenutzers](#create-a-pagerduty-test-user)**, um ein Pendant von Britta Simon in PagerDuty zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer PagerDuty-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei PagerDuty die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **PagerDuty** auf **Einmaliges Anmelden**.

    ![Link „Einmaliges Anmelden konfigurieren“][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für PagerDuty** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für PagerDuty](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenant-name>.pagerduty.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<tenant-name>.pagerduty.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den PagerDuty-Client](https://www.pagerduty.com/support/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-pagerduty-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **PagerDuty-Konfiguration** auf **PagerDuty konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![PagerDuty-Konfiguration](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der PagerDuty-Unternehmenswebsite als Administrator an.

8. Klicken Sie im oberen Menü auf **Kontoeinstellungen**.
   
    ![Konteneinstellungen](./media/active-directory-saas-pagerduty-tutorial/ic778535.png "Konteneinstellungen")

9. Klicken Sie auf **Einmaliges Anmelden**.
   
    ![Einmaliges Anmelden](./media/active-directory-saas-pagerduty-tutorial/ic778536.png "Einmaliges Anmelden")

10. Führen Sie auf der Seite **Einmaliges Anmelden aktivieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-pagerduty-tutorial/ic778537.png "Einmaliges Anmelden aktivieren")
   
    a. Öffnen Sie in Editor das Base64-codierte Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **X.509 Certificate** ein.
  
    b. Fügen Sie im Textfeld **Login URL** die **URL für den SAML-SSO-Dienst** ein, die Sie aus dem Azure-Portal kopiert haben.
  
    c. Fügen Sie im Textfeld **Logout URL** die **Abmelde-URL** ein, die Sie aus dem Azure-Portal kopiert haben.
 
    d. Wählen Sie **Einmaliges Anmelden aktivieren**.
 
    e. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-pagerduty-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-pagerduty-test-user"></a>Erstellen eines PagerDuty-Testbenutzers

Damit sich Azure AD-Benutzer bei PagerDuty anmelden können, müssen sie in PagerDuty bereitgestellt werden.  
Im Fall von PagerDuty ist die Bereitstellung eine manuelle Aufgabe.

>[!NOTE]
>Sie können Azure Active Directory-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von PagerDuty-Benutzerkonten oder mithilfe der von PagerDuty bereitgestellten APIs erstellen.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **PagerDuty** -Mandanten an.

2. Klicken Sie im oberen Menü auf **Benutzer**.

3. Klicken Sie auf **Benutzer hinzufügen**.
   
    ![Benutzer hinzufügen](./media/active-directory-saas-pagerduty-tutorial/ic778539.png "Benutzer hinzufügen")

4.  Führen Sie im Dialogfeld **Ihr Team einladen** die folgenden Schritte aus:
   
    ![Ihr Team einladen](./media/active-directory-saas-pagerduty-tutorial/ic778540.png "Ihr Team einladen")

    a. Geben Sie den **Vor- und Nachnamen** des Benutzers ein, z.B. **Britta Simon**. 
   
    b. Geben Sie die **E-Mail-Adresse** des Benutzers ein, z.B. **brittasimon@contoso.com**.
   
    c. Klicken Sie auf **Hinzufügen** und dann auf **Einladungen senden**.
   
    >[!NOTE]
    >Alle hinzugefügten Benutzer erhalten eine Einladung zum Erstellen eines PagerDuty-Kontos.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf PagerDuty gewähren.

![Zuweisen der Benutzerrolle][200]

**Um Britta Simon zu PagerDuty zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **PagerDuty** aus.

    ![PagerDuty-Link in der Anwendungsliste](./media/active-directory-saas-pagerduty-tutorial/tutorial_pagerduty_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „PagerDuty“ klicken, sollten Sie automatisch bei Ihrer PagerDuty-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pagerduty-tutorial/tutorial_general_203.png


