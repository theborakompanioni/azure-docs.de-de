---
title: 'Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Qlik Sense Enterprise konfigurieren.
services: active-directory
documentationcenter: ''
author: jeevansd
manager: femila
editor: ''

ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2016
ms.author: jeedes

---
# Tutorial: Azure Active Directory-Integration mit Qlik Sense Enterprise
In diesem Tutorial erfahren Sie, wie Sie Qlik Sense Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von Qlik Sense Enterprise in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Qlik Sense Enterprise hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Qlik Sense Enterprise anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen
Um die Azure AD-Integration mit Qlik Sense Enterprise konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Qlik Sense Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Qlik Sense Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## Hinzufügen von Qlik Sense Enterprise aus dem Katalog
Zum Konfigurieren der Integration von Qlik Sense Enterprise in Azure AD müssen Sie Qlik Sense Enterprise aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Qlik Sense Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen**.
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **Qlik Sense Enterprise** ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)
7. Wählen Sie im Ergebnisbereich **Qlik Sense Enterprise** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

## Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Qlik Sense Enterprise basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Qlik Sense Enterprise als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Qlik Sense Enterprise muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD dem **Benutzernamen** in Qlik Sense Enterprise als Wert zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Qlik Sense Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Qlik Sense Enterprise-Testbenutzers](#creating-a-qliksense-enterprise-test-user)**, um eine Entsprechung von Britta Simon in Qlik Sense Enterprise zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Qlik Sense Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Qlik Sense Enterprise die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Qlik Sense Enterprise** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6]
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Qlik Sense Enterprise anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png)
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png)
   
    a. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der Qlik Sense Enterprise-Anwendung verwendet wird, nach folgendem Muster ein: **https://\<Vollqualifizierter Qlik Sense-Hostname>:443/<Präfix für virtuellen Proxy>/samlauthn/**.
   
   > [!NOTE]
   > Beachten Sie den nachgestellten Schrägstrich am Ende des URI. Er ist erforderlich.
   > 
   > 
   
    b. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden bei Qlik Sense Enterprise konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)
   
    a. Klicken Sie auf **Metadaten herunterladen** und speichern Sie die Datei auf Ihrem Computer. Beachten Sie, dass Sie diese Metadatendatei vor dem Hochladen auf den Qlik Sense-Server ggf. bearbeiten müssen.
   
    b. Klicken Sie auf **Weiter**.
5. Bereiten Sie die XML-Datei mit den Verbundmetadaten für das Hochladen auf den Qlik Sense-Server vor.
   
   > [!NOTE]
   > Vor dem Hochladen der IdP-Metadaten auf den Qlik Sense-Server muss die Datei bearbeitet werden, um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen.
   > 
   > 
   
    ![QlikSense][qs24]
   
    a. Öffnen Sie die aus Azure heruntergeladene Datei „FederationMetaData.xml“ in einem Text-Editor.
   
    b. Suchen Sie nach dem Wert **RoleDescriptor**. Es sind vier Einträge vorhanden (jeweils zwei Paare mit öffnenden und schließenden Elementtags).
   
    c. Löschen Sie die RoleDescriptor-Tags und alle Informationen dazwischen aus der Datei.
   
    d. Speichern Sie die Datei, und bewahren Sie sie auf, um sie zu einem späteren Zeitpunkt in diesem Dokument verwenden zu können.
6. Navigieren Sie als Benutzer mit Rechten zum Erstellen von Konfigurationen für virtuelle Proxys zur Qlik Sense Qlik Management Console (QMC).
7. Klicken Sie in der QMC auf das Menüelement „Virtual Proxy“.
   
    ![QlikSense][qs6]
8. Klicken Sie am unteren Bildschirmrand auf die Schaltfläche „Create new“.
   
    ![QlikSense][qs7]
9. Der Bearbeitungsbildschirm für den virtuellen Proxy wird angezeigt. Auf der rechten Seite des Bildschirms befindet sich ein Menü zum Anzeigen von Konfigurationsoptionen.
   
    ![QlikSense][qs9]
