---
title: 'Tutorial: Azure Active Directory-Integration mit Procore SSO | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Procore SSO konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 042a41eaa6bb21670b4c12068f1b017222f64b99
ms.lasthandoff: 04/12/2017


---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a>Tutorial: Azure Active Directory-Integration mit Procore SSO

In diesem Tutorial erfahren Sie, wie Sie Procore SSO in Azure Active Directory (Azure AD) integrieren.

Die Integration von Procore SSO in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Procore SSO hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Procore SSO anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Procore SSO, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Procore SSO konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Procore SSO-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Procore SSO aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-procore-sso-from-the-gallery"></a>Hinzufügen von Procore SSO aus dem Katalog
Zum Konfigurieren der Integration von Procore SSO in Azure AD müssen Sie Procore SSO aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Procore SSO aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie **Procore SSO** im Suchfeld ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Procore SSO** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden über Azure AD bei Procore SSO mithilfe eines Testbenutzers namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Procore SSO als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Procore SSO muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Procore SSO zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens bei Procore SSO über Azure AD müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Procore SSO-Testbenutzers](#creating-a-procore-sso-test-user)** – um eine Entsprechung von Britta Simon in Procore SSO zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden über Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Procore SSO-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens bei Procore SSO über Azure AD die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Procore SSO** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. Im Abschnitt **Domäne und URLs für Procore SSO** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Procore SSO-Konfiguration** auf **Procore SSO konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. Um das einmalige Anmelden seitens **Procore SSO** zu konfigurieren, melden Sie sich als Administrator bei Ihrer Procore-Unternehmenswebsite an.

8. Klicken Sie auf der Dropdownliste der Toolbox auf **Admin**, um die Seite mit den Einstellungen für das einmalige Anmelden zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. Fügen Sie die Werte wie unten beschrieben in die Felder ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    a. Fügen Sie im Feld **Single Sign On Issuer URL** die SAML-Entitäts-ID ein, die Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie im Feld **SAML Sign On Target URL** die URL für den SAML-SSO-Dienst ein, die Sie aus dem Azure-Portal kopiert haben.

    c. Öffnen Sie jetzt die **XML-Metadatendatei**, die Sie aus dem Azure-Portal heruntergeladen haben, und kopieren Sie das Zertifikat in das Tag mit der Bezeichnung **X509Certificate**. Fügen Sie den kopierten Wert in das Feld **Single Sign On x509 Certificate** ein.

10. Klicken Sie auf **Save Changes**.

11. Nachdem Sie diese Einstellungen konfiguriert haben, müssen Sie den **Domänennamen**, mit dem Sie sich bei Procore anmelden (z.B. **contoso.com**), an das [Procore-Supportteam](https://support.procore.com/) senden, damit das Team das einmalige Anmelden im Verbund für diese Domäne aktiviert.

<!--### Next steps

To ensure users can sign-in to Procore SSO after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Procore SSO in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-procore-sso-test-user"></a>Erstellen eines Procore SSO-Testbenutzers

Führen Sie die folgenden Schritte aus, um einen Procore-Testbenutzer auf der Seite von Procore zu erstellen.

1. Melden Sie sich als Administrator bei Ihrer Procore-Unternehmenswebsite an.    

2. Klicken Sie in der Dropdownliste der Toolbox auf **Directory**, um die Verzeichnisseite für Ihr Unternehmen zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. Klicken Sie auf die Option **Add a Person**, um das Formular zu öffnen, und geben Sie die folgenden Optionen ein:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    a. Geben Sie im Textfeld **First Name** den Vornamen des Benutzers ein, z.B. **Britta**.

    b. Geben Sie im Textfeld **Last Name** den Nachnamen des Benutzers ein, z.B. **Simon**.

    c. Geben Sie im Textfeld **Email Address** die E-Mail-Adresse des Benutzers ein, z.B. **BrittaSimon@contoso.com**.

    d. Wählen Sie für **Permission Template** die Option **Apply Permission Template Later** aus.

    e. Klicken Sie auf **Erstellen**.

4. Prüfen Sie die Informationen für den neu hinzugefügten Kontakt, und aktualisieren Sie sie ggf.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. Klicken Sie auf **Save and Send Invitiation** (wenn eine Einladung per E-Mail erforderlich ist) oder auf **Save** (zum direkten Speichern), um die Benutzerregistrierung abzuschließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Procore SSO gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Procore SSO zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Procore SSO**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die Einstellungen für das einmalige Anmelden testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). Wenn Sie im Zugriffsbereich auf die Kachel „Procore SSO“ klicken, sollten Sie automatisch bei Ihrer Procore SSO-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png


