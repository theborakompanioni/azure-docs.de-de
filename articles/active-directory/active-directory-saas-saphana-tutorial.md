---
title: 'Tutorial: Azure Active Directory-Integration mit SAP HANA | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und SAP HANA konfigurieren.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: a7e73f6ee763d1005ad85935cf2d8f6b24ecf116
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a>Tutorial: Azure Active Directory-Integration mit SAP HANA

In diesem Tutorial erfahren Sie, wie Sie SAP HANA in Azure Active Directory (Azure AD) integrieren.

Die Integration von SAP HANA in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf SAP HANA hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei SAP HANA anzumelden (Single Sign-On, SSO; einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration in SAP HANA konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein SAP HANA-Abonnement, das für das einmalige Anmelden aktiviert ist
- Eine lokal, in einer öffentlichen IaaS-Umgebung, auf virtuellen Azure-Computern oder in großen SAP-Instanzen in Azure ausgeführte HANA-Instanz
- Die XSA-Webschnittstelle für die Verwaltung sowie HANA Studio, installiert auf der HANA-Instanz

> [!NOTE]
> Es empfiehlt sich nicht, die Schritte in diesem Tutorial in einer SAP HANA-Produktionsumgebung zu testen. Testen Sie die Integration zuerst in der Entwicklungs- oder Stagingumgebung der Anwendung, und verwenden Sie erst danach die Produktionsumgebung.

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Verwenden Sie die Produktionsumgebung nur, wenn dies unbedingt erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von SAP HANA aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD

## <a name="adding-sap-hana-from-the-gallery"></a>Hinzufügen von SAP HANA aus dem Katalog
Zum Konfigurieren der Integration von SAP HANA in Azure AD müssen Sie SAP HANA aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um SAP HANA aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**. 

    ![Schaltfläche „Azure Active Directory“][1]

2. Navigieren Sie zu **Unternehmensanwendungen**. Wechseln Sie dann zu **Alle Anwendungen**.

    ![Blatt „Unternehmensanwendungen“][2]
    
3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“][3]

