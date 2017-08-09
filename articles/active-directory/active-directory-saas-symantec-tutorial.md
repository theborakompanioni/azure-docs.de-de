---
title: 'Tutorial: Azure Active Directory-Integration mit Symantec Web Security Service (WSS) | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Symantec Web Security Service (WSS) konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d6e4d893-1f14-4522-ac20-0c73b18c72a5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 61576d3a915d209e7355e04432e586dcf66e7c5a
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="tutorial-azure-active-directory-integration-with-symantec-web-security-service-wss"></a>Tutorial: Azure Active Directory-Integration mit Symantec Web Security Service (WSS)

In diesem Tutorial erfahren Sie, wie Sie Ihr Symantec Web Security Service-Konto (WSS) mit Ihrem Azure AD-Konto (Azure Active Directory) integrieren, sodass WSS einen Endbenutzer, der in Azure AD mithilfe der SAML-Authentifizierung bereitgestellt wurde, authentifizieren und Richtlinienregeln auf Benutzer- oder Gruppenebene erzwingen kann.

Die Integration von Symantec Web Security Service (WSS) in Azure AD bietet die folgenden Vorteile:

- Sie verwalten alle Benutzer und Gruppen, die vom WSS-Konto verwendet werden, über das Azure AD-Portal. 

- Sie erlauben den Endbenutzern die Authentifizierung bei WSS mit ihren Azure AD-Anmeldeinformationen.

- Sie aktivieren die Erzwingung von Richtlinienregeln auf Benutzer- und Gruppenebene, die in Ihrem WSS-Konto definiert sind.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Zum Konfigurieren der Azure AD-Integration mit Symantec Web Security Service (WSS) benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein Symantec Web Security Service-Konto (WSS)

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, sollten Sie kein WSS-Konto verwenden, das derzeit in der Produktionsumgebung verwendet wird.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie nicht Ihr WSS-Konto, das derzeit in der Produktionsumgebung verwendet wird, sofern dies nicht unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial konfigurieren Sie Ihr Azure AD, um einmaliges Anmelden bei WSS mit den Endbenutzer-Anmeldeinformationen, die in Ihrem Azure AD-Konto definiert sind, zu aktivieren.
Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen der Symantec Web Security Service-App (WSS) aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-symantec-web-security-service-wss-from-the-gallery"></a>Hinzufügen von Symantec Web Security Service (WSS) aus dem Katalog
Zum Konfigurieren der Integration von Symantec Web Security Service (WSS) in Azure AD müssen Sie Symantec Web Security Service (WSS) aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Führen Sie die folgenden Schritte aus, um Symantec Web Security Service (WSS) über den Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **Symantec Web Security Service (WSS)** ein, wählen Sie im Ergebnisbereich **Symantec Web Security Service (WSS)** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

    ![Symantec Web Security Service (WSS) in der Ergebnisliste](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Symantec Web Security Service (WSS) basierend auf einer Testbenutzerin mit dem Namen „Britta Simon“.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Symantec Web Security Service (WSS) als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Symantec Web Security Service (WSS) muss eine Linkbeziehung eingerichtet werden.

Weisen Sie in Symantec Web Security Service (WSS) den Wert des **Benutzernamens** in Azure AD als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Symantec Web Security Service (WSS) müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines Symantec Web Security Service-Testbenutzers (WSS)](#create-a-symantec-web-security-service-wss-test-user)**, um eine Entsprechung von Britta Simon in Symantec Web Security Service (WSS) zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#test-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer Symantec Web Security Service-Anwendung (WSS).

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Symantec Web Security Service (WSS) die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Symantec Web Security Service (WSS)** auf **Einmaliges Anmelden**.

    ![Konfigurieren des Links für einmaliges Anmelden][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für Symantec Web Security Service (WSS)** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für Symantec Web Security Service (WSS)](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_url.png)

    a. Geben Sie im Textfeld **Bezeichner** die folgende URL ein: `https://saml.threatpulse.net:8443/saml/saml_realm`

    b. Geben Sie im Textfeld **Antwort-URL** folgende URL ein: `https://saml.threatpulse.net:8443/saml/saml_realm/bcsamlpost`

    > [!NOTE]
    > Wenden Sie sich an das [Clientsupportteam von Symantec Web Security Service (WSS)](https://www.symantec.com/contact-us), wenn die Werte für **Bezeichner** und **Antwort-URL** nicht funktionieren.

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_certificate.png) 

5. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-symantec-tutorial/tutorial_general_400.png)
    
