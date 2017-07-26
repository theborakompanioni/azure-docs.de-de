---
title: 'Tutorial: Azure Active Directory-Integration mit Tableau Server | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Server konfigurieren."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: 6b35609d88fbbf649e15863901d521886db2a4d6
ms.contentlocale: de-de
ms.lasthandoff: 06/22/2017


---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Azure Active Directory-Integration mit Tableau Server

In diesem Tutorial erfahren Sie, wie Sie Tableau Server in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tableau Server in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Tableau Server hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Server anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Tableau Server konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Tableau Server-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Tableau Server aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-tableau-server-from-the-gallery"></a>Hinzufügen von Tableau Server aus dem Katalog
Zum Konfigurieren der Integration von Tableau Server in Azure AD müssen Sie Tableau Server aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Tableau Server aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Tableau Server**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_search.png)

5. Wählen Sie im Ergebnisbereich **Tableau Server** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Tableau Server basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tableau Server als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Server muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Tableau Server den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Server müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Tableau Server-Testbenutzers](#creating-a-tableau-server-test-user)** , um eine Entsprechung von Britta Simon in Tableau Server zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Tableau Server-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tableau Server die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Tableau Server** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Tableau Server** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://azure.<domain name>.link`.
    
    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://azure.<domain name>.link`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Die vorangehenden Werte sind keine echten Werte. Später ersetzen Sie die Werte durch die tatsächliche URL und den Bezeichner auf der Konfigurationsseite von Tableau Server. 

4. Die Tableau Server-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel dieser Attributzuordnungen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/3.png)
    
5. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | username | *user.displayname* |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.


6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_400.png)
<CS>
8. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem Tableau Server-Mandanten anmelden.
   
   a. Klicken Sie in der Tableau Server-Konfiguration auf die Registerkarte **SAML** .
  
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Aktivieren Sie das Kontrollkästchen **Use SAML for single sign-on**.
   
   c. „Tableau Server return URL“: Die URL, auf die Tableau Server-Benutzer zugreifen, z.B. http://tableau_server. Es wird nicht empfohlen, http://localhost zu verwenden. Die Verwendung einer URL mit einem nachstehenden Schrägstrich (z.B. http://tableau_server/) wird nicht unterstützt. Kopieren Sie den Wert von **Tableau Server return URL**, und fügen Sie ihn im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Anmelde-URL** ein.
   
   d. „SAML entity ID“: Die Entitäts-ID zur eindeutigen Identifizierung Ihrer Tableau Server-Installation durch den IdP. Sie können in dieses Feld erneut Ihre Tableau Server-URL eingeben, es muss jedoch nicht die Tableau Server-URL verwendet werden. Kopieren Sie die **SAML-Entitäts-ID**, und fügen Sie sie im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Bezeichner** ein.
     
   e. Klicken Sie auf **Export Metadata File**, und öffnen Sie die Datei im Text-Editor. Suchen Sie nach „Assertion Consumer Service URL“ mit HTTP Post und Index 0, und kopieren Sie die URL. Fügen Sie sie nun im Abschnitt **Tableau Server-Domäne und -URLs** in das Azure AD-Textfeld **Antwort-URL** ein.
   
   f. Suchen Sie nach der Datei mit Ihren Verbundmetadaten, die Sie aus dem Azure-Portal heruntergeladen haben, und laden Sie sie in die **SAML Idp metadata file** hoch.
   
   g. Klicken Sie unten auf der Tableau Server Configuration-Seite auf **OK**.
   
    >[!NOTE] 
    >Kunden müssen ein Zertifikat in die Tableau Server SAML-SSO-Konfiguration hochladen, das im SSO-Fluss ignoriert wird.
    >Wenn Sie Hilfe bei der Konfiguration von SAML für Tableau Server benötigen, finden Sie weitere Informationen im Artikel [Konfigurieren von SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-tableau-server-test-user"></a>Erstellen eines Tableau Server-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Tableau Server. Sie müssen alle Benutzer in Tableau Server bereitstellen. 

Der username-Wert des Benutzers muss dem Wert entsprechen, den Sie im benutzerdefinierten Attribut **username** in Azure AD konfiguriert haben. Bei ordnungsgemäßer Zuordnung sollte die Integration funktionieren: [Konfigurieren des einmaligen Anmeldens in Azure AD](#configuring-azure-ad-single-sign-on)

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, wenden Sie sich an den Tableau Server-Administrator in Ihrer Organisation.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Tableau Server gewähren.

![Benutzer zuweisen][200] 

**Zum Zuweisen von Britta Simon zu Tableau Server führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Tableau Server**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Server“ klicken, sollten Sie automatisch bei Ihrer Tableau Server-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png


