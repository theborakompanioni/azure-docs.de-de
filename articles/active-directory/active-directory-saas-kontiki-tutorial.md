---
title: 'Tutorial: Azure Active Directory-Integration mit Kontiki | Microsoft Docs'
description: "Erfahren Sie, wie Sie Kontiki mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8d5e5413-da4c-40d8-b1d0-f03ecfef030b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 230facec9f8f83e8a94eaaac5a1495195ff45cc7
ms.openlocfilehash: 3af125b186e3905572dd5d621c948a6639f9b023
ms.lasthandoff: 02/17/2017


---

# <a name="tutorial-azure-active-directory-integration-with-kontiki"></a>Tutorial: Azure Active Directory-Integration mit Kontiki
In diesem Tutorial wird die Integration von Azure und Kontiki erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Kontiki-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Kontiki zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Kontiki-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Kontiki
* Konfigurieren der einmaligen Anmeldung
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-kontiki-tutorial/IC790235.png "Szenario")

## <a name="enable-the-application-integration-for-kontiki"></a>Aktivieren der Anwendungsintegration für Kontiki
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Kontiki aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Kontiki die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-kontiki-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-kontiki-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-kontiki-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-kontiki-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **Kontiki** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-kontiki-tutorial/IC790236.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Kontiki** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Kontiki](./media/active-directory-saas-kontiki-tutorial/IC790237.png "Kontiki")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Kontiki zu authentifizieren.

*Führen Sie zum Konfigurieren des einmaligen Anmeldens die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Kontiki** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kontiki-tutorial/IC790238.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Kontiki anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kontiki-tutorial/IC790239.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Kontiki-Anmelde-URL ** die von Ihren Benutzern für die Anmeldung bei Kontiki verwendete URL ein (z.B. „*https://company.mc.eval.kontiki.com/*“), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-kontiki-tutorial/IC790240.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Kontiki** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kontiki-tutorial/IC790241.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie die Metadatendatei an das Supportteam von Kontiki.
   
   >[!NOTE]
   >Das einmalige Anmelden muss vom Supportteam von Kontiki konfiguriert werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist. 
   > 
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-kontiki-tutorial/IC790242.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Für das Konfigurieren der Benutzerbereitstellung in Kontiki steht kein Aktionselement zur Verfügung. Wenn sich ein zugewiesener Benutzer über den Zugriffsbereich bei Kontiki anmelden möchte, überprüft Kontiki, ob der Benutzer vorhanden ist.  

>!NOTE] Ist noch kein Benutzerkonto verfügbar, wird es von Kontiki automatisch erstellt.
>

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern zu Kontiki folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Kontiki** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-kontiki-tutorial/IC790243.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-kontiki-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


