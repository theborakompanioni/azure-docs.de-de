---
title: 'Tutorial: Azure Active Directory-Integration mit SpringCM | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SpringCM konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: edfd06a06c730597fee4569ca1ce29092b45244a
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Tutorial: Azure Active Directory-Integration mit SpringCM

In diesem Tutorial erfahren Sie, wie Sie SpringCM in Azure Active Directory (Azure AD) integrieren.

Die Integration von SpringCM in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SpringCM hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SpringCM anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SpringCM konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SpringCM-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SpringCM aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-springcm-from-the-gallery"></a>Hinzufügen von SpringCM aus dem Katalog
Zum Konfigurieren der Integration von SpringCM in Azure AD müssen Sie SpringCM aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um SpringCM aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **SpringCM** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_search.png)

5. Wählen Sie im Ergebnisbereich die Option **SpringCM** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SpringCM basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SpringCM als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SpringCM muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SpringCM den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SpringCM müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SpringCM-Testbenutzers](#creating-a-springcm-test-user)**, um eine Entsprechung von Britta Simon in SpringCM zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SpringCM-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SpringCM die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SpringCM** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für SpringCM** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=<identifier>`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Clientsupportteam von SpringCM](https://knowledge.springcm.com/support), um diesen Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Rohdaten)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **SpringCM-Konfiguration** auf **SpringCM konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_configure.png)   

7. Melden Sie sich in einem anderen Webbrowserfenster bei der **SpringCM** -Unternehmenswebsite als Administrator an.

8. Klicken Sie im Menü oben auf **GEHE ZU**, anschließend auf **Voreinstellungen** und dann im Abschnitt **Kontovoreinstellungen** auf **SAML-SSO**.
   
    ![SAML SSO](./media/active-directory-saas-spring-cm-tutorial/ic797051.png "SAML SSO")

9. Führen Sie im Abschnitt „Identitätsanbieterkonfiguration“ die folgenden Schritte aus:
   
    ![Identitätsanbieterkonfiguration](./media/active-directory-saas-spring-cm-tutorial/ic797052.png "Identitätsanbieterkonfiguration")
    
    a. Zum Hochladen Ihres heruntergeladenen Azure Active Directory-Zertifikats klicken Sie auf **Ausstellerzertifikat auswählen** oder auf **Ausstellerzertifikat ändern**.
    
    b. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Issuer** (Aussteller) ein.
    
    c. Fügen Sie den Wert der **SAML-Dienst-URL für einmaliges Anmelden**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld **Service Provider (SP) Initiated Endpoint** (Vom Dienstanbieter initiierter Endpunkt) ein.
            
    d. Wählen Sie für **SAML Enabled** (SAML aktiviert) die Option **Enable** (Aktivieren) aus.

    e. Klicken Sie auf **Speichern**.
 
> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-spring-cm-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-springcm-test-user"></a>Erstellen eines SpringCM-Testbenutzers

Damit sich Azure Active Directory-Benutzer bei SpringCM anmelden können, müssen sie in SpringCM bereitgestellt werden. Im Fall von SpringCM ist die Bereitstellung eine manuelle Aufgabe.

>[!NOTE]
>Weitere Informationen finden Sie unter [Create and Edit a SpringCM User](http://knowledge.springcm.com/create-and-edit-a-springcm-user) (Erstellen und Bearbeiten eines SpringCM-Benutzers). 

**Führen Sie zum Bereitstellen eines Benutzerkontos in SpringCM die folgenden Schritte aus:**

1. Melden Sie sich bei der **SpringCM** -Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **GOTO** (GEHE ZU) und dann auf **ADDRESS BOOK** (ADRESSBUCH).
   
    ![Benutzer erstellen](./media/active-directory-saas-spring-cm-tutorial/ic797054.png "Benutzer erstellen")

3. Klicken Sie auf **Benutzer erstellen**.

4. Wählen Sie eine **Benutzerrolle**aus.

5. Wählen Sie **Aktivierungs-E-Mail senden**.

6. Geben Sie die erforderlichen Angaben zu Vorname, Nachname und E-Mail-Adresse eines gültigen Azure Active Directory-Kontos, das Sie bereitstellen möchten, in die entsprechenden Textfelder ein.

7. Fügen Sie den Benutzer zu einer **Sicherheitsgruppe**hinzu.

8. Klicken Sie auf **Speichern**.

  >[!NOTE]
  >Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von SpringCM-Benutzerkonten oder mithilfe der von SpringCM bereitgestellten APIs erstellen.  
  > 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf SpringCM gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SpringCM zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SpringCM** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-spring-cm-tutorial/tutorial_springcm_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.
 
Wenn Sie im Zugriffsbereich auf die Kachel „SpringCM“ klicken, sollten Sie automatisch bei Ihrer SpringCM-Anwendung angemeldet.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-spring-cm-tutorial/tutorial_general_203.png


