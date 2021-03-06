---
title: 'Tutorial: Azure Active Directory-Integration mit OpenAthens | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OpenAthens konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: bbf74768542983be2e69ffb948baf35822610839
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Tutorial: Azure Active Directory-Integration mit OpenAthens

In diesem Tutorial erfahren Sie, wie Sie OpenAthens in Azure Active Directory (Azure AD) integrieren.

Die Integration von OpenAthens in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf OpenAthens hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei OpenAthens anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit OpenAthens konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein OpenAthens-Abonnement, für das einmaliges Anmelden aktiviert ist

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von OpenAthens aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-openathens-from-the-gallery"></a>Hinzufügen von OpenAthens aus dem Katalog
Zum Konfigurieren der Integration von OpenAthens in Azure AD müssen Sie OpenAthens aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um OpenAthens aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **OpenAthens** ein, wählen Sie im Ergebnisbereich **OpenAthens** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![OpenAthens in der Ergebnisliste](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei OpenAthens basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in OpenAthens als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OpenAthens muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in OpenAthens den Wert für **Benutzername** in Azure AD als Wert für **Username** (Benutzername) zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei OpenAthens müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines OpenAthens-Testbenutzers](#create-a-openathens-test-user)**, um eine Entsprechung von Britta Simon in OpenAthens zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer OpenAthens-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei OpenAthens die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **OpenAthens** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_samlbase.png)

3. Geben Sie im Abschnitt **Domäne und URLs für OpenAthens** den Wert `https://login.openathens.net/saml/2/metadata-sp` im Textfeld **Bezeichner** ein.

    ![SSO-Informationen zur Domäne und zu den URLs für OpenAthens](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_url.png)

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-openathens-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der OpenAthens-Unternehmenswebsite als Administrator an.

7. Wählen Sie auf der Registerkarte **Management** (Verwaltung) die Option **Connections** (Verbindungen) aus. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application1.png)

8. Wählen Sie **SAML 1.1/2.0** aus, und klicken Sie auf die Schaltfläche **Configure** (Konfigurieren).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application2.png)
    
9.  Klicken Sie auf die Schaltfläche **Browse** (Durchsuchen), um die XML-Metadatendatei hochzuladen, die Sie vom Azure-Portal heruntergeladen haben. Klicken Sie auf **Add** (Hinzufügen), um die Konfiguration hinzuzufügen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application3.png)

10. Führen Sie auf der Registerkarte **Details** die folgenden Schritte aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_application4.png)

    a. Wählen Sie in **Display name mapping** (Zuordnung des Anzeigenamens) die Option **Use Attribute** (Attribut verwenden) aus.

    b. Geben Sie im Textfeld **Display name attribute** (Anzeigenamenattribut) den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.
    
    c. Wählen Sie in **Unique user mapping** (Eindeutige Benutzerzuordnung) die Option **Use Attribute** (Attribut verwenden) aus.

    d. Geben Sie im Textfeld **Unique user attribute** (Eindeutiges Benutzerattribut) den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` ein.

    e. Aktivieren Sie in **Status** alle drei Kontrollkästchen.

    f. Wählen Sie in **Create local accounts** (Lokale Konten erstellen) die Option **automatically** (automatisch) aus.

    g. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-openathens-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-openathens-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-openathens-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-openathens-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-a-openathens-test-user"></a>OpenAthens-Testbenutzer erstellen

OpenAthens unterstützt die Just-In-Time-Bereitstellung, und Benutzer werden nach erfolgreicher Authentifizierung automatisch erstellt. Sie müssen in diesem Abschnitt keine Aktion ausführen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf OpenAthens gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie zum Zuweisen von Britta Simon zu OpenAthens die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **OpenAthens** aus.

    ![OpenAthens-Link in der Anwendungsliste](./media/active-directory-saas-openathens-tutorial/tutorial_openathens_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „OpenAthens“ klicken, sollten Sie automatisch bei Ihrer OpenAthens-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-openathens-tutorial/tutorial_general_203.png


