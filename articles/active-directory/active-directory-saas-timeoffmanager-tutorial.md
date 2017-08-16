---
title: 'Tutorial: Azure Active Directory-Integration mit TimeOffManager | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und TimeOffManager konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 99523f27fe43f07081bd43f5d563e554bda4426f
ms.openlocfilehash: 3f944ffbf704694b293b4b1e5bdb4f2c93ae35a1
ms.contentlocale: de-de
ms.lasthandoff: 08/05/2017

---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Tutorial: Azure Active Directory-Integration mit TimeOffManager

In diesem Tutorial erfahren Sie, wie Sie TimeOffManager in Azure Active Directory (Azure AD) integrieren.

Die Integration von TimeOffManager in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf TimeOffManager hat.
- Sie können Benutzern ermöglichen, sich mit ihrem Azure AD-Konto automatisch bei TimeOffManager anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die TimeOffManager-Integration in Azure AD konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein TimeOffManager-Abonnement, für das einmaliges Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von TimeOffManager aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

## <a name="add-timeoffmanager-from-the-gallery"></a>Hinzufügen von TimeOffManager aus dem Katalog
Zum Konfigurieren der Integration von TimeOffManager in Azure AD müssen Sie TimeOffManager aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um TimeOffManager aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld **TimeOffManager** ein, wählen Sie im Ergebnisbereich **TimeOffManager** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Hinzufügen aus dem Katalog](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei TimeOffManager mithilfe einer Testbenutzerin namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in TimeOffManager als Pendant zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in TimeOffManager muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in TimeOffManager den Wert für **Benutzername** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit TimeOffManager zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines TimeOffManager-Testbenutzers](#create-a-timeoffmanager-test-user)**, um eine Entsprechung von Britta Simon in TimeOffManager zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer TimeOffManager-Anwendung.

**Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit TimeOffManager zu konfigurieren:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **TimeOffManager** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![SAML-basiertes Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für TimeOffManager** die folgenden Schritte aus:

     ![Abschnitt „Domäne und URLs für TimeOffManager“](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`

    > [!NOTE] 
    > Dieser Wert entspricht nicht dem tatsächlichen Wert. Aktualisieren Sie den Wert mit der richtigen Antwort-URL. Sie können diesen Wert von der **Seite „Einstellungen für einmaliges Anmelden“** abrufen, die später im Tutorial erläutert wird, oder wenden Sie sich an das [TimeOffManager-Supportteam](http://www.timeoffmanager.com/contact-us.aspx).
 
4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Zertifikat (Base64)**, und speichern Sie die Zertifikatdatei auf Ihrem Computer.

    ![Abschnitt für SAML-Signaturzertifikat](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. In diesem Abschnitt wird erläutert, wie Sie Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei TimeOffManager zu authentifizieren.
    
    Die TimeOffManger-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der SAML-Tokenattribute benutzerdefinierte Attributzuordnungen hinzufügen. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![SAML-Tokenattribute](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "SAML-Tokenattribute")
    
    | Attributname | Attributwert |
    | --- | --- |
    | Firstname |User.givenname |
    | Lastname |User.surname |
    | E-Mail |User.mail |
    
    a.  Klicken Sie für jede Datenzeile in der obigen Tabelle auf **Benutzerattribut hinzufügen**.
    
    ![SAML-Tokenattribute](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "SAML-Tokenattribute")
    
    ![SAML-Tokenattribute](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "SAML-Tokenattribute")
    
    b.  Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
    
    c.  Geben Sie im Textfeld **Attributwert** den für die Zeile angezeigten Attributwert ein.
    
    d.  Klicken Sie auf **OK**.
    
6. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. Klicken Sie im Abschnitt **TimeOffManger-Konfiguration** auf **TimeOffManger konfigurieren**, um das Fenster **Anmeldung konfigurieren** zu öffnen. Kopieren Sie die **Abmelde-URL, die SAML-Entitäts-ID und die URL für den SAML-SSO-Dienst** aus dem Abschnitt **Kurzübersicht**.

    ![TimeOffManager-Konfigurationsabschnitt](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. Melden Sie sich in einem anderen Webbrowserfenster bei der TimeOffManager-Unternehmenswebsite als Administrator an.

9. Wechseln Sie zu **Konto \> Kontooptionen \> Einstellungen für einmaliges Anmelden**.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Einstellungen für einmaliges Anmelden")
7. Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Einstellungen für einmaliges Anmelden")
   
   a. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie anschließend das gesamte Zertifikat in das Textfeld **X.509-Zertifikat** ein.
   
   b. Fügen Sie in das Textfeld **IdP Issuer** den Wert der **SAML-Entitäts-ID** ein, den Sie aus dem Azure-Portal kopiert haben.
   
   c. Fügen Sie in das Textfeld **IdP Endpoint URL** den Wert der **URL für den SAML-SSO-Dienst** ein, den Sie aus dem Azure-Portal kopiert haben.
   
   d. Wählen Sie unter **SAML erzwingen** die Option **Nein** aus.
   
   e. Wählen Sie für **Auto-Create Users** (Benutzer automatisch erstellen), die Option **Ja** aus.
   
   f. Fügen Sie in das Textfeld **Abmelde-URL** den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.
   
   g. Klicken Sie auf **Save Changes**.

11. Kopieren Sie auf der Seite **Single Sign on settings** den Wert der **Assertion Consumer Service URL**, und fügen Sie ihn im Azure-Portal in das Textfeld **Antwort-URL** im Abschnitt **Domäne und URLs für TimeOffManager** ein. 

      ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Einstellungen für einmaliges Anmelden")

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Benutzer und Gruppen --> Alle Benutzer](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeldseite „Benutzer“](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-timeoffmanager-test-user"></a>Erstellen eines TimeOffManager-Testbenutzers

Damit sich Azure AD-Benutzer bei TimeOffManager anmelden können, müssen sie in TimeOffManager bereitgestellt werden.  

TimeOffManager unterstützt die bedarfsabhängige Benutzerbereitstellung. Für Sie steht kein Aktionselement zur Verfügung.  

Die Benutzer werden automatisch während der erstmaligen und einmaligen Anmeldung hinzugefügt.

>[!NOTE]
>Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von TimeOffManager-Benutzerkonten oder mithilfe der von TimeOffManager bereitgestellten APIs erstellen.
> 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf TimeOffManager gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon TimeOffManager zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **TimeOffManager** aus.

    ![TimeOffManager in App-Liste](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die TimeOffManager-Kachel klicken, sollten Sie automatisch bei Ihrer TimeOffManager-Anwendung angemeldet werden. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png


