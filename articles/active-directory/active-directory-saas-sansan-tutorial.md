---
title: 'Tutorial: Azure Active Directory-Integration in Sansan | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Sansan konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f653a0f2-c44a-4670-b936-68c136b578ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: e1a9653d5feea910308cefabdbdfe3a6af44bbe4
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Tutorial: Azure Active Directory-Integration in Sansan

In diesem Tutorial erfahren Sie, wie Sie Sansan in Azure Active Directory (Azure AD) integrieren.

Die Integration von Sansan in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Sansan hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Sansan anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in Sansan konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Sansan-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Sansan aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sansan-from-the-gallery"></a>Hinzufügen von Sansan aus dem Katalog
Zum Konfigurieren der Integration von Sansan in Azure AD müssen Sie Sansan aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Sansan aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Sansan**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Sansan** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei Sansan mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Sansan als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Sansan muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Sansan den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Sansan müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Sansan-Testbenutzers](#creating-a-sansan-test-user)**, um ein Pendant von Britta Simon in Sansan zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Sansan-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Sansan die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Sansan** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Sansan** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: 
    
    | Environment | URL |
    |:--- |:--- |
    | PC-Web |`https://ap.sansan.com/v/saml2/<company name>/acs` |
    | Native Mobile App |`https://internal.api.sansan.com/saml2/<company name>/acs` |
    | Einstellungen für mobile Browser |`https://ap.sansan.com/s/saml2/<company name>/acs` |  

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:
    | Environment             | URL |
    | :-- | :-- |
    | PC-Web                  | `https://ap.sansan.com/v/saml2/<company name>`|
    | Native Mobile App       | `https://internal.api.sansan.com/saml2/<company name>` |
    | Einstellungen für mobile Browser | `https://ap.sansan.com/s/saml2/<company name>` |

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den Sansan-Client](https://www.sansan.com/form/contact), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Sansan-Konfiguration** auf **Sansan konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_configure.png) 

7. Zum Konfigurieren des einmaligen Anmeldens bei **Sansan** müssen Sie das heruntergeladene **Zertifikat**, **Abmelde-URL**, **SAML-Entitäts-ID** und **SAML-Dienst-URL für einmaliges Anmelden** an das [Sansan-Supportteam](https://www.sansan.com/form/contact) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

>[!NOTE]
>Die PC-Browsereinstellungen funktionieren auch für die Mobile App und den mobilen Browser mit PC-Web.  

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-sansan-test-user"></a>Erstellen einen Sansan-Testbenutzers

In diesem Abschnitt erstellen Sie in SanSan einen Benutzer namens Britta Simon. Bei der Sansan-Anwendung muss der Benutzer in der Anwendung bereitgestellt werden, bevor das einmalige Anmelden möglich ist. 

>[!NOTE]
>Wenn Sie einen Benutzer manuell oder eine Gruppe von Benutzern erstellen müssen, wenden Sie sich an das [Sansan-Supportteam](https://www.sansan.com/form/contact). 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Sansan gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Sansan zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Sansan**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Sansan“ klicken, sollten Sie automatisch bei Ihrer Sansan-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png


