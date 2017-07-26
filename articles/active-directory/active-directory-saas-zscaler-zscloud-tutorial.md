---
title: 'Tutorial: Azure Active Directory-Integration mit ZScaler ZSCloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ZScaler ZSCloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 2b6eb113e5725260bc04f50e9218939bf28b1ff0
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Tutorial: Azure Active Directory-Integration mit Zscaler ZSCloud

In diesem Tutorial erfahren Sie, wie Sie ZScaler ZSCloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von ZScaler ZSCloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ZScaler ZSCloud hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei ZScaler ZSCloud anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in ZScaler ZSCloud konfigurieren zu können, ist Folgendes erforderlich:

- Ein Azure AD-Abonnement
- Ein Zscaler ZSCloud-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Zscaler ZSCloud über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Hinzufügen von Zscaler ZSCloud über den Katalog
Zum Konfigurieren der Integration von Zscaler ZSCloud in Azure AD müssen Sie Zscaler ZSCloud über den Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Zscaler ZSCloud über den Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Zscaler ZSCloud** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Zscaler ZSCloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Zscaler ZSCloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Zscaler ZSCloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Zscaler ZSCloud muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Zscaler ZSCloud zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Zscaler ZSCloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Konfigurieren von Proxyeinstellungen](#configuring-proxy-settings)** zum Konfigurieren der Proxyeinstellungen in Internet Explorer.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Zscaler ZSCloud-Testbenutzers](#creating-a-zscaler-zscloud-test-user)**, um eine Pendant von Britta Simon in Zscaler ZSCloud zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Zscaler ZSCloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Zscaler ZSCloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Zscaler ZSCloud** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_samlbase.png)

3. Führen Sie die folgenden Schritte im Abschnitt **Domäne und URLs für Zscaler ZSCloud** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_url.png)

     Geben Sie im Textfeld **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer Zscaler ZSCloud-Anwendung verwendete URL ein.
    
    > [!NOTE] 
    > Sie müssen den Wert mit der richtigen Anmelde-URL aktualisieren. Wenden Sie sich an den [Zscaler ZSCloud-Support](https://support.zscaler.com/hc/articles/210172606-Zscaler-is-Expanding-Its-Zscloud-Global-Footprint), um diesen Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Zscaler ZSCloud-Konfiguration** auf **Zscaler ZSCloud konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Zscaler ZSCloud-Unternehmenswebsite als Administrator an.

8. Klicken Sie oben im Menü auf **Verwaltung**.
   
    ![Verwaltung](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800206.png "Verwaltung")

9. Klicken Sie unter **Administratoren & Rollen verwalten** auf **Benutzer & Authentifizierung verwalten**.   
            
    ![Benutzer & Authentifizierung verwalten](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800207.png "Benutzer & Authentifizierung verwalten")

10. Führen Sie im Abschnitt **Auswählen von Authentifizierungsoptionen für Ihre Organisation** die folgenden Schritte aus:   
                
    ![Authentifizierung](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800208.png "Authentifizierung")
   
    a. Wählen Sie **Authenticate using SAML Single Sign-On** (Mit der einmaligen SAML-Anmeldung authentifizieren).

    b. Klicken Sie auf **Einzelne Parameter der einmaligen Anmeldung für SAML konfigurieren**.

11. Führen Sie auf der Dialogfeldseite **Parameter der einmaligen Anmeldung für SAML konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Fertig**.

    ![Einmaliges Anmelden](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800209.png "des einmaligen Anmeldens")
    
    a. Fügen Sie Wert **SAML-Dienst-URL für einmalige Anmeldung** in das Textfeld **URL des SAML-Portals, zu dem Benutzer zur Authentifizierung geleitet werden** ein.
    
    b. Geben Sie im Textfeld **Attribut mit Anmeldenamen** die **NameID** ein.
    
    c. Klicken Sie auf **Zscaler pem**, um das heruntergeladene Zertifikat hochzuladen.
    
    d. Wählen Sie **Automatische SAML-Bereitstellung aktivieren**.

12. Führen Sie auf der Dialogseite **Benutzerauthentifizierung konfigurieren** die folgenden Schritte aus:

    ![Verwaltung](./media/active-directory-saas-zscaler-zscloud-tutorial/ic800210.png "Verwaltung")
    
    a. Klicken Sie auf **Speichern**.

    b. Klicken Sie auf **Jetzt aktivieren**.

## <a name="configuring-proxy-settings"></a>Konfigurieren von Proxyeinstellungen
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>So konfigurieren Sie die Proxyeinstellungen in Internet Explorer:

1. Starten Sie **Internet Explorer**.

2. Wählen Sie im Menü **Extras** die Option **Internetoptionen**, um das Dialogfeld **Internetoptionen** zu öffnen.   
    
     ![Internetoptionen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769492.png "Internetoptionen")

3. Klicken Sie auf die Registerkarte **Verbindungen** .   
  
     ![Verbindungen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769493.png "Verbindungen")

4. Klicken Sie zum Öffnen des Dialogfelds **LAN-Einstellungen** auf **LAN-Einstellungen**.

5. Führen Sie im Abschnitt "Proxyserver" die folgenden Schritte aus:   
   
    ![Proxyserver](./media/active-directory-saas-zscaler-zscloud-tutorial/ic769494.png "Proxyserver")

    a. Wählen Sie **Proxyserver für LAN verwenden** aus.

    b. Geben Sie in das Textfeld „Adresse“ **gateway.zscalerone.net**ein.

    c. Geben Sie im Textfeld „Port“ **80**ein.

    d. Wählen Sie **Proxyserver für lokale Adressen umgehen**.

    e. Klicken Sie zum Schließen des Dialogfelds **Einstellungen für lokales Netzwerk** auf **OK**.

6. Klicken Sie zum Schließen des Dialogfelds **Internetoptionen** auf **OK**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zscaler-zscloud-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-a-zscaler-zscloud-test-user"></a>Erstellen eines Testbenutzers für Zscaler ZSCloud

Damit sich Azure AD-Benutzer bei ZScaler ZSCloud anmelden können, müssen sie in ZScaler ZSCloud bereitgestellt werden.  
Im Fall von ZScaler ZSCloud ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung

1. Melden Sie sich bei Ihrem **Zscaler** -Mandanten an.

2. Klicken Sie auf **Verwaltung**.   
   
    ![Verwaltung](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781035.png "Verwaltung")

3. Klicken Sie auf **Benutzerverwaltung**.   
        
     ![Hinzufügen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Hinzufügen")

4. Klicken Sie auf der Registerkarte **Benutzer** auf **Hinzufügen**.
      
    ![Hinzufügen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781037.png "Hinzufügen")

5. Führen Sie im Abschnitt "Benutzer hinzufügen" die folgenden Schritte aus:
        
    ![Benutzer hinzufügen](./media/active-directory-saas-zscaler-zscloud-tutorial/ic781038.png "Benutzer hinzufügen")
   
    a. Geben Sie die **Benutzer-ID**, den **Benutzeranzeigenamen**, das **Kennwort** und **Kennwort bestätigen** ein, und wählen Sie dann **Gruppen** und die **Abteilung** eines gültigen AAD-Kontos aus, das Sie bereitstellen möchten.

    b. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von ZScaler ZSCloud-Benutzerkonten oder mithilfe der von ZScaler ZSCloud bereitgestellten APIs erstellen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Zscaler ZSCloud gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Zscaler ZSCloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **Zscaler ZSCloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_zscalerzscloud_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Zscaler ZSCloud“ klicken, sollten Sie automatisch bei Ihrer Zscaler ZSCloud-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscaler-zscloud-tutorial/tutorial_general_203.png


