---
title: 'Tutorial: Azure Active Directory-Integration mit Velpic SAML | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Velpic SAML konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 5325f3cca00167e6b7b687509ce43435447ad2f4
ms.lasthandoff: 04/07/2017


---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Tutorial: Azure Active Directory-Integration mit Velpic SAML

In diesem Tutorial erfahren Sie, wie Sie Velpic SAML in Azure Active Directory (Azure AD) integrieren.

Die Integration von Velpic SAML in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Velpic SAML hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Velpic SAML anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Velpic SAML konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Velpic SAML-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Velpic SAML aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-velpic-saml-from-the-gallery"></a>Hinzufügen von Velpic SAML aus dem Katalog
Zum Konfigurieren der Integration von Velpic SAML in Azure AD müssen Sie Velpic SAML aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Velpic SAML aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Velpic SAML** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Velpic SAML** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Velpic SAML basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Velpic SAML als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Velpic SAML muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert des **Benutzernamens** in Velpic SAML zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Velpic SAML müssen Sie die folgenden Schritte durchführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Velpic SAML-Testbenutzers](#creating-a-velpic-saml-test-user)**, um eine Entsprechung von Britta Simon in Velpic SAML zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Velpic SAML-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Velpic SAML die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Velpic SAML** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. Geben Sie die Details im Abschnitt **Domäne und URLs für Velpic SAML** ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert wie folgt ein: `https://<sub-domain>.velpicsaml.net`

    b. Fügen Sie im Textfeld **Bezeichner** den folgenden Wert für **URL für einmaliges Anmelden** ein: `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Beachten Sie, dass die Anmelde-URL vom Velpic SAML-Team bereitgestellt wird und dass der Bezeichnerwert zur Verfügung steht, wenn Sie das SSO-Plug-In auf der Velpic SAML-Seite konfigurieren. Sie müssen diesen Wert von der Velpic SAML-Anwendungsseite kopieren und hier einfügen.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt „Velpic SAML-Konfiguration“ auf „Velpic SAML konfigurieren“, um das Fenster „Anmeldung konfigurieren“ zu öffnen. Kopieren Sie die SAML-Entitäts-ID aus dem Abschnitt mit der Kurzübersicht.

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Velpic SAML-Unternehmenswebsite als Administrator an.

8. Klicken Sie auf die Registerkarte **Verwalten**, und wechseln Sie zum Abschnitt **Integration**. Hier müssen Sie auf die Schaltfläche **Plug-Ins** klicken, um ein neues Plug-In für die Anmeldung zu erstellen.

    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. Klicken Sie auf die Schaltfläche **Plug-In hinzufügen**.
    
    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. Klicken Sie auf der Seite „Plug-In hinzufügen“ auf die Kachel **SAML**.
    
    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. Geben Sie den Namen des neuen SAML-Plug-Ins ein, und klicken Sie auf die Schaltfläche **Hinzufügen**.

    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. Geben Sie die Details wie folgt ein:

    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    a. Geben Sie im Textfeld **Name** den Namen des SAML-Plug-Ins ein.

    b. Fügen Sie im Textfeld **Aussteller-URL** die **SAML-Entitäts-ID** ein, die Sie aus dem Fenster **Anmeldung konfigurieren** im Azure-Portal kopiert haben.

    c. Laden Sie in der **Konfiguration der Anbietermetadaten** die XML-Metadatendatei hoch, die Sie aus dem Azure-Portal heruntergeladen haben.

    d. Sie können auch die SAML-Just-in-Time-Bereitstellung aktivieren, indem Sie das Kontrollkästchen **Neue Benutzer automatisch erstellen** aktivieren. Wenn ein Benutzer nicht in Velpic vorhanden und dieses Flag nicht aktiviert ist, tritt bei der Anmeldung über Azure ein Fehler auf. Wenn das Flag aktiviert ist, wird der Benutzer zum Zeitpunkt der Anmeldung automatisch in Velpic bereitgestellt. 

    e. Kopieren Sie die **SSO-URL** aus dem Textfeld, und fügen Sie sie im Azure-Portal ein.
    
    f. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Erstellen eines Velpic SAML-Testbenutzers

Dieser Schritt ist in der Regel nicht erforderlich, da die Anwendung die Just-in-Time-Benutzerbereitstellung unterstützt. Wenn die automatische Benutzerbereitstellung nicht aktiviert ist, kann die manuelle Erstellung von Benutzern wie unten beschrieben durchgeführt werden.

Melden Sie sich als Administrator bei Ihrer Velpic SAML-Unternehmenswebsite an, und führen Sie die folgenden Schritte aus:
    
1. Klicken Sie auf die Registerkarte „Verwalten“, und wechseln Sie zum Abschnitt „Benutzer“. Klicken Sie dann auf die Schaltfläche „Neu“, um Benutzer hinzuzufügen.

    ![Benutzer hinzufügen](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. Führen Sie im Dialogfeld **Neuen Benutzer erstellen** die folgenden Schritte aus.

    ![user](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    a. Geben Sie im Textfeld **Vorname** den Vornamen von Britta Simon ein.

    b. Geben Sie im Textfeld **Nachname** den Nachnamen von Britta Simon ein.

    c. Geben Sie im Textfeld **Benutzername** den Benutzernamen von Britta Simon ein.

    d. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    e. Die übrigen Informationen sind optional und können nach Bedarf ausgefüllt werden.
    
    f. Klicken Sie auf **SPEICHERN**.    

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens bei Azure, indem Sie ihr Zugriff auf Velpic SAML gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Velpic SAML durchzuführen:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Velpic SAML** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

1. Wenn Sie im Zugriffsbereich auf die Velpic SAML-Kachel klicken, wird die Anmeldeseite der Velpic SAML-Anwendung aufgerufen. Auf der Anmeldeseite wird Ihnen die Schaltfläche **Mit Azure AD anmelden** angezeigt.

    ![Plug-In](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. Klicken Sie auf die Schaltfläche **Mit Azure AD anmelden**, um sich über das Azure AD-Konto bei Velpic anzumelden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png


