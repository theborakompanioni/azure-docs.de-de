---
title: 'Tutorial: Azure Active Directory-Integration mit IBM Kenexa Survey Enterprise | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und IBM Kenexa Survey Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 5c276c23288292a1c54dd9d57177d5072b90c9e3
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Tutorial: Azure Active Directory-Integration mit IBM Kenexa Survey Enterprise

In diesem Tutorial erfahren Sie, wie Sie IBM Kenexa Survey Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von IBM Kenexa Survey Enterprise in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf IBM Kenexa Survey Enterprise hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei IBM Kenexa Survey Enterprise anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software-as-a-Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit IBM Kenexa Survey Enterprise konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein IBM Kenexa Survey Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Es wird nicht empfohlen, zum Testen der Schritte in diesem Tutorial eine Produktionsumgebung zu verwenden.

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

* Hinzufügen von IBM Kenexa Survey Enterprise aus dem Katalog
* Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-ibm-kenexa-survey-enterprise-from-the-gallery"></a>Hinzufügen von IBM Kenexa Survey Enterprise aus dem Katalog
Zum Konfigurieren der Integration von IBM Kenexa Survey Enterprise in Azure AD müssen Sie IBM Kenexa Survey Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

Um IBM Kenexa Survey Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) auf die Schaltfläche **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie zum Hinzufügen einer Anwendung auf die Schaltfläche **Neue Anwendung**.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld als Suchbegriff **IBM Kenexa Survey Enterprise**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_search.png)

