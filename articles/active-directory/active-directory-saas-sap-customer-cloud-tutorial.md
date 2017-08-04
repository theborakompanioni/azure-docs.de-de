---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Cloud for Customer konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: e4d945525a45704f34e1d9e742220928a516f341
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer

In diesem Tutorial erfahren Sie, wie Sie SAP Cloud for Customer in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP Cloud for Customer in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD den Zugriff auf SAP Cloud for Customer steuern.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP Cloud for Customer anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit SAP Cloud for Customer konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP Cloud for Customer-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie hier eine einmonatige Testversion anfordern: [Testversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Cloud for Customer aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Hinzufügen von SAP Cloud for Customer aus dem Katalog
Zum Konfigurieren der Integration von SAP Cloud for Customer in Azure AD müssen Sie SAP Cloud for Customer aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um SAP Cloud for Customer aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie in das Suchfeld **SAP Cloud for Customer**ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

5. Wählen Sie im Ergebnisbereich **SAP Cloud for Customer** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP Cloud for Customer basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Cloud for Customer als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Cloud for Customer muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in SAP Cloud for Customer den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP Cloud for Customer müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP Cloud for Customer-Testbenutzers](#creating-a-sap-cloud-for-customer-test-user)**, um ein Pendant von Britta Simon in SAP Cloud for Customer zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SAP Cloud for Customer-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SAP Cloud for Customer die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP Cloud for Customer** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

3. Führen Sie auf der Seite **Domäne und URLs für SAP Cloud for Customer** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<server name>.crm.ondemand.com`.

    b. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Ersetzen Sie diese Werte durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner. Wenden Sie sich an das [Supportteam für den SAP Cloud for Customer-Client](https://www.sap.com/about/agreements.sap-cloud-services-customers.html), um diese Werte zu erhalten. 

4. Führen Sie im Abschnitt **Benutzerattribute** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. Wählen Sie in der Liste **Benutzer-ID** die Funktion **ExtractMailPrefix()** aus.

    b. Wählen Sie aus der Liste **Mail** das Benutzerattribut aus, das Sie für Ihre Implementierung verwenden möchten.
    Wenn Sie z.B. den Wert „EmployeeID“ als eindeutige Benutzer-ID verwenden möchten und den Attributwert in „ExtensionAttribute2“ gespeichert haben, wählen Sie „user.extensionattribute2“ aus.  

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **SAP Cloud for Customer Client-Konfiguration** auf **SAP Cloud for Customer Client konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

8. Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:
   
    a. Melden Sie sich am Portal für SAP Cloud for Customer mit Administratorrechten an.
   
    b. Navigieren Sie zum **allgemeinen Task für die Anwendungs- und Benutzerverwaltung**, und klicken Sie auf die Registerkarte **Identitätsanbieter**.
   
    c. Klicken Sie auf **New Identity Provider** (Neuer Identitätsanbieter), und wählen Sie die XML-Metadatendatei aus, die Sie aus dem Azure-Portal heruntergeladen haben. Beim Importieren der Metadaten lädt das System automatisch das erforderliche Signatur- und Verschlüsselungszertifikat hoch.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory erfordert die Assertionsverbraucherdienst-URL des Elements in der SAML-Anforderung. Aktivieren Sie daher das Kontrollkästchen **Include Assertion Consumer Service URL** (Assertionsverbraucherdienst-URL aufnehmen).
   
    e. Klicken Sie auf **Activate Single Sign-On**(Einmaliges Anmelden aktivieren).
   
    f. Speichern Sie die Änderungen.
   
    g. Klicken Sie auf die Registerkarte **My System** (Mein System).
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. Fügen Sie in das Textfeld **Azure AD Sign On URL** (Azure AD-Anmelde-URL) den Wert der **SAML-Dienst-URL für einmaliges Anmelden** ein, den Sie aus dem Azure-Portal kopiert haben.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Geben Sie an, ob der Mitarbeiter manuell zwischen der Anmeldung mit Benutzer-ID und Kennwort oder SSO wählen kann. Aktivieren Sie dazu die Option **Manual Identity Provider Selection** (Manuelle Auswahl des Identitätsanbieters).
   
    j. Geben Sie im Abschnitt **SSO-URL** die URL ein, die von Ihren Mitarbeitern für die Anmeldung beim System verwendet werden soll. 
    In der Liste **URL Sent to Employee** (An Mitarbeiter gesendete URL) können Sie zwischen den folgenden Optionen wählen:
   
    **Non-SSO URL**
   
    Das System sendet nur die normale System-URL an den Mitarbeiter. Der Mitarbeiter kann sich nicht per SSO anmelden, sondern muss stattdessen ein Kennwort oder Zertifikat verwenden.
   
    **SSO-URL** 
   
    Das System sendet nur die SSO-URL an den Mitarbeiter. Der Mitarbeiter kann sich per SSO anmelden. Die Authentifizierungsanforderung wird über den IdP umgeleitet.
   
    **Automatische Auswahl**
   
    Ist SSO nicht aktiv, sendet das System die normale System-URL an den Mitarbeiter. Wenn SSO aktiv ist, überprüft das System, ob der Mitarbeiter über ein Kennwort verfügt. Ist ein Kennwort verfügbar, werden sowohl die SSO-URL als auch die URL ohne SSO an den Mitarbeiter gesendet. Besitzt der Mitarbeiter kein Kennwort, wird jedoch nur die SSO-URL an den Mitarbeiter gesendet.
   
    k. Speichern Sie die Änderungen.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Erstellen eines SAP Cloud for Customer-Testbenutzers

In diesem Abschnitt erstellen Sie in SAP Cloud for Customer einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das [Supportteam von SAP Cloud for Customer](https://www.sap.com/about/agreements.sap-cloud-services-customers.html), um die Benutzer auf der SAP Cloud for Customer-Plattform hinzufügen zu lassen. 

> [!NOTE]
> Stellen Sie sicher, dass der Wert für „NameID“ mit dem Feld „username“ der SAP Cloud for Customer-Plattform übereinstimmt.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf SAP Cloud for Customer gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SAP Cloud for Customer zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP Cloud for Customer**aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP Cloud for Customer“ klicken, sollten Sie automatisch bei Ihrer SAP Cloud for Customer-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-customer-cloud-tutorial/tutorial_general_203.png


