---
title: 'Tutorial: Azure Active Directory-Integration mit Help Scout | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Help Scout konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0aad9910-0bc1-4394-9f73-267cf39973ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: 84cee39c28a0f7e6b9878441e504131795673020
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="tutorial-azure-active-directory-integration-with-help-scout"></a>Tutorial: Azure Active Directory-Integration mit Help Scout

In diesem Tutorial erfahren Sie, wie Sie Help Scout in Azure Active Directory (Azure AD) integrieren.

Die Integration von Help Scout in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Help Scout hat.
- Sie können Ihre Benutzer mithilfe des einmaligen Anmeldens und einem Azure AD-Benutzerkonto automatisch bei Help Scout anmelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Help Scout einzurichten, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Help Scout-Abonnement, in dem einmaliges Anmelden aktiviert ist 

> [!NOTE]
> Sie sollten die Schritte in diesem Tutorial nicht in einer Produktionsumgebung testen.

Empfehlungen zum Testen der Schritte in diesem Tutorial:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Fügen Sie Help Scout aus dem Katalog hinzu.
2. Richten Sie einmaliges Anmelden in Azure AD ein, und testen Sie es.

## <a name="add-help-scout-from-the-gallery"></a>Hinzufügen von Help Scout aus dem Katalog
Fügen Sie Help Scout zum Einrichten der Integration in Azure AD aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzu.

So fügen Sie Help Scout aus dem Katalog hinzu:

1. Wählen Sie im linken Menü des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Die Seite „Unternehmensanwendungen“][2]
    
3. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Help Scout** ein. Wählen Sie in den Suchergebnissen **Help Scout**, und wählen Sie dann **Hinzufügen**.

    ![Help Scout in der Ergebnisliste](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_addfromgallery.png)

## <a name="set-up-and-test-azure-ad-single-sign-on"></a>Einrichten des einmaligen Anmeldens in Azure AD und Testen

In diesem Abschnitt richten Sie das einmalige Anmelden von Azure AD bei Help Scout mithilfe einer Testbenutzerin namens *Britta Simon* ein und testen es.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Benutzer in Help Scout ist. Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Help Scout muss eine Linkbeziehung eingerichtet werden.

Um die Linkbeziehung herzustellen, weisen Sie in Help Scout den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Help Scout die folgenden Schritte aus:

1. [Einrichten des einmaligen Anmeldens in Azure AD](#set-up-azure-ad-single-sign-on). Richtet einen Benutzer zur Verwendung dieses Features ein.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user). Testet das einmalige Anmelden in Azure AD mit der Benutzerin Britta Simon.
3. [Erstellen eines Help Scout-Testbenutzers](#create-a-help-scout-test-user). Erstellt ein Pendant zu Britta Simon in Help Scout, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user). Richtet Britta Simon zur Verwendung des einmaligen Anmeldens in Azure AD ein.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on). Überprüft, ob die Konfiguration funktioniert.

### <a name="set-up-azure-ad-single-sign-on"></a>Einrichten des einmaligen Anmeldens in Azure AD

In diesem Abschnitt richten Sie einmaliges Anmelden in Azure AD im Azure-Portal ein. Dann richten Sie einmaliges Anmelden in Ihrer Help Scout-Anwendung ein.

So richten Sie einmaliges Anmelden in Azure AD bei Help Scout ein:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Help Scout** **Einmaliges Anmelden**.
 
    ![Einrichten des Links für einmaliges Anmelden][4]

2. Wählen Sie auf der Seite **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_samlbase.png)

3. Wenn Sie die Anwendung im IDP-initiierten Modus einrichten möchten, führen Sie im Abschnitt **Domäne und URLs für Help Scout** die folgenden Schritte aus:

    1. Geben Sie im Feld **Bezeichner** eine URL mit folgendem Muster ein: `urn:auth0:helpscout:<instancename>`.

    2. Geben Sie im Feld **Antwort-URL** eine URL mit folgendem Muster ein: `https://helpscout.auth0.com/login/callback?connection=<instancename>`.

    ![SSO-Informationen zur Domäne und zu den URLs für Help Scout](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url.png)

4. Wenn Sie die Anwendung im SP-initiierten Modus einrichten möchten, wählen Sie das Kontrollkästchen **Erweiterte URL-Einstellungen anzeigen**, und führen Sie Folgendes aus:

    * Geben Sie im Feld **URL für Anmeldung** eine URL mit folgendem Format ein: `https://secure.helpscout.net/members/login/`.

    ![SSO-Informationen zur Domäne und zu den URLs für Help Scout](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_url1.png)
 
    > [!NOTE] 
    > Die Werte in diesen URLs dienen nur Demonstrationszwecken. Aktualisieren Sie die Werte mit der eigentlichen Bezeichner-URL und Antwort-URL. Wenden Sie sich an das [Supportteam von Help Scout](mailto:help@helpscout.com), um diese Werte zu erhalten. 

5. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_certificate.png) 

