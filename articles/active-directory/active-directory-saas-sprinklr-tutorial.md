---
title: 'Tutorial: Azure Active Directory-Integration mit Sprinklr | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Sprinklr konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 6e1622cd55e3b0e8063604ac9dc0cb0673fa9753
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Tutorial: Azure Active Directory-Integration mit Sprinklr

In diesem Tutorial erfahren Sie, wie Sie Sprinklr in Azure Active Directory (Azure AD) integrieren.

Die Integration von Sprinklr in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Sprinklr haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Sprinklr anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Für die Konfiguration der Azure AD-Integration mit Sprinklr benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Sprinklr-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Sprinklr über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sprinklr-from-the-gallery"></a>Hinzufügen von Sprinklr über den Katalog
Zum Konfigurieren der Integration von Sprinklr in Azure AD müssen Sie Sprinklr über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Sprinklr über den Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Sprinklr** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_search.png)

5. Wählen Sie im Ergebnisbereich **Sprinklr** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Sprinklr mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Sprinklr als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Sprinklr muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Sprinklr den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Sprinklr zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Sprinklr-Testbenutzers](#creating-a-sprinklr-test-user)**, um eine Entsprechung von Britta Simon in Sprinklr zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Sprinklr-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Sprinklr zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Sprinklr** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Sprinklr** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.sprinklr.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.sprinklr.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an den [Sprinklr-Support](https://www.sprinklr.com/contact-us/), um diese Werte zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Sprinklr-Konfiguration** auf **Sprinklr konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Sprinklr-Unternehmenswebsite als Administrator an.

8. Wechseln Sie zu **Verwaltung \> Einstellungen**.
   
    ![Verwaltung](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Verwaltung")

9. Wechseln Sie im linken Fensterbereich zu **Partner verwalten \> Einmaliges Anmelden**.
   
    ![Partner verwalten](./media/active-directory-saas-sprinklr-tutorial/ic782908.png "Partner verwalten")

10. Klicken Sie auf **+Einmalige Anmeldungen hinzufügen**.
   
    ![Einmaliges Anmelden](./media/active-directory-saas-sprinklr-tutorial/ic782909.png "einmaliges Anmelden")

11. Führen Sie im Dialogfeld **Einmaliges Anmelden** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden](./media/active-directory-saas-sprinklr-tutorial/ic782910.png "einmaliges Anmelden")

    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B.: *WAADSSOTest*).

    b. Wählen Sie **Aktiviert**.

    c. Wählen Sie **Neues SSO-Zertifikat verwenden**.
             
    e. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.

    f. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Entity Id** ein.

    g. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity Provider Login URL** ein.

    h. Fügen Sie den Wert der **Abmelde-URL**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Identity Provider Logout URL** ein.
     
    i. Wählen Sie für **SAML-Benutzer-ID-Typ** die Option **Assertion enthält sprinklr.com-Benutzernamen des Benutzers** aus.

    j. Wählen Sie für **SAML-Benutzer-ID-Speicherort** die Option **Benutzer-ID ist das Namensbezeichnerelement der Ausstellererklärung** aus.

    k. Klicken Sie auf **Speichern**.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/ic782911.png "SAML")

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sprinklr-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-sprinklr-test-user"></a>Erstellen eines Testbenutzers für Sprinklr

1. Melden Sie sich bei der Sprinklr-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Verwaltung \> Einstellungen**.
   
    ![Verwaltung](./media/active-directory-saas-sprinklr-tutorial/ic782907.png "Verwaltung")

3. Wechseln Sie im linken Fensterbereich zu **Client verwalten \> Benutzer**.
   
    ![Einstellungen](./media/active-directory-saas-sprinklr-tutorial/ic782914.png "Einstellungen")

4. Klicken Sie auf **Benutzer hinzufügen**.
   
    ![Einstellungen](./media/active-directory-saas-sprinklr-tutorial/ic782915.png "Einstellungen")

5. Führen Sie im Dialogfeld **Benutzer bearbeiten** die folgenden Schritte aus:
   
    ![Benutzer bearbeiten](./media/active-directory-saas-sprinklr-tutorial/ic782916.png "Benutzer bearbeiten") 

    a. Geben Sie in die Textfelder **E-Mail**, **Vorname** und **Nachname** die Informationen des Azure AD-Benutzerkontos ein, das Sie bereitstellen möchten.

    b. Wählen Sie **Kennwort deaktiviert**.

    c. Wählen Sie **Sprache** aus.

    d. Wählen Sie **Benutzertyp** aus.

    e. Klicken Sie auf **Aktualisieren**.
   
     >[!IMPORTANT]
     >**Kennwort deaktiviert** muss ausgewählt sein, um einem Benutzer die Anmeldung über einen Identitätsanbieter zu ermöglichen. 
     
6. Wechseln Sie zu **Rolle**, und führen Sie dann die folgenden Schritte aus:
   
    ![Partnerrollen](./media/active-directory-saas-sprinklr-tutorial/ic782917.png "Partnerrollen")

    a. Wählen Sie aus der Liste **Global** die Option **ALL\_Permissions** aus.  

    b. Klicken Sie auf **Aktualisieren**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Sprinklr-Benutzerkonten oder mithilfe der von Sprinklr bereitgestellten APIs erstellen. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Sprinklr gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Sprinklr zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Sprinklr** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/tutorial_sprinklr_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Sprinklr“ klicken, sollten Sie automatisch bei Ihrer Sprinklr-Anmeldung angemeldet werden. Informationen zum Zugriffsbereich finden Sie in der [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sprinklr-tutorial/tutorial_general_203.png