10. Geben Sie bei aktivierter Menüoption „Identification“ die identifizierenden Informationen für die Azure-Konfiguration des virtuellen Proxys ein.
    
    ![QlikSense][qs8]
    
    a. Das Feld „Beschreibung“ enthält einen Anzeigenamen für die Konfiguration des virtuellen Proxys. Geben Sie einen Wert als Beschreibung ein.
    
    b. Mit dem Feld „Prefix“ wird der Endpunkt des virtuellen Proxys für die Verbindung mit Qlik Sense per einmaliger Azure AD-Anmeldung angegeben. Geben Sie einen eindeutigen Präfixnamen für diesen virtuellen Proxy ein.
    
    c. „Session inactivity timeout (minutes)“ ist der Timeout für Verbindungen über diesen virtuellen Proxy.
    
    d. Unter „Session cookie header name“ wird der Cookiename zum Speichern des Sitzungsbezeichners für die Qlik Sense-Sitzung angegeben, die einem Benutzer nach der erfolgreichen Authentifizierung zugeordnet wird. Dieser Name muss eindeutig sein.
11. Klicken Sie auf die Menüoption „Authentication“, um die entsprechenden Informationen anzuzeigen. Der Bildschirm „Authentication“ wird angezeigt.
    
    ![QlikSense][qs10]
    
    a. Mit dem Dropdownmenü **Anonymous access mode** wird festgelegt, ob anonyme Benutzer über den virtuellen Proxy auf Qlik Sense zugreifen können. Die Standardoption ist „No anonymous user“.
    
    b. Mit dem Dropdownmenü **Authentication method** wird festgelegt, welches Authentifizierungsschema vom virtuellen Proxy verwendet wird. Wählen Sie in der Dropdownliste die Option „SAML“ aus. Daraufhin werden weitere Optionen angezeigt.
    
    c. Geben Sie im Feld **SAML host URI** den Hostnamen ein, den Benutzer eingeben müssen, um über diesen virtuellen SAML-Proxy auf Qlik Sense zuzugreifen. Der Hostname ist der URI des Qlik Sense-Servers.
    
    d. Geben Sie im Feld **SAML entity ID** den gleichen Wert wie im Feld „SAML host URI“ ein.
    
    e. Unter **SAML IdP metadata** geht es um die Datei, die Sie zuvor im Abschnitt **Edit Federation Metadata from Azure AD Configuration** (Verbundmetadaten der Azure AD-Konfiguration bearbeiten) bearbeitet haben. **Vor dem Hochladen der IdP-Metadaten muss die Datei bearbeitet werden**, um Informationen zu entfernen und so den richtigen Ablauf zwischen Azure AD und dem Qlik Sense-Server sicherzustellen. **Verwenden Sie die obige Anleitung, falls Sie die Datei noch bearbeiten müssen.** Wenn die Datei bearbeitet wurde, können Sie auf die Schaltfläche zum Durchsuchen klicken und die bearbeitete Metadatendatei auswählen, um sie für die Konfiguration des virtuellen Proxys hochzuladen.
    
    f. Geben Sie den Attributnamen oder den Schemaverweis für das SAML-Attribut ein, das für die **UserID** steht, die von Azure AD an den Qlik Sense-Server gesendet wird. Informationen zum Schemaverweis sind auf den Azure-App-Bildschirmen zur Post-Konfiguration verfügbar. Geben Sie zum Verwenden des name-Attributs **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name** ein.
    
    g. Geben Sie den Wert für das Benutzerverzeichnis (**user directory**) ein, das Benutzern zugeordnet wird, wenn diese die Authentifizierung gegenüber dem Qlik Sense-Server über Azure AD durchführen. Hartcodierte Werte müssen in **eckige Klammern** gesetzt werden. Geben Sie den Namen des Attributs in diesem Textfeld **ohne** eckige Klammern ein, um ein Attribut zu verwenden, das in der Azure AD-SAML-Assertion gesendet wird.
    
    h. Unter **SAML signing algorithm** wird die Service Provider-Zertifikatsignatur (hier: Qlik Sense-Server) für die Konfiguration des virtuellen Proxys festgelegt. Ändern Sie den SAML-Signaturalgorithmus in **SHA-256**, wenn für den Qlik Sense-Server ein vertrauenswürdiges Zertifikat verwendet wird, das per Microsoft Enhanced RSA und AES Cryptographic Provider generiert wurde.
    
    i. Im Abschnitt „SAML attribute mapping“ können weitere Attribute, z.B. Gruppen, zur Verwendung in Sicherheitsregeln an Qlik Sense gesendet werden.
