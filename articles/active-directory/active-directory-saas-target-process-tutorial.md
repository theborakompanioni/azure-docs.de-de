---
title: 'Tutorial: Azure Active Directory-Integration mit TargetProcess | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TargetProcess konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7cb91628-e758-480d-a233-7a3caaaff50d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: d15931a5d430252bbd9ae342e1f8fde1a539355b
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="tutorial-azure-active-directory-integration-with-targetprocess"></a>Tutorial: Azure Active Directory-Integration mit TargetProcess

In diesem Tutorial erfahren Sie, wie Sie TargetProcess in Azure Active Directory (Azure AD) integrieren.

Die Integration von TargetProcess in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TargetProcess hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei TargetProcess anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit TargetProcess konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein TargetProcess-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von TargetProcess aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

## <a name="add-targetprocess-from-the-gallery"></a>Hinzufügen von TargetProcess aus dem Katalog
Zum Konfigurieren der Integration von TargetProcess in Azure AD müssen Sie TargetProcess aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um TargetProcess aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **TargetProcess** ein, wählen Sie im Ergebnisbereich **TargetProcess** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Hinzufügen von TargetProcess aus dem Katalog](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei TargetProcess anhand eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TargetProcess als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TargetProcess muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in TargetProcess den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei TargetProcess müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines TargetProcess-Testbenutzers](#create-a-targetprocess-test-user)**, um ein Pendant von Britta Simon in TargetProcess zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer TargetProcess-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in TargetProcess die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **TargetProcess** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![SAML-basierte Anmeldung](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für TargetProcess** die folgenden Schritte aus:

    ![Abschnitt „Domäne und URLs für TargetProcess“](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.tpondemand.com/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.tpondemand.com/`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den TargetProcess-Client](mailto:support@targetprocess.com), um diese Werte zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Abschnitt für SAML-Signaturzertifikat](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Speichern“](./media/active-directory-saas-target-process-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **TargetProcess-Konfiguration** auf **TargetProcess konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Abschnitt „TargetProcess-Konfiguration“](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_configure.png) 

7. Melden Sie sich bei Ihrer TargetProcess-Anwendung als Administrator an.

8. Klicken Sie im oberen Menü auf **Setup**.
   
    ![Einrichtung](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_05.png)

9. Klicken Sie auf **Einstellungen**.
   
    ![Einstellungen](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_06.png) 

10. Klicken Sie auf **Einmaliges Anmelden**.
   
    ![Klicken Sie auf „Einmaliges Anmelden“.](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_07.png) 

11. Führen Sie im Dialogfeld „Einstellungen für einmaliges Anmelden“ folgende Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-target-process-tutorial/tutorial_target_process_08.png)
    
    a. Klicken Sie auf **Einmaliges Anmelden aktivieren**.
    
    b. Fügen Sie im Textfeld **Anmelde-URL** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **Zertifikat** ein.
    
    d. Klicken Sie auf **JIT-Bereitstellung aktivieren**.

    e. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-target-process-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![So zeigen Sie die Liste der Benutzer an](./media/active-directory-saas-target-process-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-target-process-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Abschnitt „Benutzer“](./media/active-directory-saas-target-process-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-targetprocess-test-user"></a>Erstellen eines TargetProcess-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in TargetProcess.

TargetProcess unterstützt die Just-in-Time-Bereitstellung. Sie haben sie bereits unter [Konfigurieren der einmaligen Anmeldung in Azure AD](#configuring-azure-ad-single-sign-on)aktiviert.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TargetProcess gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon TargetProcess zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **TargetProcess**aus.

    ![TargetProcess in App-Liste](./media/active-directory-saas-target-process-tutorial/tutorial_target-process_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „TargetProcess“ klicken, sollten Sie automatisch bei Ihrer TargetProcess-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-target-process-tutorial/tutorial_general_203.png


