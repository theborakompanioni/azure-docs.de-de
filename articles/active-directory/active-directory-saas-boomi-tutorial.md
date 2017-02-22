---
title: 'Tutorial: Azure Active Directory-Integration mit Boomi | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Boomi konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na
ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 6a60db808388bf1fdf9441518920f2eb4e5fcc4d
ms.openlocfilehash: bafbfb710d8cdb370d21d9299c447a0dfed3c468


---
# <a name="tutorial-azure-active-directory-integration-with-boomi"></a>Tutorial: Azure Active Directory-Integration mit Boomi

In diesem Tutorial erfahren Sie, wie Sie Boomi in Azure Active Directory (Azure AD) integrieren.

Die Integration von Boomi in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Boomi hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Boomi anzumelden (Sigle Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Boomi benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SSO-fähiges Boomi-Abonnement


> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Boomi über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## <a name="adding-boomi-from-the-gallery"></a>Hinzufügen von Boomi über den Katalog
Zum Konfigurieren der Integration von Boomi in Azure AD müssen Sie Boomi aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Boomi aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **Boomi** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_01.png)

7. Wählen Sie im Ergebnisbereich **Boomi** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Boomi.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Boomi als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Boomi muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in Boomi zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Boomi müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Boomi-Testbenutzers](#creating-a-boomi-test-user)**, um eine Entsprechung von Britta Simon in Boomi zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden in Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Boomi-Anwendung zu konfigurieren.

Die Boomi-Anwendung erwartet die SAML-Assertion in einem bestimmten Format, weshalb Sie den Wert des NameIdentifier-Attributs mit der Verbund-ID des Benutzers festlegen müssen. Azure AD verwendet standardmäßig „UserPrincipalName“ für das NameIdentifier-Attribut. Für die erfolgreiche Integration müssen Sie diesen Wert jedoch anpassen, damit er mit der Verbund-ID des Benutzers in Boomi übereinstimmt. Sie können dieses Verhalten auf der Registerkarte **Attribut** ändern, wie in unten stehendem Screenshot gezeigt. Die Integration funktioniert nur nach erfolgreicher Zuordnung.

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_51.png)

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Boomi zu konfigurieren:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Boomi** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Boomi anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_04.png)

    a. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://platform.boomi.com/sso/<account name>/saml`

    b. Klicken Sie auf **Weiter**.

    > [!NOTE] 
    > Hinweis: Hierbei handelt es sich um einen Beispielwert. Sie müssen den Wert mit der richtigen Antwort-URL aktualisieren. Wenden Sie sich an das Supportteam von Boomi, um diesen Wert zu erhalten.

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Boomi** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_05.png) 

    > [!NOTE]
    > Beachten Sie, dass der NameID-Anspruchswert in der Antwort mit der im Boomi-System konfigurierten Verbund-ID übereinstimmen muss. Arbeiten Sie mit dem Boomi-Supportteam zusammen, um die entsprechende Benutzer-ID in Ihrem Unternehmen als Verbund-ID zuzuordnen. Standardmäßig legt Azure AD den NameIdentifier-Wert als UPN-Wert fest. Sie können dieses Verhalten auf der Registerkarte „Attribut“ ändern, wie in unten stehendem Screenshot gezeigt. Die Integration funktioniert nur nach erfolgreicher Zuordnung. 
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_51.png)

5. Melden Sie sich in einem anderen Webbrowserfenster bei der Boomi-Unternehmenswebsite als Administrator an. 

6. Navigieren Sie zu **Firmenname**, und wechseln Sie zu **Einrichten**.

7. Klicken Sie auf die Registerkarte **SSO-Optionen**, und führen Sie folgende Schritte aus.

    ![Einmaliges Anmelden auf App-Seite konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_06.png)

    a. Aktivieren Sie das Kontrollkästchen **Enable SAML Single Sign-On**.

    b. Klicken Sie auf **Importieren**, um das heruntergeladene Zertifikat aus Azure AD in das **Identitätsanbieterzertifikat** hochzuladen.
    
    c. Geben Sie im Textfeld **Anmelde-URL für Identitätsanbieter** den Wert für die **Remoteanmelde-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

    d. Wählen Sie als **Federation Id Location** das Optionsfeld **Federation Id is in NameID element of the Subject** aus. 

    e. Klicken Sie auf die Schaltfläche **Save** .

8. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

    ![Azure AD – einmaliges Anmelden][10]

9. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
  
    ![Azure AD – einmaliges Anmelden][11]


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_05.png) 

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_06.png) 

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-boomi-tutorial/create_aaduser_08.png) 

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.   



### <a name="creating-a-boomi-test-user"></a>Erstellen eines Boomi-Testbenutzers

Damit sich Azure AD-Benutzer bei Boomi anmelden können, müssen sie in Boomi bereitgestellt werden. Im Fall von Boomi ist die Bereitstellung eine manuelle Aufgabe.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:

1. Melden Sie sich bei der Boomi-Unternehmenswebsite als Administrator an.

2. Navigieren Sie nach der Anmeldung zu **Benutzerverwaltung** und wechseln Sie zu **Benutzer**.

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")

3. Klicken Sie auf das Symbol **+**. Das Dialogfeld **Benutzerrollen hinzufügen/verwalten** wird geöffnet.

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")

    ![Benutzer](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")

4. Geben Sie die **E-Mail-Adresse des Benutzers** ein.

5. Geben Sie den **Vornamen** und den **Nachnamen** des Benutzers ein.

6. Geben Sie die **Verbund-ID.** des Benutzers ein. Jeder Benutzer muss über eine Verbund-ID verfügen, die den Benutzer im Konto eindeutig identifiziert. 

7. Weisen Sie dem Benutzer die Rolle **Standardbenutzer** zu. Weisen Sie ihm nicht die Rolle „Administrator“ zu, da er dadurch normalen Zugriff auf Atmosphere sowie Zugriff über einmaliges Anmelden erhalten würde.

8. Klicken Sie auf **OK**.

    > [!NOTE]
    > Der Benutzer erhält keine Begrüßungs-E-Mail mit einem Kennwort, das zum Anmelden beim AtomSphere-Konto verwendet werden kann, da sein Kennwort über den Identitätsanbieter verwaltet wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Boomi-Benutzerkonten oder mithilfe der von Boomi bereitgestellten APIs erstellen. 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Boomi gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Boomi zuzuweisen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Boomi** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Benutzerliste **Britta Simon** aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
    
    ![Benutzer zuweisen][205]



### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Boomi“ klicken, sollten Sie automatisch bei Ihrer Boomi-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


