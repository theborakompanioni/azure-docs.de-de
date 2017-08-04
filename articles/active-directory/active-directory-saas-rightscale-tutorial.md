---
title: 'Tutorial: Azure Active Directory-Integration mit RightScale | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und RightScale konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 222c4414a9f736a3589b4cdd0ed934696f6c31ef
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Tutorial: Azure Active Directory-Integration in RightScale

In diesem Tutorial erfahren Sie, wie Sie RightScale in Azure Active Directory (Azure AD) integrieren.

Die Integration von RightScale in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf RightScale hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei RightScale anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in RightScale konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein RightScale-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von RightScale aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-rightscale-from-the-gallery"></a>Hinzufügen von RightScale aus dem Katalog
Zum Konfigurieren der Integration von RightScale in Azure AD müssen Sie RightScale aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um RightScale aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **RightScale**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_search.png)

5. Wählen Sie im Ergebnisbereich **RightScale** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei RightScale mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in RightScale als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in RightScale muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in RightScale den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei RightScale müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines RightScale-Testbenutzers](#creating-a-rightscale-test-user)**, um ein Pendant von Britta Simon in RightScale zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer RightScale-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei RightScale die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **RightScale** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_samlbase.png)

3. Wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten, müssen Sie im Abschnitt **Domäne und URLs für RightScale** keine Schritte ausführen, da die App bereits vorab in Azure integriert wurde.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url.png)

4. Führen Sie im Abschnitt **Domäne und URLs für RightScale** die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_url1.png)

    a. Klicken Sie auf die Option **Erweiterte URL-Einstellungen anzeigen**.

    b. Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://login.rightscale.com/`.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **RightScale-Konfiguration** auf **RightScale konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die SAML-Dienst-URL für einmaliges Anmelden** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_configure.png) 
<CS>
8. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem RightScale-Mandanten anmelden.

    a. Klicken Sie im Menü oben auf die Registerkarte **Settings**, und wählen Sie **Single Sign-On**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 

    b. Klicken Sie auf die Schaltfläche **new**, um **Your SAML Identity Providers** hinzuzufügen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
 
    c. Geben Sie im Textfeld **Display Name**den Namen Ihres Unternehmens ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)
 
    d. Wählen Sie in unten stehendem Textfeld **Allow RightScale-initiated SSO using a discovery hint** aus, und geben Sie Ihren **Domänennamen** in das unten stehende Textfeld ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

    e. Fügen Sie den Wert von **AML-Dienst-URL für einmaliges Anmelden**, den Sie im Azure-Portal kopiert haben, in RightScale in das Feld **SAML SSO Endpoint** (SAML-SSO-Endpunkt) ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

    f. Fügen Sie den Wert von **SAML-Entitäts-ID**, den Sie im Azure-Portal kopiert haben, in RightScale in das Feld für die **SAML-Entitäts-ID** ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

    g. Klicken Sie auf die Schaltfläche **Browse** (Durchsuchen), um das aus dem Azure-Portal heruntergeladene Zertifikat hochzuladen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

    h. Klicken Sie auf **Speichern**.
<CE>
> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-rightscale-test-user"></a>Erstellen eines Rightscale-Testbenutzers

In diesem Abschnitt erstellen Sie in RightScale einen Benutzer mit dem Namen Britta Simon. Lassen Sie sich beim Hinzufügen von Benutzern auf der RightScale-Plattform ggf. vom [Supportteam für den RightScale-Client](mailto:support@rightscale.com) unterstützen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf RightScale gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu RightScale zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **RightScale**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel "RightScale" klicken, sollten Sie automatisch in Ihrer RightScale-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png


