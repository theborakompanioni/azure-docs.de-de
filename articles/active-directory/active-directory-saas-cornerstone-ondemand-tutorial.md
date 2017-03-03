---
title: 'Tutorial: Azure Active Directory-Integration mit Cornerstone OnDemand | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Cornerstone OnDemand mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: bbbae7d7a5e9b0fbfe07bd3f2aa61df77b56c5df
ms.openlocfilehash: 7fce07464b90c81da7a015246d5757ecc768792a
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a>Tutorial: Azure Active Directory-Integration mit Cornerstone OnDemand
In diesem Tutorial wird die Integration von Azure und Cornerstone OnDemand erläutert.

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Cornerstone OnDemand-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Cornerstone OnDemand zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Cornerstone OnDemand-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Cornerstone OnDemand
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781593.png "Szenario")

## <a name="enable-the-application-integration-for-cornerstone-ondemand"></a>Aktivieren der Anwendungsintegration für Cornerstone OnDemand
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Cornerstone OnDemand aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Cornerstone OnDemand die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **cornerstone ondemand** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781594.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Cornerstone OnDemand** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Cornerstone OnDemand](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781595.png "Cornerstone OnDemand")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Cornerstone OnDemand zu authentifizieren.

**Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:**
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Cornerstone OnDemand** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781596.png "Einmaliges Anmelden aktivieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Cornerstone OnDemand anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781597.png "Microsoft Azure AD – einmaliges Anmelden")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Cornerstone OnDemand-Anmelde-URL** Ihre URL im Format *http://unternehmen.csod.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781598.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für Cornerstone OnDemand** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781599.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie Folgendes an das Supportteam von Cornerstone OnDemand:  
   1. Das heruntergeladene Zertifikat
   2. Den Wert der **Remoteanmelde-URL**
   3. Den Wert der **Remoteabmelde-URL**

   >[!NOTE]
   >Einmaliges Anmelden muss vom Supportteam von Cornerstone OnDemand konfiguriert werden. Sie erhalten eine Benachrichtigung vom Supportteam, wenn die Konfiguration abgeschlossen ist.
   > 
6. Bestätigen Sie die Konfiguration des einmaligen Anmeldens, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781600.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Um Azure AD-Benutzern die Anmeldung bei Cornerstone OnDemand zu ermöglichen, müssen sie in Cornerstone OnDemand bereitgestellt werden. Im Fall von Cornerstone OnDemand ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Senden Sie die Informationen (z. B. Name und E-Mail) für den Azure AD-Benutzer, den Sie bereitstellen möchten, an das Supportteam von Cornerstone OnDemand.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Cornerstone OnDemand-Benutzerkonten oder mithilfe der von Cornerstone OnDemand bereitgestellten APIs erstellen. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Cornerstone OnDemand folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Cornerstone OnDemand** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC775564.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-cornerstone-ondemand-tutorial/IC781601.png "Zuweisen von Benutzern")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


