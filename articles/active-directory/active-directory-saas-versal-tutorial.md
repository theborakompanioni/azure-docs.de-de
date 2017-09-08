---
title: 'Tutorial: Azure Active Directory-Integration mit Versal | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Versal konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5b2e53c0-61a3-4954-ae46-8c28c6368bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 546610d05d0ed261f671df651cd6cc74837063f7
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="tutorial-azure-active-directory-integration-with-versal"></a>Tutorial: Azure Active Directory-Integration mit Versal

In diesem Tutorial erfahren Sie, wie Sie Versal in Azure Active Directory (Azure AD) integrieren.

Die Integration von Versal in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Versal hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Versal anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Versal konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Versal-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Versal aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-versal-from-the-gallery"></a>Hinzufügen von Versal aus dem Katalog
Zum Konfigurieren der Integration von Versal in Azure AD müssen Sie Versal aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Versal aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Versal** ein, wählen Sie im Ergebnisbereich **Versal** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Versal in der Ergebnisliste](./media/active-directory-saas-versal-tutorial/tutorial_versal_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Versal mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Versal als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Versal muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Versal den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Versal müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Versal-Testbenutzers](#create-a-versal-test-user)**, um eine Entsprechung von Britta Simon in Versal zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Versal-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Versal die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Versal** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-versal-tutorial/tutorial_versal_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Versal** aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Versal](./media/active-directory-saas-versal-tutorial/tutorial_versal_url.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `VERSAL` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://versal.com/sso/saml/orgs/<organization_id>`

    > [!NOTE] 
    > Der Wert der Antwort-URL entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Wenden Sie sich an das [Supportteam von Versal](https://support.versal.com/hc/), um diesen Wert zu erhalten.
    
4. Die Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Der Standardwert von **Benutzer-ID** lautet **user.userprincipalname**, **Versal** erwartet jedoch, dass dieser Wert der E-Mail-Adresse des Benutzers zugeordnet ist. Hierfür können Sie das **user.mail**-Attribut aus der Liste verwenden oder den entsprechenden Attributwert gemäß der Konfiguration in Ihrer Organisation angeben.
    
    ![Benutzer-ID-Dropdownmenü](./media/active-directory-saas-versal-tutorial/tutorial_versal_attribute.png)

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-versal-tutorial/tutorial_versal_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-versal-tutorial/tutorial_general_400.png)
    
7. Zum Konfigurieren der einmaligen Anmeldung auf der **Versal**-Seite müssen Sie die heruntergeladene **Metadaten-XML-Datei** und das **SAML-Signaturzertifikat** an das [Versal-Supportteam](https://support.versal.com/hc/) senden. Es konfiguriert Ihre Versal-Organisation, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-versal-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-versal-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-versal-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-versal-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
  
### <a name="create-a-versal-test-user"></a>Erstellen eines Versal-Testbenutzers

In diesem Abschnitt erstellen Sie in Versal eine Benutzerin namens Britta Simon. Bitte beachten Sie das Supporthandbuch [Creating a SAML test user for Microsoft Azure](https://support.versal.com/hc/en-us/articles/115011672887-Creating-a-SAML-test-user) (Erstellen eines SAML-Testbenutzers für Microsoft Azure), um die Benutzerin Britta Simon innerhalb Ihrer Organisation zu erstellen. Benutzer müssen in Versal erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Versal gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Versal zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Versal** aus.

    ![Versal-Link in der Anwendungsliste](./media/active-directory-saas-versal-tutorial/tutorial_versal_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über einen in Ihre Website eingebetteten Versal-Kurs.
Im Supporthandbuch [Embedding Organizational Courses](https://support.versal.com/hc/en-us/articles/203271866-Embedding-organizational-courses) (Einbetten von Organisationskursen) für **einmalige SAML-Anmeldung** finden Sie Anweisungen zum Einbetten eines Versal-Kurses mit Unterstützung für einmaliges Anmelden in Azure AD. 

Sie müssen einen Kurs erstellen, diesen in Ihrer Organisation freigeben und zum Testen der Kurseinbettung veröffentlichen. Unter [Create a course](https://support.versal.com/hc/en-us/articles/203722528-Create-a-course) (Erstellen eines Kurses), [Publishing a course](https://support.versal.com/hc/en-us/articles/203753398-Publishing-a-course) (Veröffentlichen eines Kurses) und [Course and learner management](https://support.versal.com/hc/en-us/articles/206029467-Course-and-learner-management) (Kurs- und Schülermanagement) finden Sie weitere Informationen.  
                     

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-versal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-versal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-versal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-versal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-versal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-versal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-versal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-versal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-versal-tutorial/tutorial_general_203.png


