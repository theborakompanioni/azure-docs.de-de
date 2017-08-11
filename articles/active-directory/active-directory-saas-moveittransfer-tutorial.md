---
title: "Tutorial: Azure Active Directory-Integration mit MOVEit Transfer – Azure AD-Integration| Microsoft-Dokumentation"
description: "Informationen zur Konfiguration des einmaligen Anmeldens zwischen Azure Active Directory und MOVEit Transfer – Azure AD-Integration"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: d35aceb9be2d0ff49f86a00cc84f5deb198d88f0
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Tutorial: Azure Active Directory-Integration mit MOVEit Transfer – Azure AD-Integration

In diesem Tutorial erfahren Sie, wie Sie MOVEit Transfer – Azure AD-Integration mit Azure Active Directory (Azure AD) integrieren.

Die Integration von MOVEit Transfer – Azure AD-Integration in Azure AD bietet folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf MOVEit Transfer – Azure AD-Integration haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei MOVEit Transfer – Azure AD-Integration anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit MOVEit Transfer – Azure AD-Integration konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Abonnement von MOVEit Transfer – Azure AD-Integration

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von MOVEit Transfer – Azure AD-Integration aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Hinzufügen von MOVEit Transfer – Azure AD-Integration aus dem Katalog
Zum Konfigurieren der Integration von MOVEit Transfer – Azure AD-Integration in Azure AD müssen Sie MOVEit Transfer – Azure AD-Integration über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um MOVEit Transfer – Azure AD-Integration über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfenster **MOVEit Transfer – Azure AD-Integration** ein, wählen Sie **MOVEit Transfer – Azure AD-Integration** vom Ergebnisbereich aus, und klicken Sie anschließend auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![MOVEit Transfer – Azure AD-Integration in der Liste der Ergebnisse](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei MOVEit Transfer – Azure AD-Integration basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in MOVEit Transfer – Azure AD-Integration als Gegenpart zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in MOVEit Transfer – Azure AD-Integration muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in MOVEit Transfer – Azure AD-Integration den Wert des **Benutzernamens** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit MOVEit Transfer – Azure AD-Integration müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für MOVEit Transfer – Azure AD-Integration](#create-a-moveit-transfer---azure-ad-integration-test-user)**, um in MOVEit Transfer – Azure AD-Integration einen Gegenpart von Britta Simon zu erhalten, der mit der Darstellung des Benutzers in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren es in Ihrer MOVEit Transfer – Azure AD-Integrationsanwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit MOVEit Transfer – Azure AD-Integration zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **MOVEit Transfer – Azure AD-Integration** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. Führen Sie die folgenden Schritte auf der **Domäne und den URLs von MOVEit Transfer – Azure AD-Integration** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://contoso.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://contoso.com/<tenatid>`

    c. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`    
     
    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch den tatsächlichen Bezeichner, die Antwort-URL und die Anmelde-URL. Sie können sich später im Abschnitt **Service Provider Metadata URL (Metadaten-URL des Dienstanbieters)** auf diese Werte beziehen oder sich an [das Clientsupportteam von MOVEit Transfer – Azure AD-Integration](https://community.ipswitch.com/s/support) wenden, um diese Werte zu erhalten.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. Melden Sie sich bei Ihrem MOVEit Transfer-Mandanten als Administrator an.

7. Klicken Sie im linken Navigationsbereich auf **Einstellungen**.

    ![Abschnitt „Einstellungen“ auf App-Seite](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. Klicken Sie auf den Link **Einmaliges Anmelden** (unter **Sicherheitsrichtlinien > Benutzerauthentifizierung**).

    ![App-seitige Sicherheitsrichtlinien](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. Klicken Sie auf den Metadaten-URL-Link, um das Metadatendokument herunterzuladen.

    ![Metadaten-URL des Dienstanbieters](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * Überprüfen Sie, ob **entityID** mit dem **Bezeichner** im Abschnitt **Domäne und URLs für MOVEit Transfer – Azure AD-Integration** übereinstimmt.
    * Überprüfen Sie, ob die Standort-URL **AssertionConsumerService** mit der **Antwort-URL** im Abschnitt **Domäne und URLs für MOVEit Transfer – Azure AD-Integration** übereinstimmt.
    
    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. Klicken Sie auf die Schaltfläche **Add Identity Provider** (Identitätsanbieter hinzufügen), um einen neuen Verbundidentitätsanbieter hinzuzufügen.

    ![Identitätsanbieter hinzufügen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. Klicken Sie auf **Durchsuchen...**, um die Metadatendatei auszuwählen, die Sie aus dem Azure-Portal heruntergeladen haben, und klicken Sie anschließend auf **Add Identity Provider** (Identitätsanbieter hinzufügen), um die heruntergeladene Datei hochzuladen.

    ![SAML-Identitätsanbieter](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. Wählen Sie auf der Seite **Edit Federated Identity Provider Settings...** (Einstellungen für Verbundidentitätsanbieter bearbeiten...) unter **Aktiviert** die Option **Ja** aus, und klicken Sie anschließend auf **Speichern**.

    ![Einstellungen für den Verbundidentitätsanbieter](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. Führen Sie auf der Seite **Edit Federated Identity Provider User Settings** (Benutzereinstellungen für Verbundidentitätsanbieter) die folgenden Aktionen aus:
    
    ![Bearbeiten der Einstellungen für den Verbundidentitätsanbieter](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. Wählen Sie unter **Anmeldename** die Option **SAML NameID** (SAML-Namens-ID) aus.
    
    b. Wählen Sie als **vollständiger Name** **Andere** aus, und fügen Sie im Textfeld **Attributname** den Wert `http://schemas.microsoft.com/identity/claims/displayname` ein.
    
    c. Wählen Sie als **E-Mail** **Andere** aus, und fügen Sie im Textfeld **Attributname** den Wert `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` ein.
    
    d. Wählen Sie unter **Auto-create account on signon** (Konto bei der Anmeldung automatisch erstellen) die Option **Ja** aus.
    
    e. Klicken Sie auf die Schaltfläche **Save** .

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a>Erstellen eines Testbenutzers für MOVEit Transfer – Azure AD-Integration

In diesem Abschnitt wird in MOVEit Transfer – Azure AD-Integration ein Benutzer namens Britta Simon erstellt. MOVEit Transfer – Azure AD-Integration unterstützt die von Ihnen aktivierte Just-in-Time-Bereitstellung. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist noch kein Benutzer vorhanden, wird beim Aufrufen von MOVEit Transfer – Azure AD-Integration ein neuer Benutzer erstellt.

>[!NOTE]
>Setzen Sie sich mit dem [Supportteam von MOVEit Transfer– Azure AD-Integrations-Client](https://community.ipswitch.com/s/support) in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf MOVEit Transfer – Azure AD-Integration gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu MOVEit Transfer – Azure AD-Integration durchzuführen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **MOVEit Transfer – Azure AD-Integration** aus.

    ![Die Verknüpfung für MOVEit Transfer – Azure AD-Integration in der Liste der Anwendungen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „MOVEit Transfer – Azure AD-Integration“ klicken, sollten Sie automatisch bei Ihrer MOVEit Transfer – Azure AD-Integration-Anwendung angemeldet werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png


