---
title: 'Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP Cloud for Customer konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: f454e7e218764e00cc19ca67b0edade213834b75
ms.openlocfilehash: bec672005b0b1856faeb230f5674013a37add774


---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Tutorial: Azure Active Directory-Integration mit SAP Cloud for Customer
In diesem Tutorial erfahren Sie, wie Sie SAP Cloud for Customer in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP Cloud for Customer in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD den Zugriff auf SAP Cloud for Customer steuern.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP Cloud for Customer anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit SAP Cloud for Customer konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein SAP Cloud for Customer-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP Cloud for Customer aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Hinzufügen von SAP Cloud for Customer aus dem Katalog
Zum Konfigurieren der Integration von SAP Cloud for Customer in Azure AD müssen Sie SAP Cloud for Customer aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um SAP Cloud for Customer aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]

6. Geben Sie in das Suchfeld **SAP Cloud for Customer**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_01.png)

7. Wählen Sie im Ergebnisbereich **SAP Cloud for Customer** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Active Directory](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP Cloud for Customer basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP Cloud for Customer als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP Cloud for Customer muss eine Linkbeziehung eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei SAP Cloud for Customer müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP Cloud for Customer-Testbenutzers](#creating-an-sap-business-bydesign-test-user)**, um eine Entsprechung von Britta Simon in SAP Cloud for Customer zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer SAP Cloud for Customer-Anwendung. 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in SAP Cloud for Customer die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **SAP Cloud for Customer** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_80.png) 

2. Wählen Sie aus der Attributliste „SAML-Tokenattribute“ das Attribut „nameidentifier“ aus, und klicken Sie auf **Bearbeiten**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_84.png) 

3. Führen Sie im Dialogfeld **Benutzerattribut bearbeiten** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_85.png) 

    a. Wählen Sie aus der Liste **Attributwert** die Funktion **ExtractMailPrefix()** aus.

    b. Wählen Sie aus der Liste **Mail** das Benutzerattribut aus, das Sie für Ihre Implementierung verwenden möchten. 
    Wenn Sie z.B. den Wert „EmployeeID“ als eindeutige Benutzer-ID verwenden möchten und den Attributwert in „ExtensionAttribute2“ gespeichert haben, wählen Sie „user.extensionattribute2“ aus. 

    c. Klicken Sie auf **Fertig stellen**. 


1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **SAP Cloud for Customer** auf **Einmaliges Anmelden konfigurieren**.
   
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SAP Cloud for Customer anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_04.png) 
   
    a. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der SAP Cloud for Customer-Anwendung verwendet wird, nach folgendem Muster ein: `https://<server name>.crm.ondemand.com`
   
    b. click **Weiter**

4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für SAP Cloud for Customer** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_05.png)
   
    a. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Klicken Sie auf **Next**.

5. Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:
   
    a. Melden Sie sich am Portal für SAP Cloud for Customer mit Administratorrechten an.
   
    b. Navigieren Sie zum **allgemeinen Task für die Anwendungs- und Benutzerverwaltung**, und klicken Sie auf die Registerkarte **Identitätsanbieter**.
   
    c. Klicken Sie auf **New Identity Provider** (Neuer Identitätsanbieter), und wählen Sie die XML-Metadatendatei aus, die Sie aus dem klassischen Azure-Portal heruntergeladen haben. Beim Importieren der Metadaten lädt das System automatisch das erforderliche Signatur- und Verschlüsselungszertifikat hoch.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure AD erfordert die Assertion Consumer Service-URL des Elements in der SAML-Anforderung. Aktivieren Sie daher das Kontrollkästchen **Include Assertion Consumer Service URL** (Assertion Consumer Service-URL aufnehmen).
   
    e. Klicken Sie auf **Activate Single Sign-On**(Einmaliges Anmelden aktivieren).
   
    f. Speichern Sie die Änderungen.
   
    g. Klicken Sie auf die Registerkarte **My System** (Mein System).
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. Kopieren Sie die **SSO-URL**, und fügen Sie sie in das Textfeld **Azure AD Sign On URL** (Azure AD-Anmelde-URL) ein.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_53.png)
   
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

6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Azure-Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:  ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_06.png) 
   
    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-sapcloudforcustomer-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-an-sap-cloud-for-customer-test-user"></a>Erstellen eines SAP Cloud for Customer-Testbenutzers
In diesem Abschnitt erstellen Sie in SAP Cloud for Customer einen Benutzer mit dem Namen Britta Simon. Wenden Sie sich an das Supportteam von SAP Cloud for Customer, um die Benutzer in der SAP Cloud for Customer-Plattform hinzufügen zu lassen. 

> [!NOTE]
> Stellen Sie sicher, dass der Wert für „NameID“ mit dem Feld „username“ der SAP Cloud for Customer-Plattform übereinstimmt.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf SAP Cloud for Customer gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon SAP Cloud for Customer zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **SAP Cloud for Customer**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_sapcloudforcustomer_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP Cloud for Customer“ klicken, sollten Sie automatisch bei Ihrer SAP Cloud for Customer-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapcloudforcustomer-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


