---
title: 'Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS) | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Amazon Web Services (AWS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 61999ebea05371c53e5ff27644a68039db1aef96
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Azure Active Directory-Integration mit Amazon Web Services (AWS)

In diesem Tutorial erfahren Sie, wie Sie Amazon Web Services (AWS) in Azure Active Directory (Azure AD) integrieren.

Die Integration von Amazon Web Services (AWS) in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer auf Amazon Web Services (AWS) Zugriff hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch für Amazon Web Services (AWS) anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

<!--## Overview

To enable single sign-on with Amazon Web Services (AWS), it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Amazon Web Services (AWS).

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit Amazon Web Services (AWS) konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Amazon Web Services-Abonnement (AWS), das für das einmalige Anmelden aktiviert ist

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Hinzufügen von Amazon Web Services (AWS) aus dem Katalog
Zum Konfigurieren der Integration von Amazon Web Services (AWS) in Azure AD müssen Sie Amazon Web Services (AWS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Amazon Web Services (AWS) aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Active Directory][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Anwendungen][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Hinzufügen**.

    ![Anwendungen][3]

4. Geben Sie im Suchfeld als Suchbegriff **Amazon Web Services (AWS)** ein.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_search.png)

5. Wählen Sie im Ergebnisbereich die Option **Amazon Web Services (AWS)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Amazon Web Services (AWS) basierend auf einer Testbenutzerin mit dem Namen Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Amazon Web Services (AWS) als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Amazon Web Services (AWS) muss eine Linkbeziehung eingerichtet werden.

Diese Linkbeziehung wird hergestellt, indem Sie den Wert des **Benutzernamens** in Azure AD als Wert dem **Benutzernamen** in Amazon Web Services (AWS) zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Amazon Web Services (AWS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Testbenutzers für Amazon Web Services](#creating-an-amazon-web-services-test-user)**, um eine Entsprechung von Britta Simon in Amazon Web Services (AWS) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Amazon Web Services-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Amazon Web Services (AWS) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Amazon Web Services (AWS)** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um das einmalige Anmelden zu aktivieren.
 
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_samlbase.png)

3. Im Abschnitt **Domäne und URLs für Amazon Web Services (AWS)** muss der Benutzer keine Schritte ausführen, da die App bereits in Azure integriert ist.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_url.png)

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die XML-Datei dann auf Ihrem Computer.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_certificate.png)

5. Ihre Amazon Web Services-Softwareanwendung erwartet die SAML-Assertions in einem bestimmten Format. Konfigurieren Sie für diese Anwendung die folgenden Ansprüche. Sie können die Werte dieser Attribute über den Abschnitt **Benutzerattribute** auf der Anwendungsintegrationsseite verwalten. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_attribute.png)

6. Konfigurieren Sie das SAML-Tokenattribut im Dialogfeld **Einmaliges Anmelden** im Abschnitt **Benutzerattribute**, wie im obigen Bild gezeigt, und führen Sie die folgenden Schritte aus:
    
    | Attributname  | Attributwert | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rolle               | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    
    >[!TIP]
    >Sie müssen die Benutzerbereitstellung in Azure AD so konfigurieren, dass alle Rollen aus der AWS-Konsole abgerufen werden. Im Folgenden finden Sie die erforderlichen Schritte.

    a. Klicken Sie auf **Attribut hinzufügen**, um das Dialogfeld **Benutzerattribut hinzufügen** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_04.png)

    b. Geben Sie im Textfeld **Name** den für die Zeile angezeigten Attributnamen ein.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_attribute_05.png)

    c. Geben Sie in der Liste **Wert** den für diese Zeile angezeigten Wert ein. Fügen Sie den oben angegebenen Namespace-Wert hinzu.
    
    d. Klicken Sie auf **OK**.

7. Klicken Sie auf die Schaltfläche **Speichern**, um die Änderungen zu speichern.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_400.png)

8. Melden Sie sich in einem anderen Browserfenster bei der Unternehmenswebsite von Amazon Web Services (AWS) als Administrator an.

