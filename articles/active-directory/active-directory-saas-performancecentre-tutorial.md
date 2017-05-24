---
title: 'Tutorial: Azure Active Directory-Integration mit PerformanceCentre | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und PerformanceCentre konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 65288c32-f7e6-4eb3-a6dc-523c3d748d1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 3b1a347322e2307aadbd287cb2235e39e11f1e79
ms.contentlocale: de-de
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Tutorial: Azure Active Directory-Integration mit PerformanceCentre
Dieses Tutorial soll Ihnen zeigen, wie Sie PerformanceCentre in Azure Active Directory (Azure AD) integrieren können.  

Die Integration von PerformanceCentre in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer Zugriff auf PerformanceCentre hat. 
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei PerformanceCentre anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten: im klassischen Azure Active Directory-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit PerformanceCentre konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein PerformanceCentre-Abonnement, für das einmaliges Anmelden (SSO) aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.  

Das in diesem Tutorial beschriebene Szenario besteht im Wesentlichen aus diesen Bausteinen:

*  Hinzufügen von PerformanceCentre aus dem Katalog 
*  Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD

## <a name="add-performancecentre-from-the-gallery"></a>Hinzufügen von PerformanceCentre aus dem Katalog
Zum Konfigurieren der Integration von PerformanceCentre in Azure AD müssen Sie PerformanceCentre aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um PerformanceCentre aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie in das Suchfeld als Suchbegriff **PerformanceCentre**ein.
   
    ![Anwendungen][5]
7. Wählen Sie im Ergebnisbereich die Option **PerformanceCentre** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Anwendungen][500]

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden (SSO) von Azure AD in PerformanceCentre konfiguriert und getestet werden kann.

Damit einmaliges Anmelden (SSO) funktioniert, muss Azure AD wissen, welcher Benutzer in PerformanceCentre als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in PerformanceCentre muss eine Linkbeziehung eingerichtet werden.  

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD dem Benutzernamen ****in PerformanceCentre zuweisen.

**Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei PerformanceCentre müssen Sie die folgenden Bausteine ausführen:**

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines PerformanceCentre-Testbenutzers](#creating-a-halogen-software-test-user)** , um eine Entsprechung von Britta Simon in PerformanceCentre zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer PerformanceCentre-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens (SSO) von Azure AD bei PerformanceCentre die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure AD-Portal auf der Anwendungsintegrationsseite für **PerformanceCentre** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei PerformanceCentre anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][7] 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][8] 
   
   1. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei Ihrer PerformanceCentre-Website verwenden (beispielsweise *http://companyname.performancecentre.com/saml/SSO*).
   2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für PerformanceCentre** die folgenden Schritte aus:
   
    ![Azure AD – einmaliges Anmelden][9] 
   
  * Klicken Sie auf **Metadaten herunterladen**und speichern Sie die Datei auf Ihrem Computer.
5. Melden Sie sich bei Ihrer **PerformanceCentre** -Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Registerkarte links auf **Configure**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Registerkarte links auf **Verschiedenes** und anschließend auf **Einmaliges Anmelden**.
   
    ![Azure AD – einmaliges Anmelden][11]
8. Wählen Sie unter **Protokoll** die Option **SAML** aus.
   
    ![Azure AD – einmaliges Anmelden][12]
9. Öffnen Sie die heruntergeladene Metadatendatei, kopieren Sie den Inhalt, und fügen Sie ihn in das Textfeld **Identity Provider Metadata** (Metadaten des Identitätsanbieters) ein. Klicken Sie anschließend auf **Speichern**.
   
    ![Azure AD – einmaliges Anmelden][13]
10. Vergewissern Sie sich, dass die Werte für **Entity Base URL** (Entitätsbasis-URL) und **Entity ID URL** (Entitäts-ID-URL) korrekt sind.
    
     ![Azure AD – einmaliges Anmelden][14]
11. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
    
     ![Azure AD – einmaliges Anmelden][15]
12. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
     ![Azure AD – einmaliges Anmelden][16]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.  

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_09.png)  
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_05.png)  
   
   1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   3. Klicken Sie auf **Weiter**.
   
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_06.png) 
   
   1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   5. Klicken Sie auf **Weiter**.
   
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_08.png) 
   
   1. Notieren Sie den Wert von **Neues Kennwort**.
   2. Klicken Sie unten auf der Seite auf **Fertigstellen**.   

### <a name="create-a-performancecentre-test-user"></a>Erstellen eines PerformanceCentre-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in PerformanceCentre.

**Um einen Benutzer namens Britta Simon in PerformanceCentre zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrer PerformanceCentre-Unternehmenswebsite als Administrator an.
2. Klicken Sie im Menü auf der linken Seite auf **Interrelate** (Verknüpfen) und anschließend auf **Create Participant** (Teilnehmer erstellen).
   
    ![Benutzer erstellen][400]
3. Führen Sie auf der Dialogfeldseite **Interrelate - Create Participant** die folgenden Schritte aus:
   
    ![Benutzer erstellen][401]
   
   1. Geben Sie die erforderlichen Attribute für Britta Simon in die entsprechenden Textfelder ein.

    >[!IMPORTANT]
    >Das Attribut für den Benutzernamen von Britta in PerformanceCentre muss mit dem Benutzernamen in Azure AD identisch sein.
    > 
    > 
 
   2. Wählen Sie unter **Rolle auswählen** die Option **Clientadministrator** aus.
   3. Klicken Sie auf **Speichern**.   


### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) bei Azure zu ermöglichen, indem sie Zugriff auf PerformanceCentre erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon PerformanceCentre zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201]
2. Wählen Sie in der Anwendungsliste **PerformanceCentre**aus.
   
    ![Benutzer zuweisen][202]
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203]
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel „PerformanceCentre“ klicken, sollten Sie automatisch bei Ihrer PerformanceCentre-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_01.png
[500]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_02.png

[6]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_03.png
[8]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_04.png
[9]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_05.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png
[15]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_06.png
[16]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_50.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png
[402]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_402.png




