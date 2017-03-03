---
title: 'Tutorial: Azure Active Directory-Integration mit Directions on Microsoft | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Directions on Microsoft mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: e0c8986f-2acd-418d-a306-437abc44b640
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: efea82512dad3f2e5d1c0dd45e9e2f6db9a283d4
ms.openlocfilehash: a5538e89b1c23e4a2ad45c5515de8167ef95b46a
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Tutorial: Azure Active Directory-Integration mit Directions on Microsoft
In diesem Tutorial wird die Integration von Azure Active Directory und Directions on Microsoft erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Directions on Microsoft-Abonnement

Falls Sie noch kein Directions on Microsoft-Verbundabonnement haben, senden Sie per E-Mail eine Anforderung an **service@DirectionsOnMicrosoft.com**.

Nach Abschluss dieses Tutorials können sich die Azure Active Directory-Benutzer, die Sie Directions on Microsoft zugewiesen haben, mittels einmaliger Anmeldung bei der Anwendung anmelden.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Directions on Microsoft
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Szenario")

## <a name="enable-the-application-integration-for-directions-on-microsoft"></a>Aktivieren der Anwendungsintegration für Directions on Microsoft
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Directions on Microsoft aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Directions on Microsoft die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** **Directions on Microsoft** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Directions on Microsoft**ein, und klicken Sie dann auf **Abschließen** , um die Anwendung hinzuzufügen.
   
   ![Szenario](./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Szenario")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Directions on Microsoft zu authentifizieren.

**Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Directions on Microsoft** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Einmaliges Anmelden aktivieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Directions on Microsoft anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD – einmaliges Anmelden")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld Anmelde-URL **https://www.directionsonmicrosoft.com/user/login**, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Directions on Microsoft** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie die Metadatendatei an das Supportteam von Directions on Microsoft (*service@DirectionsOnMicrosoft.com*). Geben Sie in der E-Mail Ihre Unternehmensinformationen an, damit das Supportteam von Directions on Microsoft Ihre Verbundwebsitemitgliedschaft ausfindig machen kann.
   
   >[!NOTE]
   >Einmaliges Anmelden für Directions on Microsoft muss vom Supportteam von Directions on Microsoft aktiviert werden. Sie erhalten eine Benachrichtigung, wenn einmaliges Anmelden aktiviert wurde. 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Für das Konfigurieren der Benutzerbereitstellung in Directions on Microsoft steht kein Aktionselement zur Verfügung.  

Wenn ein zugewiesener Benutzer versucht, sich über den Zugriffsbereich bei Directions on Microsoft anzumelden, überprüft Directions on Microsoft, ob der Benutzer vorhanden ist. Ist noch kein Benutzerkonto verfügbar, wird es von Directions on Microsoft automatisch erstellt.

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Directions on Microsoft folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Directions on Microsoft** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Ja")


