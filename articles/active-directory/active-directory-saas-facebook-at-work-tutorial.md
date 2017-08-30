---
title: 'Tutorial: Azure Active Directory-Integration mit Workplace by Facebook | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workplace by Facebook konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 27e62a00832484667117d8718db9f2fc05e2f4e2
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Tutorial: Azure Active Directory-Integration mit Workplace by Facebook

In diesem Tutorial erfahren Sie, wie Sie Workplace by Facebook in Azure Active Directory (Azure AD) integrieren.

Die Integration von Workplace by Facebook in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf Workplace by Facebook steuern.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workplace by Facebook anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software-as-a-Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Für das Konfigurieren der Azure AD-Integration mit Workplace by Facebook benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Workplace by Facebook-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

Beachten Sie beim Testen der Schritte in diesem Tutorial die folgenden Empfehlungen:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Workplace by Facebook aus dem Katalog.
2. Konfigurieren und testen Sie das einmalige Anmelden in Azure AD.

## <a name="add-workplace-by-facebook-from-the-gallery"></a>Hinzufügen von Workplace by Facebook aus dem Katalog
Zum Konfigurieren der Integration von Workplace by Facebook in Azure AD müssen Sie Workplace by Facebook aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

1. Wählen Sie im linken Bereich des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen** > **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Wählen Sie oben im Dialogfeld **Neue Anwendung** aus, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie in das Suchfeld **Workplace by Facebook** ein, und wählen Sie in den Ergebnissen **Workplace by Facebook** aus. Wählen Sie dann **Hinzufügen** aus, um die Anwendung hinzuzufügen.

    ![Workplace by Facebook in der Ergebnisliste](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_search.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD mit Workplace by Facebook mithilfe der Testbenutzerin Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Workplace by Facebook als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Workplace by Facebook muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Workplace by Facebook zuweisen.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer Workplace by Facebook-Anwendung.

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Workplace by Facebook** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Um das einmalige Anmelden zu aktivieren, wählen Sie im Dialogfeld **Einmaliges Anmelden** im Feld **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Workplace by Facebook** die folgenden Schritte aus:

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company subdomain>.facebook.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL im folgenden Format ein: `https://www.facebook.com/company/<scim company id>`.

    > [!NOTE]
    > Diese Werte sind nur Beispiele. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Clientsupportteam für Workplace by Facebook](https://workplace.fb.com/faq/), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. Wählen Sie **Speichern** aus.

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_400.png)

