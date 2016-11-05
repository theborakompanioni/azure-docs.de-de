---
title: 'Tutorial: Azure Active Directory-Integration mit SAP HANA Cloud Platform | Microsoft Docs'
description: Erfahren Sie, wie Sie SAP HANA Cloud Platform mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila

ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/26/2016
ms.author: jeedes

---
# <a name="tutorial:-azure-active-directory-integration-with-sap-hana-cloud-platform"></a>Tutorial: Azure Active Directory-Integration mit SAP HANA Cloud Platform
In diesem Tutorial wird die Integration von Azure und SAP HANA Cloud Platform erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SAP HANA Cloud Platform-Konto

Nach Abschluss dieses Tutorials können sich die SAP HANA Cloud Platform zugewiesenen Azure AD-Benutzer wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben mittels einmaliger Anmeldung bei der Anwendung anmelden.

> [!IMPORTANT]
> Sie müssen Ihre eigene Anwendung bereitstellen oder eine Anwendung auf Ihrem SAP HANA Cloud Platform-Konto abonnieren, um sich zu Testzwecken einmalig anzumelden. In diesem Tutorial wird eine Anwendung auf dem Konto bereitgestellt.
> 
> 

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SAP HANA Cloud Platform
2. Konfigurieren der einmaligen Anmeldung
3. Zuweisen einer Rolle an einen Benutzer
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790795.png "Scenario")

## <a name="enabling-the-application-integration-for-sap-hana-cloud-platform"></a>Aktivieren der Anwendungsintegration für SAP HANA Cloud Platform
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SAP HANA Cloud Platform aktivieren.

### <a name="to-enable-the-application-integration-for-sap-hana-cloud-platform,-perform-the-following-steps:"></a>So aktivieren Sie die Anwendungsintegration für SAP HANA Cloud Platform:
1. Klicken Sie im linken Navigationsbereich des Azure-Verwaltungsportals auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC700994.png "Applications")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749321.png "Add application")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC749322.png "Add an application from gallerry")
6. Geben Sie in das **Suchfeld** den Begriff **SAP HANA Cloud Platform** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790796.png "Application Gallery")
7. Wählen Sie im Ergebnisbereich **SAP HANA Cloud Platform** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![SAP Hana](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793929.png "SAP Hana")
   
   ## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SAP HANA Cloud Platform zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei in Ihren SAP HANA Cloud Platform-Mandanten hochladen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)

### <a name="to-configure-single-sign-on,-perform-the-following-steps:"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SAP HANA Cloud Platform** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Configure single sign-on](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC778552.png "Configure single sign-on")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SAP HANA Cloud Platform anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790797.png "Configure Single Sign-On")
3. Melden Sie sich in einem anderen Webbrowserfenster unter „https://account.\<Landschaftshost\>.ondemand.com/cockpit“ (z.B. *https://account.hanatrial.ondemand.com/cockpit*) beim SAP HANA Cloud Platform-Cockpit an.
4. Klicken Sie auf die Registerkarte **Vertrauen** .
   
   ![Vertrauen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790800.png "Trust")
5. Führen Sie im Verwaltungsabschnitt „Vertrauen“ die folgenden Schritte aus:
   
   ![Metadaten abrufen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793930.png "Get Metadata")
   
   1. Klicken Sie auf die Registerkarte **Lokale Dienstanbieter** .
   2. Klicken Sie zum Herunterladen der SAP HANA Cloud Platform-Metadatendatei auf **Metadaten abrufen**.
6. Führen Sie im klassischen Azure AD-Portal auf der Seite **App-URL konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790798.png "Configure App URL")
   
   1. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die von den Benutzern zur Anmeldung an der **SAP HANA Cloud Platform**-Anwendung verwendet wird. Dies ist die kontospezifische URL einer geschützten Ressource in der SAP HANA Cloud Platform-Anwendung. Die URL basiert auf dem folgenden Muster: *https://\<\>\<Kontoname\>.\<Landschaftshost\>.ondemand.com/\<Pfad\_zur\_geschützten\_Ressource\>* (z.B. *https://xleavep1941203872trial.hanatrial.ondemand.com/xleave*).
      
      > [!NOTE]
      > Dies ist die URL in Ihrer SAP HANA Cloud Platform-Anwendung, für deren Authentifizierung der Benutzer erforderlich ist.
      > 
      > 
   2. Öffnen Sie die heruntergeladene SAP HANA Cloud Platform-Metadatendatei, und suchen Sie die Registerkarte **Ns3:AssertionConsumerService** .
   3. Kopieren Sie den Wert des Attributs **Speicherort**, und fügen Sie ihn in das Textfeld **Antwort-URL für die SAP HANA Cloud Platform**ein.
7. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SAP HANA Cloud Platform** auf **Metadaten herunterladen**, und speichern Sie die Datei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790799.png "Configure Single Sign-On")
8. Führen Sie im SAP HANA Cloud Platform-Cockpit im Abschnitt **Lokale Dienstanbieter** die folgenden Schritte aus:
   
   ![Verwaltung von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793931.png "Trust Management")
   
   1. Klicken Sie auf **Bearbeiten**.
   2. Wählen Sie als **Konfigurationstyp** die Option **Benutzerdefiniert**.
   3. Belassen Sie als **Name des lokalen Dienstanbieters**den Standardwert.
   4. Klicken Sie zum Generieren eines **Signaturschlüssels** und eines **Signaturzertifikat**-Schlüsselpaars auf **Schlüsselpaar generieren**.
   5. Wählen Sie als **Prinzipalweitergabe** die Option **Deaktiviert**.
   6. Wählen Sie unter **Zwangsauthentifizierung** die Option **Deaktiviert**.
   7. Klicken Sie auf **Speichern**.
