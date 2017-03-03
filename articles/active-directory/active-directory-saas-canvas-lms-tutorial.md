---
title: 'Tutorial: Azure Active Directory-Integration mit Canvas LMS | Microsoft Docs'
description: "Erfahren Sie, wie Sie Canvas LMS mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bfed291c-a33e-410d-b919-5b965a631d45
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 325d92e493f6e011367d2c85b52c92838327101e
ms.openlocfilehash: 15310828b15f266527573809adae2ddbe4de60be
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Lernprogramm: Azure Active Directory-Integration mit Canvas LMS
In diesem Lernprogramm wird die Integration von Azure und Canvas erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Canvas-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Canvas zugewiesen haben, mittels einmaligen Anmeldens auf der Canvas-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Sie können aber auch den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Canvas
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Szenario")

## <a name="enable-the-application-integration-for-canvas"></a>Aktivieren der Anwendungsintegration für Canvas
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Canvas aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Canvas die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Canvas** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Canvas** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Canvas")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Canvas zu authentifizieren.  

Zum Konfigurieren des einmaligen Anmeldens (SSO) für Canvas müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Canvas** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Canvas anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Canvas-Anmelde-URL** die URL im Format `https://<tenant-name>.instructure.com` ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um Canvas** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Canvas-Unternehmenswebsite als Administrator an.
6. Wechseln Sie zu **Kurse \> Verwaltete Konten \> Microsoft**.
   
   ![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")
7. Wählen Sie im Navigationsbereich auf der linken Seite **Authentifizierung** aus, und klicken Sie dann auf **Neue SAML-Konfiguration hinzufügen**.
   
   ![Authentifizierung](./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Authentifizierung")
8. Führen Sie auf der Seite "Current Integration" die folgenden Schritte aus.
   
   ![Aktuelle Integration](./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Aktuelle Integration")

  1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Canvas** den Wert der **Entitäts-ID**, und fügen Sie ihn in das Textfeld **IdP-Entitäts-ID** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Canvas** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Anmelde-URL** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Canvas** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein.
   4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Canvas** den Wert für **Kennwort-URL ändern**, und fügen Sie ihn in das Textfeld **Kennwortlink ändern** ein.
   5. Kopieren Sie den Wert für **Fingerabdruck** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** ein.      
      >[!TIP]
      >Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI) 
      > 
   6. Wählen Sie in der Liste **Anmeldeattribut** die Option **NameID** aus.
   7. Wählen Sie in der Liste **Bezeichnerformat** die Option **emailAddress** aus.
   8. Klicken Sie auf **Save Authentication Settings**.
9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Canvas anmelden können, müssen sie in Canvas bereitgestellt werden.

* Im Fall von Canvas ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen von Benutzerkonten die folgenden Schritte aus:**

1. Melden Sie sich bei Ihrem **Canvas** -Mandanten an.
2. Wechseln Sie zu **Kurse \> Verwaltete Konten \> Microsoft**.
   
   ![Canvas](./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Canvas")
3. Klicken Sie auf **Users**.
   
   ![Benutzer](./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Benutzer")
4. Klicken Sie auf **Add New User**.
   
   ![Benutzer](./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Benutzer")
5. Führen Sie auf der Dialogseite "Add a New User" die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Benutzer hinzufügen")
   
   1. Geben Sie im Textfeld **Full Name** den Namen des Benutzers ein.
   2. Geben Sie im Textfeld **Email** die E-Mail-Adresse des Benutzers ein.
   3. Geben Sie im Textfeld **Login** die Azure AD-E-Mail-Adresse des Benutzers ein.
   4. Aktivieren Sie **Email the user about this account creation**.
   5. Klicken Sie auf **Add User**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von Canvas-Benutzerkonten oder mithilfe der von Canvas bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Canvas folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Canvas** auf **Benutzer zuweisen**.
   
   ![Benutzer zuweisen](./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Benutzer zuweisen")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


