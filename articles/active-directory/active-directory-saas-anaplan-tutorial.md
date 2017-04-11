---
title: 'Tutorial: Azure Active Directory-Integration mit Anaplan | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Anaplan konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4a9c2914-6c8c-4a88-93e3-3753afb40e6b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: c9c2f20f608748e5a7f1b17b384650104560c9dc
ms.openlocfilehash: cbd69cdffa9548d40ebbc7ecdd68501c1e89b24a
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-anaplan"></a>Tutorial: Azure Active Directory-Integration mit Anaplan
In diesem Tutorial erfahren Sie, wie Sie Anaplan in Azure Active Directory (Azure AD) integrieren.

Diese Integration bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Anaplan haben soll.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Anaplan anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Anaplan konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein Anaplan-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Sie sollten die Schritte in diesem Tutorial nicht in einer Produktionsumgebung testen. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Anaplan über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="add-anaplan-from-the-gallery"></a>Hinzufügen von Anaplan über den Katalog
Zum Konfigurieren der Integration von Anaplan in Azure AD müssen Sie Anaplan über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Anaplan über den Katalog hinzuzufügen:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld die Zeichenfolge **Anaplan**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_01.png)
7. Wählen Sie im Ergebnisbereich die Option **Anaplan** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_001.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Anaplan konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in Anaplan als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Anaplan muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in Anaplan zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Anaplan müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen einen Anaplan-Testbenutzers](#creating-a-anaplan-test-user)** , um in Anaplan einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Anaplan-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Anaplan die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Anaplan** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Anaplan anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_03.png) 
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_04.png)
  1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://sdp.anaplan.com/frontdoor/saml/<tenant name>`
  2. Klicken Sie auf **Weiter**.

    > [!NOTE] 
    > Der Wert der Anmelde-URL ist in diesem Tutorial nur ein Platzhalter. Den tatsächlichen Wert für Ihre Umgebung können Sie beim Support von Anaplan erfragen.
    > 
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Anaplan** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_05.png)  
  1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
5. Wenden Sie sich unter [support@anaplan.com](mailto:support@anaplan.com) an das Supportteam von Anaplan, um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:
   
   * Die heruntergeladene Metadatendatei
   * Die **Entitäts-ID**
   * Die **SAML-SSO-URL** 
   * Die **Dienst-URL für einmaliges Abmelden**
6. Wählen Sie im klassischen Portal die Bestätigung für die Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_06.png)
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-anaplan-tutorial/create_aaduser_08.png)
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-anaplan-test-user"></a>Erstellen eines Anaplan-Testbenutzers
In diesem Abschnitt erstellen Sie in Anaplan einen Benutzer namens Britta Simon. Arbeiten Sie über <mailto:support@anaplan.com> mit dem Supportteam von Anaplan zusammen, um Benutzer in der Anaplan-Plattform hinzuzufügen.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Anaplan erhält.

![Benutzer zuweisen][200]

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Anaplan durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste die Option **Anaplan**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-anaplan-tutorial/tutorial_anaplan_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Anaplan“ klicken, sollten Sie automatisch bei Ihrer Anaplan-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-anaplan-tutorial/tutorial_general_205.png

