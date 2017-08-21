---
title: 'Tutorial: Azure Active Directory-Integration mit Workplace by Facebook | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workplace by Facebook konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Tutorial: Azure Active Directory-Integration mit Workplace by Facebook

In diesem Tutorial erfahren Sie, wie Sie Workplace by Facebook in Azure Active Directory (Azure AD) integrieren.

Die Integration von Workplace by Facebook in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf Workplace by Facebook steuern.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workplace by Facebook anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Workplace by Facebook konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Workplace by Facebook-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Workplace by Facebook aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Hinzufügen von Workplace by Facebook aus dem Katalog
Zum Konfigurieren der Integration von Workplace by Facebook in Azure AD müssen Sie Workplace by Facebook aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Workplace by Facebook aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **Workplace by Facebook** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Workplace by Facebook** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Workplace by Facebook mithilfe einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Workplace by Facebook als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Workplace by Facebook muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Workplace by Facebook zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Workplace by Facebook müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Konfigurieren der Häufigkeit der erneuten Authentifizierung](#configuring-reauthentication-frequency)**, um für Workplace die Anzeige einer SAML-Überprüfung einzurichten.
3. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Workplace by Facebook-Testbenutzers](#creating-a-workplace-by-facebook-test-user)**, um eine Entsprechung von Britta Simon in Workplace by Facebook zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testing Single Sign-On](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer Workplace by Facebook-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Workplace by Facebook die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Workplace by Facebook** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Workplace by Facebook** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<instancename>.facebook.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://www.facebook.com/company/<instancename>`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von Workplace by Facebook](https://workplace.fb.com/faq/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Workplace by Facebook-Konfiguration** auf **Workplace by Facebook konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Workplace by Facebook-Unternehmenswebsite als Administrator an.
  
   > [!NOTE] 
   > Im Rahmen des SAML-Authentifizierungsprozesses werden von Workplace ggf. Abfragezeichenfolgen mit einer Größe von bis zu 2,5 KB genutzt, um Parameter an Azure AD zu übergeben.

8. Navigieren Sie im **Unternehmensdashboard** zur Registerkarte **Authentifizierung**.

9. Wählen Sie unter **SAML Authentication** (SAML-Authentifizierung) in der Dropdownliste die Option **SSO Only** (Nur SSO).

10. Geben Sie die Werte, die Sie im Azure-Portal aus dem Abschnitt **Workplace by Facebook-Konfiguration** kopiert haben, in die entsprechenden Felder ein:

    *   Fügen Sie in das Textfeld **SAML-URL** den Wert der **Dienst-URL für einmalige Anmeldung** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Fügen Sie in das Textfeld **SAML-Aussteller-URL** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Fügen Sie in **SAML Logout Redirect** (SAML-Abmeldeumleitung) (optional) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Öffnen Sie das **Base64-codierte Zertifikat** im Editor, das Sie aus dem Azure-Portal heruntergeladen haben, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **SAML-Zertifikat** ein.

11. Unter Umständen müssen Sie die Audience-URL, Empfänger-URL und Assertionsverbraucherdienst-URL (ACS) eingeben, die im Abschnitt **SAML-Konfiguration** aufgeführt sind.

12. Scrollen Sie im Abschnitt nach unten, und klicken Sie auf die Schaltfläche **Test SSO** (SSO testen). Ein Popupfenster mit der Azure AD-Anmeldeseite wird angezeigt. Geben Sie Ihre Anmeldeinformationen ein, wie Sie es von der Authentifizierung kennen. 

    **Problembehandlung:** Stellen Sie sicher, das die E-Mail-Adresse, die von Azure AD zurückgegeben wird, mit dem Workplace-Konto übereinstimmt, mit dem Sie angemeldet sind.

13. Nachdem der Test erfolgreich abgeschlossen wurde, können Sie an das Ende der Seite scrollen und auf die Schaltfläche **Speichern** klicken.

14. Allen Benutzern von Workplace wird jetzt die Azure AD-Anmeldeseite für die Authentifizierung angezeigt.

15. **SAML Logout Redirect (optional)** - (SAML-Abmeldeumleitung (optional)) 

    Optional können Sie angeben, dass Sie eine SAML-Abmelde-URL konfigurieren möchten, mit der auf die Azure AD-Abmeldeseite verwiesen werden kann. Wenn diese Einstellung aktiviert und konfiguriert wird, werden Benutzer nicht mehr auf die Workplace-Abmeldeseite verwiesen. Stattdessen werden Benutzer an die URL umgeleitet, die unter der Einstellung für die SAML-Abmeldeumleitung hinzugefügt wurde.


> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configuring-reauthentication-frequency"></a>Konfigurieren der Häufigkeit der erneuten Authentifizierung

Sie können Workplace so konfigurieren, dass die Aufforderung für die SAML-Überprüfung jeden Tag, alle drei Tage, einmal pro Woche, einmal alle zwei Wochen, einmal im Monat oder nie angezeigt wird.

> [!NOTE] 
>Die Mindesteinstellung für die SAML-Überprüfung von mobilen Anwendungen ist auf einmal pro Woche festgelegt.

Sie können auch für alle Benutzer eine SAML-Zurücksetzung erzwingen, indem Sie die Schaltfläche „Require SAML authentication for all users now“ (Jetzt SAML-Authentifizierung für alle Benutzer erzwingen) verwenden.


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Create**.
 
### <a name="creating-a-workplace-by-facebook-test-user"></a>Erstellen eines Workplace by Facebook-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Workplace by Facebook erstellt. Workplace by Facebook unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt keine Aktion an. Falls ein Benutzer in Workplace by Facebook nicht vorhanden ist, wird ein neuer Benutzer erstellt, wenn Sie versuchen, auf Workplace by Facebook zuzugreifen.

>[!Note]
>Wenden Sie sich an das [Kundensupportteam von Workplace by Facebook](https://workplace.fb.com/faq/), wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Workplace by Facebook gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Workplace by Facebook zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Workplace by Facebook** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren der Benutzerbereitstellung](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png


