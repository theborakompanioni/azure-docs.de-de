---
title: 'Tutorial: Azure Active Directory-Integration mit Jobscience | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Jobscience mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 77282dcc-bbe2-4728-953d-adb4ab6a713b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 53fc622aa659f157f82dd27c5d6d2d8c9d8cc94b
ms.openlocfilehash: c881e28adf09440e329ddcae366ca787c868502b
ms.lasthandoff: 02/17/2017


---

# <a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Tutorial: Azure Active Directory-Integration mit Jobscience
In diesem Tutorial wird die Integration von Azure und Jobscience erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Jobscience-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Jobscience zugewiesen haben, mittels einmaliger Anmeldung auf der Jobscience-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Jobscience
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-jobscience-tutorial/IC784341.png "Szenario")

## <a name="enable-the-application-integration-for-jobscience"></a>Aktivieren der Anwendungsintegration für Jobscience
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Jobscience aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Jobscience die folgenden Schritte aus:**
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-jobscience-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-jobscience-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-jobscience-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Begriff **Jobscience** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-jobscience-tutorial/IC784342.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Jobscience** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Jobscience](./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Jobscience zu authentifizieren.  

Zum Konfigurieren des einmaligen Anmeldens für Jobscience müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Melden Sie sich bei der Jobscience-Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu **Setup**.
   
   ![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")
3. Klicken Sie im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
   
   ![Meine Domäne](./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meine Domäne")
4. Um zu überprüfen, ob Ihre Domäne richtig eingerichtet wurde, stellen Sie sicher, dass sich die Domäne in **Schritt 4 – bereitgestellt für Benutzer** befindet, und überprüfen Sie die **Domäneneinstellungen**.
   
   ![Domäne für Benutzer bereitgestellt](./media/active-directory-saas-jobscience-tutorial/IC784377.png "Domäne für Benutzer bereitgestellt")
5. Melden Sie sich in einem anderen Webbrowserfenster beim klassischen Azure-Portal an.
6. Klicken Sie auf der Anwendungsintegrationsseite für **Jobscience** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jobscience-tutorial/IC784360.png "Einmaliges Anmelden konfigurieren")
7. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Jobscience anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jobscience-tutorial/IC784361.png "Einmaliges Anmelden konfigurieren")
8. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Salesforce Sandbox-Anmelde-URL** die URL im Format *http://unternehmen.my.salesforce.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-jobscience-tutorial/IC784362.png "App-URL konfigurieren")
9. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Jobscience** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jobscience-tutorial/IC784363.png "Einmaliges Anmelden konfigurieren")
10. Klicken Sie auf der Jobscience-Unternehmenswebsite auf **Sicherheitskontrollen** und anschließend auf **Einstellungen für einmaliges Anmelden**.
    
    ![Sicherheitskontrollen](./media/active-directory-saas-jobscience-tutorial/IC784364.png "Sicherheitskontrollen")
11. Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
    
    ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-jobscience-tutorial/IC781026.png "Einstellungen für einmaliges Anmelden")
    
    1. Wählen Sie **SAML aktiviert**aus.
    2. Klicken Sie auf **Neu**.
