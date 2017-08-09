---
title: 'Tutorial: Azure Active Directory-Integration mit Work.com | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Work.com konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 7cfec8e9ac12d43095483696a15c0580776d3114
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a>Tutorial: Azure Active Directory-Integration mit Work.com

In diesem Tutorial erfahren Sie, wie Sie Work.com in Azure Active Directory (Azure AD) integrieren.

Die Integration von Work.com in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Work.com hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Work.com anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Work.com konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Work.com-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Work.com aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

## <a name="add-workcom-from-the-gallery"></a>Hinzufügen von Work.com aus dem Katalog
Zum Konfigurieren der Integration von Work.com in Azure AD müssen Sie Work.com aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Work.com aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Work.com** ein, wählen Sie im Ergebnisbereich **Work.com** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Hinzufügen aus dem Katalog](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Work.com basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Work.com als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Work.com muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Work.com den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Work.com müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Work.com-Testbenutzers](#create-a-workcom-test-user)**, um eine Entsprechung von Britta Simon in Work.com zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Work.com-Anwendung.

>[!NOTE]
>Um einmaliges Anmelden zu konfigurieren, müssen Sie einen benutzerdefinierten Domänennamen von Work.com einrichten. Sie müssen mindestens einen Domänennamen definieren, testen und dann in Ihrer gesamten Organisation bereitstellen.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Work.com die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Work.com** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![SAML-basierte Anmeldung](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für Work.com** aus:

    ![Abschnitt „Domäne und URLs für Work.com“](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `http://<companyname>.my.salesforce.com`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von Work.com](https://help.salesforce.com/articleView?id=000159855&type=3), um diesen Wert zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Abschnitt für SAML-Signaturzertifikat](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Save“](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Work.com-Konfiguration** auf **Work.com konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Work.com-Konfigurationsabschnitt](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. Melden Sie sich bei Ihrem Work.com-Mandanten als Administrator an.

8. Wechseln Sie zu **Setup**.
   
    ![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")

9. Klicken Sie im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
   
    ![Meine Domäne](./media/active-directory-saas-work-com-tutorial/ic767825.png "Meine Domäne")

10. Um zu überprüfen, ob Ihre Domäne richtig eingerichtet wurde, stellen Sie sicher, dass sich die Domäne in **Schritt 4 – bereitgestellt für Benutzer** befindet, und überprüfen Sie die **Domäneneinstellungen**.
   
    ![Domäne für Benutzer bereitgestellt](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domäne für Benutzer bereitgestellt")

11. Melden Sie sich bei Ihrem Work.com-Mandanten an.

12. Wechseln Sie zu **Setup**.
    
    ![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")

13. Erweitern Sie das Menü **Sicherheitskontrollen**, und klicken Sie anschließend auf **Single Sign-On Settings** (Einstellungen für einmaliges Anmelden).
    
    ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-work-com-tutorial/ic794113.png "Einstellungen für einmaliges Anmelden")

14. Führen Sie auf der Dialogfeldseite **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
    
    ![SAML aktiviert](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML aktiviert")
    
    a. Wählen Sie **SAML aktiviert**aus.
    
    b. Klicken Sie auf **Neu**.

15. Führen Sie im Abschnitt **Einstellungen für einmalige Anmelden für SAML** die folgenden Schritte aus:
    
    ![SAML-Einstellungen für einmaliges Anmelden](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML-Einstellungen für einmaliges Anmelden")
    
    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein.  
       
    > [!NOTE]
    > Bei Eingabe eines Werts für **Name** wird automatisch das Textfeld **API-Name** gefüllt.
    
    b. Fügen Sie in das Textfeld **Issuer** (Aussteller) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    c. Klicken Sie auf **Browse** (Durchsuchen), um das aus dem Azure-Portal heruntergeladene Zertifikat hochzuladen.
    
    d. Geben Sie `https://salesforce-work.com` in das Textfeld **Entity Id** (Entitäts-ID) ein.
    
    e. Wählen Sie für **SAML Identity Type** (SAML-Identitätstyp) die Option **Assertion contains the Federation ID from the User object** (Assertion enthält die Verbund-ID aus dem Benutzerobjekt) aus.
    
    f. Wählen Sie für **SAML Identity Location** (Speicherort der SAML-Identität) die Option **Identity is in the NameIdentifier element of the Subject statement** (Identität ist im NameIdentifier-Element der Subject-Anweisung enthalten) aus.
    
    g. Fügen Sie in das Textfeld für die **Identity Provider Login URL** (Anmelde-URL des Identitätsanbieters) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.

    h. Fügen Sie in das Textfeld für die **Identity Provider Logout URL** (Abmelde-URL des Identitätsanbieters) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
    
    i. Wählen Sie für **Service Provider Initiated Request Binding** (Vom Dienstanbieter initiierte Anforderungsbindung) die Option **HTTP Post** aus.
    
    j. Klicken Sie auf **Speichern**.

16. Klicken Sie im linken Navigationsbereich des klassischen Work.com-Portals auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
    
    ![Meine Domäne](./media/active-directory-saas-work-com-tutorial/ic794115.png "Meine Domäne")

17. Klicken Sie auf der Seite **Meine Domäne** im Abschnitt **Anmeldeseitenbranding** auf **Bearbeiten**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Anmeldeseitenbranding")

14. Im Abschnitt **Authentifizierungsdienst** der Seite **Anmeldeseitenbranding** wird der Name Ihrer **SAML-SSO-Einstellungen** angezeigt. Wählen Sie den Namen aus, und klicken Sie auf **Speichern**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Anmeldeseitenbranding")

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Benutzer und Gruppen -> Alle Benutzer](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Hinzufügen](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeldseite „Benutzer“](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-workcom-test-user"></a>Erstellen eines Work.com-Testbenutzers
Damit sich Azure Active Directory-Benutzer anmelden können, müssen sie in Work.com bereitgestellt werden. Im Fall von Work.com ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Melden Sie sich bei der Work.com-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Setup**.
   
    ![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")
3. Navigieren Sie zu **Benutzer verwalten \> Benutzer**.
   
    ![Benutzer verwalten](./media/active-directory-saas-work-com-tutorial/IC784369.png "Benutzer verwalten")

4. Klicken Sie auf **Neuer Benutzer**.
   
    ![Alle Benutzer](./media/active-directory-saas-work-com-tutorial/IC794117.png "Alle Benutzer")

5. Führen Sie im Abschnitt „User Edit“ (Benutzer bearbeiten) die folgenden Schritte aus, und fügen Sie die Attribute eines gültigen Azure AD-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein:
   
    ![Benutzer bearbeiten](./media/active-directory-saas-work-com-tutorial/ic794118.png "Benutzer bearbeiten")
   
    a. Geben Sie im Textfeld **First Name** den **Vornamen** des Benutzers ein, z.B. **Britta**.
    
    b. Geben Sie im Textfeld **Last Name** den **Nachnamen** des Benutzers ein, z.B. **Simon**.
    
    c. Geben Sie im Textfeld **Alias** den **Namen** des Benutzers ein, z.B. **BrittaS**.
    
    d. Geben Sie im Textfeld **Email** die **E-Mail-Adresse** des Benutzers ein, z.B. **Brittasimon@contoso.com**.
    
    e. Geben Sie im Textfeld **User Name** den Benutzernamen des Benutzers ein, z.B. **Brittasimon@contoso.com**.
    
    f. Geben Sie im Textfeld **Nick Name** den **Spitznamen** der Benutzerin **Simon** ein.
    
    g. Wählen Sie **Rolle**, **Benutzerlizenz** und **Profil** aus.
    
    h. Klicken Sie auf **Speichern**.  
      
    > [!NOTE]
    > Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Work.com gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Work.com zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Work.com** aus.

    ![Work.com in der App Liste](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Work.com“ klicken, sollten Sie automatisch bei Ihrer Work.com-Anwendung angemeldet.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png


