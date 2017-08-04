---
title: 'Tutorial: Azure Active Directory-Integration mit ServiceNow | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und ServiceNow und ServiceNow Express konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: f4180601e720351a8b0e95ccffe7712028cfba08
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017

---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Tutorial: Azure Active Directory-Integration mit ServiceNow
In diesem Tutorial erfahren Sie, wie Sie ServiceNow und ServiceNow Express in Azure Active Directory (Azure AD) integrieren.

Die Integration von ServiceNow und ServiceNow Express in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer Zugriff auf ServiceNow und ServiceNow Express haben soll.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei ServiceNow und ServiceNow Express anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Zum Konfigurieren der Azure AD-Integration mit ServiceNow und ServiceNow Express benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Für ServiceNow eine Instanz oder einen Mandanten von ServiceNow, Calgary-Version oder höher
* Für ServiceNow Express eine Instanz von ServiceNow Express, Helsinki-Version oder höher
* Für den ServiceNow-Mandanten muss das [SSO-Plug-In für mehrere Anbieter](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) aktiviert sein. Dazu kann [eine Serviceanfrage übermittelt werden](https://hi.service-now.com). 

> [!NOTE]
> Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.
> 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern.

## <a name="scenario-description"></a>Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von ServiceNow aus dem Katalog
2. Konfigurieren und Testen des einmaligen Anmeldens mit Azure AD für ServiceNow oder ServiceNow Express

## <a name="adding-servicenow-from-the-gallery"></a>Hinzufügen von ServiceNow aus dem Katalog
Zum Konfigurieren der Integration von ServiceNow oder ServiceNow Express in Azure AD müssen Sie ServiceNow aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen. 

**Führen Sie die folgenden Schritte aus, um ServiceNow aus dem Katalog hinzuzufügen:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **ServiceNow** ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. Wählen Sie im Ergebnisbereich **ServiceNow** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei ServiceNow und ServiceNow Express mithilfe eines Testbenutzers namens Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in ServiceNow als Gegenstück für einen Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in ServiceNow muss eine Linkbeziehung eingerichtet werden.
Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in ServiceNow zuweisen. Führen Sie die folgenden Schritte aus, um das einmalige Anmelden von Azure AD mit ServiceNow zu konfigurieren und zu testen:

1. **[Konfigurieren des einmaligen Anmeldens in Azure AD für ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens in Azure AD für ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
3. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines ServiceNow-Testbenutzers](#creating-a-servicenow-test-user)**, um eine Entsprechung von Britta Simon in ServiceNow zu erstellen, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
6. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

> [!NOTE]
> Wenn Sie ServiceNow konfigurieren möchten, können Sie Schritt 2 weglassen. Entsprechend können Sie Schritt 1 weglassen, wenn Sie ServiceNow Express konfigurieren möchten.
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Konfigurieren des einmaligen Anmeldens von Azure AD für ServiceNow
1. Klicken Sie im klassischen Azure AD-Portal auf der Anwendungsintegrationsseite für **ServiceNow** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ServiceNow anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Einmaliges Anmelden konfigurieren")

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC769497.png "App-URL konfigurieren")
   
    a. Geben Sie im Textfeld **ServiceNow-Anmelde-URL** die URL, die von Ihren Benutzern zum Anmelden bei Ihrer ServiceNow-Anwendung verwendet wird, im folgenden Format ein: `https://<instance-name>.service-now.com`.
   
    b. Geben Sie im Textfeld **Bezeichner** die URL, die von Ihren Benutzern zur Anmeldung bei der Kronos-Anwendung genutzt wird, im folgenden Format ein: `https://<instance-name>.service-now.com`.
   
    c. Klicken Sie auf **Weiter**

4. Damit Azure AD ServiceNow automatisch für die SAML-basierte Authentifizierung konfiguriert, geben Sie den Instanznamen, den Administratorbenutzernamen und das Administratorkennwort für ServiceNow in das Formular **Einmaliges Anmelden automatisch konfigurieren** ein, und klicken Sie auf *Konfigurieren*. Beachten Sie, dass dem angegebenen Administratorbenutzernamen in ServiceNow die Rolle **security_admin** zugewiesen sein muss, damit dies funktioniert. Um ServiceNow manuell für die Verwendung von Azure AD als SAML-Identitätsanbieter zu konfigurieren, klicken Sie auf **Diese Anwendung manuell für das einmalige Anmelden konfigurieren** und anschließend auf **Weiter**, und führen Sie die folgenden Schritte aus.
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "App-URL konfigurieren")

5. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für ServiceNow** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Einmaliges Anmelden konfigurieren")

6. Melden Sie sich bei Ihrer ServiceNow-Anwendung als Administrator an.

7. Aktivieren Sie das Plug-In *Integration – Multiple Provider Single Sign-On Installer* (Integration – SSO-Installationsprogramm für mehrere Anbieter), indem Sie die nächsten Schritte ausführen:
   
    a. Navigieren Sie im Navigationsbereich auf der linken Seite zum Abschnitt **Systemdefinition**, und klicken Sie anschließend auf **Plug-Ins**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Plug-in aktivieren")
   
    b. Suchen Sie nach *Integration – Multiple Provider Single Sign-On Installer* (Integration – SSO-Installationsprogramm für mehrere Anbieter).
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Plug-in aktivieren")
   
    c. Wählen Sie das Plug-In aus. Klicken Sie mit der rechten Maustaste, und wählen Sie **Activate/Upgrade** (Aktivieren/Upgrade).
   
    d. Klicken Sie auf die Schaltfläche **Aktivieren**.

8. Klicken Sie im Navigationsbereich auf der linken Seite auf **Properties**.  
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "App-URL konfigurieren")

9. Führen Sie im Dialogfeld **Multiple Provider SSO Properties** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "App-URL konfigurieren")
   
    a. Bei **Enable multiple provider SSO** wählen Sie **Yes** aus.
   
    b. Bei **Enable debug logging got the multiple provider SSO integration** wählen Sie **Yes** aus.
   
    c. Im Textfeld **The field on the user table that...** geben Sie **user_name** ein.
   
    d. Klicken Sie auf **Speichern**.

10. Klicken Sie im Navigationsbereich auf der linken Seite auf **x509 Certificates**.
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Einmaliges Anmelden konfigurieren")

11. Klicken Sie im Dialogfeld **X.509 Certificates** auf **New**.
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Einmaliges Anmelden konfigurieren")

12. Führen Sie im Dialogfeld **X.509 Certificates** die folgenden Schritte aus:
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Einmaliges Anmelden konfigurieren")
    
     a. Klicken Sie auf **Neu**.
    
     b. Geben Sie im Textfeld **Name** einen Namen für Ihre Konfiguration ein (z. B. **TestSAML2.0**).
    
     c. Wählen Sie **Aktiv**.
    
     d. Wählen Sie unter **Format** das Format **PEM** aus.
    
     e. Wählen Sie unter **Type** den Typ **Trust Store Cert** aus.
    
     f. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **PEM Certificate** ein.
    
     g. Klicken Sie auf **Aktualisieren**.

13. Klicken Sie im Navigationsbereich auf der linken Seite auf **Identity Providers**.
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Einmaliges Anmelden konfigurieren")

14. Klicken Sie im Dialogfeld **Identity Providers** auf **New**:
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Einmaliges Anmelden konfigurieren")

15. Klicken Sie im Dialogfeld **Identity Providers** auf **SAML2 Update1?**:
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Einmaliges Anmelden konfigurieren")

