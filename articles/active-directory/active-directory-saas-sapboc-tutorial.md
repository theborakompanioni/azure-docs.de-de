---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Business Object Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Business Object Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 7456da29aa07372156f2b9c08ab83626dab7cc45
ms.openlocfilehash: 6d517c5e302ac36e5bba2053998c75f8f4d42683
ms.contentlocale: de-de
ms.lasthandoff: 08/28/2017

---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Tutorial: Azure Active Directory-Integration mit SAP Business Object Cloud

In diesem Tutorial erfahren Sie, wie Sie SAP Business Object Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP Business Object Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf SAP Business Object Cloud steuern.
- Sie können Ihre Benutzer mithilfe des einmaligen Anmeldens und einem Azure AD-Benutzerkonto automatisch bei SAP Business Object Cloud anmelden.
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps (Software as a Service) in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP Business Object Cloud einzurichten, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Eine SAP Business Object Cloud-Instanz, für die einmaliges Anmelden aktiviert ist

> [!NOTE]
> Sie sollten die Schritte in diesem Tutorial nicht in einer Produktionsumgebung testen.

Empfehlungen zum Testen der Schritte in diesem Tutorial:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige kostenlose Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. 

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Business Object Cloud aus dem Katalog.
2. Einrichten des einmaligen Anmeldens in Azure AD und Testen.

## <a name="add-sap-business-object-cloud-from-the-gallery"></a>Hinzufügen von SAP Business Object Cloud aus dem Katalog
Zum Einrichten der Integration von SAP Business Object Cloud in Azure AD müssen Sie SAP Business Object Cloud aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

So fügen Sie SAP Business Object Cloud aus dem Katalog hinzu:

