---
title: 'Tutorial: Azure Active Directory-Integration mit Deputy | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Deputy konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 5665c3ac-5689-4201-80fe-fcc677d4430d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: c3c0079ef9ef7b3be7c8df9280e6ebfb34902801
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Tutorial: Azure Active Directory-Integration mit Deputy
Dieses Tutorial soll Ihnen zeigen, wie Sie Deputy in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Deputy in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Deputy hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Deputy anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Deputy konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Deputy-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Deputy aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="adding-deputy-from-the-gallery"></a>Hinzufügen von Deputy aus dem Katalog
Zum Konfigurieren der Integration von Deputy in Azure AD müssen Sie Deputy aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um Deputy aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **Deputy**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)
7. Wählen Sie im Ergebnisbereich **Deputy** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden (SSO) von Azure AD in Deputy konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Deputy als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Deputy muss eine Verknüpfungsbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD Benutzernamen**** in Deputy zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens (SSO) in Azure AD bei Deputy sind die folgenden Bausteine erforderlich:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Deputy-Testbenutzers](#creating-a-deputy-test-user)** , um eine Entsprechung von Britta Simon in Deputy zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren es in Ihrer Deputy-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Deputy die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Deputy** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Deputy anmelden?** die Option **Azure AD – einmaliges Anmelden** aus und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)
3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)
  1. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://<your-subdomain>.<region>.deputy.com`.
  2. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.
  3. Klicken Sie auf **Weiter**.
4. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)
   1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<your-subdomain>.<region>.deputy.com`.
   2. Klicken Sie auf **Weiter**.
   
     >[!NOTE]
     > Das Regionssuffix für Deputy ist optional und muss bei Verwendung auf eines der folgenden Suffixe festgelegt werden: au | na | eu |as |la |af |an |ent-au |ent-na |ent-eu |ent-as | ent-la | ent-af | ent-an
     > 

5. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Deputy** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)
   *  Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
6. Navigieren Sie zu folgender URL: https://(Ihre-Unterdomäne).deputy.com/exec/config/system_config. Wechseln Sie zu **Sicherheitseinstellungen**, und klicken Sie auf **Bearbeiten**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)
7. Kopieren Sie im klassischen Azure-Portal auf der Seite zum Konfigurieren der einmaligen Anmeldung für Deputy die SAML-SSO-URL. 
8. Führen Sie auf der Seite **Sicherheitseinstellungen** die folgenden Schritte aus.
   
![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

   1. Aktivieren der Anmeldung für soziale Netzwerke ****.
   2. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **OpenSSL-Zertifikat** ein.
   3. Geben Sie im Textfeld für die SAM-SSO-URL Folgendes ein: `https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
   4. Ersetzen Sie im Textfeld für die SAM-SSO-URL `<your subdomain>` durch Ihre Unterdomäne.
   5. Ersetzen Sie im Textfeld für die SAM-SSO-URL `<saml sso url>` durch die SAML-SSO-URL, die Sie aus dem klassischen Azure-Portal kopiert haben.
   6. Klicken Sie auf **Einstellungen speichern**.
9. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
       ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

   1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   3. Klicken Sie auf **Weiter**. 6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)
   
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.
   
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)
   
   1. Notieren Sie den Wert von **Neues Kennwort**.
   2. Klicken Sie unten auf der Seite auf **Fertigstellen**.   

### <a name="create-a-deputy-test-user"></a>Erstellen eines Deputy-Testbenutzers
Damit sich Azure AD-Benutzer bei Deputy anmelden können, müssen sie in Deputy bereitgestellt werden. Im Fall von Deputy ist die Bereitstellung eine manuelle Aufgabe.

#### <a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:
1. Melden Sie sich bei der Deputy-Unternehmenswebsite als Administrator an.
2. Klicken Sie oben im Navigationsbereich auf **Personen**.
   
   ![Personen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Personen")
3. Klicken Sie auf die Schaltfläche **Personen hinzufügen**, und klicken Sie auf **Einzelne Person hinzufügen**.
   
   ![Personen hinzufügen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Personen hinzufügen")
4. Führen Sie die folgenden Schritte aus, und klicken Sie auf **Speichern und einladen**.
   
   ![Neuer Benutzer](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Neuer Benutzer")
   
  1. Geben Sie in das Textfeld **Name** die Werte **Britta** und **Simon** ein.  
  2. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse eines Azure AD-Kontos ein, das Sie bereitstellen möchten.
  3. Geben Sie im Textfeld **Firma** den Namen des Unternehmens ein.
  4. Klicken Sie auf die Schaltfläche **Speichern und einladen**.

5. Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Deputy-Benutzerkonten oder mithilfe der von Deputy bereitgestellten APIs erstellen.
    
### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) bei Azure zu ermöglichen, indem sie Zugriff auf Deputy erhält.

![Benutzer zuweisen][200]

**Um Britta Simon Deputy zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal im oberen Menü der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste **Deputy**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Deputy“ klicken, sollten Sie automatisch bei Ihrer Deputy-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png