16. Führen Sie im Dialogfeld „SAML2 Update1 Properties“ folgende Schritte aus:
    
     ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für Ihre Konfiguration ein (z. B. **SAML 2.0**).

    b. Geben Sie im Textfeld **User Field** (Benutzerfeld) den Wert **email** oder **user_name** ein, je nachdem, welches Feld für die eindeutige Identifizierung von Benutzern in Ihrer ServiceNow-Bereitstellung verwendet wird. 

    > [!NOTE] 
    > Sie können Azure AD so konfigurieren, dass entweder die Azure AD-Benutzer-ID (Benutzerprinzipalname) oder die E-Mail-Adresse als eindeutiger Bezeichner im SAML-Token ausgegeben wird. Wechseln Sie dazu im klassischen Azure-Portal zum Abschnitt **ServiceNow > Attribute > Einmaliges Anmelden**, und weisen Sie dem **nameidentifier**-Attribut das gewünschte Feld zu. Der gespeicherte Wert für das ausgewählte Attribut in Azure AD (z.B. Benutzerprinzipalname) muss dem in ServiceNow gespeicherten Wert für das eingegebene Feld (z.B. user_name) entsprechen.

    c. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Identitätsanbieter-ID**, und fügen Sie ihn in das Textfeld **Identity Provider URL** ein.

    d. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Authentifizierungsanforderungs-URL**, und fügen Sie ihn in das Textfeld **Identity Provider's AuthnRequest** ein.

    e. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **Identity Provider‘s SingleLogoutRequest** ein.

    f. Geben Sie im Textfeld **ServiceNow Homepage** die URL zur Homepage Ihrer ServiceNow-Instanz ein.

    > [!NOTE] 
    > Die URL zur Homepage der ServiceNow-Instanz ist eine Verkettung von Ihrer **ServiceNow-Mandanten-URL** und **/navpage.do** (z.B. `https://fabrikam.service-now.com/navpage.do`).

    g. Geben Sie im Textfeld **Entity ID / Issuer** die URL Ihres ServiceNow-Mandaten ein.

    h. Geben Sie im Textfeld **Audience URL** die URL Ihres ServiceNow-Mandanten ein. 

    i. Geben Sie im Textfeld **Protocol Binding for the IDP's SingleLogoutRequest** die Zeichenfolge **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect** ein.

    j. Geben Sie im Textfeld „NameID Policy“ die Zeichenfolge **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**ein.

    k. Deaktivieren Sie **Create an AuthnContextClass**.

    l. Geben Sie unter **AuthnContextClassRef Method** die URL `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` ein. Dies ist nur erforderlich, wenn Ihre Organisation ausschließlich in der Cloud arbeitet. Wenn Sie AD FS oder MFA lokal für die Authentifizierung verwenden, sollten Sie diesen Wert nicht konfigurieren. 

    m. Im Textfeld **Clock Skew** geben Sie **60** ein.

    n. Für **Single Sign On Script** wählen Sie **MultiSSO_SAML2_Update1** aus.

    o. Als **x509 Certificate**wählen Sie das im vorherigen Schritt erstellte Zertifikat aus.

    p. Klicken Sie auf **Submit**. 

1. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Einmaliges Anmelden konfigurieren")

2. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Einmaliges Anmelden konfigurieren")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Konfigurieren des einmaligen Anmeldens von Azure AD für ServiceNow Express
1. Klicken Sie im klassischen Azure AD-Portal auf der Anwendungsintegrationsseite für **ServiceNow** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ServiceNow anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Einmaliges Anmelden konfigurieren")

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC769497.png "App-URL konfigurieren")
   
    a. Geben Sie im Textfeld **ServiceNow-Anmelde-URL** die URL, die von Ihren Benutzern zum Anmelden bei Ihrer ServiceNow-Anwendung verwendet wird, im folgenden Format ein: `https://<instance-name>.service-now.com`.
   
    b. Geben Sie im Textfeld **Aussteller-URL** die URL, die von Ihren Benutzern zur Anmeldung bei der ServiceNow-Anwendung genutzt wird, nach folgendem Muster ein: `https://<instance-name>.service-now.com`.
   
    c. Klicken Sie auf **Weiter**

4. Klicken Sie auf **Diese Anwendung manuell für einmaliges Anmelden konfigurieren** und dann auf **Weiter**, und führen Sie die folgenden Schritte aus.
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "App-URL konfigurieren")

5. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für ServiceNow** auf **Zertifikat herunterladen**, speichern Sie das Zertifikat lokal auf Ihrem Computer, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Einmaliges Anmelden konfigurieren")

6. Melden Sie sich bei Ihrer ServiceNow Express-Anwendung als Administrator an.

7. Klicken Sie links im Navigationsbereich auf **Einmaliges Anmelden**.  
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "App-URL konfigurieren")

