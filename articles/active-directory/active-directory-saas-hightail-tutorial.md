---
title: 'Tutorial: Azure Active Directory-Integration mit Hightail | Microsoft Docs'
description: "Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Hightail konfigurieren."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e15206ac-74b0-46e4-9329-892c7d242ec0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 61fd58063063d69e891d294e627ae40cb878d65b
ms.openlocfilehash: ba55f9b62d274aa3eb91723c62b53f54de0891b5
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Tutorial: Azure Active Directory-Integration in Hightail

In diesem Tutorial erfahren Sie, wie Sie Hightail in Azure Active Directory (Azure AD) integrieren.

Die Integration von Hightail in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Hightail hat
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Hightail anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Hightail konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Hightail-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Hightail aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-hightail-from-the-gallery"></a>Hinzufügen von Hightail aus dem Katalog
Zum Konfigurieren der Integration von Hightail in Azure AD müssen Sie Hightail aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Hightail aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Hightail**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Hightail** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand einer Testbenutzerin namens Britta Simon, wie das einmalige Anmelden von Azure AD in Hightail konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Hightail als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Hightail muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Hightail den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Hightail müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Hightail-Testbenutzers](#creating-a-hightail-test-user)**, um ein Pendant von Britta Simon in Hightail zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Hightail Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Hightail die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Hightail** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Hightail** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url.png)

     Geben Sie im Textfeld **Antwort-URL** folgende URL ein: `https://www.hightail.com/samlLogin?phi_action=app/samlLogin&subAction=handleSamlResponse`

    > [!NOTE] 
    > Der vorangehende Wert ist kein echter Wert. Sie aktualisieren den Wert mit der tatsächlichen Antwort-URL. Dies wird später in diesem Tutorial beschrieben.
 
4. Führen Sie im Abschnitt **Domäne und URLs für Hightail** die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_url1.png)

    a. Klicken Sie auf **Erweiterte URL-Einstellungen anzeigen**.

    b. Geben Sie im Textfeld **Anmelde-URL** die URL ein: `https://www.hightail.com/loginSSO`.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_certificate.png) 

5. Die Hightail-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute auf der Registerkarte **Attribut** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_attribute.png) 

6. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt wird, und führen Sie die folgenden Schritte durch:
    
    | Attributname | Attributwert |
    | ------------------- | -------------------- |
    | Vorname | user.givenname |
    | Nachname | user.surname |
    | E-Mail | user.mail |    
    | UserIdentity | user.mail |
    
    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_officespace_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.

    d. Lassen Sie den **Namespace** leer.
    
    e. Klicken Sie auf **OK**.

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **Hightail-Konfiguration** auf **Hightail konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_configure.png) 

    >[!NOTE] 
    >Bevor Sie das einmalige Anmelden für die Hightail-App konfigurieren, lassen Sie Ihre E-Mail-Domäne über das Hightail-Team zur Whitelist hinzufügen, damit alle Benutzer, die diese Domäne verwenden, das einmalige Anmelden nutzen können.


9. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem Hightail-Mandanten anmelden.
   
    a. Klicken Sie im Menü im oberen Bildschirmbereich auf die Registerkarte **Konto**, und wählen Sie **SAML konfigurieren** aus.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 

    b. Aktivieren Sie das Kontrollkästchen **SAML-Authentifizierung aktivieren**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c. Öffnen Sie das Base64-codierte Zertifikat im Editor, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **SAML-Tokensignaturzertifikat** ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 

    d. Fügen Sie in das Textfeld **SAML-Autorität (Identitätsanbieter)** den Wert der **SAML-Dienst-URL für einmalige Anmeldung** ein, den Sie aus dem Azure-Portal kopiert haben.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Wenn die Anwendung im **IDP-initiierten Modus** konfiguriert werden soll, wählen Sie **IdP-initiierte (Identitätsanbieter) Anmeldung** aus. Wenn der **SP-initiierte Modus** konfiguriert werden soll, wählen Sie **SP-initiierte (Dienstanbieter) Anmeldung** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Kopieren Sie die SAML-Verbraucher-URL für Ihre Instanz, und fügen Sie sie im Azure-Portal in das Textfeld **Antwort-URL** im Abschnitt **Domäne und URLs für Hightail** ein.
    
    g. Klicken Sie auf **Speichern**.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-hightail-test-user"></a>Erstellen eines Hightail-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in Hightail. 

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Hightail unterstützt die Just-In-Time-Benutzerbereitstellung auf Grundlage der benutzerdefinierten Ansprüche. Wenn Sie die benutzerdefinierten Ansprüche wie im Abschnitt **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** oben konfiguriert haben, wird automatisch ein Benutzer in der Anwendung erstellt, wenn dieser noch nicht vorhanden ist. 

>[!NOTE]
>Wenn Sie einen Benutzer manuell erstellen müssen, müssen Sie sich an das [Hightail-Supportteam](mailto:support@hightail.com) wenden. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Hightail gewähren.

![Benutzer zuweisen][200] 

**Zur Zuweisung von Britta Simon zu Hightail führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Hightail**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Hightail“ klicken, sollten Sie automatisch bei Ihrer Hightail-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png