12. Klicken Sie auf die Menüoption „Load Balancing“, um den Bildschirm anzuzeigen. Der Bildschirm „Load Balancing“ wird angezeigt.
    
    ![QlikSense][qs11]
13. Klicken Sie auf die Schaltfläche „Add new server node“, wählen Sie den Modulknoten oder Knoten aus, an die von Qlik Sense zu Lastenausgleichszwecken Sitzungen gesendet werden, und klicken Sie auf die Schaltfläche „Add“.
    
    ![QlikSense][qs12]
14. Klicken Sie auf die Menüoption „Advanced“, um die entsprechenden Informationen anzuzeigen. Der Bildschirm „Advanced“ wird angezeigt.
    
    ![QlikSense][qs13]
    
    a. Unter „Host white list“ sind die Hostnamen angegeben, die akzeptiert werden, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt wird. **Geben Sie den Hostnamen ein, der von Benutzern angegeben werden muss, wenn eine Verbindung mit dem Qlik Sense-Server hergestellt werden soll.** Der Hostname ist der gleiche Wert wie der SAML-Host-URI ohne „https://“.
15. Klicken Sie auf die Schaltfläche „Apply“.
    
    ![QlikSense][qs14]
16. Klicken Sie auf „OK“, um die Warnmeldung mit dem Hinweis zu akzeptieren, dass mit dem virtuellen Proxy verknüpfte Proxys neu gestartet werden.
    
    ![QlikSense][qs15]
17. Rechts auf dem Bildschirm wird das Menü „Associated items“ angezeigt. Klicken Sie auf die Menüoption „Proxies“.
    
    ![QlikSense][qs16]
18. Der Proxybildschirm wird angezeigt. Klicken Sie unten auf die Schaltfläche „Link“, um einen Proxy mit dem virtuellen Proxy zu verknüpfen.
    
    ![QlikSense][qs17]
19. Wählen Sie den Proxyknoten aus, der diese Verbindung mit dem virtuellen Proxy unterstützen soll, und klicken Sie auf die Schaltfläche „Link“. Nach dem Verknüpfen wird der Proxy in der Liste mit den zugeordneten Proxys aufgeführt.
    
    ![QlikSense][qs18] ![QlikSense][qs19]
20. Nach ungefähr fünf bis zehn Sekunden wird die Meldung „Refresh QMC“ angezeigt. Klicken Sie auf die Schaltfläche „Refresh QMC“.
    
    ![QlikSense][qs20]
21. Klicken Sie nach der QMC-Aktualisierung auf das Menüelement „Virtual proxies“. Der Eintrag mit dem neuen virtuellen SAML-Proxy wird in der Tabelle auf dem Bildschirm angezeigt. Klicken Sie einmal auf den Eintrag für den virtuellen Proxy.
    
    ![QlikSense][qs51]
22. Unten auf dem Bildschirm wird die Schaltfläche „Download SP metadata“ aktiviert. Klicken Sie auf die Schaltfläche „Download SP metadata“, um die Metadaten in einer Datei zu speichern.
    
    ![QlikSense][qs52]
23. Öffnen Sie die SP-Metadatendatei. Sehen Sie sich die Einträge **entityID** und **AssertionConsumerService** an. Diese Werte entsprechen dem **Bezeichner** und der **Anmelde-URL** in der Azure AD-Anwendungskonfiguration. Falls sie nicht übereinstimmen, sollten Sie diese Werte im Konfigurations-Assistenten der Azure AD-App ersetzen.
    
    ![QlikSense][qs53]
24. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]
25. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
    
    ![Azure AD – einmaliges Anmelden][11]

### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png)
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png)
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png)
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.

### Erstellen eines Qlik Sense Enterprise-Testbenutzers
In diesem Abschnitt erstellen Sie in Qlik Sense Enterprise einen Benutzer mit dem Namen Britta Simon. Lassen Sie sich beim Hinzufügen der Benutzer ggf. vom Qlik Sense Enterprise-Supportteam unterstützen.

### Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Qlik Sense Enterprise gewähren.

![Benutzer zuweisen][200]

**Um Britta Simon Qlik Sense Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste **Qlik Sense Enterprise**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon** aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

## Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Qlik Sense Enterprise“ klicken, sollten Sie automatisch bei Ihrer Qlik Sense Enterprise-Anwendung angemeldet werden.

## Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

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
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

<!---HONumber=AcomDC_0907_2016-->