---
title: 'Tutorial: Azure Active Directory-Integration mit TimeOffManager | Microsoft Docs'
description: "Erfahren Sie, wie Sie TimeOffManager mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/16/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: a028279bbc209b9bf9402813b687752f0320ce52
ms.openlocfilehash: 55a13e90ed5f8fd59a93bba507769e56594de568
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a>Tutorial: Azure Active Directory-Integration mit TimeOffManager
In diesem Tutorial wird die Integration von Azure und TimeOffManager erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein TimeOffManager-Abonnement, für das einmaliges Anmelden aktiviert ist

Nach Abschluss dieses Tutorials können sich die TimeOffManager zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung (SSO) auf Ihrer TimeOffManager-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für TimeOffManager
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-timeoffmanager-tutorial/IC795909.png "Szenario")

## <a name="enable-the-application-integration-for-timeoffmanager"></a>Aktivieren der Anwendungsintegration für TimeOffManager
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für TimeOffManager aktivieren.

**So aktivieren Sie die Anwendungsintegration für TimeOffManager**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-timeoffmanager-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-timeoffmanager-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-timeoffmanager-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-timeoffmanager-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **TimeOffManager** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-timeoffmanager-tutorial/IC795910.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich die Option **TimeOffManager** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![TimeOffManager](./media/active-directory-saas-timeoffmanager-tutorial/IC795911.png "TimeOffManager")

## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei TimeOffManager zu authentifizieren.  

Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei in Ihren TimeOffManager-Mandanten hochladen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>So konfigurieren Sie einmaliges Anmelden
1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **TimeOffManager** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/IC795912.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei TimeOffManager anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/IC795913.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** in das Textfeld **TimeOffManager-Antwort-URL** die TimeOffManager-AssertionConsumerService-URL ein (Beispiel: „*https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company\_id=IC34216*“, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/IC795914.png "App-URL konfigurieren")
   
   Sie können die Antwort-URL von der Seite zur Einstellung der einfachen Anmeldung beim TimeOffManager abrufen.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/IC795915.png "Einstellungen für einmaliges Anmelden")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für TimeOffManager** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/IC795916.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der TimeOffManager-Unternehmenswebsite als Administrator an.
6. Wechseln Sie zu **Konto \> Kontooptionen \> Einstellungen für einmaliges Anmelden**.
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/IC795917.png "Einstellungen für einmaliges Anmelden")
7. Führen Sie im Abschnitt **Einstellungen für einmaliges Anmelden** die folgenden Schritte aus:
   
   ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-timeoffmanager-tutorial/IC795918.png "Einstellungen für einmaliges Anmelden")
  1.  Erstellen Sie eine **Base-64-codierte** Datei aus dem heruntergeladenen Zertifikat.  
   
       >[!TIP] 
       >Weitere Informationen finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(in englischer Sprache).
       > 
  2.  Öffnen Sie das Base-64-codierte Zertifikat im Editor, kopieren Sie den Inhalt des Zertifikats in die Zwischenablage, und fügen Sie anschließend das gesamte Zertifikat in das Textfeld **X.509-Zertifikat** ein.
  3.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für TimeOffManager** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **IdP-Aussteller** ein.
  4.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für TimeOffManager** den Wert der **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **IdP Endpoint-URL** ein.
  5.  Wählen Sie unter **SAML erzwingen** die Option **Nein** aus.
  6.  Wählen Sie für **Auto-Create Users** (Benutzer automatisch erstellen), die Option **Ja** aus.
  7.  Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für TimeOffManager** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Abmelde-URL** ein. Klicken Sie dann auf **Änderungen speichern**.

1. Wählen Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für TimeOffManager** die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-timeoffmanager-tutorial/IC795919.png "Einmaliges Anmelden konfigurieren")
2. Klicken Sie oben im Menü auf **Attribute** to open the **SAML Token Attribute** zu öffnen.
   
   ![Attribute](./media/active-directory-saas-timeoffmanager-tutorial/IC795920.png "Attribute")
3. So fügen Sie die erforderlichen Attributzuordnungen hinzu:
   
   ![SAML-Tokenattribute](./media/active-directory-saas-timeoffmanager-tutorial/123.png "SAML-Tokenattribute")
   
   | Attributname | Attributwert |
   | --- | --- |
   | E-Mail |User.mail |
   | Firstname |User.givenname |
   | Lastname |User.surname |
  1.  Klicken Sie für jede Datenzeile in der obigen Tabelle auf **Benutzerattribut hinzufügen**.
  2.  Geben Sie im Textfeld **Attribute Name** den für die Zeile angezeigten Attributnamen ein.
  3.  Geben Sie im Textfeld **Attribute Value** den für die Zeile angezeigten Attributwert ein.
  4.  Klicken Sie auf **Fertig stellen**.
4. Klicken Sie auf **Änderungen übernehmen**.

## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren
Damit sich Azure AD-Benutzer bei TimeOffManager anmelden können, müssen sie in TimeOffManager bereitgestellt werden.  

TimeOffManager unterstützt die bedarfsabhängige Benutzerbereitstellung. Für Sie steht kein Aktionselement zur Verfügung.  

Die Benutzer werden automatisch während der erstmaligen und einmaligen Anmeldung hinzugefügt.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von TimeOffManager-Benutzerkonten oder mithilfe der von TimeOffManager bereitgestellten APIs erstellen.
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**So weisen Sie TimeOffManager Benutzer zu**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **TimeOffManager** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-timeoffmanager-tutorial/IC795922.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-timeoffmanager-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