9. Klicken Sie auf die **Startseite der Konsole**.
   
    ![Einmaliges Anmelden konfigurieren][11]

10. Klicken Sie in der Kategorie **Sicherheit, Identität und Compliance** auf **IAM**.
   
    ![Einmaliges Anmelden konfigurieren][12]

11. Klicken Sie auf **Identitätsanbieter**, und klicken Sie dann auf **Anbieter erstellen**.
   
    ![Einmaliges Anmelden konfigurieren][13]

12. Führen Sie auf der Dialogfeldseite **Anbieter konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren][14]
 
      a. Wählen Sie für **Anbietertyp** die Option **SAML** aus.

      b. Geben Sie im Textfeld **Anbietername** einen Anbieternamen ein (z.B.: *WAAD*).

      c. Klicken Sie auf **Datei auswählen**, um die heruntergeladene Metadatendatei hochzuladen.

      d. Klicken Sie auf **Nächster Schritt**.

13. Klicken Sie auf der Dialogfeldseite **Anbieterinformationen überprüfen** auf **Erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren][15]

14. Klicken Sie auf **Rollen**, und klicken Sie dann auf **Neue Rolle erstellen**. 
    
    ![Einmaliges Anmelden konfigurieren][16]

15. Führen Sie im Dialogfeld **Rollennamen festlegen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren][17] 

      a. Geben Sie im Textfeld **Rollenname** einen Rollennamen ein (z.B.: *TestUser*). 

      b. Klicken Sie auf **Nächster Schritt**.

16. Führen Sie im Dialogfeld **Rollentyp auswählen** die folgenden Schritte aus: 
    
    ![Einmaliges Anmelden konfigurieren][18] 

      a. Wählen Sie **Rolle für Identitätsanbieterzugriff**aus. 

      b. Klicken Sie im Abschnitt **Zugriff mit einmaliger Webanmeldung für SAML-Anbieter gewähren** auf **Auswählen**.

17. Führen Sie im Dialogfeld **Vertrauensstellung herstellen** die folgenden Schritte aus:  
    
    ![Einmaliges Anmelden konfigurieren][19] 

      a. Wählen Sie als SAML-Anbieter den zuvor erstellten SAML-Anbieter aus (z.B.: *WAAD*).
  
      b. Klicken Sie auf **Nächster Schritt**.

18. Klicken Sie im Dialogfeld **Vertrauenswürdigkeit der Rolle überprüfen** auf **Nächster Schritt**.
    
    ![Einmaliges Anmelden konfigurieren][32]

19. Klicken Sie im Dialogfeld **Richtlinie anhängen** auf **Nächster Schritt**.
    
    ![Einmaliges Anmelden konfigurieren][33]

20. Führen Sie im Dialogfeld **Überprüfung** die folgenden Schritte aus:
    
    ![Einmaliges Anmelden konfigurieren][34]
 
      a. Klicken Sie auf **Rolle erstellen**.

    b. Erstellen Sie so viele Rollen, wie Sie benötigen, und ordnen Sie diese dem Identitätsanbieter zu.