6. Weitere Informationen zum Konfigurieren des einmaligen Anmeldens auf Seiten von Symantec Web Security Service (WSS) finden Sie in der WSS-Onlinedokumentation. Die heruntergeladene **Metadaten-XML**-Datei muss in das WSS-Portal importiert werden. Wenden Sie sich an das [Supportteam von Symantec Web Security Service (WSS)](https://www.symantec.com/contact-us), wenn Sie Unterstützung bei der Konfiguration im WSS-Portal benötigen.

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

   ![Erstellen eines Azure AD-Testbenutzers][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Bereich des Azure-Portals auf die Schaltfläche **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-symantec-tutorial/create_aaduser_01.png)

2. Navigieren Sie zu **Benutzer und Gruppen**, und klicken Sie dann auf **Alle Benutzer**, um die Liste mit den Benutzern anzuzeigen.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-symantec-tutorial/create_aaduser_02.png)

3. Klicken Sie oben im Dialogfeld **Alle Benutzer** auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.

    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-symantec-tutorial/create_aaduser_03.png)

4. Führen Sie im Dialogfeld **Neuer Benutzer** die folgenden Schritte aus:

    ![Dialogfeld „Benutzer“](./media/active-directory-saas-symantec-tutorial/create_aaduser_04.png)

    a. Geben Sie in das Feld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie im Feld **Benutzername** die E-Mail-Adresse des Benutzers Britta Simon ein.

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld **Kennwort** angezeigt wird.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="create-a-symantec-web-security-service-wss-test-user"></a>Erstellen eines Symantec Web Security Service-Testbenutzers (WSS)

In diesem Abschnitt erstellen Sie in Symantec Web Security Service (WSS) einen Benutzer namens Britta Simon. Der entsprechende Endbenutzername kann manuell im WSS-Portal erstellt werden. Sie können aber auch warten, bis die Benutzer/Gruppen in Azure AD bereitgestellt und nach wenigen Mitnuten (~15 Minuten) mit dem WSS-Portal synchronisiert werden. Benutzer müssen erstellt und aktiviert werden, damit Sie einmaliges Anmelden verwenden können. Die öffentliche IP-Adresse des Endbenutzercomputers, der zum Browsen zu Websites verwendet wird, muss ebenfalls im Portal von Symantec Web Security Service (WSS) bereitgestellt werden.

> [!NOTE]
> [Klicken Sie hier](http://www.bing.com/search?q=my+ip+address&qs=AS&pq=my+ip+a&sc=8-7&cvid=29A720C95C78488CA3F9A6BA0B3F98C5&FORM=QBLH&sp=1), um die öffentliche IP-Adresse Ihres Computers zu erhalten.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Symantec Web Security Service (WSS) gewähren.

![Zuweisen der Benutzerrolle][200] 

**Führen Sie die folgenden Schritte aus, um Britta Simon Symantec Web Security Service (WSS) zuzuweisen:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste **Symantec Web Security Service (WSS)** aus.

    ![Symantec Web Security Service-Link (WSS) in der Anwendungsliste](./media/active-directory-saas-symantec-tutorial/tutorial_symantecwebsecurityservicewss_app.png)  

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202]

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt Testen Sie die Funktion des einmaligen Anmeldens, nachdem Sie Ihr WSS-Konto für die Verwendung von Azure AD für die SAML-Authentifizierung konfiguriert haben.

Nachdem Sie Ihren Webbrowser für die Weiterleitung von Datenverkehr an WSS konfiguriert haben, werden Sie auf die Azure-Anmeldeseite umgeleitet, wenn Sie in Ihrem Webbrowser öffnen und zu einer Website browsen. Geben Sie die Anmeldeinformationen des Testendbenutzers, der in Azure AD bereitgestellt wurde (d.h. BrittaSimon), und das zugehörige Kennwort ein. Nach der Authentifizierung können Sie zu der gewünschten Website browsen. Wenn Sie auf WSS-Seite eine Richtlinienregel erstellt haben, die verhindert, dass BrittaSimon zu einer bestimmten Website browst, sollte Ihnen die WSS-Sperrseite angezeigt werden, wenn Sie versuchen, als Benutzerin BrittaSimon zu dieser Website zu browsen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-symantec-tutorial/tutorial_general_203.png


