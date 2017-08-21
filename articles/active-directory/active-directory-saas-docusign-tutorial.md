---
title: 'Tutorial: Azure Active Directory-Integration mit DocuSign | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und DocuSign konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 29c99fdf39d366df90abc070f7b836320935035c
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Tutorial: Azure Active Directory-Integration mit DocuSign

In diesem Tutorial erfahren Sie, wie Sie DocuSign in Azure Active Directory (Azure AD) integrieren.

Die Integration von DocuSign in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf DocuSign haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei DocuSign anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit DocuSign konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein DocuSign-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von DocuSign aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-docusign-from-the-gallery"></a>Hinzufügen von DocuSign aus dem Katalog
Zum Konfigurieren der Integration von DocuSign in Azure AD müssen Sie DocuSign über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um DocuSign aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld das Wort **DocuSign**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_search.png)

5. Wählen Sie im Ergebnisbereich die Option **DocuSign** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei DocuSign mithilfe eines Testbenutzers namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in DocuSign als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in DocuSign muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in DocuSign zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei DocuSign müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines DocuSign-Testbenutzers](#creating-a-docusign-test-user)**, um eine Entsprechung von Britta Simon in DocuSign zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer DocuSign-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in DocuSign die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **DocuSign** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_samlbase.png)

3. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_certificate.png) 

4. Klicken Sie im Abschnitt **DocuSign-Konfiguration** im Azure-Portal auf **Configure DocuSign** (DocuSign konfigurieren), um das Anmeldungsfenster für die Konfiguration zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_configure.png)

5. Melden Sie sich in einem anderen Webbrowserfenster im **DocuSign-Verwaltungsportal** als Administrator an.

6. Klicken Sie im Navigationsmenü auf der linken Seite auf **Domänen**.
   
    ![Konfigurieren der einmaligen Anmeldung][51]

7. Klicken Sie im rechten Bereich auf **Claim Domain**(Domäne anfordern).
   
    ![Konfigurieren der einmaligen Anmeldung][52]

8. Geben Sie im Dialogfeld **Claim a domain** (Domäne anfordern) im Textfeld **Domänenname** Ihre Unternehmensdomäne ein, und klicken Sie auf **Claim** (Anfordern). Achten Sie darauf, die Domäne zu überprüfen, und vergewissern Sie sich, dass der Status „Aktiv“ lautet.
   
    ![Konfigurieren der einmaligen Anmeldung][53]

9. Klicken Sie im Menü auf der linken Seite auf **Identitätsanbieter**  
   
    ![Konfigurieren der einmaligen Anmeldung][54]
10. Klicken Sie im rechten Bereich auf **Add Identity Provider**(Identitätsanbieter hinzufügen). 
   
    ![Konfigurieren der einmaligen Anmeldung][55]

11. Führen Sie auf der Seite mit den **Identitätsanbietereinstellungen** die folgenden Schritte aus:
   
    ![Konfigurieren der einmaligen Anmeldung][56]

    a. Geben Sie im Textfeld **Name** einen eindeutigen Namen für die Konfiguration ein. Verwenden Sie keine Leerzeichen.

    b. Fügen Sie die **SAML-Entitäts-ID** in das Textfeld **Aussteller des Identitätsanbieters** ein.

    c. Fügen Sie die **SAML-Dienst-URL für einmaliges Anmelden** in das Feld **Anmelde-URL für Identitätsanbieter** ein.

    d. Fügen Sie die **Abmelde-URL** in das Feld **Abmelde-URL für Identitätsanbieter** ein.

    e. Wählen Sie **Sign AuthN Request**(Authentifizierungsanforderung signieren).

    f. Wählen Sie für **Send AuthN request by** (Authentifizierungsanforderung senden per) die Option **POST** aus.

    g. Wählen Sie für **Send logout request by** (Abmeldeanforderung senden per) die Option **GET** aus.

12. Wählen Sie im Abschnitt **Benutzerdefinierte Attributzuordnung** das Feld aus, das Sie dem Azure AD-Anspruch zuordnen möchten. In diesem Beispiel wird der Anforderung **emailaddress** der Wert von **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** zugeordnet. Dies ist der Standard-Anspruchsname aus Azure AD für den E-Mail-Anspruch. 
   
    > [!NOTE]
    > Verwenden Sie den entsprechenden **Benutzerbezeichner** , um den Benutzer aus Azure AD der DocuSign-Benutzerzuordnung zuzuordnen. Wählen Sie das richtige Feld, und geben Sie den entsprechenden Wert basierend auf den Einstellungen Ihrer Organisation ein.
          
    ![Konfigurieren der einmaligen Anmeldung][57]

13. Klicken Sie im Abschnitt **Zertifikat des Identitätsanbieters** auf **Zertifikat hinzufügen**, und laden Sie anschließend das aus dem Azure AD-Portal heruntergeladene Zertifikat hoch.   
   
    ![Konfigurieren der einmaligen Anmeldung][58]

14. Klicken Sie auf **Speichern**.

15. Klicken Sie im Abschnitt **Identitätsanbieter** auf **Aktionen** und dann auf **Endpunkte**.   
   
    ![Konfigurieren der einmaligen Anmeldung][59]
 
16. Führen Sie im **DocuSign-Verwaltungsportal** im Abschnitt **SAML 2.0-Endpunkte anzeigen** die folgenden Schritte aus:
   
    ![Konfigurieren der einmaligen Anmeldung][60]
   
    a. Kopieren Sie die **Service Provider Issuer URL** (Aussteller-URL des Dienstanbieters), und fügen Sie sie dann in das Feld **Bezeichner** im Abschnitt **DocuSign Domain and URLs** (DocuSign-Domäne und -URLs) des Azure-Portal nach folgendem Muster ein: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/login/sp/<uniqueID>`
   
    b. Kopieren Sie die **Service Provider Login URL** (Anmelde-URL des Dienstanbieters), und fügen Sie sie dann in das Feld **Anmelde-URL** im Abschnitt **DocuSign Domain and URLs** (DocuSign-Domäne und -URLs) des Azure-Portal nach folgendem Muster ein: `https://<subdomain>.docusign.com/organization/<uniqueID>/saml2/`

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_url.png)
      
    c.  Klicken Sie unten auf der Seite auf **Schließen**
    
17. Klicken Sie im Azure-Portal auf **Speichern**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-docusign-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-docusign-test-user"></a>Erstellen eines DocuSign-Testbenutzers

Die Anwendung unterstützt die **Just-in-Time-Benutzerbereitstellung** (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf DocuSign gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon DocuSign zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **DocuSign** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-docusign-tutorial/tutorial_docusign_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „DocuSign“ klicken, sollten Sie automatisch bei Ihrer DocuSign-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Konfigurieren der Benutzerbereitstellung](active-directory-saas-docusign-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_29.png
[100]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_203.png


