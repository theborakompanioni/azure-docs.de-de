---
title: 'Tutorial: Azure Active Directory-Integration mit Mimecast Admin Console | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Mimecast Admin Console konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 1e6fb68d239ee3a66899f520a91702419461c02b
ms.openlocfilehash: f401f592d79ad954aa466de74d3e3fbb18aa9a5b
ms.contentlocale: de-de
ms.lasthandoff: 08/16/2017

---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Tutorial: Azure Active Directory-Integration mit Mimecast Admin Console

In diesem Tutorial erfahren Sie, wie Sie Mimecast Admin Console in Azure Active Directory (Azure AD) integrieren.

Die Integration von Mimecast Admin Console in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Mimecast Admin Console hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Mimecast Admin Console anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Mimecast Admin Console konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Mimecast Admin Console-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Mimecast Admin Console aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-mimecast-admin-console-from-the-gallery"></a>Hinzufügen von Mimecast Admin Console aus dem Katalog
Zum Konfigurieren der Integration von Mimecast Admin Console in Azure AD müssen Sie Mimecast Admin Console über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Mimecast Admin Console aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Mimecast Admin Console** ein, wählen Sie im Ergebnisbereich **Mimecast Admin Console** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Mimecast Admin Console in der Ergebnisliste](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Mimecast Admin Console mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Mimecast Admin Console als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mimecast Admin Console muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Mimecast Admin Console den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Mimecast Admin Console müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Mimecast Admin Console-Testbenutzers](#create-a-mimecast-admin-console-test-user)**, um eine Entsprechung von Britta Simon in Mimecast Admin Console zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Mimecast Admin Console-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Mimecast Admin Console die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Mimecast Admin Console** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Mimecast Admin Console** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Mimecast Admin Console](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    Geben Sie im Textfeld **Anmelde-URL** diese URL ein:
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > Die Anmelde-URL ist regionsspezifisch.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Mimecast Admin Console-Konfiguration** auf **Mimecast Admin Console konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Mimecast Admin Console-Konfiguration](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei Mimecast Admin Console als Administrator an.

8. Wechseln Sie zu **Dienste \> Anwendung**.

    ![Dienste](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Dienste")

9. Klicken Sie auf **Authentifizierungsprofile**.

    ![Authentifizierungsprofile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Authentifizierungsprofile")
    
10. Klicken Sie auf **Neues Authentifizierungsprofil**.

    ![Neue Authentifizierungsprofile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "Neue Authentifizierungsprofile")

11. Führen Sie im Abschnitt **Authentifizierungsprofil** die folgenden Schritte aus:

    ![Authentifizierungsprofil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentifizierungsprofil")
    
    a. Geben Sie im Textfeld **Beschreibung** einen Namen für die Konfiguration ein.
    
    b. Wählen Sie **SAML-Authentifizierung für Mimecast Admin Console aktivieren**aus.
    
    c. Wählen Sie für **Anbieter** die Option **Azure Active Directory** aus.
    
    d. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld für den **Issuer URL** (Aussteller-URL) ein.
    
    e. Fügen Sie die **URL für den SAML-SSO-Dienst**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Login URL** (Anmelde-URL) ein.

    f. Fügen Sie die **URL für den SAML-SSO-Dienst**, die Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Logout URL** (Abmelde-URL) ein.
    
    >[!NOTE]
    >Die Werte für Anmelde-URL und Abmelde-URL sind bei Mimecast Admin Console gleich.
    
    g. Öffnen Sie das Base64-Zertifikat, das Sie aus dem Azure-Portal heruntergeladen haben, in Editor, entfernen Sie die erste Zeile (*--*) und die letzte Zeile (*--*), und kopieren Sie den übrigen Inhalt in die Zwischenablage. Fügen Sie ihn anschließend in das Textfeld **Identity Provider Certificate (Metadata)** (Identitätsanbieterzertifikat (Metadaten)) ein.
    
    h. Wählen Sie **Einmaliges Anmelden zulassen**aus.
    
    i. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985). 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-mimecast-admin-console-test-user"></a>Erstellen eines Mimecast Admin Console-Testbenutzers

Damit sich Azure AD-Benutzer bei Mimecast Admin Console anmelden können, müssen Sie in Mimecast Personal Portal bereitgestellt werden. Im Fall von Mimecast Admin Console ist die Bereitstellung eine manuelle Aufgabe.

* Sie müssen eine Domäne registrieren, bevor Sie Benutzer erstellen können.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei **Mimecast Admin Console** als Administrator an.
2. Wechseln Sie zu **Verzeichnisse \> Intern**.
   
   ![Verzeichnisse](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Verzeichnisse")
3. Klicken Sie auf **Neue Domäne registrieren**.
   
   ![Neue Domäne registrieren](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Neue Domäne registrieren")
4. Nachdem die neue Domäne erstellt wurde, klicken Sie auf **Neue Adresse**.
   
   ![Neue Adresse](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "Neue Adresse")
5. Führen Sie im Dialogfeld „Neue Adresse“ die folgenden Schritte aus:
   
   ![Speichern](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Speichern")
   
   a. Geben Sie die Attribute **E-Mail-Adresse**, **Globaler Name**, **Kennwort** und **Kennwort bestätigen** eines gültigen Azure AD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.

   b. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Mimecast Admin Console-Benutzerkonten oder mithilfe der von Mimecast Admin Console bereitgestellten APIs erstellen. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mimecast Admin Console gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Mimecast Admin Console zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Mimecast Admin Console** aus.

    ![Mimecast Admin Console-Link in der Anwendungsliste](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Mimecast Admin Console“ klicken, sollten Sie automatisch bei Ihrer Mimecast Admin Console-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png