4. Geben Sie im Suchfeld **SAP HANA** ein, wählen Sie im Ergebnisbereich **SAP HANA** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen. 

    ![Die neue Anwendung](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei SAP HANA mithilfe einer Testbenutzerin namens Britta Simon.

Damit das einmalige Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in SAP HANA als Pendant eines Benutzers in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in SAP HANA muss eine Linkbeziehung eingerichtet werden.

Weisen Sie den Wert für **Benutzername** aus Azure AD in SAP HANA als Wert für **Benutzername** zu, um eine Linkbeziehung herzustellen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei SAP HANA müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines SAP HANA-Testbenutzers](#creating-a-sap-hana-test-user)**, um ein Pendant von Britta Simon in SAP HANA zu erhalten, das mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testing Single Sign-On](#testing-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal und konfigurieren das einmalige Anmelden in Ihrer SAP HANA-Anwendung.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei SAP HANA die folgenden Schritte aus:**

1. Klicken Sie im Azure-Portal auf der Anwendungsintegrationsseite für **SAP HANA** auf **Einmaliges Anmelden**.

    ![Einmaliges Anmelden konfigurieren][4]

2. Wählen Sie im Dialogfeld **Einmaliges Anmelden** als **Modus** die Option **SAML-basierte Anmeldung** aus, um einmaliges Anmelden zu aktivieren.
 
    ![Dialogfeld „Einmaliges Anmelden“](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. Führen Sie im Abschnitt **Domäne und URLs für SAP HANA** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    a. Geben Sie im Textfeld **Bezeichner** Folgendes ein: `HA100` 

    b. Geben Sie im Textfeld **Antwort-URL** eine URL nach folgendem Muster ein: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`

    > [!NOTE] 
    > Hierbei handelt es sich um Beispielwerte. Aktualisieren Sie diese Werte mit dem eigentlichen Bezeichner und der Antwort-URL. Wenden Sie sich an das [Supportteam für den SAP HANA-Client](https://cloudplatform.sap.com/contact.html), um diese Werte zu erhalten. 

4. Klicken Sie im Abschnitt **SAML-Signaturzertifikat** auf **Metadaten-XML**, und speichern Sie die Metadatendatei dann auf Ihrem Computer.

    ![Downloadlink für das Zertifikat](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    >Wenn das Zertifikat nicht aktiv ist, aktivieren Sie es, in dem Sie in Azure AD auf das Kontrollkästchen „Neues Zertifikat aktivieren“ klicken. 

5. Die SAP HANA-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen: Hier wurde die **Benutzer-ID** mit der **ExtractMailPrefix()**-Funktion von **user.mail** zugeordnet. Das Ergebnis ist der Präfixwert der E-Mail des Benutzers, und dies ist die eindeutige Benutzer-ID. Diese wird in jeder erfolgreichen Antwort an die SAP HANA-Anwendung gesendet.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. Im Abschnitt **Benutzerattribute** des Dialogfelds **Einmaliges Anmelden**:

    a. Wählen Sie in der Dropdownliste **Benutzer-ID** den Eintrag **ExtractMailPrefix** aus.
    
    b. Wählen Sie in der Dropdownliste **E-Mail** den Eintrag **user.mail** aus.

7. Klicken Sie auf die Schaltfläche **Save** .

    ![Schaltfläche „Einmaliges Anmelden konfigurieren“](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. Zum Konfigurieren des einmaligen Anmeldens auf der Seite von **SAP HANA** melden Sie sich bei Ihrer **HANA XSA-Webkonsole** an, indem Sie zum entsprechenden HTTPS-Endpunkt navigieren.

    > [!Note]
    > In der Standardkonfiguration leitet die URL die Anforderung an einen Anmeldebildschirm weiter. Dafür sind die Anmeldeinformationen eines authentifizierten SAP HANA-Datenbankbenutzers erforderlich, um den Anmeldeprozess abzuschließen. Der Benutzer, der sich anmeldet, muss über die Berechtigungen verfügen, die für die Durchführung von SAML-Verwaltungsaufgaben erforderlich sind.

9. Navigieren Sie in der XSA-Webschnittstelle zu **SAML Identity Provider**, und klicken Sie am unteren Rand des Bildschirms auf die Schaltfläche **+**, um das Fenster „Add Identity Provider Info“ anzuzeigen. Führen Sie dort die folgenden Schritte aus:

    ![Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap1.png)

    a. Fügen Sie im Fenster **Add Identity Provider Info** den Inhalt der aus dem Azure-Portal heruntergeladenen XML-Metadatendatei in das Textfeld **Metadata** ein.

    ![Einstellungen für Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap2.png)

    b. Wenn der Inhalt des XML-Dokuments gültig ist, extrahiert der Analyseprozess die Informationen, die hier eingegeben werden müssen: in die Felder **Subject, Entity ID und Issuer** im Bildschirmbereich „General Data“ sowie in die URL-Felder im Bildschirmbereich „Destination“, z.B. **Base URL und SingleSignOn URL (*)**.

    ![Einstellungen für Identitätsanbieter hinzufügen](./media/active-directory-saas-saphana-tutorial/sap3.png)

    c. Geben Sie im Bildschirmbereich „General Data“ im Feld „Name“ einen Namen für den neuen SAML-SSO-Identitätsanbieter ein.

    > [!Note]
    > Der Name des SAML-Identitätsanbieters ist zwingend erforderlich und muss eindeutig sein. Er wird in der Liste der verfügbaren SAML-Identitätsanbieter aufgeführt, die angezeigt wird, wenn Sie SAML als Authentifizierungsmethode für SAP HANA-XS-Anwendungen auswählen, die z.B. in Bildschirmbereich „Authentication“ des XS Artifact Administration-Tools verwendet wird.

10. Speichern Sie die Details des neuen SAML-Identitätsanbieters. Wählen Sie **Save**, um die Details des SAML-Identitätsanbieters zu speichern und den Anbieter zur Liste der bekannten SAML-Identitätsanbieter hinzuzufügen.

    ![Schaltfläche „Speichern“](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. Filtern Sie in HANA Studio in den Systemeigenschaften der Registerkarte **Konfiguration** die Einstellungen nach **saml**, und ändern Sie den **assertion_timeout**-Wert von **10 sec** zu **120 sec**.

    ![assertion_timeout-Einstellung](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> Während Sie die App einrichten, können Sie im [Azure-Portal](https://portal.azure.com) eine Kurzfassung dieser Anweisungen lesen.  Nachdem Sie diese App aus dem Abschnitt **Active Directory > Unternehmensanwendungen** heruntergeladen haben, klicken Sie einfach auf die Registerkarte **Einmaliges Anmelden**, und rufen Sie die eingebettete Dokumentation über den Abschnitt **Konfiguration** um unteren Rand der Registerkarte auf. Weitere Informationen zur eingebetteten Dokumentation finden Sie hier: [Eingebettete Azure AD-Dokumentation]( https://go.microsoft.com/fwlink/?linkid=845985).
> 

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

![Azure AD-Benutzer erstellen][100]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **Azure-Portals** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. Wechseln Sie zu **Benutzer und Gruppen**, und klicken Sie auf **Alle Benutzer**, um die Liste der Benutzer anzuzeigen.
    
    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. Klicken Sie oben im Dialogfeld auf **Hinzufügen**, um das Dialogfeld **Benutzer** zu öffnen.
 
    ![Schaltfläche „Hinzufügen“](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. Führen Sie auf der Dialogfeldseite **Benutzer** die folgenden Schritte aus:
 
    ![Dialogfeld „Benutzer“](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    a. Geben Sie in das Textfeld **Name** den Namen **BrittaSimon** ein.

    b. Geben Sie in das Textfeld **Benutzername** die **E-Mail-Adresse** von Britta Simon ein.

    c. Wählen Sie **Kennwort anzeigen** aus, und notieren Sie sich den Wert des **Kennworts**.

    d. Klicken Sie auf **Erstellen**.
 
### <a name="creating-a-sap-hana-test-user"></a>Erstellen eines SAP HANA-Testbenutzers

Damit sich Azure AD-Benutzer bei SAP HANA anmelden können, müssen sie in SAP HANA bereitgestellt werden.
SAP HANA unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Wenn Sie manuell einen Benutzer erstellen müssen, führen Sie die folgenden Schritte aus:

>[!Note]
>Sie können die vom Benutzer verwendete externe Authentifizierung ändern.
Externe Benutzer werden mithilfe eines externen Systems authentifiziert, z.B. mit Kerberos. Um detaillierte Informationen zu externen Identitäten zu erhalten, wenden Sie sich an Ihren [Domänenadministrator](https://cloudplatform.sap.com/contact.html).

1. Öffnen Sie [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) als Administrator, und aktivieren Sie den Datenbankbenutzer für SAML-SSO.

    ![Benutzer erstellen](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. Aktivieren Sie das unsichtbare Kontrollkästchen links neben **SAML**, und folgen Sie dem Link „Configure“.

3. Klicken Sie auf **Add**, um den SAML-Identitätsanbieter hinzuzufügen, wählen Sie den geeigneten Anbieter aus, und klicken Sie auf **OK**.

4. Fügen Sie die **External Identity** (z.B. BrittaSimon) ein, oder wählen Sie **Any**, und klicken Sie auf **OK**.

    >[!Note]
    >Wenn das Kontrollkästchen „Any“ nicht aktiviert ist, muss der Benutzername in HANA exakt dem Namen des Benutzers im Benutzerprinzipalnamen vor dem Domänensuffix entsprechen (BrittaSimon@contoso.com würde also in HANA zu „BrittaSimon“).

5. Weisen Sie dem Benutzer zu Testzwecken alle **XS**-Rollen zu.

    ![Rollen zuweisen](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > Sie sollten nur die Berechtigungen zuweisen, die für Ihre Anwendungsfälle erforderlich sind.

6. Speichern Sie den Benutzer.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie Zugriff auf SAP HANA gewähren.

![Zuweisen der Benutzerrolle][200] 

**Um Britta Simon zu SAP HANA zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Öffnen Sie im Azure-Portal die Anwendungsansicht, navigieren Sie zur Verzeichnisansicht, wechseln Sie dann zu **Unternehmensanwendungen**, und klicken Sie auf **Alle Anwendungen**.

    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **SAP HANA** aus.

    ![Benutzer zuweisen](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. Klicken Sie im Menü auf der linken Seite auf **Benutzer und Gruppen**.

    ![Link „Benutzer und Gruppen“][202] 

4. Klicken Sie auf die Schaltfläche **Hinzufügen**. Wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“][203]

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Benutzerliste **Britta Simon** aus.

6. Klicken Sie im Dialogfeld **Benutzer und Gruppen** auf die Schaltfläche **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf **Zuweisen**.
    
### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „SAP HANA“ klicken, sollten Sie automatisch bei Ihrer SAP HANA-Anwendung angemeldet werden.
Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png


