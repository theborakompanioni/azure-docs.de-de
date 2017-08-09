---
title: 'Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Qlik Sense Enterprise konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 3fd6bcce24ca4a1170bd8a9301dff1a02d638773
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise

In diesem Tutorial erfahren Sie, wie Sie Qlik Sense Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von Qlik Sense Enterprise in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Qlik Sense Enterprise hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Qlik Sense Enterprise anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Qlik Sense Enterprise konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Qlik Sense Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Qlik Sense Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Hinzufügen von Qlik Sense Enterprise aus dem Katalog
Zum Konfigurieren der Integration von Qlik Sense Enterprise in Azure AD müssen Sie Qlik Sense Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Qlik Sense Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Qlik Sense Enterprise** ein, wählen Sie im Ergebnisbereich **Qlik Sense Enterprise** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Qlik Sense Enterprise in der Ergebnisliste](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Qlik Sense Enterprise basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Qlik Sense Enterprise als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Qlik Sense Enterprise muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Qlik Sense Enterprise den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Qlik Sense Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Qlik Sense Enterprise-Testbenutzers](#create-a-qlik-sense-enterprise-test-user)**, um eine Entsprechung von Britta Simon in Qlik Sense Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Qlik Sense Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Qlik Sense Enterprise die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Qlik Sense Enterprise** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für Qlik Sense Enterprise** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Qlik Sense Enterprise](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`.
    
    > [!NOTE]
    > Beachten Sie den nachgestellten Schrägstrich am Ende des URI. Er ist erforderlich.
    
    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein:
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit der tatsächlichen Anmelde-URL und dem Bezeichner. Diese werden später in diesem Tutorial erläutert. Wenden Sie sich alternativ an das [Clientsupportteam von Qlik Sense Enterprise](https://www.qlik.com/us/services/support), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. Bereiten Sie die XML-Datei mit den Verbundmetadaten für das Hochladen auf den Qlik Sense-Server vor.
   
    > [!NOTE]
    > Vor dem Hochladen der IdP-Metadaten auf den Qlik Sense-Server muss die Datei bearbeitet werden, um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen.
    
    ![QlikSense][qs24]
   
    a. Öffnen Sie die aus dem Azure-Portal heruntergeladene Datei „FederationMetaData.xml“ in einem Text-Editor.
   
    b. Suchen Sie nach dem Wert **RoleDescriptor**.  Es sind vier Einträge vorhanden (jeweils zwei Paare mit öffnenden und schließenden Elementtags).
   
    c. Löschen Sie die RoleDescriptor-Tags und alle Informationen dazwischen aus der Datei.
   
    d. Speichern Sie die Datei, und bewahren Sie sie auf, um sie zu einem späteren Zeitpunkt in diesem Dokument verwenden zu können.

7. Navigieren Sie als Benutzer mit Rechten zum Erstellen von Konfigurationen für virtuelle Proxys zur Qlik Sense Qlik Management Console (QMC).

8. Klicken Sie in der QMC auf das Menüelement **Virtual Proxy**.
   
    ![QlikSense][qs6] 

9. Klicken Sie am unteren Bildschirmrand auf die Schaltfläche **Create new** (Neu erstellen).
   
    ![QlikSense][qs7]

10. Der Bearbeitungsbildschirm für den virtuellen Proxy wird angezeigt.  Auf der rechten Seite des Bildschirms befindet sich ein Menü zum Anzeigen von Konfigurationsoptionen.
   
    ![QlikSense][qs9]

11. Geben Sie bei aktivierter Menüoption „Identification“ die identifizierenden Informationen für die Azure-Konfiguration des virtuellen Proxys ein.
    
    ![QlikSense][qs8]  
    
    a. Das Feld **Description** (Beschreibung) enthält einen Anzeigenamen für die Konfiguration des virtuellen Proxys.  Geben Sie einen Wert als Beschreibung ein.
    
    b. Mit dem Feld **Prefix** wird der Endpunkt des virtuellen Proxys für die Verbindung mit Qlik Sense per einmaligem Anmelden von Azure AD angegeben.  Geben Sie einen eindeutigen Präfixnamen für diesen virtuellen Proxy ein.
    
    c. **Session inactivity timeout (minutes)** (Timeout bei Sitzungsinaktivität (Minuten)) ist das Timeout für Verbindungen über diesen virtuellen Proxy.
    
    d. Unter **Session cookie header name** (Headername für Sitzungscookie) wird der Cookiename zum Speichern des Sitzungsbezeichners für die Qlik Sense-Sitzung angegeben, die einem Benutzer nach der erfolgreichen Authentifizierung zugeordnet wird.  Dieser Name muss eindeutig sein.

12. Klicken Sie auf die Menüoption „Authentication“, um die entsprechenden Informationen anzuzeigen.  Der Bildschirm „Authentication“ wird angezeigt.
    
    ![QlikSense][qs10]
    
    a. Mit dem Dropdownmenü **Anonymous access mode** wird festgelegt, ob anonyme Benutzer über den virtuellen Proxy auf Qlik Sense zugreifen können.  Die Standardoption ist „No anonymous user“.
    
    b. Mit dem Dropdownmenü **Authentication method** (Authentifizierungsmethode) wird festgelegt, welches Authentifizierungsschema vom virtuellen Proxy verwendet wird.  Wählen Sie in der Dropdownliste die Option „SAML“ aus.  Daraufhin werden weitere Optionen angezeigt.
    
    c. Geben Sie im Feld **SAML host URI** den Hostnamen ein, den Benutzer eingeben müssen, um über diesen virtuellen SAML-Proxy auf Qlik Sense zuzugreifen.  Der Hostname ist der URI des Qlik Sense-Servers.
    
    d. Geben Sie im Feld **SAML entity ID**den gleichen Wert wie im Feld „SAML host URI“ ein.
    
    e. Unter **SAML IdP metadata** (SAML-IdP-Metadaten) wird die Datei gewählt, die Sie zuvor im Abschnitt **Edit Federation Metadata from Azure AD Configuration** (Verbundmetadaten der Azure AD-Konfiguration bearbeiten) bearbeitet haben.  **Vor dem Hochladen der IdP-Metadaten muss die Datei bearbeitet werden** , um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen.  **Verwenden Sie die obige Anleitung, falls Sie die Datei noch bearbeiten müssen.**  Wenn die Datei bearbeitet wurde, können Sie auf die Schaltfläche zum Durchsuchen klicken und die bearbeitete Metadatendatei auswählen, um sie für die Konfiguration des virtuellen Proxys hochzuladen.
    
    f. Geben Sie den Attributnamen oder den Schemaverweis für das SAML-Attribut ein, das für die **UserID** steht, die von Azure AD an den Qlik Sense-Server gesendet wird.  Informationen zum Schemaverweis sind auf den Azure-App-Bildschirmen zur Post-Konfiguration verfügbar.  Weitere Informationen zum Verwenden des Namensattributs finden Sie unter `enter http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.
    
    g. Geben Sie den Wert für das Benutzerverzeichnis ( **user directory** ) ein, das Benutzern zugeordnet wird, wenn diese die Authentifizierung gegenüber dem Qlik Sense-Server über Azure AD durchführen.  Hartcodierte Werte müssen in **eckige Klammern []** gesetzt werden.  Geben Sie den Namen des Attributs in diesem Textfeld **ohne** eckige Klammern ein, um ein Attribut zu verwenden, das in der Azure AD-SAML-Assertion gesendet wird.
    
    h. Unter **SAML signing algorithm** wird die Service Provider-Zertifikatsignatur (hier: Qlik Sense-Server) für die Konfiguration des virtuellen Proxys festgelegt.  Ändern Sie den SAML-Signaturalgorithmus in **SHA-256**, wenn für den Qlik Sense-Server ein vertrauenswürdiges Zertifikat verwendet wird, das per Microsoft Enhanced RSA und AES Cryptographic Provider generiert wurde.
    
    i. Im Abschnitt „SAML attribute mapping“ können weitere Attribute, z.B. Gruppen, zur Verwendung in Sicherheitsregeln an Qlik Sense gesendet werden.

13. Klicken Sie auf die Menüoption **LOAD BALANCING**, um die entsprechenden Informationen anzuzeigen.  Der Bildschirm „Load Balancing“ wird angezeigt.
    
    ![QlikSense][qs11]

14. Klicken Sie auf die Schaltfläche **Add new server node** (Neuen Serverknoten hinzufügen), wählen Sie den Modulknoten oder Knoten aus, an die von Qlik Sense zu Lastenausgleichszwecken Sitzungen gesendet werden, und klicken Sie auf die Schaltfläche **Add** (Hinzufügen).
    
    ![QlikSense][qs12]

15. Klicken Sie auf die Menüoption „Advanced“, um die entsprechenden Informationen anzuzeigen. Der Bildschirm „Advanced“ wird angezeigt.
    
    ![QlikSense][qs13]
    
    Unter „Host white list“ sind die Hostnamen angegeben, die akzeptiert werden, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt wird.  **Geben Sie den Hostnamen ein, der von Benutzern angegeben werden muss, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt werden soll.** Der Hostname ist der gleiche Wert wie der SAML-Host-URI ohne „https://“.

16. Klicken Sie auf die Schaltfläche **Apply** (Anwenden).
    
    ![QlikSense][qs14]

17. Klicken Sie auf „OK“, um die Warnmeldung mit dem Hinweis zu akzeptieren, dass mit dem virtuellen Proxy verknüpfte Proxys neu gestartet werden.
    
    ![QlikSense][qs15]

18. Rechts auf dem Bildschirm wird das Menü „Associated items“ angezeigt.  Klicken Sie auf die Menüoption **Proxies**.
    
    ![QlikSense][qs16]

19. Der Proxybildschirm wird angezeigt.  Klicken Sie unten auf die Schaltfläche **Link**, um einen Proxy mit dem virtuellen Proxy zu verknüpfen.
    
    ![QlikSense][qs17]

20. Wählen Sie den Proxyknoten aus, der diese Verbindung mit dem virtuellen Proxy unterstützen soll, und klicken Sie auf die Schaltfläche **Link**.  Nach dem Verknüpfen wird der Proxy in der Liste mit den zugeordneten Proxys aufgeführt.
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. Nach ungefähr fünf bis zehn Sekunden wird die Meldung „Refresh QMC“ angezeigt.  Klicken Sie auf die Schaltfläche **Refresh QMC** (QMC aktualisieren).
    
    ![QlikSense][qs20]

22. Klicken Sie nach der QMC-Aktualisierung auf das Menüelement **Virtual proxies**. Der Eintrag mit dem neuen virtuellen SAML-Proxy wird in der Tabelle auf dem Bildschirm angezeigt.  Klicken Sie einmal auf den Eintrag für den virtuellen Proxy.
    
    ![QlikSense][qs51]

23. Unten auf dem Bildschirm wird die Schaltfläche „Download SP metadata“ aktiviert.  Klicken Sie auf die Schaltfläche **Download SP metadata** (SP-Metadaten herunterladen), um die Metadaten in einer Datei zu speichern.
    
    ![QlikSense][qs52]

24. Öffnen Sie die SP-Metadatendatei.  Sehen Sie sich die Einträge **entityID** und **AssertionConsumerService** an.  Diese Werte entsprechen dem **Bezeichner** und der **Anmelde-URL** in der Azure AD-Anwendungskonfiguration. Fügen Sie diese Werte in der Azure AD-Anwendungskonfiguration im Abschnitt **Domäne und URLs für Qlik Sense Enterprise**. Sofern sie nicht übereinstimmen, ersetzen Sie sie auch im Konfigurations-Assistenten der Azure AD-App.
    
    ![QlikSense][qs53]

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

   ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

   ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

   ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

   ![Dialogfeld „Benutzer“](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

   b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

   c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

   d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a>Erstellen eines Qlik Sense Enterprise-Testbenutzers

In diesem Abschnitt erstellen Sie in Qlik Sense Enterprise einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Clientsupportteam von Qlik Sense Enterprise](https://www.qlik.com/us/services/support), um die Benutzer in der Qlik Sense Enterprise-Plattform hinzuzufügen. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Qlik Sense Enterprise gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon Qlik Sense Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Qlik Sense Enterprise**.

    ![Qlik Sense Enterprise-Link in der Anwendungsliste](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Qlik Sense Enterprise“ klicken, sollten Sie automatisch bei Ihrer Qlik Sense Enterprise-Anwendung angemeldet werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png


