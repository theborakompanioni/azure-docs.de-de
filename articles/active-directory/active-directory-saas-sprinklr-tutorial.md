---
title: 'Tutorial: Azure Active Directory-Integration mit Sprinklr | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Sprinklr mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b33938a1-25a5-484c-8e75-7dc6de2d534d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/08/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 451369e21e7471180b6cd8c77d62b157d0bcddff
ms.openlocfilehash: 19bd7971e172f32367ee2dae903389e52709a306
ms.lasthandoff: 12/14/2016


---
# <a name="tutorial-azure-active-directory-integration-with-sprinklr"></a>Tutorial: Azure Active Directory-Integration mit Sprinklr
In diesem Tutorial wird die Integration von Azure und Sprinklr erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Sprinklr-Mandant

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Sprinklr zugewiesen haben, mittels einmaligen Anmeldens auf der Sprinklr-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Sprinklr
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-sprinklr-tutorial/IC782900.png "Szenario")

## <a name="enabling-the-application-integration-for-sprinklr"></a>Aktivieren der Anwendungsintegration für Sprinklr
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Sprinklr aktivieren.

### <a name="to-enable-the-application-integration-for-sprinklr-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für Sprinklr:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-sprinklr-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-sprinklr-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-sprinklr-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-sprinklr-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie in das **Suchfeld** als Suchbegriff **Sprinklr** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-sprinklr-tutorial/IC782901.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **Sprinklr** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Sprinklr](./media/active-directory-saas-sprinklr-tutorial/IC782902.png "Sprinklr")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Sprinklr zu authentifizieren.  
Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Sprinklr** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/IC782903.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer Sprinklr anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/IC782904.png "Einmaliges Anmelden konfigurieren")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Sprinklr-Anmelde-URL** die URL im Format *https://\<Mandantenname\>.sprinklr.com* ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-sprinklr-tutorial/IC782905.png "App-URL konfigurieren")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Sprinklr** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/IC782906.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der Sprinklr-Unternehmenswebsite als Administrator an.

6. Wechseln Sie zu **Verwaltung \> Einstellungen**.
   
    ![Verwaltung](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

7. Wechseln Sie im linken Fensterbereich zu **Partner verwalten \> Einmaliges Anmelden**.
   
    ![Partner verwalten](./media/active-directory-saas-sprinklr-tutorial/IC782908.png "Partner verwalten")

8. Klicken Sie auf **+Einmalige Anmeldungen hinzufügen**.
   
    ![Einmaliges Anmelden](./media/active-directory-saas-sprinklr-tutorial/IC782909.png "einmaliges Anmelden")

9. Führen Sie im Dialogfeld **Einmaliges Anmelden** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden](./media/active-directory-saas-sprinklr-tutorial/IC782910.png "einmaliges Anmelden")
   
    a. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein (z.B.: *WAADSSOTest*).
   
    b. Wählen Sie **Aktiviert**.
   
    c. Wählen Sie **Neues SSO-Zertifikat verwenden**.

    d. Erstellen Sie eine **Base64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
      
    > [!TIP]
    > Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
   
    e. Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie ihn anschließend in das Textfeld **Zertifikat des Identitätsanbieters** ein.
   
    f. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Sprinklr** den Wert der **Identitätsanbieter-ID**, und fügen Sie ihn in das Textfeld **Entitäts-ID** ein.
   
    g. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Sprinklr** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.
   
    h. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Sprinklr** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.
   
    i. Wählen Sie für **SAML-Benutzer-ID-Typ** die Option **Assertion enthält sprinklr.com-Benutzernamen des Benutzers** aus.
   
    j. Wählen Sie für **SAML-Benutzer-ID-Speicherort** die Option **Benutzer-ID ist das Namensbezeichnerelement der Ausstellererklärung** aus.
   
    k. Schließen Sie das Textfeld mit **Speichern**.
       
    ![SAML](./media/active-directory-saas-sprinklr-tutorial/IC782911.png "SAML")

10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-sprinklr-tutorial/IC782912.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich AAD-Benutzer anmelden können, müssen sie für den Zugriff innerhalb der Sprinklr-Anwendung bereitgestellt werden.  
In diesem Abschnitt wird beschrieben, wie in Sprinklr AAD-Benutzerkonten erstellt werden.

### <a name="to-provision-a-user-account-in-sprinklr-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten in Sprinklr die folgenden Schritte aus:
1. Melden Sie sich bei der Sprinklr-Unternehmenswebsite als Administrator an.

2. Wechseln Sie zu **Verwaltung \> Einstellungen**.
   
    ![Verwaltung](./media/active-directory-saas-sprinklr-tutorial/IC782907.png "Verwaltung")

3. Wechseln Sie im linken Fensterbereich zu **Client verwalten \> Benutzer**.
   
    ![Einstellungen](./media/active-directory-saas-sprinklr-tutorial/IC782914.png "Einstellungen")

4. Klicken Sie auf **Benutzer hinzufügen**.
   
    ![Einstellungen](./media/active-directory-saas-sprinklr-tutorial/IC782915.png "Einstellungen")

5. Führen Sie im Dialogfeld **Benutzer bearbeiten** die folgenden Schritte aus:
   
    ![Benutzer bearbeiten](./media/active-directory-saas-sprinklr-tutorial/IC782916.png "Benutzer bearbeiten")
   
    a. Geben Sie in die Textfelder **E-Mail**, **Vorname** und **Nachname** die Informationen des Azure AD-Benutzerkontos ein, das Sie bereitstellen möchten.
   
    b. Wählen Sie **Kennwort deaktiviert**.
   
    c. Wählen Sie eine **Sprache**aus.
   
    d. Wählen Sie einen **Benutzertyp**aus.
   
    e. Klicken Sie auf **Aktualisieren**.
   
    > [!IMPORTANT]
    > **Kennwort deaktiviert** muss ausgewählt sein, um einem Benutzer die Anmeldung über einen Identitätsanbieter zu ermöglichen.
    > 
    > 

6. Wechseln Sie zu **Rolle**, und führen Sie dann die folgenden Schritte aus:
   
    ![Partnerrollen](./media/active-directory-saas-sprinklr-tutorial/IC782917.png "Partnerrollen")
   
    a. Wählen Sie aus der Liste **Global** die Option **ALL\_Permissions** aus.
    
    b. Klicken Sie auf **Aktualisieren**.

> [!NOTE]
> Sie können Azure AD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Sprinklr-Benutzerkonten oder mithilfe der von Sprinklr bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-sprinklr-perform-the-following-steps"></a>So weisen Sie Sprinklr Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Sprinklr** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-sprinklr-tutorial/IC782918.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-sprinklr-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


