---
title: 'Tutorial: Azure Active Directory-Integration mit Huddle | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Huddle mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 65188de42470fc1b6c8b3ef5c0ef65b5b455c683
ms.openlocfilehash: 752cc3c6abaecfcc7fd8bcf7462102336ebaa97b
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Tutorial: Azure Active Directory-Integration mit Huddle
In diesem Tutorial wird die Integration von Azure und Huddle erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Huddle-Abonnement, für das das einmalige Anmelden (SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Huddle zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens auf Ihrer Huddle-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie auch die [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md) nutzen.

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Huddle
* Konfigurieren der einmaligen Anmeldung
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787830.png "Einmaliges Anmelden konfigurieren")

## <a name="enable-the-application-integration-for-huddle"></a>Aktivieren der Anwendungsintegration für Huddle
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Huddle aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Huddle die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-huddle-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-huddle-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-huddle-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **Huddle** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-huddle-tutorial/IC787831.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Huddle** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Huddle zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Huddle** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787833.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Huddle anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787834.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Huddle-Anmelde-URL** die URL Ihres Huddle-Mandanten im Format „*http://company.huddle.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787835.png "App-URL konfigurieren")
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für Huddle** die folgenden Schritte aus:
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787836.png "Einmaliges Anmelden konfigurieren")
   
   1. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   2. Kopieren Sie den Wert **Aussteller-URL**, den Wert **SAML-SSO-URL** und das heruntergeladene Zertifikat, und senden Sie diese Informationen an das Supportteam von Huddle.  
   
    >[!NOTE]
    >Das einmalige Anmelden muss vom Supportteam von Huddle aktiviert werden. Sie erhalten eine Benachrichtigung, sobald die Konfiguration abgeschlossen ist. 
    > 
5. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-huddle-tutorial/IC787837.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Huddle anmelden können, müssen sie in Huddle bereitgestellt werden. Im Fall von Huddle ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei der **Huddle** -Unternehmenswebsite als Administrator an.
2. Klicken Sie auf **Arbeitsbereich**.
3. Klicken Sie auf **Personen \> Personen einladen**.
   
   ![Personen](./media/active-directory-saas-huddle-tutorial/IC787838.png "Personen")
4. Führen Sie im Abschnitt **Neue Einladung erstellen** die folgenden Schritte aus:
   
   ![Neue Einladung](./media/active-directory-saas-huddle-tutorial/IC787839.png "Neue Einladung")
   
   1. Wählen Sie in der Liste **Team zum Einladen von Personen auswählen** die Option **Team** aus.
   2. Geben Sie die **E-Mail-Adresse** eines gültigen AAD-Benutzerkontos, das Sie bereitstellen möchten, in das entsprechende Textfeld ein.
   3. Klicken Sie auf **Einladen**.   
   
    >[!NOTE]
    >Der Besitzer des Azure AD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird. 
    > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Huddle-Benutzerkonten oder mithilfe der von Huddle bereitgestellten APIs erstellen. 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Huddle folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Huddle** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-huddle-tutorial/IC787840.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-huddle-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


