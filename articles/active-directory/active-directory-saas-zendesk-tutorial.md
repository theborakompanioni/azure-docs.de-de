---
title: 'Tutorial: Azure Active Directory-Integration mit Zendesk | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Zendesk konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 51c06d838c5ed6286dfb99ea25faaaf33bad5f3c
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Tutorial: Azure Active Directory-Integration mit Zendesk

In diesem Tutorial erfahren Sie, wie Sie Zendesk in Azure Active Directory (Azure AD) integrieren.

Die Integration von Zendesk in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Zendesk hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Zendesk anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Zendesk konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Zendesk-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Zendesk aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-zendesk-from-the-gallery"></a>Hinzufügen von Zendesk aus dem Katalog
Zum Konfigurieren der Integration von Zendesk in Azure AD müssen Sie Zendesk aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Zendesk aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Zendesk** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_search.png)

5. Wählen Sie im Ergebnisbereich **Zendesk** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Zendesk mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Zendesk als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Zendesk muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Zendesk zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Zendesk müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Zendesk-Testbenutzers](#creating-a-zendesk-test-user)**, um eine Entsprechung von Britta Simon in Zendesk zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Zendesk-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Zendesk die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Zendesk** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Zendesk** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** den Wert im folgenden Format ein: `https://<subdomain>.zendesk.com`.

    b. Geben Sie im Textfeld **Bezeichner** einen Wert nach folgendem Muster ein: `https://<subdomain>.zendesk.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und Bezeichner-URL. Wenden Sie sich an das [Supportteam von Zendesk](https://support.zendesk.com/hc/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise), um diese Werte zu erhalten. 

4. Zendesk erwartet die SAML-Assertionen in einem bestimmten Format. Es gibt keine vorgeschriebenen SAML-Attribute, Sie können aber optional anhand der folgenden Schritte ein Attribut aus dem Abschnitt **Benutzerattribute** hinzufügen: 

     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes1.png)

    a. Klicken Sie auf das Kontrollkästchen **View and edit all the other attributes** (Andere Attribute anzeigen und bearbeiten).
     
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_attributes2.png)
   
    b. Klicken Sie auf **Add Attribute** (Attribut hinzufügen), um das Dialogfeld **Add Attribute** (Attribut hinzufügen) zu öffnen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_attribute_05.png)

    c. Geben Sie im Textfeld **Name** den Attributnamen ein (z.B. **emailaddress**).
    
    d. Wählen Sie in der Liste **Value** (Wert) den Attributwert (als **user.mail**) aus.
    
    e. Klicken Sie auf **OK**.
 
    > [!NOTE] 
    > Sie verwenden Erweiterungsattribute, um Attribute hinzuzufügen, die standardmäßig nicht in Azure AD verfügbar sind. Klicken Sie auf [User attributes that can be set in SAML](https://support.zendesk.com/hc/en-us/articles/203663676-Using-SAML-for-single-sign-on-Professional-and-Enterprise-) (Benutzerattribute für SAML), um die vollständige Liste der SAML-Attribute abzurufen, die **Zendesk** akzeptiert. 

5. Kopieren Sie im Abschnitt **SAML-Signaturzertifikat** den Wert **FINGERABDRUCK** des Zertifikats.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_certificate.png) 

6. Klicken Sie im Abschnitt **Zendesk-Konfiguration** auf **Zendesk konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Zendesk-Unternehmenswebsite als Administrator an.

8. Klicken Sie auf **Administrator**.

9. Klicken Sie im linken Navigationsbereich auf **Einstellungen** und anschließend auf **Sicherheit**.

10. Führen Sie auf der Seite **Sicherheit** die folgenden Schritte aus: 
   
     ![Sicherheit](./media/active-directory-saas-zendesk-tutorial/ic773089.png "Sicherheit")

    ![Einmaliges Anmelden](./media/active-directory-saas-zendesk-tutorial/ic773090.png "Einmaliges Anmelden")

     a. Klicken Sie auf die Registerkarte **Admin & Agents**.

     b. Wählen Sie **Einmaliges Anmelden (SSO) und SAML** und anschließend **SAML** aus.

     c. Fügen Sie in das Textfeld **SAML SSO URL** (SAML-Anmelde-URL) den Wert der **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben. 

     d. Fügen Sie in das Textfeld **Remote Logout URL** (Remoteabmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
        
     e. Fügen Sie in das Textfeld **Certificate Fingerprint** (Zertifikatsfingerabdruck) den Wert **Fingerabdruck** des Zertifikats ein, das Sie aus dem Azure-Portal kopiert haben.
     
     f. Klicken Sie auf **Speichern**.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-zendesk-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 

### <a name="creating-a-zendesk-test-user"></a>Erstellen eines Zendesk-Testbenutzers

Damit sich Azure AD-Benutzer bei **Zendesk** anmelden können, müssen sie in **Zendesk** bereitgestellt werden.  
Abhängig von der Rolle, die in den Apps zugewiesen wurde, ist dies das erwartete Verhalten:

 1. **Endbenutzer**-Konten werden bei der Anmeldung automatisch bereitgestellt.
 2. **Agent**- und **Admin**-Konten müssen vor dem Anmelden manuell in **Zendesk** bereitgestellt werden.
 
**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Zendesk** -Mandanten an.

2. Wählen Sie die Registerkarte **Kundenliste** .

3. Wählen Sie die Registerkarte **Benutzer** aus, und klicken Sie auf **Hinzufügen**.
   
    ![Benutzer hinzufügen](./media/active-directory-saas-zendesk-tutorial/ic773632.png "Benutzer hinzufügen")
4. Geben Sie die E-Mail-Adresse eines vorhandenen Azure AD-Kontos ein, das Sie bereitstellen möchten, und klicken Sie auf **Speichern**.
   
    ![Neuer Benutzer](./media/active-directory-saas-zendesk-tutorial/ic773633.png "Neuer Benutzer")

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Zendesk-Benutzerkonten oder mithilfe der von Zendesk bereitgestellten APIs erstellen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Zendesk gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Zendesk zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Zendesk** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-zendesk-tutorial/tutorial_zendesk_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Zendesk“ klicken, sollten Sie automatisch bei Ihrer Zendesk-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zendesk-tutorial/tutorial_general_203.png