5. Wählen Sie in der Ergebnisliste den Eintrag **IBM Kenexa Survey Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![IBM Kenexa Survey Enterprise in der Ergebnisliste](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei IBM Kenexa Survey Enterprise mithilfe einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit SSO funktioniert, muss in Azure AD die Entsprechung des IBM Kenexa Survey Enterprise-Benutzers in Azure AD identifiziert werden. Anders ausgedrückt: Für Azure AD muss zwischen einem Azure AD-Benutzer und einem entsprechenden Benutzer in IBM Kenexa Survey Enterprise eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in IBM Kenexa Survey Enterprise als Wert des **Benutzernamens** in Azure AD zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei IBM Kenexa Survey Enterprise müssen Sie die Bausteine in den nächsten beiden Abschnitten ausführen:

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

In diesem Abschnitt aktivieren Sie wie folgt das einmalige Anmelden von Azure AD im Azure-Portal und führen die entsprechende Konfiguration in Ihrer IBM Kenexa Survey Enterprise-Anwendung durch:

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **IBM Kenexa Survey Enterprise** auf **Einmaliges Anmelden**.

    ![IBM Kenexa Survey Enterprise – Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Feld **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für IBM Kenexa Survey Enterprise** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für IBM Kenexa Survey Enterprise](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://surveys.kenexa.com/<companycode>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL im folgenden Format ein: `https://surveys.kenexa.com/<companycode>/tools/sso.asp`

    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [IBM Kenexa Survey Enterprise-Supportteam](https://www.ibm.com/support/home/?lnk=fcw), um die richtigen Werte zu erhalten.

4. Klicken Sie unter **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Link zum Herunterladen des Zertifikats (Base64)](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_certificate.png) 

    Die IBM Kenexa Survey Enterprise-Anwendung erwartet die SAML-Assertionen (Security Assertions Markup Language) in einem bestimmten Format. Hierzu müssen Sie der Konfiguration Ihrer SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der Wert des Benutzer-ID-Anspruchs in der Antwort muss mit der SSO-ID übereinstimmen, die im Kenexa-System konfiguriert ist. Arbeiten Sie mit dem [IBM Kenexa Survey Enterprise-Supportteam](https://www.ibm.com/support/home/?lnk=fcw) zusammen, um die richtige Benutzer-ID in Ihrer Organisation als Internet Datagram Protocol (IDP) für das einmalige Anmelden zuzuordnen. 

    Standardmäßig wird die Benutzer-ID in Azure AD als Wert für den Benutzerprinzipalnamen (User Principal Name, UPN) festgelegt. Sie können diesen Wert auf der Registerkarte **Attribut** ändern. Dies ist im folgenden Screenshot dargestellt. Die Integration funktioniert erst, nachdem Sie die Zuordnung korrekt durchgeführt haben.
    
    ![Dialogfeld „Benutzerattribute“](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_attribute.png)   

5. Klicken Sie auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren – Schaltfläche „Speichern“](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_400.png)

6. Klicken Sie zum Öffnen des Fensters **Anmeldung konfigurieren** unter **IBM Kenexa Survey Enterprise-Konfiguration** auf **IBM Kenexa Survey Enterprise konfigurieren**. 
 
    ![Link „IBM Kenexa Survey Enterprise konfigurieren“](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_configure.png)

7. Kopieren Sie die Werte für die **Abmelde-URL**, **SAML-Entitäts-ID** und **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

8. Kopieren Sie im Fenster **Anmeldung konfigurieren** unter **Kurzübersicht** die Werte für **Abmelde-URL**, **SAML-Entitäts-ID** und **URL für den SAML-SSO-Dienst**.

9. Senden Sie zum Konfigurieren des einmaligen Anmeldens aufseiten von **IBM Kenexa Survey Enterprise** das heruntergeladene **Zertifikat (Base64)** und die Werte für die **Abmelde-URL**, **SAML-Entitäts-ID** und **URL für den SAML-SSO-Dienst** an das [IBM Kenexa Survey Enterprise-Supportteam](https://www.ibm.com/support/home/?lnk=fcw).

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen anzeigen. Nachdem Sie diese App über den Abschnitt **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden** und rufen anschließend die eingebettete Dokumentation über den Abschnitt **Konfiguration** am unteren Rand auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation](https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im Azure-Portal wie folgt einen Testbenutzer mit dem Namen Britta Simon:

![Erstellen eines Azure AD-Testbenutzers][100]

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_01.png) 

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Create**.
 
### <a name="create-an-ibm-kenexa-survey-enterprise-test-user"></a>Erstellen eines IBM Kenexa Survey Enterprise-Testbenutzers

In diesem Abschnitt erstellen Sie in IBM Kenexa Survey Enterprise einen Benutzer mit dem Namen Britta Simon. 

Sie können mit dem [IBM Kenexa Survey Enterprise-Supportteam](https://www.ibm.com/support/home/?lnk=fcw) zusammenarbeiten, um im IBM Kenexa Survey Enterprise-System Benutzer zu erstellen und jeweils die SSO-ID für diese Benutzer zuzuordnen. Dieser SSO-ID-Wert sollte auch dem Benutzer-ID-Wert aus Azure AD zugeordnet werden. Sie können diese Standardeinstellung auf der Registerkarte **Attribut** ändern.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf IBM Kenexa Survey Enterprise gewähren.

![Zuweisen der Benutzerrolle][200] 

Führen Sie die folgenden Schritte aus, um IBM Kenexa Survey Enterprise den Benutzer Britta Simon zuzuweisen:

1. Öffnen Sie im Azure-Portal die Ansicht **Anwendungen**. Navigieren Sie zur Ansicht **Verzeichnis**, wählen Sie **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Links „Unternehmensanwendungen“ und „Alle Anwendungen“][201] 

2. Wählen Sie in der Liste **Anwendungen** die Option **IBM Kenexa Survey Enterprise** aus.

    ![Link „IBM Kenexa Survey Enterprise“ in der Liste „Anwendungen“](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_app.png) 

3. Klicken Sie auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**, und wählen Sie dann im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen**.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste **Benutzer** die Option **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel **IBM Kenexa Survey Enterprise** klicken, sollten Sie automatisch bei Ihrer IBM Kenexa Survey Enterprise-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png

 