8. Klicken Sie im Dialogfeld **Einmaliges Anmelden** oben rechts auf das Konfigurationssymbol, und legen Sie die folgenden Eigenschaften fest:
   
    ![App-URL konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "App-URL konfigurieren")
   
    a. Schieben Sie den Umschalter der Option **Enable multiple provider SSO** (SSO für mehrere Anbieter aktivieren) nach rechts.
   
    b. Schieben Sie den Umschalter für **Enable debug logging for the multiple provider SSO integration** (Debugprotokollierung für die SSO-Integration für mehrere Anbieter aktivieren) nach rechts.
   
    c. Im Textfeld **The field on the user table that...** geben Sie **user_name** ein.
9. Klicken Sie im Dialogfeld **Einmaliges Anmelden** auf **Neues Zertifikat hinzufügen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Einmaliges Anmelden konfigurieren")
10. Führen Sie im Dialogfeld **X.509 Certificates** die folgenden Schritte aus:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Einmaliges Anmelden konfigurieren")
    
    a. Geben Sie im Textfeld **Name** einen Namen für Ihre Konfiguration ein (z. B. **TestSAML2.0**).
    
    b. Wählen Sie **Aktiv**.
    
    c. Wählen Sie unter **Format** das Format **PEM** aus.
    
    d. Wählen Sie unter **Type** den Typ **Trust Store Cert** aus.
    
    e. Erstellen Sie eine Base64-codierte Datei aus dem heruntergeladenen Zertifikat.
    
    > [!NOTE]
    > Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
    > 
    > 
    
    f. Öffnen Sie das Base64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **PEM Certificate** ein.
    
    g. Klicken Sie auf **Aktualisieren**.
11. Klicken Sie im Dialogfeld **Einmaliges Anmelden** auf **Add New IdP** (Neuen IdP hinzufügen).
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Einmaliges Anmelden konfigurieren")
12. Führen Sie im Dialogfeld **Add New Identity Provider** (Neuen Identitätsanbieter hinzufügen) unter **Configure Identity Provider** (Identitätsanbieter konfigurieren) die folgenden Schritte aus:
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Einmaliges Anmelden konfigurieren")

    a. Geben Sie im Textfeld **Name** einen Namen für Ihre Konfiguration ein (z.B. **SAML 2.0**).

    b. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Identitätsanbieter-ID**, und fügen Sie ihn in das Textfeld **Identity Provider URL** ein.

    c. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Authentifizierungsanforderungs-URL**, und fügen Sie ihn in das Textfeld **Identity Provider's AuthnRequest** ein.

    d. Kopieren Sie im klassischen Azure AD-Portal den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **Identity Provider‘s SingleLogoutRequest** ein.

    e. Wählen Sie als **Identity Provider Certificate** (Identitätsanbieterzertifikat) das Zertifikat aus, das Sie im vorherigen Schritt erstellt haben.


1. Klicken Sie auf **Advanced Settings** (Erweiterte Einstellungen), und führen Sie unter **Additional Identity Provider Properties** (Weitere Identitätsanbietereigenschaften) die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Einmaliges Anmelden konfigurieren")
   
    a. Geben Sie im Textfeld **Protocol Binding for the IDP's SingleLogoutRequest** die Zeichenfolge **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect** ein.
   
    b. Geben Sie im Textfeld **NameID Policy** die Zeichenfolge **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified** ein.    
   
    c. Geben Sie in **AuthnContextClassRef Method** die Zeichenfolge **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password** ein.
   
    d. Deaktivieren Sie **Create an AuthnContextClass**.

