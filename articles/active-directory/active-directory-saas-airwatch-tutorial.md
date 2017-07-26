---
title: 'Tutorial: Azure Active Directory-Integration mit AirWatch | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und AirWatch konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 96a3bb1c-96c6-40dc-8ea0-060b0c2a62e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: c9a658de882d7421af3bdef8594493cd52985ad4
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Lernprogramm: Azure Active Directory-Integration mit AirWatch

In diesem Tutorial erfahren Sie, wie Sie AirWatch in Azure Active Directory (Azure AD) integrieren.

Die Integration von AirWatch in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf AirWatch hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei AirWatch anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit AirWatch konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein AirWatch-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von AirWatch aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-airwatch-from-the-gallery"></a>Hinzufügen von AirWatch aus dem Katalog
Zum Konfigurieren der Integration von AirWatch in Azure AD müssen Sie AirWatch aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um AirWatch aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **AirWatch** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_search.png)

5. Wählen Sie im Ergebnisbereich **AirWatch** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden mit Azure AD bei AirWatch mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in AirWatch als Entsprechung für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in AirWatch muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in AirWatch zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei AirWatch müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines AirWatch-Testbenutzers](#creating-a-airwatch-test-user)**, um eine Entsprechung von Britta Simon in AirWatch zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer AirWatch-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in AirWatch die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **AirWatch** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für AirWatch** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_url.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<subdomain>.awmdm.com/AirWatch/Login?gid=companycode`.

    b. Geben Sie im Textfeld **Bezeichner** den Wert `AirWatch` ein.

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Ersetzen Sie diesen Wert durch die tatsächliche Anmelde-URL. Wenden Sie sich an das [Supportteam von AirWatch](http://www.air-watch.com/company/contact-us/), um diesen Wert zu erhalten. 
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_certificate.png) 

5. Klicken Sie im Abschnitt **AirWatch-Konfiguration** auf **AirWatch konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_configure.png) 

6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_general_400.png)
<CS>
7. Melden Sie sich in einem anderen Webbrowserfenster bei der AirWatch-Unternehmenswebsite als Administrator an.

8. Klicken Sie im linken Navigationsbereich auf **Konten**, und klicken Sie dann auf **Administratoren**.
   
   ![Administratoren](./media/active-directory-saas-airwatch-tutorial/ic791920.png "Administratoren")

9. Erweitern Sie das Menü **Einstellungen**, und klicken Sie dann auf **Verzeichnisdienste**.
   
   ![Einstellungen](./media/active-directory-saas-airwatch-tutorial/ic791921.png "Einstellungen")

10. Klicken Sie auf die Registerkarte **Benutzer**, geben Sie im Textfeld **Basis-DN** Ihren Domänennamen ein, und klicken Sie dann auf **Speichern**.
   
   ![Benutzer](./media/active-directory-saas-airwatch-tutorial/ic791922.png "Benutzer")

11. Klicken Sie auf die Registerkarte **Server** .
   
   ![Server](./media/active-directory-saas-airwatch-tutorial/ic791923.png "Server")

12. Führen Sie die folgenden Schritte aus:
    
    ![Hochladen](./media/active-directory-saas-airwatch-tutorial/ic791924.png "Hochladen")   
    
    a. Wählen Sie unter **Verzeichnistyp** die Option **Keiner** aus.

    b. Aktivieren Sie **Use SAML For Authentication**.

    c. Klicken Sie auf **Upload**, um das heruntergeladene Zertifikat hochzuladen.

13. Führen Sie im Abschnitt **Request** die folgenden Schritte aus:
    
    ![Anforderung](./media/active-directory-saas-airwatch-tutorial/ic791925.png "Anforderung")  

    a. Wählen Sie als **Anforderungsbindungstyp** die Option **POST** aus.

    b. Kopieren Sie im Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Airwatch** den Wert für **SAML-Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **SSO-URL des Identitätsanbieters** ein.

    c. Wählen Sie als **NameID-Format** die Option **E-Mail-Adresse** aus.

    d. Klicken Sie auf **Speichern**.

14. Klicken Sie erneut auf die Registerkarte **User** .
    
    ![Benutzer](./media/active-directory-saas-airwatch-tutorial/ic791926.png "Benutzer")

15. Führen Sie im Abschnitt **Attribute** die folgenden Schritte aus:
    
    ![Attribut](./media/active-directory-saas-airwatch-tutorial/ic791927.png "Attribut")

    a. Geben Sie im Textfeld **Objektbezeichner** die URL **http://schemas.microsoft.com/identity/claims/objectidentifier** ein.

    b. Geben Sie in das Textfeld **Benutzername** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    c. Geben Sie in das Textfeld **Anzeigename** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    d. Geben Sie in das Textfeld **Vorname** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. Geben Sie in das Textfeld **Nachname** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.

    f. Geben Sie in das Textfeld **E-Mail** Folgendes ein: **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    g. Klicken Sie auf **Speichern**.

<CE>

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-airwatch-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-airwatch-test-user"></a>Erstellen eines AirWatch-Testbenutzers

Damit sich Azure AD-Benutzer bei AirWatch anmelden können, müssen sie in AirWatch bereitgestellt werden.

* Im Fall von AirWatch ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der **AirWatch** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im Navigationsbereich links auf **Konten**, und klicken Sie dann auf **Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-airwatch-tutorial/ic791929.png "Benutzer")
3. Klicken Sie im Menü **Benutzer** auf **Listenansicht**, und klicken Sie dann auf **Hinzufügen \> Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-airwatch-tutorial/ic791930.png "Benutzer hinzufügen")
4. Führen Sie im Dialogfeld **Add / Edit User** die folgenden Schritte aus:

   ![Benutzer hinzufügen](./media/active-directory-saas-airwatch-tutorial/ic791931.png "Benutzer hinzufügen")   
   1. Geben Sie in die Textfelder **Benutzername**, **Kennwort**, **Kennwort bestätigen**, **Vorname**, **Nachname** und **E-Mail-Adresse** die Informationen eines gültigen Azure Active Directory-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von AirWatch-Benutzerkonten oder mithilfe der von AirWatch bereitgestellten APIs erstellen.
>  

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf AirWatch gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon AirWatch zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **AirWatch** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-airwatch-tutorial/tutorial_airwatch_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie die Einstellungen für einmaliges Anmelden testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-airwatch-tutorial/tutorial_general_203.png


