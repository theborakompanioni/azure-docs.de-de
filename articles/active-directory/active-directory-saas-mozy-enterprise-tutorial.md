---
title: 'Tutorial: Azure Active Directory-Integration mit Mozy Enterprise | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Mozy Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 489b5e62-85c2-45c9-8766-326632d48114
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: ac73aadcb8205f24f9d2dbce5af76f53bbcb9753
ms.contentlocale: de-de
ms.lasthandoff: 07/04/2017


---
# <a name="tutorial-azure-active-directory-integration-with-mozy-enterprise"></a>Tutorial: Azure Active Directory-Integration mit Mozy Enterprise

In diesem Tutorial erfahren Sie, wie Sie Mozy Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von Mozy Enterprise in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Mozy Enterprise hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Mozy Enterprise anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Mozy Enterprise konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Mozy Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Mozy Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-mozy-enterprise-from-the-gallery"></a>Hinzufügen von Mozy Enterprise aus dem Katalog
Zum Konfigurieren der Integration von Mozy Enterprise in Azure AD müssen Sie Mozy Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Mozy Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **Mozy Enterprise** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Mozy Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Mozy Enterprise basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Mozy Enterprise als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Mozy Enterprise muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Mozy Enterprise den Wert des **Benutzernamens** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Mozy Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Mozy Enterprise-Testbenutzers](#creating-a-mozy-enterprise-test-user)**, um eine Entsprechung von Britta Simon in Mozy Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Mozy Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Mozy Enterprise die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Mozy Enterprise** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Mozy Enterprise** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_url.png)

    Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<tenantname>.Mozyenterprise.com`.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Kundensupportteam von Mozy Enterprise](http://support.mozy.com/), um diesen Wert zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_400.png)

6. Klicken Sie im Abschnitt **Mozy Enterprise-Konfiguration** auf **Mozy Enterprise konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_configure.png) 

7. Melden Sie sich in einem anderen Webbrowserfenster bei der Mozy Enterprise-Unternehmenswebsite als Administrator an.

8. Klicken Sie im Abschnitt **Konfiguration** auf **Authentication Policy** (Authentifizierungsrichtlinie).
   
   ![Authentifizierungsrichtlinie](./media/active-directory-saas-mozy-enterprise-tutorial/ic777314.png "Authentifizierungsrichtlinie")

9. Führen Sie im Abschnitt **Authentifizierungsrichtlinie** die folgenden Schritte aus:
   
   ![Authentifizierungsrichtlinie](./media/active-directory-saas-mozy-enterprise-tutorial/ic777315.png "Authentifizierungsrichtlinie")
   
   a. Wählen Sie für **Directory Service** (Verzeichnisdienst) die Option **Anbieter** aus.
   
   b. Wählen Sie **LDAP-Push verwenden**aus.
   
   c. Klicken Sie auf die Registerkarte **SAML-Authentifizierung** .
   
   d. Fügen Sie den Wert der **URL für den SAML-SSO-Dienst**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld für die **Authentication URL** (Authentifizierungs-URL) ein.
   
   e. Fügen Sie den Wert der **SAML-Entitäts-ID**, den Sie aus dem Azure-Portal kopiert haben, in das Textfeld für den **SAML-Endpunkt** ein.
   
   f. Öffnen Sie das heruntergeladene Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie anschließend das gesamte Zertifikat in das Textfeld **SAML-Zertifikat** ein.
   
   g. Wählen Sie **SSO für Administratoren zur Anmeldung mit den Netzwerkanmeldeinformationen aktivieren**aus.
   
   h. Klicken Sie auf **Änderungen speichern**.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-mozy-enterprise-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-mozy-enterprise-test-user"></a>Erstellen eines Mozy Enterprise-Testbenutzers

Damit sich Azure AD-Benutzer bei Mozy Enterprise anmelden können, müssen sie in Mozy Enterprise bereitgestellt werden. Im Fall von Mozy Enterprise ist die Bereitstellung eine manuelle Aufgabe.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Mozy Enterprise-Benutzerkonten oder mithilfe der von Mozy Enterprise bereitgestellten APIs erstellen.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Mozy Enterprise** -Mandanten an.

2. Klicken Sie auf **Benutzer** und dann auf **Add New User** (Neuen Benutzer hinzufügen).
   
   ![Benutzer](./media/active-directory-saas-mozy-enterprise-tutorial/ic777317.png "Benutzer")
   
   >[!NOTE]
   >Die Option **Add New User** wird nur angezeigt, wenn **Mozy** unter **Authentication Policy** als Anbieter ausgewählt ist. Wenn die SAML-Authentifizierung konfiguriert ist, werden die Benutzer automatisch bei ihrer ersten Anmeldung mittels SSO hinzugefügt.
    
3. Führen Sie im Dialogfeld „Neuer Benutzer“ die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-mozy-enterprise-tutorial/ic777318.png "Benutzer hinzufügen")
   
   a. Wählen Sie in der Liste **Gruppe auswählen** eine Gruppe aus.
   
   b. Wählen Sie in der Liste **Benutzertyp** einen Typ aus.
   
   c. Geben Sie im Textfeld **Benutzername** den Namen des Azure AD-Benutzers ein.
   
   d. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Azure AD-Benutzers ein.
   
   e. Wählen Sie **E-Mail mit Anweisungen an Benutzer senden**aus.
   
   f. Klicken Sie auf **Benutzer hinzufügen**.

     >[!NOTE]
     > Nach dem Erstellen des Benutzers erhält der Azure AD-Benutzer eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Mozy Enterprise gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Mozy Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Mozy Enterprise** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_mozyenterprise_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Mozy Enterprise“ klicken, sollte die Anmeldeseite der Mozy Enterprise-Anwendung angezeigt werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mozy-enterprise-tutorial/tutorial_general_203.png


