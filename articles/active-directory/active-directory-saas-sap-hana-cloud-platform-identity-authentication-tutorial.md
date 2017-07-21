---
title: 'Tutorial: Azure Active Directory-Integration mit SAP HANA Cloud Platform Identity Authentication | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP HANA Cloud Platform Identity Authentication konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9b3f305e71502119e5dee6592347cf30bafef157
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017

---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a>Tutorial: Azure Active Directory-Integration mit SAP HANA Cloud Platform Identity Authentication

In diesem Tutorial erfahren Sie, wie Sie SAP HANA Cloud Platform Identity Authentication in Azure Active Directory (Azure AD) integrieren. SAP HANA Cloud Platform Identity Authentication wird als Proxy-IdP für den Zugriff auf SAP-Anwendungen verwendet, wobei Azure AD der Haupt-IdP ist.

Die Integration von SAP HANA Cloud Platform Identity Authentication in Azure AD bietet Ihnen folgende Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf die SAP-Anwendung hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP-Anwendungen anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP HANA Cloud Platform Identity Authentication zu konfigurieren, benötigen Sie die folgenden Elemente:

- Ein Azure AD-Abonnement
- Ein **SAP HANA Cloud Platform Identity Authentication**-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist


>[!NOTE] 
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP HANA Cloud Platform Identity Authentication aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

Bevor wir uns mit den technischen Details beschäftigen, müssen Sie die Konzepte verstehen, die hier behandelt werden. Durch den Verbund von SAP HANA Cloud Platform Identity Authentication und Azure Active Directory können Sie einmaliges Anmelden (SSO) über Anwendungen oder Dienste, die von AAD (als IdP) geschützt sind, mit SAP-Anwendungen und -Diensten, die von SAP HANA Cloud Platform Identity Authentication geschützt sind, implementieren.

Derzeit dient SAP HANA Cloud Platform Identity Authentication als Proxyidentitätsanbieter für SAP-Anwendungen. Azure Active Directory dient in diesem Setup als führender Identitätsanbieter. 

Die folgende Abbildung veranschaulicht dies:    

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

Mit diesem Setup wird der SAP HANA Cloud Platform Identity Authentication-Mandant als eine vertrauenswürdige Anwendung in Azure Active Directory konfiguriert. 

Alle SAP-Anwendungen und -Dienste, die Sie auf diese Weise schützen möchten, werden anschließend in der SAP HANA Cloud Platform Identity Authentication-Verwaltungskonsole konfiguriert.

Dies bedeutet, dass die Autorisierung zum Gewähren von Zugriff auf SAP-Anwendungen und -Dienste für ein solches Setup in SAP HANA Cloud Platform Identity Authentication erfolgen muss (statt die Autorisierung in Azure Active Directory zu konfigurieren).

Durch Konfigurieren von SAP HANA Cloud Platform Identity Authentication über den Azure Active Directory Marketplace als eine Anwendung müssen Sie nicht die erforderlichen einzelnen Ansprüche/SAML-Assertionen und -Transformationen konfigurieren, die benötigt werden, um ein gültiges Authentifizierungstoken für SAP-Anwendungen zu erstellen.

>[!NOTE] 
>Web-SSO wurde gegenwärtig nur von beiden Parteien getestet. Datenflüsse, die für die Kommunikation zwischen Apps und APIs oder zwischen APIs benötigt werden, sollten funktionieren, wurden aber noch nicht getestet. Sie werden im Rahmen späterer Aktivitäten getestet.
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-the-gallery"></a>Hinzufügen von SAP HANA Cloud Platform Identity Authentication aus dem Katalog
Um die Integration von SAP HANA Cloud Platform Identity Authentication in Azure AD zu konfigurieren, müssen Sie SAP HANA Cloud Platform Identity Authentication aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie zum Hinzufügen von SAP HANA Cloud Platform Identity Authentication aus dem Katalog die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des [**Azure-Verwaltungsportals**](https://portal.azure.com) auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld den Begriff **SAP HANA Cloud Platform Identity Authentication** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. Wählen Sie im Ergebnisbereich **SAP HANA Cloud Platform Identity Authentication** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden (SSO) von Azure AD bei SAP HANA Cloud Platform Identity Authentication basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in SAP HANA Cloud Platform Identity Authentication als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP HANA Cloud Platform Identity Authentication muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD dem **Benutzernamen** in SAP HANA Cloud Platform Identity Authentication zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD bei SAP HANA Cloud Platform Identity Authentication sind die folgenden Bausteine erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP HANA Cloud Platform Identity Authentication-Testbenutzers](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** – um eine Entsprechung von Britta Simon in SAP HANA Cloud Platform Identity Authentication zu erhalten, die mit ihrer Azure AD-Darstellung verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer SAP HANA Cloud Platform Identity Authentication-Anwendung.

Die SAP HANA Cloud Platform Identity Authentication-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

**Führen Sie zum Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD bei SAP HANA Cloud Platform Identity Authentication die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **SAP HANA Cloud Platform Identity Authentication** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren][5]

