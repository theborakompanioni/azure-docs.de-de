---
title: 'Tutorial: Azure Active Directory-Integration mit Bonusly | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Bonusly konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 42875d53-0769-4520-a6af-7308b5240d6b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: af00ed428dda846a06466df1e30ee7e8a7e5758c
ms.openlocfilehash: c160829519a14a55e76a2ecedcb286663dc89d65
ms.lasthandoff: 02/15/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Tutorial: Azure Active Directory-Integration mit Bonusly

In diesem Tutorial erfahren Sie, wie Sie Bonusly in Azure Active Directory (Azure AD) integrieren.

Die Integration von Bonusly in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Bonusly hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Bonusly anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Verwaltungsportal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Bonusly konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Bonusly-Abonnement, für das einmaliges Anmelden aktiviert ist


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Bonusly aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-bonusly-from-the-gallery"></a>Hinzufügen von Bonusly aus dem Katalog
Zum Konfigurieren der Integration von Bonusly in Azure AD müssen Sie Bonusly aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Bonusly aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Verwaltungsportals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Bonusly** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_001.png)

5. Wählen Sie im Ergebnisbereich die Option **Bonusly** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Bonusly mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Bonusly als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Bonusly muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in Bonusly zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Bonusly müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Bonusly-Testbenutzers](#creating-a-bonusly-test-user)**, um eine Entsprechung von Britta Simon in Certify zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden mit Azure AD im Azure-Verwaltungsportal und konfigurieren das einmalige Anmelden in Ihrer Bonusly-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Bonusly die folgenden Schritte aus:**

1. Klicken Sie im Azure-Verwaltungsportal auf der Anwendungsintegrationsseite für **Bonusly** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_01.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Bonusly** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_02.png)

    a. Geben Sie im Textfeld **Bezeichner** den Wert `bonusly` ein.
    
    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://bonus.ly/saml/<APP-ID>/consume`
    
4. Führen Sie im Abschnitt **Domäne und URLs für Bonusly** die folgenden Schritte aus, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_03.png)

    a. Klicken Sie auf die Option **Erweiterte URL-Einstellungen anzeigen**.

    b. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://bonus.ly/saml/<your_subdomain>/consume`

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte durch die tatsächliche Anmelde-URL, den tatsächlichen Bezeichner und die tatsächliche Antwort-URL ersetzen. Hier empfehlen wir Ihnen, den eindeutigen Wert der Zeichenfolge im Bezeichner zu verwenden. Wenden Sie sich an das [Supportteam von Bonusly](mailto:sales@easyterritory.com), um diese Werte zu erhalten.

5. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Neues Zertifikat erstellen**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_04.png)     

6. Klicken Sie im Dialogfeld **Neues Zertifikat erstellen** auf das Kalendersymbol, und wählen Sie ein **Ablaufdatum** aus. Klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_general_300.png)

7. Wählen Sie im Abschnitt **SAML-Signaturzertifikat** die Option **Neues Zertifikat aktivieren**, und klicken Sie auf die Schaltfläche **Speichern**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_05.png)

8. Klicken Sie im Popupfenster **Rolloverzertifikat** auf **OK**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_general_400.png)

9. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_06.png) 

10. Klicken Sie im Abschnitt **Bonusly-Konfiguration** auf **Bonusly konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_07.png) 

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_08.png)

11. Melden Sie sich in einem anderen Webbrowserfenster bei der Bonusly-Unternehmenswebsite als Administrator an.

12. Klicken Sie in der Symbolleiste oben auf **Einstellungen**, und wählen Sie dann **Integrationen und Apps** aus.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_002.png)

13. Wählen Sie unter **Einmaliges Anmelden** **SAML** aus.

14. Führen Sie auf der Dialogseite **SAML** die folgenden Schritte aus:

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_003.png)

    a. Geben Sie im Textfeld für die **Ziel-URL für IdP SSO** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    b. Geben Sie im Textfeld **Anmelde-URL für IdP** den Wert für die **SAML-Dienst-URL für einmaliges Anmelden** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    c. Geben Sie im Textfeld **IdP-Aussteller** den Wert der **SAML-Entitäts-ID** aus dem Konfigurationsfenster der Azure AD-Anwendung ein.

    d. Kopieren Sie den Fingerabdruckwert aus dem heruntergeladenen Zertifikat, und fügen Sie ihn in das Textfeld **Cert-Fingerabdruck** ein.

    e. Klicken Sie auf **Speichern**.

    > [!NOTE] 
    > Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](https://www.youtube.com/watch?v=YKQF266SAxI&feature=youtu.be).



### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im Azure-Verwaltungsportal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Verwaltungsportals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-bonusly-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**. 



### <a name="creating-a-bonusly-test-user"></a>Erstellen eines Bonusly-Testbenutzers

Damit sich Azure AD-Benutzer bei Bonusly anmelden können, müssen sie in Bonusly bereitgestellt werden.
Im Fall von Bonusly ist die Bereitstellung eine manuelle Aufgabe.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1. Melden Sie sich bei der Bonusly-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf **Einstellungen**.

    ![Neuer Benutzer](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_010.png "Neuer Benutzer")

3. Klicken Sie auf die Registerkarte **Users and bonuses** .

    ![Neuer Benutzer](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_011.png "Neuer Benutzer")

4. Klicken Sie auf **Manage Users**.

    ![Neuer Benutzer](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_012.png "Neuer Benutzer")

5. Klicken Sie auf **Benutzer hinzufügen**.

    ![Neuer Benutzer](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_013.png "Neuer Benutzer")

6. Führen Sie im Abschnitt **Add User** die folgenden Schritte aus:

    ![Neuer Benutzer](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_014.png "Neuer Benutzer")

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse des Kontos von Britta Simon ein.

    d. Klicken Sie auf **Speichern**.

    > [!NOTE] 
    > Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Bonus.ly-Benutzerkonten oder mithilfe von APIs erstellen, die von Bonus.ly zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden.



### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Bonusly gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Bonusly zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Verwaltungsportal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **Bonusly** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonusly-tutorial/tutorial_bonusly_50.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    


### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Bonusly“ klicken, sollten Sie automatisch bei Ihrer Bonusly-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bonusly-tutorial/tutorial_general_203.png
