---
title: 'Tutorial: Azure Active Directory-Integration mit 8x8 Virtual Office | Microsoft Docs'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und 8x8 Virtual Office konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 149896e820d7873752fc40ee8270a8fc00ee0b0c
ms.openlocfilehash: 96c5d5aafaff24d618f930a1b5fcedcc5c52a9a9
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Tutorial: Azure Active Directory-Integration mit 8x8 Virtual Office
In diesem Tutorial erfahren Sie, wie Sie 8x8 Virtual Office in Azure Active Directory (Azure AD) integrieren.

Diese Integration bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf 8x8 Virtual Office haben soll.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei 8x8 Virtual Office anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit 8x8 Virtual Office konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein für einmaliges Anmelden (Single-Sign On, SSO) geeignetes 8x8 Virtual Office-Abonnement

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

    To test the steps in this tutorial, you should follow these recommendations:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist, das einmalige Anmelden (SSO) von Microsoft Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von 8x8 Virtual Office über den Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Microsoft Azure AD

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Hinzufügen von 8x8 Virtual Office über den Katalog
Zum Konfigurieren der Integration von 8x8 Virtual Office in Azure AD müssen Sie 8x8 Virtual Office über den Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um 8x8 Virtual Office über den Katalog hinzuzufügen:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld die Zeichenfolge **8x8 Virtual Office**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Wählen Sie im Ergebnisbereich die Option **8x8 Virtual Office** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Auswählen der App im Katalog](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Microsoft Azure AD
Dieser Abschnitt veranschaulicht anhand eines Testbenutzers namens Britta Simon, wie das einmalige Anmelden (SSO) von Microsoft Azure AD in 8x8 Virtual Office konfiguriert und getestet wird.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in 8x8 Virtual Office als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in 8x8 Virtual Office muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als **Benutzernamen** in 8x8 Virtual Office zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Microsoft Azure AD mit 8x8 Virtual Office müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Microsoft Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden von Microsoft Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines 8x8 Virtual Office-Testbenutzers](#creating-a-8x8-virtual-office-test-user)** , um in 8x8 Virtual Office einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon das einmalige Anmelden von Microsoft Azure AD zu ermöglichen.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-microsoft-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Microsoft Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden (SSO) von Microsoft Azure AD im klassischen Portal und konfigurieren es in Ihrer 8x8 Virtual Office-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Microsoft Azure AD mit 8x8 Virtual Office die folgenden Schritte aus:**

1. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **8x8 Virtual Office** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei 8x8 Virtual Office anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 
3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus, und klicken Sie anschließend auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)
  1. Geben Sie im Textfeld **Antwort-URL** Folgendes ein: `https://sso.8x8.com/saml2`
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für 8x8 Virtual Office** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)
  1. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
5. Melden Sie sich bei Ihrem 8x8 Virtual Office-Mandanten als Administrator an.
6. Wählen Sie im Anwendungsbereich die Option **Virtual Office Account Mgr** (Virtual Office-Konto-Manager) aus.
   
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)
7. Wählen Sie das Konto **Business** (Unternehmen) aus, und klicken Sie anschließend auf die Schaltfläche **Sign In** (Anmelden).
   
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)
8. Klicken Sie auf der Menüleiste auf die Registerkarte **Accounts** (Konten).
   
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)
9. Klicken Sie in der Liste unter „Accounts“ (Konten) auf **Single Sign On** (Einmaliges Anmelden).
   
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)
10. Wählen Sie als Authentifizierungsmethode die Option **Single Sign On** (Einmaliges Anmelden) aus, und klicken Sie anschließend auf **SAML**.
    
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)
11. Kopieren Sie die **SAML-SSO-URL**, die **Dienst-URL für einmalige Abmeldung** und die **Aussteller-URL** aus Azure AD in die Felder für **Anmelde-URL**, **Abmelde-URL** und **Aussteller-URL** von 8x8 Virtual Office. 
    
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)
    
    ![App-seitige Konfiguration](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)
12. Klicken Sie auf die Schaltfläche **Durchsuchen**, um das aus Azure AD heruntergeladene Zertifikat hochzuladen, und dann auf die Schaltfläche **Speichern**.
13. Wählen Sie im klassischen Portal die Bestätigung für die Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]
14. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-8x8-virtual-office-test-user"></a>Erstellen eines 8x8 Virtual Office-Testbenutzers
In diesem Abschnitt wird in 8x8 Virtual Office ein Benutzer namens Britta Simon erstellt. 8x8 Virtual Office unterstützt die standardmäßig aktivierte Just-in-Time-Bereitstellung.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugriff auf 8x8 Virtual Office ein neuer Benutzer erstellt. 

>[!NOTE]
>Setzen Sie sich mit dem Supportteam von 8x8 Virtual Office in Verbindung, wenn Sie einen Benutzer manuell erstellen müssen. 
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf 8x8 Virtual Office erhält.

![Benutzer zuweisen][200]

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu 8x8 Virtual Office durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste die Option **8x8 Virtual Office**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
In diesem Abschnitt wird die Microsoft Azure AD-Konfiguration für einmaliges Anmelden mithilfe des Zugriffsbereichs getestet.

Wenn Sie im Zugriffsbereich auf die Kachel „8x8 Virtual Office“ klicken, sollten Sie automatisch bei Ihrer 8x8 Virtual Office-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png

