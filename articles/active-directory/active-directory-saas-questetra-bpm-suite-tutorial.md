---
title: 'Tutorial: Azure Active Directory-Integration in Questetra BPM Suite | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und der Questetra BPM Suite konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: fb6d5b73-e491-4dd2-92d6-94e5aba21465
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 89d1ebc090a7b667885f5dcbab14312efd767f0b
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Lernprogramm: Azure Active Directory-Integration in Questetra BPM Suite

In diesem Tutorial erfahren Sie, wie Sie die Questetra BPM Suite in Azure Active Directory (Azure AD) integrieren.

Die Integration der Questetra BPM Suite in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf die Questetra BPM Suite Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für die Questetra BPM Suite anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in die Questetra BPM Suite konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Questetra BPM Suite-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen der Questetra BPM Suite aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

## <a name="add-questetra-bpm-suite-from-the-gallery"></a>Hinzufügen der Questetra BPM Suite aus dem Katalog
Zum Konfigurieren der Integration der Questetra BPM Suite in Azure AD müssen Sie die Questetra BPM Suite der Liste mit den verwalteten SaaS-Apps aus dem Katalog hinzufügen.

**Führen Sie die folgenden Schritte aus, um die Questetra BPM Suite aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Questetra BPM Suite** ein, wählen Sie im Ergebnisbereich **Questetra BPM Suite** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Hinzufügen aus dem Katalog](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei der Questetra BPM Suite mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Gegenbenutzer in der Questetra BPM Suite zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in der Questetra BPM Suite muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in der Questetra BPM Suite den Wert für **Benutzername** in Azure AD als Wert für **Username** (Benutzername) zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei der Questetra BPM Suite müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Questetra BPM Suite-Testbenutzers](#create-a-questetra-bpm-suite-test-user)**, um eine Entsprechung von Britta Simon in der Questetra BPM Suite zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Questetra BPM Suite-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in der Questetra BPM Suite die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für die **Questetra BPM Suite** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![SAML-basierte Anmeldung](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Questetra BPM Suite** die folgenden Schritte aus:

    ![Abschnitt „Domäne und URLs für Questetra BPM Suite“](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.questetra.net/saml/SSO/alias/bpm`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.questetra.net/`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Sie erhalten diese Werte aus dem Abschnitt **SP Information** (SP-Informationen) auf der **Questetra BPM Suite**-Unternehmenswebsite, wie weiter unten im Tutorial erläutert, oder wenden Sie sich an das [Supportteam der Questetra BPM Suite](https://www.questetra.com/contact/). 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Abschnitt für SAML-Signaturzertifikat](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Save“](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Questetra BPM Suite-Konfiguration** auf **Questetra BPM Suite konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Abschnitt „Questetra BPM-Suite-Konfiguration“](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der **Questetra BPM Suite** -Unternehmenswebsite als Administrator an.

8. Klicken Sie im oberen Menü auf **Systemeinstellungen**. 
   
    ![Azure AD – einmaliges Anmelden][10]

9. Klicken Sie zum Öffnen der Seite **SingleSignOnSAML** auf **SSO (SAML)**. 
   
    ![Azure AD – einmaliges Anmelden][11]

10. Führen Sie auf der **Questetra BPM-Suite**-Unternehmenswebsite im Abschnitt **SP Information** (SP-Informationen) die folgenden Schritte aus:

    a. Kopieren Sie die **ACS-URL**, und fügen Sie sie im Azure-Portal in das Textfeld **Anmelde-URL** im Abschnitt **Domäne und URLs für Questetra BPM Suite** ein.
    
    b. Kopieren Sie die **Entitäts-ID**, und fügen Sie sie im Azure-Portal in das Textfeld **Bezeichner** im Abschnitt **Domäne und URLs für Questetra BPM Suite** ein.

11. Führen Sie auf der Unternehmenswebsite von **Questetra BPM Suite** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren][15]
   
    a. Wählen Sie **Einmaliges Anmelden aktivieren**aus.
   
    b. Fügen Sie in das Textfeld **Entity ID** (Entitäts-ID) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    c. Fügen Sie in das Textfeld **Sign-in page URL** (URL der Anmeldeseite) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    d. Fügen Sie in das Textfeld **Sign-out page URL** (URL der Abmeldeseite) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    e. Geben Sie im Textfeld **NameID format** (NameID-Format) `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress` ein.

    f. Öffnen Sie in Editor das **Base64**-codierte Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Verification certificate** (Überprüfungszertifikat) ein. 

    g. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-questetra-bpm-suite-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-questetra-bpm-suite-test-user"></a>Erstellen eines Questetra BPM Suite-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in der Questetra BPM Suite.

**Führen Sie die folgenden Schritte aus, um eine Benutzerin namens Britta Simon in der Questetra BPM Suite zu erstellen:**

1. Melden Sie sich auf der Questetra BPM Suite-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Systemeinstellungen > Benutzerliste > Neuer Benutzer**. 
3. Führen Sie im Dialogfeld „Neuer Benutzer“ die folgenden Schritte aus: 
   
    ![Testbenutzer erstellen][300] 
   
    a. Geben Sie im Textfeld **Name** als **Namen** des Benutzers **britta.simon@contoso.com** ein.
   
    b. Geben Sie im Textfeld **Email** (E-Mail) als **E-Mail-Adresse** des Benutzers **britta.simon@contoso.com** ein.
   
    c. Geben Sie im Textfeld **Password** (Kennwort) ein **Kennwort** des Benutzers ein.
    
    d. Klicken Sie auf **Neuen Benutzer hinzufügen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf die Questetra BPM Suite gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon der Questetra BPM Suite zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Questetra BPM Suite**aus.

    ![Questetra BPM Suite in der Liste der Apps](./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_questetra-bpm-suite_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel für die Questetra BPM Suite klicken, sollten Sie automatisch an Ihrer Questetra BPM Suite-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_04.png
[10]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_04.png
[15]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_08.png

[100]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/tutorial_general_203.png
[300]: ./media/active-directory-saas-questetra-bpm-suite-tutorial/questera_bpm_suite_11.png 