6. Wählen Sie **Speichern** aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-helpscout-tutorial/tutorial_general_400.png)
    
7. Zum Einrichten des einmaligen Anmeldens bei Help Scout senden Sie die heruntergeladene Metadaten-XML-Datei an das [Help Scout-Supportteam](mailto:help@helpscout.com). Das Help Scout-Supportteam wendet diese Einstellung an, damit die Verbindung für einmalige SAML-Anmeldung auf beiden Seiten richtig eingerichtet ist.

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen! Nachdem Sie diese App durch Auswahl von **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus. Sie können über den Bereich **Konfiguration** am unteren Rand auf die eingebettete Dokumentation zugreifen. Weitere Informationen finden Sie unter [Verwalten des einmaligen Anmeldens für Unternehmens-Apps]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal eine Testbenutzerin mit dem Namen Britta Simon.

![Erstellen eines Azure AD-Testbenutzers][100]

So erstellen Sie einen Testbenutzer in Azure AD:

1. Wählen Sie im linken Menü des Azure-Portals die Option **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_01.png)

2. Um die Liste der Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** und dann **Alle Benutzer** aus.

    ![„Benutzer und Gruppen“ und dann „Alle Benutzer“ auswählen](./media/active-directory-saas-helpscout-tutorial/create_aaduser_02.png)

3. Um das Dialogfeld **Benutzer** zu öffnen, wählen Sie oben auf der Seite **Alle Benutzer** die Option **Hinzufügen**.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:

    1. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    2. Geben Sie im Feld **Benutzername** die E-Mail-Adresse der Benutzerin Britta Simon ein.

    3. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    4. Klicken Sie auf **Erstellen**.

        ![Dialogfeld „Benutzer“](./media/active-directory-saas-helpscout-tutorial/create_aaduser_04.png)

 
### <a name="create-a-help-scout-test-user"></a>Erstellen eines Help Scout-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in Help Scout. Help Scout unterstützt die standardmäßig aktivierte Just-in-Time-Bereitstellung (JIT).

In diesem Abschnitt müssen Sie keine Aktion oder Aufgabe ausführen. Falls ein Benutzer nicht bereits in Help Scout vorhanden ist, wird beim Versuch, auf Help Scout zuzugreifen, ein neuer Benutzer erstellt.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon die Verwendung des einmaligen Anmeldens von Azure AD, indem Sie ihrem Benutzerkonto Zugriff auf Help Scout gewähren.

![Zuweisen der Benutzerrolle][200] 

So weisen Sie Britta Simon Help Scout zu:

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Help Scout** aus.

    ![Help Scout-Link in der Anwendungsliste](./media/active-directory-saas-helpscout-tutorial/tutorial_helpscout_app.png)  

3. Wählen Sie im linken Menü **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202]

4. Wählen Sie **Hinzufügen**. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie auf der Seite **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Wählen Sie auf der Seite **Benutzer und Gruppen** die Option **Auswählen**.

7. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „Help Scout“ auswählen, sollten Sie automatisch bei Ihrer Help Scout-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-helpscout-tutorial/tutorial_general_203.png


