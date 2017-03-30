---
title: 'Tutorial: Azure Active Directory-Integration mit OfficeSpace Software | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und OfficeSpace Software konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 95d8413f-db98-4e2c-8097-9142ef1af823
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: fb61a2d622b0d829a134b6ce5dfef6e9fb44fa1e
ms.lasthandoff: 03/22/2017


---
# <a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Tutorial: Azure Active Directory-Integration mit OfficeSpace Software

In diesem Tutorial erfahren Sie, wie Sie OfficeSpace Software in Azure Active Directory (Azure AD) integrieren.

Die Integration von OfficeSpace Software in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf OfficeSpace Software Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei OfficeSpace Software anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit OfficeSpace Software konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein OfficeSpace Software-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von OfficeSpace Software aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-officespace-software-from-the-gallery"></a>Hinzufügen von OfficeSpace Software aus dem Katalog
Zum Konfigurieren der Integration von OfficeSpace Software in Azure AD müssen Sie OfficeSpace Software aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um OfficeSpace Software aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **OfficeSpace Software** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_001.png)

5. Wählen Sie im Ergebnisbereich **OfficeSpace Software** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden von Azure AD in OfficeSpace Software konfiguriert und getestet wird.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in OfficeSpace Software als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in OfficeSpace Software muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in OfficeSpace Software zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei OfficeSpace Software müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines OfficeSpace Software-Testbenutzers](#creating-an-officespace-software-test-user)**, um eine Entsprechung von Britta Simon in OfficeSpace Software zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer OfficeSpace Software-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in OfficeSpace Software die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **OfficeSpace Software** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für OfficeSpace Software** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_02.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<company name>.officespacesoftware.com/users/sign_in/saml`

    b. Geben Sie im Textfeld **Bezeichner** einen Wert nach folgendem Muster ein: `<company name>.officespacesoftware.com`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen durch die tatsächliche Anmelde-URL und den tatsächlichen Bezeichner ersetzt werden. Wenden Sie sich an das [Supportteam von OfficeSpace Software](mailto:support@officespacesoftware.com), um diese Werte zu erhalten. 

4. Ihre OfficeSpace Software-Anwendung erwartet die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_03.png)

5. Wählen Sie im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute** den Eintrag **user.mail** als **Benutzerbezeichner** aus, und führen Sie für jede in der folgenden Tabelle aufgeführte Zeile die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | --- | --- |    
    | E-Mail | user.mail |
    | Name | user.displayname |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_08.png) 

7. Klicken Sie auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt mit der **OfficeSpace Software-Konfiguration** auf **OfficeSpace Software konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_09.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_10.png)

9. Melden Sie sich in einem anderen Webbrowserfenster beim OfficeSpace Software-Mandanten als Administrator an.

10. Navigieren Sie zu **Einstellungen**, und klicken Sie auf **Connectors**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_002.png)

11. Klicken Sie auf **SAML-Authentifizierung**.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_003.png)

12. Führen Sie im Abschnitt für die **SAML-Authentifizierung** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_004.png)

    a. Geben Sie im Textfeld für die **Abmeldeanbieter-URL** den Wert für die **Abmelde-URL** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    b. Geben Sie im Textfeld für die **Ziel-URL für Client-IdP** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c. Kopieren Sie den **Fingerabdruckwert** aus dem heruntergeladenen Zertifikat, und fügen Sie ihn in das Textfeld **Client idp cert fingerprint** (Fingerabdruck des Client-IdP-Zertifikats) ein. 

    d. Klicken Sie auf **Einstellungen speichern**.

    > [!NOTE]
    > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI) 
  

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-officespace-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 



### <a name="creating-an-officespace-software-test-user"></a>Erstellen eines OfficeSpace Software-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in OfficeSpace Software. OfficeSpace Software unterstützt die standardmäßig aktivierte Just-in-Time-Bereitstellung.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugriff auf OfficeSpace Software ein neuer Benutzer erstellt.

> [!NOTE]
> Wenden Sie sich an das [Supportteam von OfficeSpace Software](mailto:support@officespacesoftware.com), wenn Sie einen Benutzer manuell erstellen müssen.


### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf OfficeSpace Software gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon OfficeSpace Software zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **OfficeSpace Software** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-officespace-tutorial/tutorial_officespace_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „OfficeSpace Software“ klicken, sollten Sie automatisch an Ihrer OfficeSpace Software-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-officespace-tutorial/tutorial_general_203.png
