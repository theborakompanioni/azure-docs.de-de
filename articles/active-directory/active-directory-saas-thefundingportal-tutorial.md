---
title: 'Tutorial: Azure Active Directory-Integration mit The Funding Portal | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und The Funding Portal konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4663cc8a-976a-4c6c-b3b4-1e5df9b66744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: d0bfc793bb26c551f85706eaec857962a3415e1f
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Tutorial: Azure Active Directory-Integration mit The Funding Portal

In diesem Tutorial erfahren Sie, wie Sie The Funding Portal in Azure Active Directory (Azure AD) integrieren.

Die Integration von The Funding Portal in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf The Funding Portal hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei The Funding Portal anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit The Funding Portal konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein The Funding Portal-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von The Funding Portal aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-the-funding-portal-from-the-gallery"></a>Hinzufügen von The Funding Portal aus dem Katalog
Zum Konfigurieren der Integration von The Funding Portal in Azure AD müssen Sie The Funding Portal aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um The Funding Portal über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **The Funding Portal**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_search.png)

5. Wählen Sie im Ergebnisbereich **The Funding Portal** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei The Funding Portal basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in The Funding Portal als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in The Funding Portal muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in The Funding Portal den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei The Funding Portal müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines The Funding Portal-Testbenutzers](#creating-the-funding-portal-test-user)**, um ein Pendant von Britta Simon in The Funding Portal zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer The Funding Portal-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in The Funding Portal die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **The Funding Portal** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für The Funding Portal** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.regenteducation.net/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<subdomain>.regenteducation.net`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den The Funding Portal-Client](mailto:info@regenteducation.com), um diese Werte zu erhalten. 

4. Die The Funding Portal-Anwendung erwartet, dass die SAML-Assertionen ein Attribut namens „externalId1“ enthalten. Der Wert von „externalId1“ sollte ein bekannte „studentID“ sein. Konfigurieren Sie für diese Anwendung den Anspruch „externalId1“. Sie können die Werte dieser Attribute auf der Registerkarte **Benutzerattribute** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_attribute.png)

5. Konfigurieren Sie das SAML-Tokenattribut im Abschnitt **Benutzerattribute** im Dialogfeld **Einmaliges Anmelden**, wie in der Abbildung gezeigt, und führen Sie die folgenden Schritte aus:

    | Attributname | Attributwert |
    | ------------------- | ---------------- |
    | externalId1 | user.extensionattribute1 |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_attribute_05.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    c. Wählen Sie aus der Liste **Attributwert** das Attribut aus, das Sie für Ihre Implementierung verwenden möchten. Wählen Sie beispielsweise „user.extensionattribute1“ aus, wenn Sie den StudentID-Wert unter „ExtensionAttribute1“ gespeichert haben.
    
    d. Klicken Sie auf **OK**.
 
6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_certificate.png) 

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_400.png)

8. Zum Konfigurieren des einmaligen Anmeldens bei **The Funding Portal** müssen Sie die heruntergeladene **XML-Metadatendatei** an das [Supportteam von The Funding Portal](mailto:info@regenteducation.com) senden. Es führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-the-funding-portal-test-user"></a>Erstellen eines The Funding Portal-Testbenutzers

In diesem Abschnitt erstellen Sie in The Funding Portal einen Benutzer namens Britta Simon. Arbeiten Sie mit dem [Supportteam für The Funding Portal](mailto:info@regenteducation.com) zusammen, um den Testbenutzer hinzuzufügen und SSO zu aktivieren.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf The Funding Portal gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon The Funding Portal zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **The Funding Portal**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „The Funding Portal“ klicken, sollten Sie automatisch bei Ihrer The Funding Portal-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png


