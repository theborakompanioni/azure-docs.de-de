---
title: 'Tutorial: Azure Active Directory-Integration mit Samanage | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Samanage mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f0db4fb0-7eec-48c2-9c7a-beab1ab49bc2
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eacd69207e21a5a7c3ef71902bc6c20c37f2003c
ms.openlocfilehash: 0936f7e0b5e1967b629b31cac3c817d04a75287f
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-samanage"></a>Tutorial: Azure Active Directory-Integration mit Samanage
In diesem Tutorial erfahren Sie, wie Sie Samanage in Azure Active Directory (Azure AD) integrieren.

Die Integration von Samanage in Azure AD bietet folgende Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf Samanage haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei Samanage anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit Samanage konfigurieren zu können, benötigen Sie Folgendes:

* Ein gültiges Azure-Abonnement
* Einen Mandanten in Samanage

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Samanage über den Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="adding-samanage-from-the-gallery"></a>Hinzufügen von Samanage über den Katalog
Zum Konfigurieren der Integration von Samanage in Azure AD müssen Sie Samanage über den Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Samanage aus dem Katalog hinzuzufügen:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-samanage-tutorial/tutorial_general_01.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-samanage-tutorial/tutorial_general_02.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-samanage-tutorial/tutorial_general_03.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-samanage-tutorial/tutorial_general_04.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Salesforce** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_01.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Samanage** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Samanage](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_02.png "Samanage")

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in Samanage konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Samanage als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Samanage muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in Samanage zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei Samanage müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Samanage-Testbenutzers](#creating-a-Samanage-test-user)** , um in Samanage einen Gegenpart von Britta Simon zu erhalten, der mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-sso"></a>Konfigurieren der einmaligen Anmeldung (SSO) von Azure AD
In diesem Abschnitt ermöglichen Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer Samanage-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Samanage die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Samanage** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-samanage-tutorial/tutorial_general_05.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer Samanage anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_03.png "Microsoft Azure AD – einmaliges Anmelden")
3. Führen Sie auf der Dialogfeldseite zum Konfigurieren der App-Einstellungen die folgenden Schritte aus:
   
   ![App-URL konfigurieren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_04.png "App-URL konfigurieren") 
 1. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://<Company Name>.samanage.com/saml_login/<Company Name>`. 
 2. Klicken Sie auf **Weiter**.
 
   >[!NOTE]
   >Hinweis: Hierbei handelt es sich um Beispielwerte. Die Werte müssen mit der tatsächlichen Anmelde-URL aktualisiert werden. Informationen dazu, wie Sie diese Werte erhalten, finden Sie in Schritt 8.c. Alternativ können Sie sich auch an Samanage wenden.
   > 
 
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Samanage** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_05.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Samanage-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf **Dashboard**, und wählen Sie im linken Navigationsbereich **Setup** aus.
   
   ![Dashboard](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Dashboard")
7. Klicken Sie auf **Einmaliges Anmelden**.
   
   ![Einmaliges Anmelden](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_002.png "des einmaligen Anmeldens")
8. Führen Sie im Abschnitt **Login using SAML** (Mit SAML anmelden) die folgenden Schritte aus:
   
   ![Mit SAML anmelden](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_003.png "Mit SAML anmelden")
 1. Klicken Sie auf **Enable Single Sign-On with SAML**(Einmaliges Anmelden mit SAML aktivieren).  
 2. Geben Sie im Textfeld **Identitätsanbieter-URL** den Wert der **Identitätsanbieter-ID** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.    
 3. Vergewissern Sie sich, dass die **Anmelde-URL** der **Anmelde-URL** aus Schritt 3 entspricht.
 4. Geben Sie im Textfeld **Abmelde-URL** den Wert für die **Remoteabmelde-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.
 5. Geben Sie im Textfeld **SAML Issuer** (SAML-Aussteller) den in Ihrem Identitätsanbieter festgelegten App-ID-URI an.
 6. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Paste your Identity Provider x.509 Certificate below** (X.509-Zertifikat des Identitätsanbieters einfügen) ein.
 7. Klicken Sie auf **Create users if they do not exist in Samanage**(Benutzer erstellen, wenn sie nicht in Samanage vorhanden sind).
 8. Klicken Sie auf **Aktualisieren**.
9. Wählen Sie im klassischen Portal die Bestätigung für die Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_06.png "Einmaliges Anmelden konfigurieren")
10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_07.png "Einmaliges Anmelden konfigurieren")

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_00.png)

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_01.png)
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_02.png)
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_03.png)
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_04.png)
 1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus. 
 2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein. 
 3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_05.png) 
 1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.   
 2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein. 
 3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein. 
 4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus. 
 5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_06.png)
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-samanage-tutorial/create_aaduser_07.png)  
 1. Notieren Sie den Wert von **Neues Kennwort**. 
 2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-samanage-test-user"></a>Erstellen eines Samanage-Testbenutzers
Damit sich Azure AD-Benutzer bei Samanage anmelden können, müssen sie in Samanage bereitgestellt werden. Im Fall von Samanage ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Samanage-Unternehmenswebsite als Administrator an.
2. Klicken Sie auf **Dashboard**, und wählen Sie im linken Navigationsbereich die Option **Setup** aus.
   
   ![Setup](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_001.png "Setup")
3. Klicken Sie auf die Registerkarte **Benutzer** .
   
   ![Benutzer](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_006.png "Benutzer")
4. Klicken Sie auf **Neuer Benutzer**.
   
   ![Neuer Benutzer](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_007.png "Neuer Benutzer")
5. Geben Sie **Name** und **E-Mail-Adresse** eines vorhandenen Azure AD-Kontos ein, das Sie bereitstellen möchten, und klicken Sie auf **Benutzer erstellen**.
   
   ![Benutzer erstellen](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_008.png "Benutzer erstellen")
   
   >[!NOTE]
   >Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Samanage-Benutzerkonten oder mithilfe der von Samanage bereitgestellten APIs erstellen.
   >  

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf Samanage erhält.

![Zuweisen von Benutzern](./media/active-directory-saas-samanage-tutorial/assign_aaduser_00.png "Zuweisen von Benutzern")

**Führen Sie die folgenden Schritte aus, um die Zuweisung von Britta Simon zu Samanage durchzuführen:**

1. Klicken Sie in der Verzeichnisansicht des klassischen Portals auf der oberen Menüleiste auf **Anwendungen** , um die Anwendungsansicht zu öffnen.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-samanage-tutorial/assign_aaduser_01.png "Zuweisen von Benutzern")
2. Wählen Sie in der Anwendungsliste **Samanage**aus.
   
    ![Configure single sign-on](./media/active-directory-saas-samanage-tutorial/tutorial_samanage_08.png)
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-samanage-tutorial/assign_aaduser_02.png "Zuweisen von Benutzern")
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-samanage-tutorial/assign_aaduser_03.png "Zuweisen von Benutzern")

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Samanage“ klicken, sollten Sie automatisch bei Ihrer Samanage-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


