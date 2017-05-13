---
title: 'Tutorial: Azure Active Directory-Integration mit iLMS | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und iLMS konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f8b63c22a3f5a6916264acd22a80649ac7cd12f
ms.openlocfilehash: 22c72020200138e78835ed7dd2661f18b824c785
ms.contentlocale: de-de
ms.lasthandoff: 05/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a>Tutorial: Azure Active Directory-Integration mit iLMS

In diesem Tutorial erfahren Sie, wie Sie iLMS in Azure Active Directory (Azure AD) integrieren.

Die Integration von iLMS in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf iLMS hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei iLMS anzumelden (einmaliges Anmelden, Single Sign-On).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit iLMS konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein iLMS-Abonnement, das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von iLMS aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-ilms-from-the-gallery"></a>Hinzufügen von iLMS aus dem Katalog
Zum Konfigurieren der Integration von iLMS in Azure AD müssen Sie iLMS aus dem Katalog zur Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um iLMS aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das **Azure Active Directory**-Symbol. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld den Suchbegriff **iLMS** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. Wählen Sie im Ergebnisbereich die Option **iLMS** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei iLMS basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in iLMS als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in iLMS muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert für den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in iLMS zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei iLMS müssen Sie die folgenden Bausteine ausführen:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines iLMS-Testbenutzers](#creating-an-ilms-test-user)**, um eine Entsprechung von Britta Simon in iLMS zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer iLMS-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in iLMS die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **iLMS** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für iLMS** die folgenden Schritte aus, wenn Sie die Anwendung im **IDP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    a. Fügen Sie im Textfeld**Bezeichner** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Identifier** kopieren.

    b. Fügen Sie im Textfeld **Antwort-URL** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Endpoint (URL)** kopieren. Der Wert weist folgendes Muster auf: `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`.

    >[!Note]
    >Hinweis: Der Wert „123456“ ist nur ein Beispiel für einen Bezeichner.

4. Aktivieren Sie **Erweiterte URL-Einstellungen anzeigen**, wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    Fügen Sie im Textfeld **Anmelde-URL** den Wert ein, den Sie im iLMS-Verwaltungsportal im Abschnitt **Service Provider** der SAML-Einstellungen aus dem Feld **Endpoint (URL)** kopieren. Der Wert weist folgendes Muster auf: `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`.        

5. Um die JIT-Bereitstellung zu aktivieren, erwartet Ihre iLMS-Anwendung die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie die folgenden Ansprüche für diese Anwendung. Sie können die Werte dieser Attribute im Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/4.png)
    
    Erstellen Sie die Attribute **department, region** und **division**, und fügen Sie die Namen dieser Attribute in iLMS hinzu. Alle oben gezeigten Attribute sind erforderlich.    

    > [!NOTE] 
    > Sie müssen die Option **Create Un-recognized User Account** in iLMS aktivieren, um diese Attribute zuzuordnen. Befolgen Sie die Anweisungen [hier](http://support.inspiredelearning.com/customer/portal/articles/2204526), um eine Vorstellung von der Konfiguration der Attribute zu erhalten.

6. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname | Attributwert |
    | ---------------| --------------- |    
    | division | user.department |
    | region | user.state |
    | department | user.jobtitle |

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.
    
    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein.
    
    d. Klicken Sie auf **OK**.

7. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. Klicken Sie auf die Schaltfläche **Save** .

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. Melden Sie sich in einem anderen Webbrowserfenster im **iLMS-Verwaltungsportal** als Administrator an.

10. Klicken Sie auf der Registerkarte **Settings** auf **SSO:SAML**, um die SAML-Einstellungen zu öffnen, und führen Sie die folgenden Schritte aus:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/1.png) 

    a. Erweitern Sie den Abschnitt **Service Provider**, und kopieren Sie die Werte für **Identifier** und **Endpoint (URL)**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/2.png) 

    b. Klicken Sie im Abschnitt **Identity Provider** auf **Import Metadata**.
    
    c. Wählen Sie die Datei mit **Metadaten**, die Sie aus dem Abschnitt **SAML-Signaturzertifikat** des Azure-Portals heruntergeladen haben.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    d. Wenn Sie die JIT-Bereitstellung aktivieren möchten, um iLMS-Konten für unbekannte Benutzer zu erstellen, führen Sie die folgenden Schritte aus:
        
       - Aktivieren Sie das Kontrollkästchen **Create Un-recognized User Account**.
       
       ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  Ordnen Sie die Attribute in Azure AD den Attributen in iLMS zu. Geben Sie in der Spalte mit den Attributen den Attributnamen oder den Standardwert an.

    e. Wechseln Sie zur Registerkarte **Business Rules**, und führen Sie die folgenden Schritte aus: 
        
       ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/5.png)

       - Aktivieren Sie das Kontrollkästchen **Create Un-recognized Regions, Divisions and Departments**, um Regionen, Sparten und Abteilungen zu erstellen, die zum Zeitpunkt des einmaligen Anmeldens noch nicht vorhanden waren.
        
       - Aktivieren Sie das Kontrollkästchen **Update User Profile During Sign-in**, um anzugeben, ob das Benutzerprofil bei jedem einmaligen Anmelden aktualisiert werden soll. 
        
       - Wenn das Kontrollkästchen **Update Blank Values for Non Mandatory Fields in User Profile** aktiviert ist, führen optionale Felder im Profil, die während des Anmeldevorgangs leer sind, dazu, dass das iLMS-Profil für diese Felder ebenfalls leere Werte enthält.
        
       - Aktivieren Sie das Kontrollkästchen **Send Error Notification Email**, und geben Sie die E-Mail-Adresse des Benutzers ein, der bei Fehlern Benachrichtigungs-E-Mails erhalten soll.

