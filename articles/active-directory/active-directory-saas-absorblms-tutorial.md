---
title: 'Tutorial: Azure Active Directory-Integration mit Absorb LMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Absorb LMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ba9f1b3d-a4a0-4ff7-b0e7-428e0ed92142
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: e42c541ce15b2e025c7019e91ae3c6c74483b6c2
ms.lasthandoff: 04/21/2017


---
# <a name="tutorial-azure-active-directory-integration-with-absorb-lms"></a>Tutorial: Azure Active Directory-Integration mit Absorb LMS

In diesem Tutorial erfahren Sie, wie Sie Absorb LMS in Azure Active Directory (Azure AD) integrieren.

Die Integration von Absorb LMS in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Absorb LMS hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Absorb LMS anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zum Integrieren von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Absorb LMS konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Absorb LMS-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Absorb LMS über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-absorb-lms-from-the-gallery"></a>Hinzufügen von Absorb LMS über den Katalog
Zum Konfigurieren der Integration von Absorb LMS in Azure AD müssen Sie Absorb LMS aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Absorb LMS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **Absorb LMS** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Absorb LMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Absorb LMS mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Absorb LMS als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Absorb LMS muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Absorb LMS zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Absorb LMS müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Absorb LMS-Testbenutzers](#creating-an-absorb-lms-test-user)**, um eine Entsprechung von Britta Simon in Nexonia zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Absorb LMS-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Absorb LMS die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Absorb LMS** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Absorb LMS** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url.png)

    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.myabsorb.com/Account/SAML`

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<subdomain>.myabsorb.com/Account/SAML`

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_url2.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.myabsorb.com/`.
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem tatsächlichen Bezeichner, der Antwort-URL und der Anmelde-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Zeichenfolgenwert für Bezeichner, Antwort-URL und Anmelde-URL zu verwenden. Wenden Sie sich an das [Supportteam von Absorb LMS](https://www.absorblms.com/support), um diese Werte zu erhalten. 

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_general_400.png)
    
7. Klicken Sie im Abschnitt **Absorb LMS-Konfiguration** auf **Absorb LMS konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster bei der Absorb LMS-Unternehmenswebsite als Administrator an.

9. Klicken Sie auf der Administratorbenutzeroberfläche auf das **Kontosymbol**. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/1.png)

10. Klicken Sie auf **Portaleinstellungen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/2.png)
    
11.    Klicken Sie auf die Registerkarte **Benutzer** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/3.png)

12. Führen Sie die folgenden Schritte aus, um auf die Konfigurationsfelder für einmaliges Anmelden zuzugreifen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/4.png)

    a. Wählen Sie den geeigneten **Modus** aus.

    > [!NOTE]
    > Modus: Es wird die Initiierung durch IdP und SP unterstützt.

    b. Öffnen Sie das Zertifikat, dass Sie aus dem Azure-Portal heruntergeladen haben, in Editor, entfernen Sie die Tags **---BEGIN CERTIFICATE---** und **---END CERTIFICATE---**, und fügen Sie dann den restlichen Inhalt in das Textfeld **Schlüssel** ein.
    
    c. Wählen Sie in **ID-Eigenschaft** das entsprechende Attribut aus, das Sie als die Benutzer-ID in Azure AD konfiguriert haben (wenn z.B. in Azure AD der Benutzerprinzipalname ausgewählt wurde, wird hier der Benutzername ausgewählt).

    d. Fügen Sie als **Anmelde-URL** den Wert für **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie im Azure-Portal im Fenster **Anmelden konfigurieren** kopiert haben.

    e. Fügen Sie als **Abmelde-URL** den Wert für **Abmelde-URL** ein, den Sie im Azure-Portal im Fenster **Anmelden konfigurieren** kopiert haben.

13. Aktivieren Sie **Only Allow SSO Login** (Nur SSO-Anmeldung zulassen).

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/5.png)

14. Klicken Sie auf **Speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-absorblms-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.

### <a name="creating-an-absorb-lms-test-user"></a>Erstellen eines Absorb LMS-Testbenutzers

Damit sich Azure AD-Benutzer bei Absorb LMS anmelden können, müssen sie in Absorb LMS bereitgestellt werden.  
Bei Absorb LMS ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Absorb LMS-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf die Registerkarte **Benutzer**.

    ![Invite People](./media/active-directory-saas-absorblms-tutorial/absorblms_users.png)

3. Klicken Sie auf der Registerkarte **Benutzer** auf **Benutzer**.

    ![Invite People](./media/active-directory-saas-absorblms-tutorial/absorblms_userssub.png)

4.  Wählen Sie in der Dropdownliste **Neu hinzufügen** die Option **Benutzer** aus.

    ![Invite People](./media/active-directory-saas-absorblms-tutorial/absorblms_createuser.png)

5. Führen Sie auf der Seite **Benutzer hinzufügen** die folgenden Schritte aus:

    ![Invite People](./media/active-directory-saas-absorblms-tutorial/user.png)

    a. Geben Sie im Textfeld **Vorname** den Vornamen ein, z.B. Britta.

    b. Geben Sie im Textfeld **Nachname** den Nachnamen ein, z.B. Simon.
    
    c. Geben Sie im Textfeld **Benutzername** den Benutzernamen ein, z.B. Britta Simon.

    d. Geben Sie im Textfeld **Kennwort** das Kennwort von Britta Simon ein.

    e. Wählen Sie in der Dropdownliste **Kennwort bestätigen** das Bestätigungskennwort von Britta Simon aus.
    
    f. Legen Sie es als **AKTIV** fest.    

6. Klicken Sie auf **Speichern**.
 
### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf Absorb LMS gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Absorb LMS durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Absorb LMS** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-absorblms-tutorial/tutorial_absorblms_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Absorb LMS“ klicken, sollten Sie automatisch bei Ihrer Absorb LMS-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](https://msdn.microsoft.com/library/dn308586).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-absorblms-tutorial/tutorial_general_203.png


