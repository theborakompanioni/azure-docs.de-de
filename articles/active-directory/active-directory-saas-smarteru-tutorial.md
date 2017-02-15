---
title: 'Tutorial: Azure Active Directory-Integration mit SmarterU | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie SmarterU mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 95fe3212-d052-4ac8-87eb-ac5305227e85
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/12/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 451369e21e7471180b6cd8c77d62b157d0bcddff
ms.openlocfilehash: 058f84f57c18735d42b95a74aee74468ccef0f8d


---
# <a name="tutorial-azure-active-directory-integration-with-smarteru"></a>Tutorial: Azure Active Directory-Integration mit SmarterU
In diesem Tutorial wird die Integration von Azure und SmarterU erläutert.  
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein SmarterU-Mandant

Nach Abschluss dieses Tutorials können sich die SmarterU zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer SmarterU-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für SmarterU
2. Konfigurieren der einmaligen Anmeldung
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-smarteru-tutorial/IC777320.png "Szenario")

## <a name="enabling-the-application-integration-for-smarteru"></a>Aktivieren der Anwendungsintegration für SmarterU
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für SmarterU aktivieren.

### <a name="to-enable-the-application-integration-for-smarteru-perform-the-following-steps"></a>So aktivieren Sie die Anwendungsintegration für SmarterU:
1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-smarteru-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-smarteru-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-smarteru-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-smarteru-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie in das **Suchfeld** **SmarterU** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-smarteru-tutorial/IC777321.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **SmarterU** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![SmarterU](./media/active-directory-saas-smarteru-tutorial/IC777322.png "SmarterU")

## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei SmarterU zu authentifizieren.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **SmarterU** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777323.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei SmarterU anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777324.png "Einmaliges Anmelden konfigurieren")

3. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für SmarterU** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal unter **c:\\SmarterUMetaData.cer**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777325.png "Einmaliges Anmelden konfigurieren")

4. Melden Sie sich in einem anderen Webbrowserfenster bei der SmarterU-Unternehmenswebsite als Administrator an.

5. Klicken Sie oben auf der Symbolleiste auf **Kontoeinstellungen**.
   
    ![Konteneinstellungen](./media/active-directory-saas-smarteru-tutorial/IC777326.png "Konteneinstellungen")

6. Führen Sie auf der Kontenkonfigurationsseite die folgenden Schritte aus:
   
    ![Externe Autorisierung](./media/active-directory-saas-smarteru-tutorial/IC777327.png "Externe Autorisierung")
 
    a. Wählen Sie **Externe Autorisierung aktivieren**.
   
    b. Wählen Sie im Abschnitt **Master-Anmeldesteuerelement** die Registerkarte **SmarterU** aus.
   
    c. Wählen Sie im Abschnitt **Benutzer-Standardanmeldung** die Registerkarte **SmarterU** aus.
   
    d. Wählen Sie **Okta aktivieren**.
   
    e. Kopieren Sie den Inhalt der heruntergeladenen Metadatendatei, und fügen Sie ihn in das Textfeld **Okta-Metadaten** ein.

    f. Klicken Sie auf **Speichern**.

7. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-smarteru-tutorial/IC777328.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung
Damit sich Azure AD-Benutzer bei SmarterU anmelden können, müssen sie in SmarterU bereitgestellt werden.  
Im Fall von SmarterU ist die Bereitstellung eine manuelle Aufgabe.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:
1. Melden Sie sich bei Ihrem **SmarterU** -Mandanten an.

2. Wechseln Sie zu **Benutzer**.

3. Führen Sie im Abschnitt "Benutzer“ die folgenden Schritte aus:
   
    ![Neuer Benutzer](./media/active-directory-saas-smarteru-tutorial/IC777329.png "Neuer Benutzer")
   
    a. Klicken Sie auf **+Benutzer**.
   
    b. Geben Sie die zugehörigen Attributwerte des Azure AD-Benutzerkontos in die folgenden Textfelder ein: **Primäre E-Mail-Adresse**, **Mitarbeiter-ID**, **Kennwort**, **Kennwort bestätigen**, **Angegebener Name**, **Nachname**.
   
    c. Klicken Sie auf **Aktiv**.
   
    d. Klicken Sie auf **Speichern**.

> [!NOTE]
> Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von SmarterU-Benutzerkonten oder mithilfe der von SmarterU bereitgestellten APIs erstellen.
> 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### <a name="to-assign-users-to-smarteru-perform-the-following-steps"></a>So weisen Sie SmarterU Benutzer zu:
1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **SmarterU** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-smarteru-tutorial/IC777330.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-smarteru-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Dec16_HO2-->