2. Führen Sie unter **Additional Service Provider Properties** (Weitere Service Provider-Eigenschaften) die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Einmaliges Anmelden konfigurieren")
   
    a. Geben Sie im Textfeld **ServiceNow Homepage** die URL zur Homepage Ihrer ServiceNow-Instanz ein.
   
    > [!NOTE]
    > Die URL zur Homepage der ServiceNow-Instanz ist eine Verkettung Ihrer **ServiceNow-Mandanten-URL** und **/navpage.do** (z.B. `https://fabrikam.service-now.com/navpage.do`).
    > 
    > 
   
    b. Geben Sie im Textfeld **Entity ID / Issuer** die URL Ihres ServiceNow-Mandaten ein.
   
    c. Geben Sie im Textfeld **Audience URI** (Benutzergruppen-URI) die URL Ihres ServiceNow-Mandanten ein. 
   
    d. Im Textfeld **Clock Skew** geben Sie **60** ein.
   
    e. Geben Sie im Textfeld **User Field** (Benutzerfeld) den Wert **email** oder **user_name** ein, je nachdem, welches Feld für die eindeutige Identifizierung von Benutzern in Ihrer ServiceNow-Bereitstellung verwendet wird.
   
    > [!NOTE]
    > Sie können Azure AD so konfigurieren, dass entweder die Azure AD-Benutzer-ID (Benutzerprinzipalname) oder die E-Mail-Adresse als eindeutiger Bezeichner im SAML-Token ausgegeben wird. Wechseln Sie dazu im klassischen Azure-Portal zum Abschnitt **ServiceNow > Attribute > Einmaliges Anmelden**, und weisen Sie dem **nameidentifier**-Attribut das gewünschte Feld zu. Der gespeicherte Wert für das ausgewählte Attribut in Azure AD (z.B. Benutzerprinzipalname) muss dem in ServiceNow gespeicherten Wert für das eingegebene Feld (z.B. user_name) entsprechen.
    > 
    > 
   
    f. Klicken Sie auf **Speichern**. 

3. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Einmaliges Anmelden konfigurieren")

4. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für ServiceNow aktivieren.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>So konfigurieren Sie die Benutzerbereitstellung
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ServiceNow** auf **Benutzerbereitstellung konfigurieren**. 
   
    ![Benutzerbereitstellung](./media/active-directory-saas-servicenow-tutorial/IC769498.png "Benutzerbereitstellung")

2. Legen Sie auf der Seite **Geben Sie Ihre ServiceNow-Anmeldeinformationen ein, um die automatische Benutzerbereitstellung zu aktivieren.** die folgenden Konfigurationseinstellungen fest:
   
     a. Geben Sie im Textfeld **ServiceNow-Instanzname** den Namen des ServiceNow-Instanz ein.
   
     b. Geben Sie im Textfeld **Administratorbenutzername für ServiceNow** den Namen des ServiceNow-Administratorkontos ein.
   
     c. Geben Sie im Textfeld **ServiceNow-Administratorkennwort** das Kennwort für dieses Konto ein.
   
     d. Klicken Sie auf **Überprüfen**, um die Konfiguration zu überprüfen.
   
     e. Klicken Sie auf **Weiter**, um die Seite **Nächste Schritte** zu öffnen.
   
     f. Wenn Sie alle Benutzer für diese Anwendung bereitstellen möchten, wählen Sie**Dieser Anwendung automatisch alle Benutzerkonten im Verzeichnis bereitstellen**. 
   
    ![Nächste Schritte](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Nächste Schritte")
   
     g. Klicken Sie auf der Seite **Nächste Schritte** auf **Abgeschlossen**, um die Konfiguration zu speichern.

### <a name="creating-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
   
    b. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
   
    c. Klicken Sie auf **Weiter**.

6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
   
   b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
   
   c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
   
   d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
   
   e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    a. Notieren Sie den Wert von **Neues Kennwort**.
   
    b. Klicken Sie auf **Fertig stellen**.   

### <a name="creating-a-servicenow-test-user"></a>Erstellen eines ServiceNow-Testbenutzers
In diesem Abschnitt erstellen Sie in ServiceNow einen Benutzer namens Britta Simon. In diesem Abschnitt erstellen Sie in ServiceNow einen Benutzer namens Britta Simon. Wenden Sie sich an das Supportteam von ServiceNow, wenn Sie nicht wissen, wie Sie in Ihrem ServiceNow- oder ServiceNow Express-Konto einen Benutzer hinzufügen.

### <a name="assigning-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf ServiceNow gewähren.

![Benutzer zuweisen][200] 

**Um Britta Simon ServiceNow zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 

2. Wählen Sie in der Anwendungsliste den Eintrag **ServiceNow** aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 

4. Wählen Sie in der Liste „Alle Benutzer“ den Eintrag **Britta Simon**aus.

5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="testing-single-sign-on"></a>Testen der einmaligen Anmeldung
In diesem Abschnitt soll Ihre Azure AD-Konfiguration für das einmalige Anmelden mithilfe des Zugriffsbereichs getestet werden.

Wenn Sie im Zugriffsbereich auf die Kachel „ServiceNow“ klicken, sollten Sie automatisch bei Ihrer ServiceNow-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png