6. Wählen Sie im Abschnitt **Workplace by Facebook-Konfiguration** die Option **Workplace by Facebook konfigurieren** aus, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Konfiguration von Workplace by Facebook](./media/active-directory-saas-facebook-at-work-tutorial/config.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Workplace by Facebook-Unternehmenswebsite als Administrator an.
  
   > [!NOTE] 
   > Im Rahmen des SAML-Authentifizierungsprozesses werden von Workplace ggf. Abfragezeichenfolgen mit einer Größe von bis zu 2,5 KB genutzt, um Parameter an Azure AD zu übergeben.

8. Navigieren Sie im **Unternehmensdashboard** zur Registerkarte **Authentifizierung**.

9. Wählen Sie unter **SAML Authentication** (SAML-Authentifizierung) in der Dropdownliste die Option **SSO Only** (Nur SSO).

10. Geben Sie die Werte, die Sie im Azure-Portal aus dem Abschnitt **Workplace by Facebook-Konfiguration** kopiert haben, in die entsprechenden Felder ein:

    *   Fügen Sie in das Textfeld **SAML URL** den Wert der **Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Fügen Sie in das Textfeld **SAML Issuer URL** (SAML-Aussteller-URL) den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Fügen Sie in **SAML Logout Redirect** (SAML-Abmeldeumleitung) (optional) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
    *   Öffnen Sie das **Base64-codierte Zertifikat**, das Sie aus dem Azure-Portal heruntergeladen haben, in Editor. Kopieren Sie den Inhalt in die Zwischenablage, und fügen Sie ihn in das Textfeld **SAML Certificate** (SAML-Zertifikat) ein.

11. Unter Umständen müssen Sie die Audience-URL, Empfänger-URL und Assertionsconsumerdienst-URL (ACS) eingeben, die im Abschnitt **SAML-Konfiguration** aufgeführt sind.

12. Scrollen Sie im Abschnitt nach unten, und wählen Sie dann **Test SSO** (SSO testen) aus. Ein Popupfenster mit der Azure AD-Anmeldeseite wird angezeigt. Geben Sie zur Authentifizierung wie gewohnt Ihre Anmeldeinformationen ein. Stellen Sie sicher, das die E-Mail-Adresse, die von Azure AD zurückgegeben wird, mit dem Workplace-Konto übereinstimmt, mit dem Sie angemeldet sind.

13. Wenn der Test erfolgreich abgeschlossen wurde, scrollen Sie zum unteren Rand der Seite, und wählen Sie **Speichern** aus.

14. Allen Benutzern von Workplace wird jetzt die Azure AD-Anmeldeseite für die Authentifizierung angezeigt.

Sie können eine SAML-Abmelde-URL konfigurieren, mit der auf die Azure AD-Abmeldeseite verwiesen werden kann. Wenn diese Einstellung aktiviert und konfiguriert wird, werden Benutzer nicht mehr auf die Workplace-Abmeldeseite verwiesen. Stattdessen werden Benutzer an die URL umgeleitet, die unter der Einstellung für die SAML-Abmeldeumleitung hinzugefügt wurde.


> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen. Nachdem Sie diese App aus dem Abschnitt **Active Directory** > **Unternehmensanwendungen** heruntergeladen haben, wählen Sie einfach die Registerkarte **Einmaliges Anmelden** aus, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie unter [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="configure-reauthentication-frequency"></a>Konfigurieren der Häufigkeit der erneuten Authentifizierung

Sie können Workplace so konfigurieren, dass die Aufforderung für die SAML-Überprüfung jeden Tag, alle drei Tage, einmal pro Woche, einmal alle zwei Wochen, einmal im Monat oder nie angezeigt wird.

> [!NOTE] 
>Die Mindesteinstellung für die SAML-Überprüfung von mobilen Anwendungen ist auf einmal pro Woche festgelegt.

Sie können auch eine SAML-Zurücksetzung für alle Benutzer erzwingen. Verwenden Sie hierzu die Schaltfläche **Require SAML authentication for all users now** (SAML-Authentifizierung jetzt für alle Benutzer erzwingen).


### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

1. Wählen Sie im linken Bereich des **Azure-Portals** die Option **Azure Active Directory** aus.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_01.png) 

2. Navigieren Sie zum Anzeigen der Benutzerliste zu **Benutzer und Gruppen**, und wählen Sie dann **Alle Benutzer** aus.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_02.png) 

3. Zum Öffnen des Dialogfelds **Benutzer** wählen Sie **Hinzufügen**.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** folgende Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-facebook-at-work-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie es.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-workplace-by-facebook-test-user"></a>Erstellen eines Workplace by Facebook-Testbenutzers

In diesem Abschnitt wird ein Benutzer mit dem Namen Britta Simon in Workplace by Facebook erstellt. Workplace by Facebook unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt keine Aktion an. Falls ein Benutzer in Workplace by Facebook nicht vorhanden ist, wird ein neuer Benutzer erstellt, wenn Sie versuchen, auf Workplace by Facebook zuzugreifen.

>[!Note]
>Wenden Sie sich an das [Clientsupportteam von Workplace by Facebook](https://workplace.fb.com/faq/), wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure, indem Sie ihr Zugriff auf Workplace by Facebook gewähren.

![Benutzer zuweisen][200] 

1. Öffnen Sie im Azure-Portal die Anwendungsansicht. Navigieren Sie zur Verzeichnisansicht und anschließend zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Workplace by Facebook** aus.

    ![Der Link von Workplace by Facebook in der Anwendungsliste](./media/active-directory-saas-facebook-at-work-tutorial/tutorial_workplacebyfacebook_app.png) 

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“][202] 

4. Wählen Sie **Hinzufügen**. Wählen Sie im Bereich **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Wählen Sie im Dialogfeld **Benutzer und Gruppen** die Option **Auswählen**.

7. Wählen Sie im Dialogfeld **Zuweisung hinzufügen** die Option **Zuweisen** aus.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich.
Weitere Informationen finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen finden Sie unter [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md).
* Lesen Sie [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).
* Erfahren Sie mehr über das [Konfigurieren der Benutzerbereitstellung](active-directory-saas-facebook-at-work-provisioning-tutorial.md).



<!--Image references-->

[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_203.png


