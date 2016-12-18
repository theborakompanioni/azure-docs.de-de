---
title: 'Tutorial: Azure Active Directory-Integration mit IBM Kenexa Survey Enterprise | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und IBM Kenexa Survey Enterprise konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: c7aac6da-f4bf-419e-9e1a-16b460641a52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: d6cee6b4d47264bb0b91d3169490af0b08c05711


---
# <a name="tutorial-azure-active-directory-integration-with-ibm-kenexa-survey-enterprise"></a>Tutorial: Azure Active Directory-Integration mit IBM Kenexa Survey Enterprise
In diesem Tutorial erfahren Sie, wie Sie IBM Kenexa Survey Enterprise in Azure Active Directory (Azure AD) integrieren.

Die Integration von IBM Kenexa Survey Enterprise in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf IBM Kenexa Survey Enterprise hat.
* Sie können Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei IBM Kenexa Survey Enterprise anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit IBM Kenexa Survey Enterprise konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein IBM Kenexa Survey Enterprise-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von IBM Kenexa Survey Enterprise aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ibm-kenexa-survey-enterprise-from-the-gallery"></a>Hinzufügen von IBM Kenexa Survey Enterprise aus dem Katalog
Zum Konfigurieren der Integration von IBM Kenexa Survey Enterprise in Azure AD müssen Sie IBM Kenexa Survey Enterprise aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um IBM Kenexa Survey Enterprise aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **IBM Kenexa Survey Enterprise**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_01.png)
7. Wählen Sie im Ergebnisbereich **IBM Kenexa Survey Enterprise** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit IBM Kenexa Survey Enterprise basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in IBM Kenexa Survey Enterprise als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in IBM Kenexa Survey Enterprise muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert des **Benutzernamens** in IBM Kenexa Survey Enterprise zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei IBM Kenexa Survey Enterprise müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines IBM Kenexa Survey Enterprise-Testbenutzers](#creating-an-kenexasurvey-test-user)** , um ein Gegenstück zu Britta Simon in IBM Kenexa Survey Enterprise zu erhalten, das mit ihrer Repräsentation in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im klassischen Portal und konfigurieren das einmalige Anmelden in Ihrer IBM Kenexa Survey Enterprise-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei IBM Kenexa Survey Enterprise die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **IBM Kenexa Survey Enterprise** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei IBM Kenexa Survey Enterprise anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_03.png)
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_04.png)
   
    a. Geben Sie im Textfeld **Bezeichner** eine URL nach folgendem Muster ein: `https://surveys.kenexa.com/<company code>`
   
    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://surveys.kenexa.com/<company code>/tools/sso.asp`
   
    c. Klicken Sie auf **Weiter**.
   
   > [!NOTE]
   > Hinweis: Hierbei handelt es sich um Beispielwerte. Sie müssen diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL aktualisieren. Wenden Sie sich an das Supportteam von IBM Kenexa Survey Enterprise, um diese Werte zu erhalten.
   > 
   > 
4. Klicken Sie auf der Seite **Einmaliges Anmelden bei IBM Kenexa Survey Enterprise konfigurieren** auf **Zertifikat herunterladen**, und speichern Sie die Datei auf Ihrem Computer:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_05.png) 
5. Wenden Sie sich an das Supportteam von IBM Kenexa, um SSO (Single Sign-On, einmaliges Anmelden) für Ihre Anwendung konfigurieren zu lassen, und stellen Sie Folgendes bereit:
   
    •  Die heruntergeladene Zertifikatdatei
   
    • Die **Aussteller-URL**
   
    • Die **SAML-SSO-URL**
   
    •  Die **Dienst-URL für einmalige Abmeldung**
   
   > [!NOTE]
   > Beachten Sie, dass der NameID-Anspruchswert in der Antwort mit der im Kenexa-System konfigurierten SSO-ID übereinstimmen muss. Arbeiten Sie mit dem Kenexa-Supportteam zusammen, um die entsprechende Benutzer-ID in Ihrem Unternehmen als SSO-ID zuzuordnen. Standardmäßig legt Azure AD den NameIdentifier-Wert als UPN-Wert fest. Sie können dieses Verhalten auf der Registerkarte „Attribut“ ändern, wie in unten stehendem Screenshot gezeigt. Die Integration funktioniert nur nach erfolgreicher Zuordnung. 
   > 
   > 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_51.png)
6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.
   
    ![Azure AD – einmaliges Anmelden][10]
7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
   
    ![Azure AD – einmaliges Anmelden][11]
8. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **IBM Kenexa Survey Enterprise** im Menü oben auf **Attribute**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_06.png)
9. Führen Sie im Dialogfeld **SAML-Tokenattribute** die folgenden Schritte aus:
   
    a. Wählen Sie das Attribut von **NameIdentifier** und klicken Sie auf das Symbol **Bearbeiten**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_07.png)
   
    b. Geben Sie in der Liste **Attributwert** den Attributwert der SSO-ID ein, die im Kenexa-System konfiguriert ist.
   
    c. Klicken Sie unten auf der Seite auf **Abschließen**

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_09.png) 
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-kenexasurvey-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-an-ibm-kenexa-survey-enterprise-test-user"></a>Erstellen eines IBM Kenexa Survey Enterprise-Testbenutzers
In diesem Abschnitt erstellen Sie in IBM Kenexa Survey Enterprise einen Benutzer mit dem Namen Britta Simon. Arbeiten Sie mit dem IBM Kenexa-Supportteam zusammen, um die SSO-ID für alle Benutzer zuzuordnen. Dieser SSO-ID-Wert sollte auch dem NameIdentifier-Wert aus Azure AD zugeordnet werden. Sie können diese Standardeinstellungen auf der Registerkarte „Attribut“ ändern.

> [!NOTE]
> Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von IBM Kenexa Survey Enterprise in Verbindung.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie für Britta Simon das einmalige Anmelden von Azure, indem Sie ihr Zugriff auf IBM Kenexa Survey Enterprise gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu IBM Kenexa Survey Enterprise zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **IBM Kenexa Survey Enterprise**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kenexasurvey-tutorial/tutorial_kenexasurvey_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „IBM Kenexa Survey Enterprise“ klicken, sollten Sie automatisch bei Ihrer IBM Kenexa Survey Enterprise-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-kenexasurvey-tutorial/tutorial_general_205.png



<!--HONumber=Nov16_HO3-->


