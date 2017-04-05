---
title: 'Tutorial: Azure Active Directory-Integration mit Splunk Enterprise und Splunk Cloud | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Splunk Enterprise und Splunk Cloud konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: b78e9b7161207a74880e912241d5e965b353d1c5
ms.lasthandoff: 03/28/2017


---

# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a>Tutorial: Azure Active Directory-Integration mit Splunk Enterprise und Splunk Cloud

In diesem Tutorial erfahren Sie, wie Sie Splunk Enterprise und Splunk Cloud in Azure Active Directory (Azure AD) integrieren.

Die Integration von Splunk Enterprise und Splunk Cloud in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Splunk Enterprise und Splunk Cloud hat.
- Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Splunk Enterprise und Splunk Cloud anzumelden (einmaliges Anmelden, SSO).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Splunk Enterprise und Splunk Cloud konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Splunk Enterprise- oder Splunk Cloud-Abonnement, für das einmaliges Anmelden aktiviert ist


>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
>

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Splunk Enterprise und Splunk Cloud aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a>Hinzufügen von Splunk Enterprise und Splunk Cloud aus dem Katalog
Zum Konfigurieren der Integration von Splunk Enterprise und Splunk Cloud in Azure AD müssen Sie Splunk Enterprise und Splunk Cloud aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Splunk Enterprise und Splunk Cloud aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendungen][4]

6. Geben Sie in das Suchfeld **Splunk Enterprise oder Splunk Cloud** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. Wählen Sie im Ergebnisbereich die Option **Splunk Enterprise und Splunk Cloud** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie anhand eines Testbenutzers namens Britta Simon das einmalige Anmelden von Azure AD mit Splunk Enterprise und Splunk Cloud.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Splunk Enterprise und Splunk Cloud als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Splunk Enterprise und Splunk Cloud muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Splunk Enterprise und Splunk Cloud zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Splunk Enterprise und Splunk Cloud müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für Splunk Enterprise und Splunk Cloud](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**, um eine Entsprechung von Britta Simon in Splunk Enterprise und Splunk Cloud zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden (SSO) von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Splunk Enterprise- und Splunk Cloud-Anwendung.


**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit Splunk Enterprise und Splunk Cloud zu konfigurieren:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Splunk Enterprise und Splunk Cloud** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
     
    ![Einmaliges Anmelden konfigurieren][6] 

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Splunk Enterprise und Splunk Cloud anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. Geben Sie im Textfeld **Anmelde-URL** die URL, die von Ihren Benutzern zur Anmeldung bei Splunk Enterprise und Splunk Cloud verwendet wird, nach folgendem Muster ein: `https://<splunkserverUrl>/en-US/app/launcher/home`
  2. Geben Sie im Textfeld **Bezeichner** die URL Ihres Splunk-Servers ein.
  3. Geben Sie im Textfeld **Antwort-URL** die URL nach folgendem Muster ein: `https://<splunkserver>/saml/acs`
  4. Klicken Sie auf **Weiter**.
 
4. Führen Sie auf der Seite **Einmaliges Anmelden bei Splunk Enterprise und Splunk Cloud konfigurieren** die folgenden Schritte aus:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.

5. Wenden Sie sich an das Supportteam von Splunk Enterprise und Splunk Cloud, um SSO für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:

    * Die heruntergeladenen **Verbundmetadaten**
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
    
    ![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
 
    ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
  
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a>Erstellen eines Testbenutzers für Splunk Enterprise und Splunk Cloud

In diesem Abschnitt erstellen Sie in Splunk Enterprise und Splunk Cloud einen Benutzer mit dem Namen Britta Simon. Arbeiten Sie mit dem Supportteam von Splunk Enterprise und Splunk Cloud zusammen, um die Benutzer zur Plattform von Splunk Enterprise und Splunk Cloud hinzuzufügen.


### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Splunk Enterprise und Splunk Cloud gewähren.

![Benutzer zuweisen][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Splunk Enterprise und Splunk Cloud zuzuweisen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste die Option **Splunk Enterprise und Splunk Cloud** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.

    ![Benutzer zuweisen][203]

4. Wählen Sie in der Benutzerliste **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Splunk Enterprise und Splunk Cloud“ klicken, sollten Sie automatisch bei Ihrer Splunk Enterprise und Splunk Cloud-Anwendung angemeldet werden.


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png

