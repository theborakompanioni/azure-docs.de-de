---
title: 'Tutorial: Azure Active Directory-Integration mit ClickTime | Microsoft Docs'
description: "Erfahren Sie, wie Sie ClickTime mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 0837cb33bf438fb7fd9665d21d411f0170cdd393
ms.openlocfilehash: d5171cdc9048837385bfb99d553e496a9f56846e
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Lernprogramm: Azure Active Directory-Integration mit ClickTime
In diesem Tutorial erfahren Sie, wie Sie ClickTime in Azure Active Directory (Azure AD) integrieren.

Die Integration von ClickTime in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf ClickTime Zugriff hat.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ClickTime anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit ClickTime konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein ClickTime-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung.

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ClickTime aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-clicktime-from-the-gallery"></a>Hinzufügen von ClickTime aus dem Katalog
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für ClickTime aktivieren.

### <a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ClickTime
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-clicktime-tutorial/tic700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-clicktime-tutorial/tic749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-clicktime-tutorial/tic749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **ClickTime** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-clicktime-tutorial/tic777275.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **ClickTime** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![ClickTime](./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ClickTime mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ClickTime als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ClickTime muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in ClickTime zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei ClickTime müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines ClickTime-Testbenutzers](#creating-a-clicktime-test-user)**, um eine Entsprechung von Britta Simon in ClickTime zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei ClickTime zu authentifizieren.  

> [!IMPORTANT]
> Damit Sie einmaliges Anmelden für Ihren ClickTime-Mandanten konfigurieren können, müssen Sie sich zunächst an den technischen Support von ClickTime wenden, um dieses Feature zu aktivieren.
> 
> 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei ClickTime die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ClickTime** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-clicktime-tutorial/tic777277.png "Einmaliges Anmelden aktivieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ClickTime anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tic777278.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 
   
    a. Geben Sie im Textfeld **IdentifierL** die URL nach folgendem Muster ein: **https://app.clicktime.com/sp/**
   
    b. Geben Sie im Textfeld **Reply URL** die URL nach folgendem Muster ein: **https://app.clicktime.com/Login/**
   
    c. Klicken Sie auf **Weiter**.
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um ClickTime** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tic777279.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der ClickTime-Unternehmenswebsite als Administrator an.
6. Klicken Sie oben auf der Symbolleiste auf **Preferences**, und klicken Sie dann auf **Sicherheitseinstellungen**.
7. Führen Sie im Konfigurationsabschnitt **Single Sign-On Preferences** die folgenden Schritte aus:
   
   ![Sicherheitseinstellungen](./media/active-directory-saas-clicktime-tutorial/tic777280.png "Sicherheitseinstellungen")
   
   a.  Wählen Sie **Zulassen** für das einmalige Anmelden (SSO) mit **Azure AD**.
   
   b.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ClickTime** den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **Identity Provider Endpoint (Identitätsanbieterendpunkt)** ein.
   
   c.  Öffnen Sie das Base64-codierte Zertifikat in **Editor**, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **X.509 Zertifikate** ein.
   
   d.  Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clicktime-tutorial/tic777281.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei ClickTime anmelden können, müssen sie in ClickTime bereitgestellt werden.  
Im Fall von ClickTime ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **ClickTime** -Mandanten an.
2. Klicken Sie auf der Symbolleiste oben auf **Firma** und dann auf **Personen**.
   
   ![Personen](./media/active-directory-saas-clicktime-tutorial/tic777282.png "Personen")
3. Klicken Sie auf **Person hinzufügen**.
   
   ![Person hinzufügen](./media/active-directory-saas-clicktime-tutorial/tic777283.png "Person hinzufügen")
4. Führen Sie im Abschnitt "New Person" die folgenden Schritte aus:
   
   ![Personen](./media/active-directory-saas-clicktime-tutorial/tic777284.png "Personen")
   
   a.  Geben Sie im Textfeld **email address** die E-Mail-Adresse Ihres Azure AD-Kontos ein.
   
   b.  Geben Sie im Textfeld **full name** den Namen des Azure AD-Kontos ein.  
   
   > [!NOTE]
   > Wenn Sie möchten, können Sie zusätzliche Eigenschaften des neuen Personenobjekts festlegen.
   > 
   > 
   
   c.  Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von ClickTime-Benutzerkonten oder mithilfe der von ClickTime bereitgestellten APIs erstellen.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon das einmalige Anmelden bei Azure, indem Sie ihr Zugriff auf ClickTime gewähren.

![Benutzer zuweisen][200]

Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Um Britta Simon ClickTime zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **ClickTime**aus.
   
    ![Configure single sign-on](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

## <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „ClickTime“ klicken, sollten Sie automatisch bei Ihrer ClickTime-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png

