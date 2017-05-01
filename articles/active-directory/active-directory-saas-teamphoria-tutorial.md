---
title: 'Tutorial: Azure Active Directory-Integration mit Teamphoria | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Teamphoria konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.lasthandoff: 04/12/2017


---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a>Lernprogramm: Azure Active Directory-Integration mit Teamphoria

In diesem Tutorial erfahren Sie, wie Sie Teamphoria in Azure Active Directory (Azure AD) integrieren.

Die Integration von Teamphoria in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Teamphoria hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Teamphoria anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Integration mit Teamphoria benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Teamphoria-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Teamphoria aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-teamphoria-from-the-gallery"></a>Hinzufügen von Teamphoria aus dem Katalog
Zum Konfigurieren der Integration von Teamphoria in Azure AD müssen Sie Teamphoria aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Teamphoria aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **Teamphoria** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. Wählen Sie im Ergebnisbereich **Teamphoria** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Teamphoria basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Teamphoria als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Teamphoria muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Teamphoria zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Teamphoria müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Teamphoria-Testbenutzers](#creating-a-teamphoria-test-user)**, um in Teamphoria einen Gegenpart für Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Teamphoria-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Teamphoria die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Teamphoria** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Teamphoria** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** die URL im folgenden Format ein: `https://<sub-domain>.teamphoria.com/login`.    

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen mit der tatsächlichen Anmelde-URL aktualisiert werden. Wenden Sie sich an das [Supportteam für Teamphoria-Clients](https://www.teamphoria.com/), um die Anmelde-URL abzurufen. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie das Zertifikat auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Teamphoria-Konfiguration** auf **Teamphoria konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. Um einmaliges Anmelden auf **Teamphoria**-Seite zu konfigurieren, melden Sie sich als Administrator bei Ihrer Teamphoria-Anwendung an.

8. Wechseln Sie in der linken Symbolleiste zur Option **ADMIN SETTINGS** (Administratoreinstellungen), und klicken Sie auf der Registerkarte „Configure“ (Konfigurieren) auf **SINGLE SIGN-ON** (Einmaliges Anmelden), um das SSO-Konfigurationsfenster zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. Klicken Sie in der oberen rechten Ecke auf die Option **ADD NEW IDENTITY PROVIDER** (Neue Identitätsanbieter hinzufügen), um das Formular zum Hinzufügen der Einstellungen für SSO zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. Geben Sie die Details wie unten beschrieben in den Feldern ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **DISPLAY NAME** (Anzeigename): Geben Sie den Anzeigenamen des Plug-Ins auf der Administratorseite ein.

    b. **BUTTON NAME** (Schaltflächenname): Der Name der Registerkarte, die auf der Anmeldeseite für die Anmeldung über SSO angezeigt wird.

    c. **CERTIFICATE** (Zertifikat): Öffnen Sie das zuvor aus dem Azure-Portal heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn in dieses Feld ein.

    d. **ENTRY POINT** (Einstiegspunkt): Fügen Sie die zuvor aus dem Azure-Portal kopierte **URL für den SAML-SSO-Dienst** ein.

    e. Setzen Sie die Option auf **ON** (Ein), und klicken Sie auf **SAVE** (Speichern).    

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-teamphoria-test-user"></a>Erstellen eines Teamphoria-Testbenutzers

Damit sich Azure AD-Benutzer bei Teamphoria anmelden können, müssen sie in Teamphoria bereitgestellt werden. Im Fall von Teamphoria ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei der Teamphoria-Unternehmenswebsite als Administrator an.

2. Klicken Sie in der linken Symbolleiste auf **ADMIN**-Einstellungen, und klicken Sie auf der Registerkarte **MANAGE** (Verwalten) auf **USERS** (Benutzer), um die Administratorseite für Benutzer zu öffnen.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. Klicken Sie auf die Option **MANUAL INVITE** (Manuell einladen).

    ![Invite People](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)    

4. Führen Sie auf dieser Seite die folgende Aktion aus. 
    
    ![Invite People](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)    

    a. Geben Sie im Textfeld **EMAIL ADDRESS** (E-Mail-Adresse) die **E-Mail-Adresse** von Britta Simon ein.

    b. Geben Sie im Textfeld **FIRST NAME** (Vorname) den Namen **Britta** ein.

    c. Geben Sie im Textfeld **LAST NAME** (Nachname) den Namen **Simon** ein.

    d. Klicken Sie auf **INVITE 1 USER** (1 Benutzer einladen). Der Benutzer muss die Einladung annehmen, um im System erstellt zu werden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Teamphoria gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Teamphoria durchzuführen:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Teamphoria** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png


