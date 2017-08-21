---
title: 'Tutorial: Azure Active Directory-Integration mit Workday | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workday konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e9da692e-4a65-4231-8ab3-bc9a87b10bca
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 164d5c644f120fa86e2b690649241892764b64b7
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="tutorial-azure-active-directory-integration-with-workday"></a>Tutorial: Azure Active Directory-Integration mit Workday

In diesem Tutorial erfahren Sie, wie Sie Workday in Azure Active Directory (Azure AD) integrieren.

Die Integration von Workday in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Workday hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workday anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Workday konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Workday-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Workday aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-workday-from-the-gallery"></a>Hinzufügen von Workday aus dem Katalog
Zum Konfigurieren der Integration von Workday in Azure AD müssen Sie Workday aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Workday aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld das Wort **Workday**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/tutorial_workday_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Workday** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/tutorial_workday_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Workday mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Workday als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Workday muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Workday zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Workday müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Workday-Testbenutzers](#creating-a-workday-test-user)**, um ein Pendant von Britta Simon in Workday zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Workday-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Workday die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Workday** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_workday_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Workday** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_workday_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `https://impl.workday.com/<tenant>/login-saml2.htmld`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://impl.workday.com/<tenant>/login-saml.htmld`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und Antwort-URL. Ihre Antwort-URL muss eine Unterdomäne aufweisen (z.B. www, wd2, wd3, wd3-impl, wd5, wd5-impl). *http://www.myworkday.com* ist möglich, *http://myworkday.com* hingegen nicht. Wenden Sie sich an das [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html), um diese Werte zu erhalten. 
 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_workday_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Workday-Konfiguration** auf **Workday konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_workday_configure.png) 
<CS>
7. Melden Sie sich in einem anderen Webbrowserfenster auf der Workday-Unternehmenswebsite als Administrator an.