11. Klicken Sie auf die Schaltfläche **Save**, um die Änderungen zu speichern.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> Während der Einrichtung der App können Sie im [Azure-Portal](https://portal.azure.com) nun eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden** und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** im unteren Bereich auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
    
### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-ilms-test-user"></a>Erstellen eines iLMS-Testbenutzers

Die Anwendung unterstützt die Just-in-Time-Benutzerbereitstellung (JIT). Nach der Authentifizierung werden Benutzer in der Anwendung automatisch erstellt. JIT funktioniert, wenn Sie während der Konfiguration der SAML-Einstellungen im iLMS-Verwaltungsportal das Kontrollkästchen **Create Un-recognized User Account** aktiviert haben.

Wenn Sie manuell einen Benutzer erstellen müssen, führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei der iLMS-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der Registerkarte **Users** auf **Register User**, um die Seite **Register User** zu öffnen. 
   
   ![Mitarbeiter hinzufügen](./media/active-directory-saas-ilms-tutorial/3.png)

3. Führen Sie auf der Seite **Register User** die folgenden Schritte aus.

    ![Mitarbeiter hinzufügen](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    a. Geben Sie im Textfeld **First Name** den Vornamen ein, z.B. Britta.
   
    b. Geben Sie im Textfeld **Last Name** den Nachnamen ein, z.B. Simon.

    c. Geben Sie im Textfeld **Email ID** die E-Mail-Adresse des Kontos von Britta Simon ein.

    d. Wählen Sie in der Dropdownliste **Region** den Wert für die Region aus.

    e. Wählen Sie in der Dropdownliste **Division** den Wert für die Sparte aus.

    f. Wählen Sie in der Dropdownliste **Department** den Wert für die Abteilung aus.

    g. Klicken Sie auf **Speichern**.

    > [!NOTE] 
    > Sie können eine Registrierungs-E-Mail an den Benutzer senden, indem Sie das Kontrollkästchen **Send Registration Mail** aktivieren.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf iLMS gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon zu iLMS zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **iLMS** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „iLMS“ klicken, sollten Sie automatisch bei Ihrer iLMS-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png