9. Klicken Sie auf die Registerkarte **Vertrauenswürdiger Identitätsanbieter**, und klicken Sie dann auf **Vertrauenswürdigen Identitätsanbieter hinzufügen**.
   
   ![Verwaltung von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790802.png "Trust Management")
   
   > [!NOTE]
   > Um die Liste der vertrauenswürdigen Identitätsanbieter verwalten zu können, müssen Sie den Konfigurationstyp „Benutzerdefinierte“ im Abschnitt „Lokale Dienstanbieter“ ausgewählt haben. Beim Standard-Konfigurationstyp haben Sie ein nicht-bearbeitbares und implizites Vertrauen gegenüber dem SAP ID-Dienst. Bei der Option „Keine“ haben Sie keine Vertrauenseinstellungen.
   > 
   > 
10. Klicken Sie auf die Registerkarte **Allgemein** und dann auf **Durchsuchen**, um die heruntergeladene Metadatendatei hochzuladen.
    
    ![Verwaltung von Vertrauensstellungen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC793932.png "Trust Management")
    
    > [!NOTE]
    > Nach dem Hochladen der Metadatendatei werden die Werte für **URL für einmaliges Anmelden**, **URL für einmaliges Abmelden** und **Signaturzertifikat** automatisch ausgefüllt.
    > 
    > 
11. Klicken Sie auf die Registerkarte **Attribute** .
12. Führen Sie im Abschnitt **Attribute** die folgenden Schritte aus:
    
    ![Attribute](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790804.png "Attributes")
    
    1. Fügen Sie durch Klicken auf **Assertion-Attribut hinzufügen**die folgenden Assertion-Based-Attribute hinzu:
       
       | Assertion-Attribut | Prinzipal-Attribut |
       | --- | --- |
       | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname |firstname |
       | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname |Lastname |
       | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress |E-Mail |
    
    > [!NOTE]
    > Die Konfiguration der Attribute hängt davon ab, wie die Anwendung(en) auf HPC entwickelt wurden, d. h. welche Attribute in der SAML-Antwort erwartet wird/werden und unter welchem Namen (Prinzipal-Attribut) sie auf dieses Attribut im Code zugreifen.
    > 
    > a.  Das **Standard-Attribut** im Screenshot dient nur der Veranschaulichung. Es ist nicht erforderlich, damit das Szenario funktioniert.  
    > 
    > b.  Die Namen und Werte für das im Screenshot gezeigte **Prinzipal Attribut** ist abhängig davon, wie die Anwendung entwickelt wird. Es ist möglich, dass die Anwendung andere Zuordnungen erfordert.
    > 
    > 
13. Wählen Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für SAP HANA Cloud Platform** die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC796933.png "Configure Single Sign-On")

Als optionalen Schritt können Sie Assertion-Gruppen für Ihren Azure Active Directory-Identitätsanbieter konfigurieren.

> [!NOTE]
> Mithilfe von Gruppen auf SAP HANA Cloud Platform können Sie auf dynamische Weise eine oder mehrere Rollen in Ihren SAP HANA Cloud Platform-Anwendungen zuweisen, durch die Werte der Attribute in der SAML 2.0-Assertion definiert. Beispiel: Wenn die Assertion das Attribut „*contract=temporary*“ enthält, kann es sein, dass Sie alle betroffenen Benutzer der Gruppe „*TEMPORÄR*“ hinzufügen möchten. Die Gruppe „*TEMPORÄR*“ enthält unter Umständen eine oder mehrere Rollen aus mindestens einer Anwendung, die unter Ihrem SAP HANA Cloud Platform-Konto bereitgestellt wurde.
> 
> Verwenden Sie Assertion-Gruppen, wenn Sie viele Benutzer einer oder mehreren Rolle(n) auf Ihrem SAP HANA Cloud Platform-Konto zuweisen möchten. Wenn Sie nur einen einzelnen Benutzer oder eine geringe Anzahl von Benutzern zu einer oder mehreren speziellen Rolle(n) zuweisen möchten, empfiehlt es sich, diese direkt im SAP HANA Cloud Platform-Cockpit auf der Registerkarte**Authorizations**zuzuweisen.
> 
> 

## <a name="assigning-a-role-to-a-user"></a>Zuweisen einer Rolle an einen Benutzer
Um Azure AD-Benutzer für die Anmeldung bei SAP HANA Cloud Platform zu aktivieren, müssen Sie ihnen Rollen in der SAP HANA Cloud Platform zuweisen.

### <a name="to-assign-a-role-to-a-user,-perform-the-following-steps:"></a>So weisen Sie einem Benutzer eine Rolle zu:
1. Melden Sie sich bei Ihrem **SAP HANA Cloud Platform** -Cockpit an.
2. Führen Sie die folgenden Schritte aus:
   
   ![Authorizations](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790805.png "Authorizations")
   
   1. Klicken Sie auf **Autorisierung**.
   2. Klicken Sie auf die Registerkarte **Benutzer** .
   3. Geben Sie im Textfeld **Benutzer** die E-Mail-Adresse des Benutzers ein.
   4. Klicken Sie auf **Zuweisen** , um den Benutzer einer Rolle zuzuweisen.
   5. Klicken Sie auf **Speichern**.

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-sap-hana-cloud-platform,-perform-the-following-steps:"></a>So weisen Sie SAP HANA Cloud Platform Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **SAP HANA Cloud Platform** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC790806.png "Assign Users")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-sap-hana-cloud-platform-tutorial/IC767830.png "Yes")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

<!--HONumber=Oct16_HO2-->