8. Wechseln Sie zu **Menü \> Workbench**.
   
    ![Workbench](./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

9. Wechseln Sie zu **Kontoverwaltung**.
   
    ![Kontoverwaltung](./media/active-directory-saas-workday-tutorial/IC782924.png "Kontoverwaltung")

10. Wechseln Sie zu **Mandanten-Setup bearbeiten – Sicherheit**.
   
    ![Mandantensicherheit bearbeiten](./media/active-directory-saas-workday-tutorial/IC782925.png "Mandantensicherheit bearbeiten")

11. Führen Sie im Abschnitt **Umleitungs-URLs** die folgenden Schritte aus:
   
    ![Umleitungs-URLs](./media/active-directory-saas-workday-tutorial/IC7829581.png "Umleitungs-URLs")
   
    a. Klicken Sie auf **Zeile hinzufügen**.
   
    b. Geben Sie in den Textfeldern **Umleitungs-URL für Anmeldung** und **Mobile Redirect URL** (Mobile Umleitungs-URL) die **Anmelde-URL** ein, die Sie im Abschnitt **Domäne und URLs für Workday** im Azure-Portal eingegeben haben.
   
    c. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** die **Abmelde-URL**, und fügen Sie ihn in das Textfeld **Logout Redirect URL** (Umleitungs-URL für Abmeldung) ein.
   
    d.  Geben Sie im Textfeld **Umgebung** den Umgebungsnamen ein.  

    >[!NOTE]
    > Der Wert des Umgebungsattributs wird an den Wert der Mandanten-URL gebunden:  
    >- Wenn der Domänenname der Workday-Mandanten-URL mit „impl“ beginnt (z.B. *https://impl.workday.com/\<tenant\>/login-saml2.htmld*) muss das Attribut für die **Umgebung** auf Implementierung festgelegt werden.  
    >- Wenn der Domänenname anders beginnt, müssen Sie sich mit dem [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) in Verbindung setzen, um den passenden Wert für die **Umgebung** zu erhalten.

12. Führen Sie im Abschnitt **SAML-Setup** die folgenden Schritte aus:
   
    ![SAML-Setup](./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-Setup")
   
    a.  Wählen Sie die Option **SAML-Authentifizierung aktivieren**.
   
    b.  Klicken Sie auf **Zeile hinzufügen**.

13. Führen Sie im Abschnitt „SAML-Identitätsanbieter“ die folgenden Schritte aus:
   
    ![SAML-Identitätsanbieter](./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-Identitätsanbieter")
   
    a. Geben Sie im Textfeld „Name des Identitätsanbieters“ einen Anbieternamen ein (z.B *SPInitiatedSSO*).
   
    b. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert für die **SAML-Entitäts-ID**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
   
    c. Aktivieren Sie **Enable Workday Initiated Logout** (Workday-initiierte Abmeldung ermöglichen).
   
    d. Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert der **Abmelde-URL**, und fügen Sie ihn in das Textfeld **Logout Request URL** (Anforderungs-URL für Abmeldung) ein.

    e. Klicken Sie auf **Identity Provider Public Key Certificate** (Öffentliches Schlüsselzertifikat des Identitätsanbieters), und klicken Sie dann auf **Erstellen**. 

    ![Erstellen](./media/active-directory-saas-workday-tutorial/IC782928.png "Erstellen")

    f. Klicken Sie auf **Öffentlichen x509-Schlüssel erstellen**. 

    ![Erstellen](./media/active-directory-saas-workday-tutorial/IC782929.png "Erstellen")


14. Führen Sie im Abschnitt **Öffentlichen x509-Schlüssel anzeigen** die folgenden Schritte aus: 
   
    ![Öffentlichen x509-Schlüssel anzeigen](./media/active-directory-saas-workday-tutorial/IC782930.png "Öffentlichen x509-Schlüssel anzeigen") 
   
    a. Geben Sie im Textfeld **Name** einen Namen für das Zertifikat ein (z.B. *PPE\_SP*).
   
    b. Geben Sie im Textfeld **Gültig ab** den „Gültig-ab“-Attributwert des Zertifikats ein.
   
    c.  Geben Sie im Textfeld **Gültig bis** den „Gültig-bis“-Attributwert des Zertifikats ein.
   
    > [!NOTE]
    > Sie können die „Gültig ab“- und „Gültig bis“-Daten per Doppelklick aus dem heruntergeladenen Zertifikat abrufen.  Die Datumsangaben sind in der Registerkarte **Details** aufgeführt.
    > 
    >
   
    d.  Öffnen Sie das Base64-codierte Zertifikat in Editor, und kopieren Sie seinen Inhalt.
   
    e.  Fügen Sie in der Textbox **Zertifikat** den Inhalt der Zwischenablage ein.
   
    f.  Klicken Sie auf **OK**.

15. Führen Sie die folgenden Schritte aus: 
   
    ![SSO-Konfiguration](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguratio.png "SSO-Konfiguration")
   
    a.  Aktivieren Sie das **private Schlüsselpaar „X. 509“**.
   
    b.  Geben Sie im Textfeld **Dienstanbieter-ID** Folgendes ein: **http://www.workday.com**
   
    c.  Wählen Sie **SP-initiierte SAML-Authentifizierung aktivieren**.
   
    d.  Kopieren Sie im Azure-Portal im Fenster **Anmeldung konfigurieren** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **IdP SSO Service URL** (Dienst-URL des Identitätsanbieters für einmaliges Anmelden) ein.
   
    e. Wählen Sie **SP-initiierte Authentifizierungsanfrage nicht verkleinern**.
   
    f. Wählen Sie als **Authentication Request Signature Method** (Signaturmethode für Authentifizierungsanfragen) die Option **SHA256** aus. 
   
    ![Signaturmethode für Authentifizierungsanfragen](./media/active-directory-saas-workday-tutorial/WorkdaySSOConfiguration.png "Signaturmethode für Authentifizierungsanfragen") 
   
    g. Klicken Sie auf **OK**. 
   
    ![OK](./media/active-directory-saas-workday-tutorial/IC782933.png "OK")
<CE>

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
>

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workday-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-workday-test-user"></a>Erstellen eines Workday-Testbenutzers

Um einen Testbenutzer in Workday bereitzustellen, müssen Sie sich an das [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) wenden.
Das [Supportteam für den Workday-Client](https://www.workday.com/en-us/partners-services/services/support.html) erstellt den Benutzer für Sie.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Workday gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Workday zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Workday** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workday-tutorial/tutorial_workday_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Tutorial: Konfigurieren von Workday für die automatische Benutzerbereitstellung in lokalem Active Directory und Azure Active Directory](active-directory-saas-workday-inbound-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workday-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workday-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workday-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workday-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workday-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workday-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workday-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workday-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workday-tutorial/tutorial_general_203.png


