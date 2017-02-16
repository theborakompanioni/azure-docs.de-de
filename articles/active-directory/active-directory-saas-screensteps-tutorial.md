---
title: 'Tutorial: Azure Active Directory-Integration mit ScreenSteps | Microsoft Docs'
description: "Erfahren Sie, wie Sie ScreenSteps mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/03/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: dd50971e16da2cec046dd6dbe2f94dd58d01fb09
ms.openlocfilehash: c2f0e5b1348a25f4cb4c92b2a837fd22207e5dad


---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Tutorial: Azure Active Directory-Integration mit ScreenSteps
In diesem Tutorial wird die Integration von Azure und ScreenSteps erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein ScreenSteps-Mandant

Nach Abschluss dieses Tutorials können sich die ScreenSteps zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer ScreenSteps-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für ScreenSteps
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Szenario")

## <a name="enabling-the-application-integration-for-screensteps"></a>Aktivieren der Anwendungsintegration für ScreenSteps
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für ScreenSteps aktivieren.

### <a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für ScreenSteps:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie im **Suchfeld** den Suchbegriff **ScreenSteps** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich die Option **ScreenSteps** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei ScreenSteps zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ScreenSteps** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ScreenSteps anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Einmaliges Anmelden konfigurieren")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die ScreenSteps-Anmelde-URL** **die URL im Format „*https://\<Mandantenname\>.ScreenSteps.com*“ ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778521.png "App-URL konfigurieren")

4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für ScreenSteps** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich in einem anderen Webbrowserfenster bei der ScreenSteps-Unternehmenswebsite als Administrator an.

6. Klicken Sie auf **Kontoverwaltung**.
   
    ![Kontoverwaltung](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Kontoverwaltung")

7. Klicken Sie auf **Remoteauthentifizierung**.
   
    ![Remoteauthentifizierung](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remoteauthentifizierung")

8. Klicken Sie auf **Authentication Endpoint erstellen**.
   
    ![Remoteauthentifizierung](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remoteauthentifizierung")

9. Führen Sie im Abschnitt **Authentication Endpoint erstellen** die folgenden Schritte aus:
   
    ![Erstellen eines Authentifizierungsendpunkts](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Erstellen eines Authentifizierungsendpunkts")
   
    1. Geben Sie in das Textfeld **Titel** einen Titel ein.
    2. Wählen Sie in der Liste **Modus** die Option **SAML** aus.
    3. Klicken Sie auf **Erstellen**.

10. Führen Sie im Abschnitt **Remote Authentication Endpoint** die folgenden Schritte aus:
    
    ![Remote Authentication Endpoint](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote Authentication Endpoint")
    
    1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ScreenSteps** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteanmelde-URL** ein.

    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ScreenSteps** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteabmelde-URL** ein.

    3. Klicken Sie auf **Datei auswählen**, und laden Sie dann das heruntergeladene Zertifikat hoch.

    4. Klicken Sie auf **Aktualisieren**.

11. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure AD-Benutzer bei **ScreenSteps** anmelden können, müssen sie in **ScreenSteps** bereitgestellt werden.  
Im Fall von **ScreenSteps**ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten in ScreenSteps die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **ScreenSteps** -Mandanten an.

2. Klicken Sie auf **Kontoverwaltung**.
   
    ![Kontoverwaltung](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Kontoverwaltung")

3. Klicken Sie auf **Users**.
   
    ![Benutzer](./media/active-directory-saas-screensteps-tutorial/IC778544.png "Benutzer")

4. Klicken Sie auf **Benutzer erstellen**.
   
    ![Alle Benutzer](./media/active-directory-saas-screensteps-tutorial/IC778545.png "Alle Benutzer")

5. Wählen Sie aus der Liste **Benutzerrolle** eine Rolle für den Benutzer.

6. Geben Sie im Abschnitt „Benutzerrolle“ in die entsprechenden Textfelder die Daten für **Vorname**, **Nachname**, **E-Mail-Adresse**, **Anmeldung**, **Kennwort** und **Kennwortbestätigung** eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
   
    ![Neuer Benutzer](./media/active-directory-saas-screensteps-tutorial/IC778546.png "Neuer Benutzer")

7. Wählen Sie im Abschnitt Gruppen die Option **Authentifizierungsgruppe (SAML)** aus, und klicken Sie dann auf **Benutzer erstellen**.
   
    ![Gruppen](./media/active-directory-saas-screensteps-tutorial/IC778547.png "Gruppen")

> [!NOTE]
> Sie können ScreenSteps-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von ScreenSteps-Benutzerkonten oder mithilfe der von FM:Systems bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>So weisen Sie ScreenSteps Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **ScreenSteps** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Jan17_HO1-->


