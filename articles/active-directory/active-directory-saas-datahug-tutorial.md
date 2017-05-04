---
title: 'Tutorial: Azure Active Directory-Integration mit Datahug | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Datahug konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: e0bfa7620feeb1bad33dd2fe4b32cb237d3ce158
ms.openlocfilehash: ec431dd5ccfa53e4b975e46da247704dd1e15c2c
ms.lasthandoff: 04/21/2017


---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a>Tutorial: Azure Active Directory-Integration mit Datahug

In diesem Tutorial erfahren Sie, wie Sie Datahug in Azure Active Directory (Azure AD) integrieren.

Die Integration von Datahug in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Datahug hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Datahug anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zum Integrieren von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Datahug konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Datahug-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Datahug über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-datahug-from-the-gallery"></a>Hinzufügen von Datahug über den Katalog
Zum Konfigurieren der Integration von Datahug in Azure AD müssen Sie Datahug aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Datahug aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Datahug** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Datahug** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Datahug mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Datahug als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Datahug muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Datahug zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Datahug müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Datahug-Testbenutzers](#creating-a-datahug-test-user)**, um eine Entsprechung von Britta Simon in Nexonia zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Datahug-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Datahug die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Datahug** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Datahug** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://apps.datahug.com/identity/<uniqueID>`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://apps.datahug.com/identity/<uniqueID>/acs`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** die URL folgendermaßen ein: `https://apps.datahug.com/`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge für Bezeichner und Antwort-URL zu verwenden. Wenden Sie sich an das [Supportteam von Datahug](http://datahug.com/about/contact-us/), um diese Werte zu erhalten. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  Aktivieren Sie **Erweiterte Einstellungen für die Zertifikatsignatur**, und führen Sie die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    a. Wählen Sie unter **Signaturoption** die Option **SAML-Assertion signieren** aus.
    
    b. Wählen Sie unter **Signaturalgorithmus** die Option **SHA1** aus.
 
7. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. Klicken Sie im Abschnitt **Datahug-Konfiguration** auf **Datahug konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID** und die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. Zum Konfigurieren der einmaligen Anmeldung auf der **Datahug**-Seite müssen Sie die heruntergeladene **Metadaten-XML-Datei**, die **SAML-Entitäts-ID** und die **SAML Single Sign-On-Dienst-URL** an den [Datahug-Support](http://datahug.com/about/contact-us/) senden. Er richtet die Anwendung ein, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-datahug-test-user"></a>Erstellen eines Datahug-Testbenutzers

Damit sich Azure AD-Benutzer bei Datahug anmelden können, müssen sie in Datahug bereitgestellt werden.  
Bei Datahug ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Datahug-Unternehmenswebsite als Administrator an.

2. Zeigen Sie auf das **Zahnrad** in der rechten oberen Ecke, und klicken Sie auf **Einstellungen**.
   
   ![Mitarbeiter hinzufügen](./media/active-directory-saas-datahug-tutorial/1.png)

3. Wählen Sie **Personen** aus, und klicken Sie auf die Registerkarte **Benutzer hinzufügen**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-datahug-tutorial/2.png)

4. Geben Sie die E-Mail-Adresse der Person ein, für die Sie ein Konto erstellen möchten, und klicken Sie auf **Hinzufügen**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > Sie können eine Registrierungs-E-Mail an den Benutzer senden, indem Sie das Kontrollkästchen zum **Senden einer Begrüßungs-E-Mail** aktivieren.    
    > Wenn Sie ein Konto für Salesforce erstellen, senden Sie keine Begrüßungs-E-Mail.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Datahug gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Datahug durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Datahug** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
Wenn Sie im Zugriffsbereich auf die Kachel „Datahug“ klicken, sollten Sie automatisch bei Ihrer Datahug-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png


