---
title: 'Tutorial: Azure Active Directory-Integration mit Salesforce | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Salesforce konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2d7d420-dc91-41b8-a6b3-59579e043b35
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 639e40ca7e406a1726033e9f5c5363c289087589
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-salesforce"></a>Tutorial: Azure Active Directory-Integration mit Salesforce

In diesem Tutorial erfahren Sie, wie Sie Salesforce in Azure Active Directory (Azure AD) integrieren.

Die Integration von Salesforce in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Salesforce haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Salesforce anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Salesforce konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Salesforce-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Salesforce aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-salesforce-from-the-gallery"></a>Hinzufügen von Salesforce aus dem Katalog
Zum Konfigurieren der Integration von Salesforce in Azure AD müssen Sie Salesforce aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Salesforce aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld **Salesforce** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_search.png)

5. Wählen Sie im Ergebnisbereich **Salesforce** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Salesforce mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Salesforce als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Salesforce muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** in Salesforce zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Salesforce müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Salesforce-Testbenutzers](#creating-a-salesforce-test-user)**, um eine Entsprechung von Britta Simon in Salesforce zu erhalten, die mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Salesforce-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD in Salesforce zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Salesforce** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Salesforce** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_url.png)

    Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: 
   * Enterprise-Konto: `https://<subdomain>.my.salesforce.com`
   * Developer-Konto: `https://<subdomain>-dev-ed.my.salesforce.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von Salesforce](https://help.salesforce.com/support), um diese Werte zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Salesforce-Konfiguration** auf **Salesforce konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**. 

    ![Konfigurieren des einmaligen Anmeldens](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_configure.png) 
<CS>
7.  Öffnen Sie eine neue Registerkarte in Ihrem Browser, und melden Sie sich mit Ihrem Salesforce-Administratorkonto an.

8.  Klicken Sie im Navigationsbereich unter **Administrator** auf **Sicherheitssteuerelemente**, um den entsprechenden Abschnitt zu erweitern. Klicken Sie dann auf **Einstellungen für einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png)

9.  Klicken Sie auf der Seite **Einstellungen für einmaliges Anmelden** auf die Schaltfläche **Bearbeiten**.
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png)

      > [!NOTE]
      > Wenn Sie keine Einstellungen für die einmalige Anmeldung für Ihr Salesforce-Konto aktivieren können, müssen Sie sich an das [Kundensupportteam von Salesforce](https://help.salesforce.com/support) wenden. 

10. Wählen Sie **SAML aktiviert**, und klicken Sie dann auf **Speichern**.

      ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png)
11. Um Ihre SAML-Einstellungen für die einmalige Anmeldung zu konfigurieren, klicken Sie auf **Neu**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png)

12. Nehmen Sie auf der Seite **SAML-Einstellung für einmaliges Anmelden bearbeiten** folgende Konfigurationen vor:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png)

    a. Geben Sie in das Feld **Name** einen Anzeigenamen für diese Konfiguration ein. Bei Eingabe eines Werts für **Name** wird automatisch das Textfeld **API-Name** gefüllt.

    b. Fügen Sie den Wert **SAML-Entitäts-ID** in das Feld **Aussteller** in Salesforce ein.

    c. Geben Sie in das **Entitäts-ID-Textfeld**Ihren Salesforce-Domänennamen nach folgendem Muster ein:
      
      * Enterprise-Konto: `https://<subdomain>.my.salesforce.com`
      * Developer-Konto: `https://<subdomain>-dev-ed.my.salesforce.com`
      
    d. Klicken Sie auf **Durchsuchen** oder **Datei auswählen**, um das Dialogfeld **Datei zum Hochladen auswählen** zu öffnen. Wählen Sie Ihr Salesforce-Zertifikat aus, und klicken Sie dann zum Hochladen des Zertifikats auf **Öffnen**.

    e. Wählen Sie für **SAML-Identitätstyp** die Option **Erklärung enthält salesforce.com-Benutzernamen des Benutzers**.

    f. Wählen Sie für **SAML-Identitätsstandort** die Option **Identität ist das Namensbezeichnerelement der Ausstellererklärung**.

    g. Fügen Sie die **Dienst-URL für einmaliges Anmelden** in das Feld **Anmelde-URL für Identitätsanbieter** in Salesforce ein.
    
    h. Wählen Sie für **Vom Dienstanbieter initiierte Anforderungsbindung** die Einstellung **HTTP-Umleitung**.
    
    i. Klicken Sie abschließend auf **Speichern** , um Ihre SAML-Einstellungen für die einmalige Anmeldung anzuwenden.

13. Klicken Sie in Salesforce im linken Navigationsbereich auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie dann auf **Meine Domäne**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png)

14. Führen Sie einen Bildlauf nach unten zum Abschnitt **Authentifizierungskonfiguration** aus, und klicken Sie auf die Schaltfläche **Bearbeiten**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png)

15. Wählen Sie im Abschnitt **Authentifizierungsdienst** den Anzeigenamen der SAML-SSO-Konfiguration aus, und klicken Sie dann auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png)

    > [!NOTE]
    > Wenn mehrere Authentifizierungsdienste aktiviert sind, werden Benutzer aufgefordert, einen Authentifizierungsdienst zur Anmeldung auszuwählen, wenn das einmalige Anmelden bei der Salesforce-Umgebung initiiert wird. Wenn Sie dies nicht möchten, sollten Sie **alle anderen Authentifizierungsdienste deaktiviert lassen**.
<CE>    
> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-salesforce-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-salesforce-test-user"></a>Erstellen eines Salesforce-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Salesforce erstellt. Salesforce unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.
Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Falls ein Benutzer nicht bereits in Salesforce vorhanden ist, wird beim Versuch, auf Salesforce zuzugreifen, ein neuer Benutzer erstellt.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf Salesforce gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Salesforce zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Salesforce** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-salesforce-tutorial/tutorial_salesforce_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Um Ihre Einstellungen für die einmalige Anmeldung zu testen, öffnen Sie den Zugriffsbereich unter [https://myapps.microsoft.com](https://myapps.microsoft.com/), melden Sie sich beim Testkonto an, und klicken Sie auf **Salesforce**.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren der Benutzerbereitstellung](active-directory-saas-salesforce-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforce-tutorial/tutorial_general_203.png


