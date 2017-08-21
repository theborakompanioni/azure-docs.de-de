---
title: 'Tutorial: Azure Active Directory-Integration in Picturepark | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Picturepark konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 31c21cd4-9c00-4cad-9538-a13996dc872f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 1c009aa1fdd3140a4466cf762b6c9687e74ce4c7
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Tutorial: Azure Active Directory-Integration in Picturepark

In diesem Tutorial erfahren Sie, wie Sie Picturepark in Azure Active Directory (Azure AD) integrieren.

Die Integration von Picturepark in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Picturepark hat.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Picturepark anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Picturepark konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Picturepark-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Picturepark aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-picturepark-from-the-gallery"></a>Hinzufügen von Picturepark aus dem Katalog
Zum Konfigurieren der Integration von Picturepark in Azure AD müssen Sie Picturepark aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Picturepark aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Namen **Picturepark** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Picturepark** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Picturepark mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Picturepark als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Picturepark muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Picturepark den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD mit Picturepark müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Picturepark-Testbenutzers](#creating-a-picturepark-test-user)**, um ein Pendant von Britta Simon in Picturepark zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Picturepark-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Picturepark die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Picturepark** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Picturepark** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<companyname>.picturepark.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: 
    
    |  |
    |--|
    | `https://<companyname>.current-picturepark.com`|
    | `https://<companyname>.picturepark.com`|
    | `https://<companyname>.next-picturepark.com`|
    | |

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den Picturepark-Client](https://picturepark.com/about/contact/), um diese Werte zu erhalten. 
 
4. Kopieren Sie im Abschnitt **SAML-Signaturzertifikat** den Wert **FINGERABDRUCK** des Zertifikats.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Picturepark-Konfiguration** auf **Picturepark konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Picturepark-Unternehmenswebsite als Administrator an.

8. Klicken Sie oben auf der Symbolleiste auf **Verwaltungstools**, und klicken Sie dann auf **Verwaltungskonsole**.
   
    ![Verwaltungskonsole](./media/active-directory-saas-picturepark-tutorial/ic795062.png "Verwaltungskonsole")

9. Klicken Sie auf **Authentifizierung** und dann auf **Identitätsanbieter**.
   
    ![Authentifizierung](./media/active-directory-saas-picturepark-tutorial/ic795063.png "Authentifizierung")

10. Führen Sie im Abschnitt **Identitätsanbieterkonfiguration** die folgenden Schritte aus:
   
    ![Identitätsanbieterkonfiguration](./media/active-directory-saas-picturepark-tutorial/ic795064.png "Identitätsanbieterkonfiguration")
   
    a. Klicken Sie auf **Hinzufügen**.
  
    b. Geben Sie einen Namen für die Konfiguration ein.
   
    c. Wählen Sie **Als Standard festlegen**.
   
    d. Fügen Sie in das Textfeld **Aussteller-URI** den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    e. Fügen Sie im Textfeld **Trusted Issuer Thumb Print** (Fingerabdruck eines vertrauenswürdigen Anbieters) den Wert von **Fingerabdruck** ein, den Sie aus dem Abschnitt **SAML-Signaturzertifikat** kopiert haben. 

11. Klicken Sie auf **JoinDefaultUsersGroup**.

12. Um das Attribut **E-Mail-Adresse** im Textfeld **Anspruch** festzulegen, geben Sie `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein, und klicken Sie auf **Speichern**.

      ![Konfiguration](./media/active-directory-saas-picturepark-tutorial/ic795065.png "Konfiguration")

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-picturepark-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-picturepark-test-user"></a>Erstellen eines Picturepark-Testbenutzers

Damit sich Azure AD-Benutzer bei Picturepark anmelden können, müssen sie in Picturepark bereitgestellt werden. Im Fall von Picturepark ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Picturepark** -Mandanten an.

2. Klicken Sie oben in der Symbolleiste auf **Verwaltungstools**, und klicken Sie dann auf **Benutzer**.
   
    ![Benutzer](./media/active-directory-saas-picturepark-tutorial/ic795067.png "Benutzer")

3. Klicken Sie auf der Registerkarte **Benutzerübersicht** auf **Neu**.
   
    ![Benutzerverwaltung](./media/active-directory-saas-picturepark-tutorial/ic795068.png "Benutzerverwaltung")

4. Führen Sie im Dialogfeld **Benutzer erstellen** die folgenden Schritte für einen gültigen Azure Active Directory-Benutzer aus, den Sie bereitstellen möchten:
   
    ![Benutzer erstellen](./media/active-directory-saas-picturepark-tutorial/ic795069.png "Benutzer erstellen")
   
    a. Geben Sie im Textfeld **E-Mail-Adresse** die **E-Mail-Adresse** des Benutzers ein: **BrittaSimon@contoso.com**.  
   
    b. Geben Sie in den Textfeldern **Kennwort** und **Kennwort bestätigen** das **Kennwort** von Britta Simon ein. 
   
    c. Geben Sie im Textfeld **Vorname** den **Vornamen** des Benutzers ein, z.B. **Britta**. 
   
    d. Geben Sie im Textfeld **Nachname** den **Nachnamen** des Benutzers ein, z.B. **Simon**.
   
    e. Geben Sie im Textfeld **Unternehmen** den **Unternehmensnamen** des Benutzers ein. 
   
    f. Wählen Sie im Textfeld **Land** das **Land** des Benutzers aus.
  
    g. Geben Sie im Textfeld **PLZ** die **Postleitzahl** des Orts ein.
   
    h. Geben Sie im Textfeld **Ort** den **Ortsnamen** des Benutzers ein.

    i. Wählen Sie eine **Sprache**aus.
   
    j. Klicken Sie auf **Erstellen**.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Picturepark-Benutzerkonten oder mithilfe der von Picturepark bereitgestellten APIs erstellen.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Picturepark gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Picturepark zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Picturepark** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-picturepark-tutorial/tutorial_picturepark_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Picturepark“ klicken, sollten Sie automatisch bei Ihrer Picturepark-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-picturepark-tutorial/tutorial_general_203.png