12. Führen Sie im Dialogfeld **Einstellungen für einmaliges Anmelden mit SAML – Bearbeiten** die folgenden Schritte aus:
    
    ![SAML-Einstellungen für einmaliges Anmelden](./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML-Einstellungen für einmaliges Anmelden")
    
    1. Geben Sie im Textfeld **Name** einen Namen für die Konfiguration ein.
    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Jobscience** den Wert der **Aussteller-URL**, und fügen Sie ihn in das Textfeld **Aussteller** ein.
    3. Geben Sie im Textfeld **Entitäts-ID** die folgende URL ein: **https://salesforce-jobscience.com**.
    4. Klicken Sie auf **Durchsuchen** , um Ihr Azure AD-Zertifikat hochzuladen.
    5. Wählen Sie als **SAML-Identitätstyp** die Option **Assertion enthält die Verbund-ID aus dem Benutzerobjekt** aus.
    6. Wählen Sie für **SAML Identity Location** (Speicherort der SAML-Identität) die Option **Identity is in the NameIdentifier element of the Subject statement** (Identität ist im NameIdentifier-Element der Subject-Anweisung enthalten) aus.
    7. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Jobscience** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL des Identitätsanbieters** ein.
    8. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Jobscience** den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL des Identitätsanbieters** ein.
    9. Klicken Sie auf **Speichern**.
13. Klicken Sie im linken Navigationsbereich im Abschnitt **Verwalten** auf **Domänenverwaltung**, um den entsprechenden Abschnitt zu erweitern, und klicken Sie anschließend auf **Meine Domäne**, um die Seite **Meine Domäne** zu öffnen. 
    
    ![Meine Domäne](./media/active-directory-saas-jobscience-tutorial/IC767825.png "Meine Domäne")
14. Klicken Sie auf der Seite **Meine Domäne** im Abschnitt **Anmeldeseitenbranding** auf **Bearbeiten**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-jobscience-tutorial/IC767826.png "Anmeldeseitenbranding")
15. Im Abschnitt **Authentifizierungsdienst** der Seite **Anmeldeseitenbranding** wird der Name Ihrer **SAML-SSO-Einstellungen** angezeigt. Wählen Sie den Namen aus, und klicken Sie auf **Speichern**.
    
    ![Anmeldeseitenbranding](./media/active-directory-saas-jobscience-tutorial/IC784366.png "Anmeldeseitenbranding")
16. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-jobscience-tutorial/IC784367.png "Einmaliges Anmelden konfigurieren")

Klicken Sie zum Abrufen der Anmelde-URL für vom Dienstanbieter initiiertes einmaliges Anmelden im Menüabschnitt **Sicherheitskontrollen** auf **Einstellungen für einmaliges Anmelden**.

![Sicherheitskontrollen](./media/active-directory-saas-jobscience-tutorial/IC784368.png "Sicherheitskontrollen")

Klicken Sie auf das SSO-Profil, das Sie im obigen Schritt erstellt haben. Auf dieser Seite wird die URL für einmaliges Anmelden für Ihr Unternehmen angezeigt (z.B. *https://unternehmensname.my.salesforce.com?so=unternehmens-id*).

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren
Damit sich Azure AD-Benutzer bei Jobscience anmelden können, müssen sie in Jobscience bereitgestellt werden.  
Im Fall von Jobscience ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der **Jobscience** -Unternehmenswebsite als Administrator an.
2. Wechseln Sie zu „Setup“.
   
   ![Setup](./media/active-directory-saas-jobscience-tutorial/IC784358.png "Setup")
3. Navigieren Sie zu **Benutzer verwalten \> Benutzer**.
   
   ![Benutzer](./media/active-directory-saas-jobscience-tutorial/IC784369.png "Benutzer")
4. Klicken Sie auf **Neuer Benutzer**.
   
   ![Alle Benutzer](./media/active-directory-saas-jobscience-tutorial/IC784370.png "Alle Benutzer")
5. Führen Sie im Dialogfeld **Benutzer bearbeiten** die folgenden Schritte aus:
   
   ![Benutzer bearbeiten](./media/active-directory-saas-jobscience-tutorial/IC784371.png "Benutzer bearbeiten")
   
   1. Geben Sie den Vornamen, den Nachnamen, den Alias, die E-Mail-Adresse, den Benutzernamen und den Spitznamen des Azure AD-Benutzers, den Sie bereitstellen möchten, in die entsprechenden Textfelder ein.
   2. Klicken Sie auf **Speichern**.
   
   >[!NOTE]
   >Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
   > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Jobscience-Benutzerkonten oder mithilfe der von Jobscience bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Jobscience folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Jobscience** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-jobscience-tutorial/IC784372.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-jobscience-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


