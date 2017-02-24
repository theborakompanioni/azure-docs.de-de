---
title: 'Tutorial: Azure Active Directory-Integration mit New Relic | Microsoft Docs'
description: "Erfahren Sie, wie Sie New Relic mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/26/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 04a045f41965b093aab71e59cd9b5f328b44de84
ms.openlocfilehash: 5ae6afea4d55eb03624602f483a2dc9620e33bb7


---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Tutorial: Azure Active Directory-Integration mit New Relic
In diesem Tutorial wird erläutert, wie einmaliges Anmelden (SSO) für Azure Active Directory und New Relic eingerichtet wird.

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein New Relic-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die Azure Active Directory-Benutzer, die Sie New Relic zugewiesen haben, mittels einmaliger Anmeldung über den AAD-Zugriffsbereich anmelden.

1. Aktivieren der Anwendungsintegration für New Relic
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-new-relic-tutorial/IC797030.png "Szenario")

## <a name="enabling-the-application-integration-for-new-relic"></a>Aktivieren der Anwendungsintegration für New Relic
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für New Relic aktivieren.

**Um die Anwendungsintegration für New Relic zu aktivieren, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-new-relic-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-new-relic-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-new-relic-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **New Relic** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-new-relic-tutorial/IC797031.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **New Relic** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![New Relic](./media/active-directory-saas-new-relic-tutorial/IC797032.png "New Relic")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Konto in Azure Active Directory bei New Relic zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **New Relic** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-new-relic-tutorial/IC769534.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei New Relic anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-new-relic-tutorial/IC797033.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **New Relic-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer New Relic-Anwendung verwendete URL ein, und klicken Sie dann auf **Weiter**. 
   
   Bei der App-URL handelt es sich um die URL Ihres New Relic-Mandanten (z.B. *https://rpm.newrelic.com*):
   
   ![App-URL konfigurieren](./media/active-directory-saas-new-relic-tutorial/IC797034.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für New Relic** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-new-relic-tutorial/IC797035.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der **New Relic** -Unternehmenswebsite als Administrator an.
6. Klicken Sie im oberen Menü auf **Kontoeinstellungen**.
   
   ![Konteneinstellungen](./media/active-directory-saas-new-relic-tutorial/IC797036.png "Konteneinstellungen")
7. Klicken Sie auf die Registerkarte **Sicherheit und Authentifizierung** und anschließend auf die Registerkarte **Einmaliges Anmelden**.
   
   ![Einmaliges Anmelden](./media/active-directory-saas-new-relic-tutorial/IC797037.png "des einmaligen Anmeldens")
8. Führen Sie auf der Dialogfeldseite für SAML die folgenden Schritte aus:
   
   ![SAML](./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")
   
   1. Klicken Sie auf **Datei auswählen** , um Ihr heruntergeladenes Azure Active Directory-Zertifikat hochzuladen.
   2. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für New Relic** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteanmelde-URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Seite **Einmaliges Anmelden konfigurieren für New Relic** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmeldestart-URL** ein.
   4. Klicken Sie auf **Änderungen speichern**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-new-relic-tutorial/IC797039.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Damit sich Azure Active Directory-Benutzer bei New Relic anmelden können, müssen sie in New Relic bereitgestellt werden. Im Fall von New Relic ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten in New Relic die folgenden Schritte aus:**

1. Melden Sie sich bei der **New Relic** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im oberen Menü auf **Kontoeinstellungen**.
   
   ![Konteneinstellungen](./media/active-directory-saas-new-relic-tutorial/IC797040.png "Konteneinstellungen")
3. Klicken Sie links im Bereich **Konto** auf **Zusammenfassung** und anschließend auf **Benutzer hinzufügen**.
   
   ![Konteneinstellungen](./media/active-directory-saas-new-relic-tutorial/IC797041.png "Konteneinstellungen")
4. Führen Sie im Dialogfeld **Aktive Benutzer** die folgenden Schritte aus:
   
   ![Aktive Benutzer](./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktive Benutzer")
   
   1. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse eines gültigen Azure Active Directory-Benutzers ein, den Sie bereitstellen möchten.
   2. Wählen Sie für **Rolle** die Option **Benutzer** aus.
   3. Klicken Sie auf **Diesen Benutzer hinzufügen**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von New Relic-Benutzerkonten oder mithilfe der von New Relic bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Um New Relic Benutzer zuzuweisen, führen Sie die folgenden Schritte durch:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **New Relic** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-new-relic-tutorial/IC797043.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-new-relic-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Feb17_HO1-->


