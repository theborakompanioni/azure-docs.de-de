---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Business Object Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Business Object Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 85723eeb9c030dc51bdad47ea8e0996ff7868336
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017

---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a>Tutorial: Azure Active Directory-Integration mit SAP Business Object Cloud

In diesem Tutorial erfahren Sie, wie Sie SAP Business Object Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP Business Object Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf SAP Business Object Cloud steuern.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP Business Object Cloud anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP Business Object Cloud konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP Business Object Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Business Object Cloud aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-business-object-cloud-from-the-gallery"></a>Hinzufügen von SAP Business Object Cloud aus dem Katalog
Zum Konfigurieren der Integration von SAP Business Object Cloud in Azure AD müssen Sie SAP Business Object Cloud aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um SAP Business Object Cloud aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **SAP Business Object Cloud** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. Wählen Sie im Ergebnisbereich **SAP Business Object Cloud** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP Business Object Cloud basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Business Object Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Business Object Cloud muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SAP Business Object Cloud den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP Business Object Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP Business Object Cloud-Testbenutzers](#creating-an-sap-business-object-cloud-test-user)**, um eine Entsprechung von Britta Simon in SAP Business Object Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SAP Business Object Cloud-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SAP Business Object Cloud die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Business Object Cloud** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. Führen Sie die folgenden Schritte im Abschnitt **Domäne und URLs für SAP Business Object Cloud** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<sub-domain>.projectorca.cloud/`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `<sub-domain>.projectorca.cloud`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Kundensupportteam von SAP Business Object Cloud](https://www.sap.com/product/analytics/cloud-analytics.support.html), um die Anmelde-URL zu erhalten. Sie können den Bezeichner zum Herunterladen der SAP Business Object Cloud-Metadaten über die Verwaltungskonsole erhalten. Dies wird später in diesem Tutorial erläutert. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. Melden Sie sich in einem anderen Webbrowserfenster bei der SAP Business Object Cloud-Unternehmenswebsite als Administrator an.

7. Wechseln Sie zu **Menü > System > Verwaltung**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config1.png)    
8. Klicken Sie auf der Registerkarte **Sicherheit** auf **[Bearbeiten – Stiftsymbol]**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config2.png)    
9. Wählen Sie **Einmalige SAML-Anmeldung (SSO)** als Authentifizierungsmethode aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. Klicken Sie auf **Herunterladen**, um die Metadaten des Dienstanbieters herunterzuladen. Rufen Sie den **entityID**-Wert aus der Datei ab, und fügen Sie ihn in das Textfeld **Bezeichner** im Abschnitt **Domäne und URLs für SAP Business Objekt Cloud** im Azure-Portal ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. Klicken Sie im Schritt zum **Hochladen der Metadaten des Identitätsanbieters** auf die Schaltfläche **Hochladen**, um die Metadatendatei hochzuladen, die Sie aus dem Azure-Portal heruntergeladen haben. 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. Wählen Sie aus der Liste das entsprechende **Benutzerattribut** aus, das Sie für Ihre Implementierung verwenden möchten, um es dem Identitätsanbieter zuzuordnen. Verwenden Sie die Option für die benutzerdefinierte SAML-Zuordnung, um ein benutzerdefiniertes Attribut auf der Benutzerseite einzugeben. Sie können auch „Email“ oder „USER ID“ als Benutzerattribut auswählen. Beispiel: Hier wird **Email** ausgewählt, da der Benutzer-ID-Anspruch mit dem Attribut „userprincipalname“ im Abschnitt **Benutzerattribute** im Azure-Portal zugeordnet wurde. Dadurch wird eine eindeutige Benutzer-E-Mail bereitgestellt, die in jeder erfolgreichen SAML-Antwort an die SAP Business Object Cloud-Anwendung gesendet wird.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. Geben Sie Ihre E-Mail-Adresse in das Textfeld **Anmeldeinformationen (E-Mail)** ein, und klicken Sie auf die Schaltfläche **Konto überprüfen**, damit das System die Anmeldeinformationen zu Ihrem Konto hinzufügen kann.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. Klicken Sie auf das Symbol **Speichern**.

    ![Speichern](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-sap-business-object-cloud-test-user"></a>Erstellen eines SAP Business Object Cloud-Testbenutzers

Damit sich Azure AD-Benutzer bei SAP Business Object Cloud anmelden können, müssen sie in SAP Business Object Cloud bereitgestellt werden.  
Bei SAP Business Object Cloud wird die Bereitstellung manuell ausgeführt.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der SAP Business Object Cloud-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Menü > Sicherheit > Benutzer**.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. Klicken Sie auf der Seite **Benutzer** auf **+**, um neue Benutzerdetails hinzuzufügen, und führen Sie die folgenden Schritte aus:

    ![Invite People](./media/active-directory-saas-sapboc-tutorial/user4.png)

    a. Geben Sie im Textfeld **BENUTZER-ID** die Benutzer-ID des Benutzers ein, z.B. Britta.

    b. Geben Sie im Textfeld **VORNAME** den Vornamen des Benutzers ein, z.B. Britta.

    c. Geben Sie im Textfeld **NACHNAME** den Nachnamen des Benutzers ein, z.B. Simon.

    d. Geben Sie im Textfeld **ANZEIGENAME** den vollständigen Namen des Benutzers ein, z.B. Britta Simon.

    e. Geben Sie im Textfeld **E-MAIL** die E-Mail-Adresse des Benutzers ein, z.B. brittasimon@contoso.com.

    f. Wählen Sie die geeignete Rolle für den Benutzer aus.

      ![Rolle](./media/active-directory-saas-sapboc-tutorial/user3.png)

    g. Klicken Sie auf das Symbol **Speichern**. 


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie Zugriff auf SAP Business Object Cloud gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SAP Business Object Cloud zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Liste der Anwendungen **SAP Business Object Cloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP Business Object Cloud“ klicken, sollten Sie automatisch bei Ihrer SAP Business Object Cloud-Anwendung angemeldet werden.
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


