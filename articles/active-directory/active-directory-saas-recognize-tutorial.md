---
title: 'Tutorial: Azure Active Directory-Integration mit Recognize | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Recognize konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 97d85183d0307c41a3b879d440d87a6fb0c53190
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Tutorial: Azure Active Directory-Integration mit Recognize

In diesem Tutorial erfahren Sie, wie Sie Recognize in Azure Active Directory (Azure AD) integrieren.

Die Integration von Recognize in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Recognize hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Recognize anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Recognize konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Recognize-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Recognize aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-recognize-from-the-gallery"></a>Hinzufügen von Recognize aus dem Katalog
Zum Konfigurieren der Integration von Workrite in Azure AD müssen Sie Recognize aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Recognize aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **Recognize** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_search.png)

5. Wählen Sie im Ergebnisbereich **Recognize** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Recognize mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Recognize als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Recognize muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Recognize den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Recognize müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Recognize-Testbenutzers](#creating-a-recognize-test-user)**, um ein Pendant von Britta Simon in Recognize zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Recognize-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Recognize die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Recognize** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Recognize** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://recognizeapp.com/<your-domain>/saml/sso`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://recognizeapp.com/<your-domain>`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den Recognize-Client](mailto:support@recognizeapp.com), um die Abmelde-URL zu erhalten. Der Bezeichnerwert kann über die Dienstanbietermetadaten-URL im Abschnitt mit den SSO-Einstellungen abgerufen werden, der später in diesem Tutorial vorgestellt wird. zu erstellen und zu verwalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Recognize-Konfiguration** auf **Recognize konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei Ihrem Recognize-Mandanten als Administrator an.

8. Klicken Sie auf der rechten oberen Ecke auf **Menü**. Wechseln Sie zu **Company Admin** (Unternehmensadministrator).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_000.png)

9. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_001.png)

10. Führen Sie im Abschnitt **SSO Settings** (SSO-Einstellungen) die folgenden Schritte aus:
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_002.png)
    
    a. Wählen Sie für **Enable SSO** (SSO aktivieren) die Option **ON** (EIN) aus.

    b. Fügen Sie in das Textfeld **IdP-Entitäts-ID** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    c. Fügen Sie in das Textfeld **SSO Target URL** (SSO-Ziel-URL) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    d. Fügen Sie in das Textfeld **Slo target url** (SLO-Ziel-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben. 
    
    e. Öffnen Sie die heruntergeladene Datei **Zertifikat (Base64)** in Editor, kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat** ein.
    
    f. Klicken Sie auf die Schaltfläche **Save settings** (Einstellungen speichern). 

11. Kopieren Sie neben dem Abschnitt **SSO Settings** (SSO-Einstellungen) die URL unter **Service Provider Metadata url** (Metadaten-URL des Dienstanbieters).
   
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_003.png)

12. Öffnen Sie den **Metadaten-URL-Link** in einem leeren Browserfenster, um das Metadatendokument herunterzuladen. Kopieren Sie anschließend den EntityDescriptor-Wert (entityID) in der Datei, und fügen Sie ihn im Azure-Portal im Abschnitt **Domäne und URLs für Recognize** in das Textfeld **Bezeichner** ein.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-recognize-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-recognize-test-user"></a>Erstellen eines Recognize-Testbenutzers

Damit sich Azure AD-Benutzer bei Recognize anmelden können, müssen sie in Recognize bereitgestellt werden. Im Fall von Recognize muss die Bereitstellung manuell durchgeführt werden.

Diese App unterstützt keine SCIM-Bereitstellung. Sie verfügt aber über eine Benutzersynchronisierung, die Benutzer bereitstellt. 

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Recognize-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der rechten oberen Ecke auf **Menü**. Wechseln Sie zu **Company Admin** (Unternehmensadministrator).

3. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.

4. Führen Sie im Abschnitt **User Sync** (Benutzersynchronisierung) die folgenden Schritte aus.
   
   ![Neuer Benutzer](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_005.png "Neuer Benutzer")
   
   a. Wählen Sie für **Sync Enabled** (Synchronisierung aktiviert) die Option **ON** (EIN) aus.
   
   b. Wählen Sie für **Choose sync provider** (Synchronisierungsanbieter auswählen) die Option **Microsoft/Office 365** aus.
   
   c. Klicken Sie auf **Run User Sync** (Benutzersynchronisierung ausführen).

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Recognize gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Recognize zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Recognize**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-recognize-tutorial/tutorial_recognize_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Recognize“ klicken, sollten Sie automatisch in Ihrer Recognize-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-recognize-tutorial/tutorial_general_203.png