21. Konfigurieren Sie jetzt die Benutzerbereitstellung so, dass alle Rollen aus AWS abgerufen werden.

    a. Melden Sie sich mit Ihrem Stammkonto bei der AWS-Konsole an.

    b. Klicken Sie in der oberen rechten Ecke auf Ihren Namen, und klicken Sie dann auf die Option **My Security Credentials**. Daraufhin wird ein Bildschirm mit einer Warnmeldung geöffnet. Klicken Sie auf die Schaltfläche **Security Credentials**, um den Bildschirm zu übergehen.
        
       ![Einmaliges Anmelden konfigurieren][36]

       ![Einmaliges Anmelden konfigurieren][37]

    c. Klicken Sie im Abschnitt „Access Keys“ auf die Schaltfläche **Create New Access Key**. Dieser Vorgang generiert die ID des Zugriffsschlüssels und einen Tokenwert.
    
       ![Einmaliges Anmelden konfigurieren][38]

    d. Kopieren Sie beide Werte, und laden Sie sie auch herunter, damit sie nicht verloren gehen.

    e. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für Amazon Web Services (AWS) auf **Bereitstellung**.
        
       ![Einmaliges Anmelden konfigurieren][35]

    f. Legen Sie den Bereitstellungsmodus auf **Automatisch** fest.
        
       ![Einmaliges Anmelden konfigurieren][39]

    g. Fügen Sie in den Feldern **clientsecret** und **Geheimes Token** die entsprechenden Werte ein, die Sie aus der AWS-Konsole kopiert haben.
    
    h. Sie können auf die Schaltfläche **Verbindung testen** klicken, um die Verbindung zu testen. Wenn dieser Vorgang erfolgreich war, können Sie den Bereitstellungsconnector starten.
       
       ![Einmaliges Anmelden konfigurieren][40]

    i. Legen Sie den Bereitstellungsstatus jetzt auf **Ein** fest. Nun wird damit begonnen, die Rollen aus der Anwendung abzurufen.

       ![Einmaliges Anmelden konfigurieren][41]

    > [!NOTE]
    > Der Azure AD-Bereitstellungsdienst wird in bestimmten Zeitabständen ausgeführt, um die Rollen aus AWS zu synchronisieren. Es sollten alle Identitätsanbieterrollen aus AWS in Azure AD angezeigt werden, und Sie können diese beim Zuweisen der Anwendung zu Benutzern oder Gruppen verwenden.

<!--### Next steps

To ensure users can sign-in to Amazon Web Services (AWS) after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Amazon Web Services (AWS) prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Amazon Web Services (AWS) in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Amazon Web Services (AWS) users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, einen Testbenutzer namens Britta Simon im Azure-Portal zu erstellen.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-amazon-web-service-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-an-amazon-web-services-test-user"></a>Erstellen eines Testbenutzers für Amazon Web Services

Damit sich Azure AD-Benutzer bei Amazon Web Services (AWS) anmelden können, müssen sie in Amazon Web Services (AWS) bereitgestellt werden. Im Fall von Amazon Web Services (AWS) ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich bei der Unternehmenswebsite von **Amazon Web Services (AWS)** als Administrator an.

2. Klicken Sie auf das Symbol für die **Startseite der Konsole** . 
   
    ![Einmaliges Anmelden konfigurieren][11]

3. Klicken Sie auf „Identitäts- und Zugriffsverwaltung“. 
   
    ![Einmaliges Anmelden konfigurieren][28]

4. Klicken Sie im Dashboard auf **Benutzer** und dann auf **Neue Benutzer erstellen**. 
   
    ![Einmaliges Anmelden konfigurieren][29]

5. Führen Sie im Dialogfeld „Benutzer erstellen“ die folgenden Schritte aus: 
   
    ![Einmaliges Anmelden konfigurieren][30]   
    
    a. Geben Sie in die Textfelder unter **Benutzernamen eingeben** den Benutzernamen („userprincipalname“) von Britta Simon in Azure AD ein.

    b. Klicken Sie auf **Erstellen**.
        
### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Amazon Web Services (AWS) gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon Amazon Web Services (AWS) zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Amazon Web Services (AWS)** aus.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Benutzer zuweisen][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Benutzer zuweisen][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Wählen Sie auf der Registerkarte **Rolle auswählen** die geeignete Rolle für den Benutzer. Alle Rollen werden mit dem Namen der Rolle und dem Namen des Identitätsanbieters angezeigt. Auf diese Weise können Sie die Rollen aus AWS problemlos identifizieren.

8. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Amazon Web Services (AWS)“ klicken, sollten Sie automatisch in Ihrer Anwendung Amazon Web Services (AWS) angemeldet werden. 

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_203.png
[11]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_general_15.png

[28]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795037.png
[30]: ./media/active-directory-saas-amazon-web-service-tutorial/ic795038.png
[32]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service-tutorial/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[36]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/active-directory-saas-amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png

