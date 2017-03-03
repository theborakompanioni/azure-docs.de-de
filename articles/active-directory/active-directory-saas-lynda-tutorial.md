---
title: 'Tutorial: Azure Active Directory-Integration mit Lynda.com | Microsoft Docs'
description: "Erfahren Sie, wie Sie Lynda.com mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: f6c92789-8b64-4049-bac9-8cb928398433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/02/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07b9df5ab5c7df0089f001a26200bb9c3fb1c508
ms.openlocfilehash: 7f4f1c22e0b3382a9de770800ccb733c61d4f5b0
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-lyndacom"></a>Tutorial: Azure Active Directory-Integration mit Lynda.com
In diesem Tutorial wird die Integration von Azure und Lynda.com erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Lynda.com-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Lynda.com zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Lynda.com-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Lynda.com
2. Konfigurieren der einmaligen Anmeldung (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-lynda-tutorial/IC781046.png "Szenario")

## <a name="enabling-the-application-integration-for-lyndacom"></a>Aktivieren der Anwendungsintegration für Lynda.com
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Lynda.com aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Lynda.com die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-lynda-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-lynda-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-lynda-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-lynda-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Lynda.com** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-lynda-tutorial/IC777524.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Lynda.com** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Lynda.com](./media/active-directory-saas-lynda-tutorial/IC777525.png "Lynda.com")
   
## <a name="configuring-single-sign-on"></a>Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Lynda.com zu authentifizieren.

>[!IMPORTANT]
>Damit Sie das einmalige Anmelden für Ihren Lynda.com-Mandanten konfigurieren können, müssen Sie sich zunächst an den technischen Support von Lynda.com wenden, um dieses Feature zu aktivieren. 
> 

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Lynda.com** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lynda-tutorial/IC777526.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Lynda.com anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lynda-tutorial/IC777527.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld **Lynda.com-Anmelde-URL** Ihre Lynda.com-Mandanten-URL ein (z.B.: *https://shib.lynda.com/Shibboleth.sso/InCommon?providerId=https://sts.windows-ppe.net/6247032d-9415-403c-b72b-277e3fb6f2c8/&target=https://shib.lynda.com/InCommon*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-lynda-tutorial/IC781047.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Lynda.com** auf **Metadaten herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lynda-tutorial/IC777529.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie die heruntergeladene Metadatendatei an das Supportteam von Lynda.com. Das Supportteam von Lynda.com übernimmt das Konfigurieren des einmaligen Anmeldens für Sie.
6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-lynda-tutorial/IC777530.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung

Für das Konfigurieren der Benutzerbereitstellung in Lynda.com steht kein Aktionselement zur Verfügung.  
Wenn sich ein zugewiesener Benutzer über den Zugriffsbereich bei Lynda.com anmelden möchte, überprüft Lynda.com, ob der Benutzer vorhanden ist.  

Ist noch kein Benutzerkonto verfügbar, wird es von Lynda.com automatisch erstellt.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Lynda.com-Benutzerkonten oder mithilfe der von Lynda.com bereitgestellten APIs erstellen. 
> 

## <a name="assigning-users"></a>Zuweisen von Benutzern
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Lynda.com folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Lynda.com** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-lynda-tutorial/IC777531.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-lynda-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


