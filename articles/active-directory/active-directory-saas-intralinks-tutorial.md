---
title: 'Tutorial: Azure Active Directory-Integration mit Intralinks | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Intralinks konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 147f2bf9-166b-402e-adc4-4b19dd336883
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: ee7fd5b88ac806104002ffb41af11bab4fd1b2dc
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="tutorial-azure-active-directory-integration-with-intralinks"></a>Tutorial: Azure Active Directory-Integration mit Intralinks

In diesem Tutorial erfahren Sie, wie Sie Intralinks in Azure Active Directory (Azure AD) integrieren.

Die Integration von Intralinks in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Intralinks hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Intralinks anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Intralinks konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Intralinks-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptbestandteilen:

1. Hinzufügen von Intralinks aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-intralinks-from-the-gallery"></a>Hinzufügen von Intralinks aus dem Katalog
Zum Konfigurieren der Integration von Intralinks in Azure AD müssen Sie Intralinks aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Intralinks aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Intralinks**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Intralinks** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Intralinks basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Intralinks als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Intralinks muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Intralinks den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Intralinks müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Intralinks-Testbenutzers](#creating-an-intralinks-test-user)**, um ein Pendant zu Britta Simon in Intralinks zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Intralinks-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Intralinks die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Intralinks** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Intralinks** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam für den Intralinks-Client](https://www.intralinks.com/contact-1), um diesen Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

6. Zum Konfigurieren des einmaligen Anmeldens bei **Intralinks** müssen Sie die heruntergeladene **XML-Metadatendatei** an das [Intralinks-Supportteam](https://www.intralinks.com/contact-1) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-intralinks-test-user"></a>Erstellen eines Intralinks-Testbenutzers

In diesem Abschnitt erstellen Sie in Intralinks einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Supportteam von Intralinks](https://www.intralinks.com/contact-1), um die Benutzer auf der Intralinks-Plattform hinzufügen zu lassen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf Intralinks gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Intralinks zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Intralinks**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.

### <a name="add-intralinks-via-or-elite-application"></a>Hinzufügen einer Intralinks VIA- oder Elite-Anwendung

Intralinks verwendet die gleiche SSO-Identitätsplattform für alle anderen Intralinks-Anwendungen mit Ausnahme der Anwendung Deal Nexus. Wenn Sie also die Verwendung weiterer Intralinks-Anwendungen planen, müssen Sie zuerst mithilfe des oben beschriebenen Verfahrens das einmalige Anmelden (SSO) für eine primäre Intralinks-Anwendung konfigurieren.

Danach können Sie das unten beschrieben Verfahren verwenden, um weitere Intralinks-Anwendungen zu Ihrem Mandanten hinzuzufügen, die diese primäre Anwendung für das einmalige Anmelden verwenden. 

>[!NOTE]
>Beachten Sie, dass dieses Feature nur für Kunden mit SKU „Azure AD Premium“ verfügbar ist, nicht für Kunden mit SKU „Free“ oder „Basic“.

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]


2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Intralinks**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_search.png)

5. Führen Sie unter **Intralinks App hinzufügen** die folgenden Schritte aus:

    ![Hinzufügen einer Intralinks VIA- oder Elite-Anwendung](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_addapp.png)

    a. Geben Sie im Textfeld **Name** den entsprechenden Namen der Anwendung ein, z.B. **Intralinks Elite**.

    b. Klicken Sie auf die Schaltfläche **Hinzufügen**.

6.  Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Intralinks** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

7. Wählen Sie im Dialogfeld **Einmaliges Anmelden** für **Modus** die Option **Anmeldung über Link** aus.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_linkedsignon.png)

8. Rufen Sie die vom Dienstanbieter initiierte SSO-URL vom [Intralinks-Team](https://www.intralinks.com/contact-1) für die andere Intralinks-Anwendung ab, und geben Sie sie wie unten gezeigt in **Anmelde-URL konfigurieren** ein. 
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_intralinks_customappurl.png)
    
     Geben Sie im Textfeld „Anmelde-URL“ die URL, die von Ihren Benutzern zur Anmeldung bei der Intralinks-Anwendung verwendet wird, nach folgendem Muster ein:
   
    `https://<company name>.Intralinks.com/?PartnerIdpId=https://sts.windows.net/<AzureADTenantID>`

9. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-intralinks-tutorial/tutorial_general_400.png)

10. Weisen Sie die Anwendung Benutzern oder Gruppen zu, wie im Abschnitt **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** beschrieben.

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Intralinks“ klicken, sollten Sie automatisch bei Ihrer Intralinks-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-intralinks-tutorial/tutorial_general_203.png


