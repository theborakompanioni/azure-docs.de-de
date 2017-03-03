---
title: 'Tutorial: Azure Active Directory-Integration mit Clever | Microsoft Docs'
description: "Erfahren Sie, wie Sie Clever mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: ee53124019cbee9c42f94cde411a9b653413011a
ms.openlocfilehash: a219a63f4de17c18d58bfd529e8372b99ccb4e41
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Lernprogramm: Azure Active Directory-Integration mit Clever
In diesem Lernprogramm wird die Integration von Azure und Clever erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Clever-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Clever zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Clever-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Clever
* Konfigurieren der einmaligen Anmeldung
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-clever-tutorial/IC798977.png "Szenario")

## <a name="enable-the-application-integration-for-clever"></a>Aktivieren der Anwendungsintegration für Clever
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Clever aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Clever die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-clever-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-clever-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-clever-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Clever** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-clever-tutorial/IC798978.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Clever** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Clever](./media/active-directory-saas-clever-tutorial/IC798979.png "Clever")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Clever zu authentifizieren.  

Die Clever-Anwendung erwartet die SAML-Assertionen in einem bestimmten Format. Daher müssen Sie Ihrer Konfiguration der **SAML-Tokenattribute** benutzerdefinierte Attributzuordnungen hinzufügen.  

Der folgende Screenshot zeigt ein Beispiel für diese Attributzuordnungen:

![Attribute](./media/active-directory-saas-clever-tutorial/IC798980.png "Attribute")

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Clever** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC784682.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Clever anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798981.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Clever-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer Clever-Anwendung verwendete URL ein (z.B. *https://clever.com/in/azsandbox*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-clever-tutorial/IC798982.png "App-URL konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Clever** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798983.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Clever-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste auf **Instant Login**.
   
   ![Sofortige Anmeldung](./media/active-directory-saas-clever-tutorial/IC798984.png "Sofortige Anmeldung")
7. Führen Sie auf der Seite **Instant Login** die folgenden Schritte aus:
   
   ![Sofortige Anmeldung](./media/active-directory-saas-clever-tutorial/IC798985.png "Sofortige Anmeldung")
   
   1. Geben Sie die **Login URL**ein.  
   
      >[!NOTE]
      >Die **Login URL** ist ein benutzerdefinierter Wert. Sie erhalten diesen Wert vom Clever-Supportteam.
      > 
   2. Wählen Sie als **Identitätssystem** die Option **ADFS** aus.
   3. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-clever-tutorial/IC798986.png "Einmaliges Anmelden konfigurieren")
9. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
   
   ![Attribute](./media/active-directory-saas-clever-tutorial/IC795920.png "Attribute")
10. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
    
    ![SAML-Tokenattribute](./media/active-directory-saas-clever-tutorial/IC795921.png "SAML-Tokenattribute")
    
    | Attributname | Attributwert |
    | --- | --- |
    | clever.student.credentials.district\_benutzername |User.userprincipalname |
    
    1. Klicken Sie für jede Datenzeile in der obigen Tabelle auf **add user attribute**.
    2. Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
    3. Geben Sie im Textfeld **Attribute Value** den für die Zeile angezeigten Attributwert ein.
    4. Klicken Sie auf **Fertig stellen**.
11. Klicken Sie auf **Änderungen übernehmen**.

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren
Damit sich Azure AD-Benutzer bei Clever anmelden können, müssen sie in Clever bereitgestellt werden.

Im Fall von Clever ist die Bereitstellung eine manuelle Aufgabe, die von Ihrem Clever-Support-Team ausgeführt werden muss.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Clever-Benutzerkonten oder mithilfe der von Clever bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Clever folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Clever** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-clever-tutorial/IC798987.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-clever-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


