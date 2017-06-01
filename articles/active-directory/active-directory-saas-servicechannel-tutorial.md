---
title: 'Tutorial: Azure Active Directory-Integration mit ServiceChannel | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ServiceChannel konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: 7e1dad18ff0ae9a9102b789b2cb32e7b96ed3d38
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a>Tutorial: Azure Active Directory-Integration mit ServiceChannel

In diesem Tutorial erfahren Sie, wie Sie ServiceChannel in Azure Active Directory (Azure AD) integrieren.

Die Integration von ServiceChannel in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf ServiceChannel haben soll.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ServiceChannel anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit ServiceChannel konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein ServiceChannel-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ServiceChannel aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-servicechannel-from-the-gallery"></a>Hinzufügen von ServiceChannel aus dem Katalog
Zum Konfigurieren der Integration von ServiceChannel in Azure AD müssen Sie ServiceChannel aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um ServiceChannel aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **ServiceChannel** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. Wählen Sie im Ergebnisbereich die Option **ServiceChannel** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ServiceChannel mithilfe eines Testbenutzers namens „Britta Simon“.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ServiceChannel als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ServiceChannel muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in ServiceChannel zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei ServiceChannel müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ServiceChannel-Testbenutzers](#creating-a-servicechannel-test-user)**: Zum Testen des einmaligen Anmeldens über Azure AD mit dem Testbenutzer „Britta Simon“.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer ServiceChannel-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei ServiceChannel die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **ServiceChannel** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. Führen Sie die folgenden Schritte auf der Seite **Domäne und URLs für ServiceChannel** aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `http://adfs.<domain>.com/adfs/service/trust` ein.

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<customer domain>.servicechannel.com/saml/acs`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL aktualisieren. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von ServiceChannel](https://servicechannel.zendesk.com/hc/en-us), um diese Werte zu erhalten.

4. Die ServiceChannel-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: **NameIdentifier (Benutzer-ID)** ist der einzige verpflichtende Anspruch. Der Standardwert ist **user.userprincipalname**, ServiceChannel erwartet allerdings, dass dies **user.mail** zugeordnet werden muss. Wenn Sie planen, die bedarfsabhängige Benutzerbereitstellung zu aktivieren, sollten Sie die folgenden Anforderungen wie unten gezeigt hinzufügen. Der **Rollenanspruch** muss **user.assignedroles** zugeordnet werden, das die Rolle des Benutzers enthält.  

    Weitere Anleitungen zu Anforderungen finden Sie im ServiceChannel-Handbuch [hier](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example).
      
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > Klicken Sie bitte [hier](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/), um herauszufinden, wie Sie die **Rolle** in Azure AD konfigurieren.

5. Klicken Sie im Abschnitt **Benutzerattribute** auf **Alle weiteren Benutzerattribute anzeigen und bearbeiten**, und legen Sie die Attribute fest.

    | Attributname | Attributwert |
    | --- | --- |    
    | Rolle| user.assignedroles |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.
    
6. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. Klicken Sie auf **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. Klicken Sie im Abschnitt **ServiceChannel-Konfiguration** auf **ServiceChannel konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Notieren Sie bitte die **SAML-Entitäts-ID** aus dem Abschnitt mit der **Kurzübersicht**.

9. Für die Konfiguration des einmaligen Anmeldens auf der **ServiceChannel**-Seite müssen Sie das heruntergeladene **Zertifikat (Base64)** und die **SAML-Entitäts-ID** an das [ServiceChannel-Supportteam](https://servicechannel.zendesk.com/hc/en-us) senden. Er führt die Einrichtung durch, damit die SAML-SSO-Verbindung auf beiden Seiten richtig festgelegt ist.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 

### <a name="creating-a-servicechannel-test-user"></a>Erstellen eines ServiceChannel-Testbenutzers

Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung. Nach der Authentifizierung werden in der Anwendung automatisch Benutzer erstellt. Bitte wenden Sie sich für die vollständige Benutzerbereitstellung an das [ServiceChannel-Supportteam](https://servicechannel.zendesk.com/hc/en-us).

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie „Britta Simon“ die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ServiceChannel gewähren.

![Benutzer zuweisen][200] 

**Rühren Sie die folgenden Schritte aus, um „Britta Simon“ ServiceChannel zuzuweisen:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **ServiceChannel** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ServiceChannel“ klicken, sollten Sie automatisch bei Ihrer ServiceChannel-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png