1. Wählen Sie im linken Menü des [Azure-Portals](https://portal.azure.com) die Option **Azure Active Directory** aus. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Die Seite „Unternehmensanwendungen“][2]
    
3. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SAP Business Object Cloud** ein.

    ![Das Suchfeld](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Wählen Sie im Ergebnisbereich **SAP Business Object Cloud** aus, und wählen Sie dann **Hinzufügen**.

    ![SAP Business Object Cloud in der Ergebnisliste](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a>Einrichten des einmaligen Anmeldens in Azure AD und Testen

In diesem Abschnitt richten Sie das einmalige Anmelden von Azure AD bei SAP Business Object Cloud mithilfe einer Testbenutzerin namens *Britta Simon* ein und testen es.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer der entsprechende Benutzer in SAP Business Object Cloud ist. Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Business Object Cloud muss eine Linkbeziehung eingerichtet werden.

Um die Linkbeziehung herzustellen, weisen Sie in SAP Business Object Cloud für **Benutzername** den Wert von **Benutzername** in Azure AD zu.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP Business Object Cloud folgende Aufgaben aus:

1. [Einrichten des einmaligen Anmeldens in Azure AD](#set-up-azure-ad-single-sign-on). Richtet einen Benutzer zur Verwendung dieses Features ein.
2. [Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user). Testet das einmalige Anmelden in Azure AD mit der Benutzerin Britta Simon.
3. [Erstellen eines SAP Business Object Cloud-Testbenutzers](#create-an-sap-business-object-cloud-test-user). Erstellt ein Pendant zu Britta Simon in SAP Business Object Cloud, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. [Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user). Richtet Britta Simon zur Verwendung des einmaligen Anmeldens in Azure AD ein.
5. [Testen des einmaligen Anmeldens](#test-single-sign-on). Überprüft, ob die Konfiguration funktioniert.

### <a name="set-up-azure-ad-single-sign-on"></a>Einrichten des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie einmaliges Anmelden in Azure AD im Azure-Portal. Dann richten Sie einmaliges Anmelden in Ihrer SAP Business Object Cloud-Anwendung ein.

So richten Sie einmaliges Anmelden in Azure AD mit SAP Business Object Cloud ein:

1. Wählen Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Business Object Cloud** die Option **Einmaliges Anmelden**.

    ![„Einmaliges Anmelden“ auswählen][4]

2. Wählen Sie auf der Seite **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus.
 
    ![„SAML-basierte Anmeldung“ auswählen](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Führen Sie die folgenden Schritte unter **Domäne und URLs für SAP Business Object Cloud** aus:

    1. Geben Sie im Feld **URL für Anmeldung** eine URL mit diesem Muster ein: 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. Geben Sie im Feld **Bezeichner** eine URL mit folgendem Muster ein:
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![URLs der Seite „Domäne und URLs für SAP Business Object Cloud“](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > Die Werte in diesen URLs dienen nur Demonstrationszwecken. Aktualisieren Sie die Werte mit der tatsächlichen Anmelde-URL und der Bezeichner-URL. Um die Anmelde-URL zu erhalten, wenden Sie sich an das [SAP Business Object Cloud-Clientsupportteam](https://www.sap.com/product/analytics/cloud-analytics.support.html). Sie können die Bezeichner-URL abrufen, indem Sie die SAP Business Object Cloud-Metadaten aus der Verwaltungskonsole herunterladen. Dies wird weiter unten im Tutorial erläutert. 

4. Wählen Sie unter **SAML-Signaturzertifikat** die Option **Metadaten-XML**. Speichern Sie dann die Metadatendatei auf Ihrem Computer.

    ![„Metadaten-XML“ wählen](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Wählen Sie **Speichern** aus.

    ![„Speichern“ auswählen](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der SAP Business Object Cloud-Unternehmenswebsite als Administrator an.

7. Wählen Sie **Menü** > **System** > **Administration**.
    
    ![„Menü“, „System“ und dann „Administration“ wählen](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. Wählen Sie auf der Registerkarte **Security** das Symbol **Bearbeiten** (Stift) aus.
    
    ![Auf der Registerkarte „Security“ das Symbol „Bearbeiten“ auswählen](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. Wählen Sie als **Authentication Method** die Option **SAML Single Sign-On (SSO)** aus.

    ![„SAML Single Sign-On (SSO)“ als Authentifizierungsmethode auswählen](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. Um die Metadaten des Dienstanbieters herunterzuladen, (Schritt 1), wählen Sie **Download**. Suchen Sie in der Metadatendatei den Wert **entityID**, und kopieren Sie ihn. Fügen Sie im Azure-Portal unter **Domäne und URLs für SAP Business Object Cloud** den Wert in das Feld **Bezeichner** ein.

    ![Wert „entityID“ kopieren und einfügen](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. Um die Metadaten des Dienstanbieters (Schritt 2) in der Datei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben, wählen Sie unter **Upload Identity Provider metadata** die Option **Upload**.  

    ![Unter „Upload Identity Provider metadata“ die Option „Upload“ wählen](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. Wählen Sie in der Liste **User Attribute** das Benutzerattribut aus (Schritt 3), das Sie für Ihre Implementierung verwenden möchten. Dieses Benutzerattribut wird dem Identitätsanbieter zugeordnet. Um ein benutzerdefiniertes Attribut auf der Seite des Benutzers einzugeben, verwenden Sie die Option **Custom SAML Mapping**. Alternativ können Sie entweder **Email** oder **USER ID** als Benutzerattribut wählen. In unserem Beispiel haben wir **Email** ausgewählt, weil wir den Benutzer-ID-Anspruch mit dem **userprincipalname**-Attribut im Abschnitt **Benutzerattribute** im Azure-Portal zugeordnet haben. Hiermit wird eine individuelle Benutzer-E-Mail bereitgestellt, die in jeder erfolgreichen SAML-Antwort an die SAP Business Object Cloud-Anwendung gesendet wird.

    ![„Benutzerattribut“ auswählen](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. Um das Konto mit dem Identitätsanbieter zu überprüfen (Schritt 4), geben Sie im Feld **Login Credential (Email)** die E-Mail-Adresse des Benutzers ein. Wählen Sie dann **Verify Account**. Das System fügt dem Benutzerkonto Anmeldeinformationen hinzu.

    ![E-Mail-Adresse eingeben und „Konto überprüfen“ wählen](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Wählen Sie das Symbol **Speichern** aus.

    ![Symbol „Speichern“](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen! Nachdem Sie diese App durch Auswahl von **Active Directory** > **Unternehmensanwendungen** hinzugefügt haben, wählen Sie die Registerkarte **Einmaliges Anmelden** aus. Sie können über den Bereich **Konfiguration** am unteren Rand auf die eingebettete Dokumentation zugreifen. Weitere Informationen finden Sie unter [Verwalten des einmaligen Anmeldens für Unternehmens-Apps]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im Azure-Portal eine Testbenutzerin mit dem Namen Britta Simon.

So erstellen Sie einen Testbenutzer in Azure AD:

1. Wählen Sie im linken Menü des Azure-Portals die Option **Azure Active Directory** aus.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. Um die Liste der Benutzer anzuzeigen, wählen Sie **Benutzer und Gruppen** und dann **Alle Benutzer** aus.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. Zum Öffnen des Dialogfelds **Benutzer** wählen Sie **Hinzufügen**.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. Führen Sie im Dialogfeld **Benutzer** die folgenden Schritte aus:
 
    1. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    2. Geben Sie im Feld **Benutzername** die E-Mail-Adresse der Benutzerin Britta Simon ein.

    3. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    4. Klicken Sie auf **Erstellen**.

        ![Dialogfeld „Benutzer“](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Azure AD-Benutzer erstellen][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a>Erstellen eines SAP Business Object Cloud-Testbenutzers

Damit sich Azure AD-Benutzer bei SAP Business Object Cloud anmelden können, müssen sie in SAP Business Object Cloud bereitgestellt werden. In SAP Business Object Cloud wird die Bereitstellung manuell ausgeführt.

So stellen Sie ein Benutzerkonto bereit:

1. Melden Sie sich bei der SAP Business Object Cloud-Unternehmenswebsite als Administrator an.

2. Wählen Sie **Menü** > **Security** > **Users**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. Um neue Benutzerdetails hinzuzufügen, wählen Sie auf der Seite **Users** die Option **+**. 

    ![Seite „Add Users“](./media/active-directory-saas-sapboc-tutorial/user4.png)

    Führen Sie anschließend die folgenden Schritte aus:

    1. Geben Sie im Textfeld **USER ID** die Benutzer-ID des Benutzers ein, z.B. **Britta**.

    2. Geben Sie im Textfeld **FIRST NAME** den Vornamen des Benutzers ein (z.B. **Britta**).

    3. Geben Sie im Textfeld **LAST NAME** den Nachnamen des Benutzers ein (z.B. **Simon**).

    4. Geben Sie im Textfeld **DISPLAY NAME** den vollständigen Namen des Benutzers ein (z.B. **Britta Simon**).

    5. Geben Sie im Textfeld **E-MAIL** die E-Mail-Adresse des Benutzers ein, z.B. **brittasimon@contoso.com**.

    6. Wählen Sie auf der Seite **Select Roles** die passende Rolle für den Benutzer, und wählen Sie dann **OK**.

      ![Rolle wählen](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. Wählen Sie das Symbol **Speichern** aus.    


### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie der Benutzerin Britta Simon die Verwendung des einmaligen Anmeldens von Azure AD, indem Sie ihrem Benutzerkonto Zugriff auf SAP Business Object Cloud gewähren.

So weisen Sie Britta Simon SAP Business Object Cloud zu:

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, und navigieren Sie zur Verzeichnisansicht. Wählen Sie **Unternehmensanwendungen** und dann **Alle Anwendungen** aus.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **SAP Business Object Cloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Wählen Sie im linken Menü **Benutzer und Gruppen** aus.

    ![Benutzer und Gruppen auswählen][202] 

4. Wählen Sie **Hinzufügen**. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Die Seite „Zuweisung hinzufügen“][203]

5. Wählen Sie auf der Seite **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Wählen Sie auf der Seite **Benutzer und Gruppen** die Option **Auswählen**.

7. Wählen Sie auf der Seite **Zuweisung hinzufügen** die Option **Zuweisen** aus.

![Zuweisen der Benutzerrolle][200] 
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich die Kachel „SAP Business Object Cloud“ wählen, sollten Sie automatisch bei Ihrer SAP Business Object Cloud-Anwendung angemeldet werden.

Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png