3. Im Abschnitt **Benutzerattribute** des Dialogfelds **Einmaliges Anmelden** erwartet die SAP-Anwendung möglicherweise ein Attribut, z.B. „firstName“. Fügen Sie das Attribut „firstName“ im Dialogfeld „SAML-Tokenattribute“ hinzu.
 1. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.
 
    ![Einmaliges Anmelden konfigurieren][6]

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. Geben Sie im Textfeld **Attributname** den Attributnamen „firstName“ ein.
 3. Wählen Sie in der Liste **Attributwert** den Attributwert „user.givenname“ aus.
 4. Klicken Sie auf **OK**.

4. Führen Sie im Abschnitt **Domäne und URLs für SAP HANA Cloud Platform Identity Authentication** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. Geben Sie im Textfeld **Anmelde-URL** die Anmelde-URL für die SAP-Anwendung ein.
 2. Geben Sie im Textfeld **Bezeichner** den Wert im folgenden Format ein: `<entity-id>.accounts.ondemand.com`. 
    * Wenn Sie diesen Wert nicht kennen, nutzen Sie die SAP HANA Cloud Platform Identity Authentication-Dokumentation zu [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html) (SAML 2.0-Mandantenkonfiguration).

5. Klicken Sie im Abschnitt **Konfiguration für SAP HANA Cloud Platform Identity Authentication** auf **SAP HANA Cloud Platform Identity Authentication konfigurieren**, um das Dialogfeld **Anmeldung konfigurieren** zu öffnen. Klicken Sie anschließend auf **SAML-XML-Metadaten**, und speichern Sie die Datei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. Wechseln Sie zum Konfigurieren von SSO für Ihre Anwendung zur SAP HANA Cloud Platform Identity Authentication-Verwaltungskonsole. Die URL hat das folgende Muster: `https://<tenant-id>.accounts.ondemand.com/admin`.
 * Befolgen Sie dann die Dokumentation für SAP HANA Cloud Platform Identity Authentication unter [Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html) (Konfigurieren von Microsoft Azure AD als Identitätsanbieter des Unternehmens für SAP HANA Cloud Platform Identity Authentication). 

7. Klicken Sie im Azure-Verwaltungsportal auf die Schaltfläche **Speichern**.
8. Führen Sie die folgenden Schritte nur aus, wenn Sie SSO für eine andere SAP-Anwendung hinzufügen und aktivieren möchten. Wiederholen Sie die Schritte aus Abschnitt „Hinzufügen von SAP HANA Cloud Platform Identity Authentication aus dem Katalog“, um eine weitere Instanz von „SAP HANA Cloud Platform Identity Authentication“ hinzuzufügen.
9. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **SAP HANA Cloud Platform Identity Authentication** auf **Anmeldung über Link**.

    ![Konfigurieren der Anmeldung über Link](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. Speichern Sie dann die Konfiguration.

>[!NOTE] 
>Die neue Anwendung nutzt die SSO-Konfiguration für die vorherige SAP-Anwendung. Achten Sie darauf, in der SAP HANA Cloud Platform Identity Authentication-Verwaltungskonsole die gleichen Identitätsanbieter des Unternehmens zu verwenden.
>

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im neuen Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.
  2. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.
  3. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.
  4. Klicken Sie auf **Erstellen**. 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a>Erstellen eines SAP HANA Cloud Platform Identity Authentication-Testbenutzers

Sie müssen keinen Benutzer in SAP HANA Cloud Platform Identity Authentication erstellen. Benutzer im Azure AD-Benutzerspeicher können die Funktionalität für einmaliges Anmelden (SSO) verwenden.

SAP HANA Cloud Platform Identity Authentication unterstützt die Option für Identitätsverbund. Durch diese Option kann die Anwendung überprüfen, ob die vom Identitätsanbieter des Unternehmens authentifizierten Benutzer im Benutzerspeicher von SAP HANA Cloud Platform Identity Authentication vorhanden sind. 

In der Standardeinstellung ist die Option für Identitätsverbund deaktiviert. Wenn Identitätsverbund aktiviert ist, können nur die Benutzer, die in SAP HANA Cloud Platform Identity Authentication importiert werden, auf die Anwendung zugreifen. 

Weitere Informationen zum Aktivieren oder Deaktivieren von Identitätsverbund in SAP HANA Cloud Platform Identity Authentication finden Sie unter „Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication“ (Aktivieren von Identitätsverbund in SAP HANA Cloud Platform Identity Authentication) in [Configure Identity Federation with the User Store of SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html) (Konfigurieren von Identitätsverbund mit dem Benutzerspeicher von SAP HANA Cloud Platform Identity Authentication).

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf SAP HANA Cloud Platform Identity Authentication gewähren.

![Benutzer zuweisen][200] 

**Führen Sie zum Zuweisen von Britta Simon zu SAP HANA Cloud Platform Identity Authentication die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP HANA Cloud Platform Identity Authentication** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel für SAP HANA Cloud Platform Identity Authentication klicken, sollten Sie automatisch bei Ihrer SAP HANA Cloud Platform Identity Authentication-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png
