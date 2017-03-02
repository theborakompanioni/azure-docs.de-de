---
title: 'Tutorial: Azure Active Directory-Integration mit Bonus.ly | Microsoft Docs'
description: "Erfahren Sie, wie Sie Bonus.ly mit Azure Active Directory verwenden können, um einmaliges Anmelden, die automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 29fea32a-fa20-47b2-9e24-26feb47b0ae6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2c45278318c154051469b4e4c9e4e7b63463ff1e
ms.openlocfilehash: a527286cf3d51263faf67a59ed6efeeb62b05b9d
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bonusly"></a>Lernprogramm: Azure Active Directory-Integration mit Bonus.ly
In diesem Lernprogramm wird die Integration von Azure und Bonus.ly veranschaulicht. 

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Testkonto bei Bonus.ly

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Bonus.ly
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bonus-tutorial/IC773679.png "Szenario")

## <a name="enable-the-application-integration-for-bonusly"></a>Aktivieren der Anwendungsintegration für Bonus.ly
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Bonus.ly aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Bonus.ly die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-bonus-tutorial/IC773680.png "Einmaliges Anmelden aktivieren")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bonus-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bonus-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bonus-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Bonus.ly** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bonus-tutorial/IC773681.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Bonus.ly** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773682.png "Bonusly")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Bonus.ly zu authentifizieren.  

Zum Konfigurieren des einmaligen Anmeldens (SSO) für Bonus.ly müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Bonus.ly** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bonus.ly anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773683.png "Einmaliges Anmelden konfigurieren")
*3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld mit der **Bonus.ly-Mandanten-URL** die URL im Format **https://\<Mandantenname\>.Bonus.ly** ein, und klicken Sie dann auf **Weiter**: 
   
   ![App-URL konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773684.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden für Bonus.ly konfigurieren** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal als **C:\\Bonusly.cer**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773685.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Browserfenster mit Ihrem **Bonus.ly** -Mandanten an.
6. Klicken Sie in der Symbolleiste oben auf **Einstellungen**, und wählen Sie dann **Integrationen und Apps** aus.
   
   ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773686.png "Bonusly")
7. Wählen Sie unter **Einmaliges Anmelden** **SAML** aus.
8. Führen Sie auf der Dialogseite **SAML** die folgenden Schritte aus:
   
   ![Bonusly](./media/active-directory-saas-bonus-tutorial/IC773687.png "Bonusly")   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP SSO target URL** ein.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert für **Aussteller-ID**, und fügen Sie ihn in das Textfeld **IdP Issuer** ein.
   3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bonus.ly** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP Login URL** ein.
   4. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Cert-Fingerabdruck** ein.
   
    >[!TIP]
    >Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI).
    > 
9. Klicken Sie auf **Speichern**.
10. Wählen Sie im klassischen Microsoft Azure-Portal die Bestätigung der Konfiguration aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bonus-tutorial/IC773689.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Bonus.ly anmelden können, müssen sie in Bonus.ly bereitgestellt werden. 

* Im Fall von Bonus.ly ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich in einem Browserfenster mit Ihrem Bonus.ly-Mandanten an.
2. Klicken Sie auf **Einstellungen**.
 
   ![Einstellungen](./media/active-directory-saas-bonus-tutorial/IC781041.png "Einstellungen")
3. Klicken Sie auf die Registerkarte **Users and bonuses** .
   
   ![Benutzer und Boni](./media/active-directory-saas-bonus-tutorial/IC781042.png "Benutzer und Boni")
4. Klicken Sie auf **Manage Users**.
   
   ![Benutzer verwalten](./media/active-directory-saas-bonus-tutorial/IC781043.png "Benutzer verwalten")
5. Klicken Sie auf **Benutzer hinzufügen**.
   
   ![Benutzer hinzufügen](./media/active-directory-saas-bonus-tutorial/IC781044.png "Benutzer hinzufügen")
6. Führen Sie im Dialogfeld **Add User** die folgenden Schritte aus:
   
   ![Benutzer hinzufügen](./media/active-directory-saas-bonus-tutorial/IC781045.png "Benutzer hinzufügen")  
   1. Geben Sie in die entsprechenden Textfelder die „**E-Mail-Adresse**, den **Vornamen** und den **Nachnamen**“ eines gültigen AAD-Kontos ein, das Sie bereitstellen möchten.
   2. Klicken Sie auf **Speichern**.
   
     >[!NOTE]
     >Der Besitzer des AAD-Kontos erhält eine E-Mail mit einem Link zur Bestätigung des Kontos, bevor es aktiv wird.
     >  

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Bonus.ly-Benutzerkonten oder mithilfe von APIs erstellen, die von Bonus.ly zum Bereitstellen von AAD-Benutzerkonten bereitgestellt werden.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Bonus.ly folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für Bonus.ly auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-bonus-tutorial/IC773690.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bonus-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


