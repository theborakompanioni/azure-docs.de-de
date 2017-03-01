---
title: 'Tutorial: Azure Active Directory-Integration mit Optimizely | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Optimizely konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 49352a5e8255468bbc54c02e0cd9242d49002dbd
ms.openlocfilehash: 2275daf25fd916932dbf54a7b069ecbfcf3e68cf
ms.lasthandoff: 12/08/2016


---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Tutorial: Azure Active Directory-Integration mit Optimizely
In diesem Tutorial erfahren Sie, wie Sie Optimizely Learn in Azure Active Directory (Azure AD) integrieren.

Die Integration von Optimizely in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Optimizely hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Optimizely anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Optimizely konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein **Optimizely** -Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Optimizely aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-optimizely-from-the-gallery"></a>Hinzufügen von Optimizely aus dem Katalog
Zum Konfigurieren der Integration von Optimizely in Azure AD müssen Sie Optimizely aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um Optimizely aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Optimizely**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Wählen Sie im Ergebnisbereich **Optimizely** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Optimizely basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Optimizely als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Optimizely muss eine Linkbeziehung eingerichtet werden.
Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Optimizely zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Optimizely müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Optimizely-Testbenutzers](#creating-an-optimizely-test-user)** , um eine Entsprechung von Britta Simon in Optimizely zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und in Ihrer Optimizely-Anwendung zu konfigurieren.

Die Optimizely-Anwendung erwartet, dass die SAML-Assertions ein Attribut namens „email“ enthalten. Der Wert von „email“ sollte eine in Optimizely bekannte E-Mail-Adresse sein, die von Azure AD authentifiziert werden kann. Konfigurieren Sie für diese Anwendung den Anspruch „email“. Sie können die Werte dieser Attribute auf der Registerkarte **Attribute** der Anwendung verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: 

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Optimizely die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **Optimizely** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren][5]

2. Fügen Sie das Attribut „email“ im Dialogfeld „SAML-Tokenattribute“ hinzu.
   
    a. Klicken Sie unten auf der Seite auf **add user attribute** to open the **Benutzerattribut hinzufügen** zu öffnen. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)
   
    b. Geben Sie im Textfeld **Attributname** den Attributnamen „email“ ein.
   
    c. Wählen Sie aus der Liste **Attributwert** den Attributwert „userprincipalname“ oder einen anderen Wert aus, der eine in Azure AD und Optimizely bekannte E-Mail-Adresse enthält.
   
    d. Klicken Sie auf **Fertig stellen**.

3. Klicken Sie im oberen Menü auf **Schnellstart**.
   
    ![Einmaliges Anmelden konfigurieren][6]

4. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Optimizely** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][7] 

5. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Optimizely anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    a. Geben Sie im Textfeld **Anmelde-URL** Folgendes ein: `https://app.optimizely.net/contoso`

    b. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `urn:auth0:optimizely:contoso`

    c. Klicken Sie auf **Weiter**. 

    > [!NOTE] 
    > Die Werte für **Anmelde-URL** und **Bezeichner** sind nur Platzhalter für die tatsächlichen Werte. Anweisungen zum Abrufen der tatsächlichen Werte aus Optimizely finden Sie weiter unten in diesem Tutorial.

1. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Optimizely** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)
   
    a. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
   
    b. Kopieren Sie die **Dienst-URL für einmalige Anmeldung**.

2. Wenden Sie sich an das Supportteam von Optimizely, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:
   
   * Ihr heruntergeladenes Zertifikat 
   * Die Dienst-URL für einmaliges Anmelden
     
     Als Antwort auf Ihre E-Mail stellt Optimizely Ihnen die Anmelde-URL (SP-initiiertes SSO) und den Bezeichner (ID der Dienstanbieterentität).

3. Wechseln Sie zurück zur Seite **App-Einstellungen konfigurieren** , und führen Sie die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)
   
    a. Geben Sie im Textfeld **Anmelde-URL** die von Optimizely bereitgestellte **SP-initiierte SSO-URL** ein.
   
    b. Geben Sie im Textfeld **Bezeichner** die von Optimizely bereitgestellte **ID der Dienstanbieterentität** ein.
   
    c. Klicken Sie auf **Next**.

4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Optimizely** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][10]
   
    a. Wählen Sie die Bestätigung zur Konfiguration des einmaligen Anmeldens aus.
   
    b. Klicken Sie auf **Weiter**.

5. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

6. Melden Sie sich in einem anderen Browserfenster in Ihrer Optimizely-Anwendung als Administrator an.

7. Klicken Sie in der rechten oberen Ecke auf den Namen Ihres Kontos und dann auf **Account Settings**.
   
    ![Azure AD – einmaliges Anmelden](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

8. Aktivieren Sie auf der Registerkarte „Account“ das Kontrollkästchen **Enable SSO** (SSO aktivieren), das Sie im Abschnitt **Overview (Übersicht)** unter „Single Sign On“ finden.
   
    ![Azure AD – einmaliges Anmelden](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.
Wählen Sie in der Benutzerliste **Britta Simon**aus.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 
   
    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-an-optimizely-test-user"></a>Erstellen eines Optimizely-Testbenutzers
In diesem Abschnitt erstellen Sie in Optimizely einen Benutzer mit dem Namen Britta Simon.

1. Klicken Sie auf der Startseite auf **Collaborators** .

2. Klicken Sie auf **New Collaborator** , um einen neuen Projektmitarbeiter zum Projekt hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3. Geben Sie die E-Mail-Adresse ein, und weisen Sie ihr eine Rolle zu. Klicken Sie auf **Invite**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

1. Der Benutzer erhält eine E-Mail-Einladung. Der Benutzer muss sich mit dieser E-Mail-Adresse bei Optimizely anmelden.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Optimizely gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu Optimizely zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Optimizely**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Liste „Alle Benutzer“ den Eintrag **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Optimizely“ klicken, sollten Sie automatisch bei Ihrer Optimizely-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png

