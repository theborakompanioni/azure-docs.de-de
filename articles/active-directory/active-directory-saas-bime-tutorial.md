---
title: 'Tutorial: Azure Active Directory-Integration mit Bime | Microsoft Docs'
description: "Erfahren Sie, wie Sie Bime mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: bdcf0729-c880-4c95-b739-0f6345b17dd8
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: bd3dd077bef87a78904ffd5d2be469b6b8bc8959
ms.openlocfilehash: 7857480d033e4d570aa48569e08bb30846b280f6
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bime"></a>Lernprogramm: Azure Active Directory-Integration mit Bime
In diesem Lernprogramm wird die Integration von Azure und Bime erläutert.  
Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Bime-Mandanten

Nach Abschluss dieses Tutorials können sich die Azure AD-Benutzer, die Sie Bime zugewiesen haben, mittels einmaligen Anmeldens auf Ihrer Bime-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Bime
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-bime-tutorial/IC775552.png "Szenario")

## <a name="enable-the-application-integration-for-bime"></a>Aktivieren der Anwendungsintegration für Bime
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Bime aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Bime die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-bime-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-bime-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-bime-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-bime-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Bime** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-bime-tutorial/IC775553.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Bime** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Bime](./media/active-directory-saas-bime-tutorial/IC775554.png "Bime")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Bime zu authentifizieren.  

Zum Konfigurieren des einmaligen Anmeldens (SSO) für Bime müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen. Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Bime** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC771709.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Bime anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775555.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Bime-Anmelde-URL** die URL im Format *https://\<Mandantenname\>.Bimeapp.com* ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-bime-tutorial/IC775556.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren um Bime** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal unter **C:\\Bime.cer**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775557.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Bime-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste auf **Administration** und dann auf **Konto**.
   
   ![Admin](./media/active-directory-saas-bime-tutorial/IC775558.png "Admin")
7. Führen Sie auf der Kontenkonfigurationsseite die folgenden Schritte aus:
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775559.png "Einmaliges Anmelden konfigurieren")
   
   1. Aktivieren Sie **Enable SAML authentication**.
   2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Bime** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteanmelde-URL** ein.
   3. Kopieren Sie den Wert für **Fingerabdruck** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Fingerabdruck des Zertifikats** ein.       
      
      >[!TIP]
      >Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI). 
      > 
   4. Klicken Sie auf **Speichern**.
8. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-bime-tutorial/IC775560.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Bime anmelden können, müssen sie in Bime bereitgestellt werden.  

* Im Fall von Bime ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei Ihrem **Bime** -Mandanten an.
2. Klicken Sie auf der Symbolleiste auf **Administration** und dann auf **Benutzer**.
   
   ![Admin](./media/active-directory-saas-bime-tutorial/IC775561.png "Admin")
3. Klicken Sie unter **Benutzerliste** auf **Neuen Benutzer hinzufügen** („+“).
   
   ![Benutzer](./media/active-directory-saas-bime-tutorial/IC775562.png "Benutzer")
4. Führen Sie auf der Dialogfeldseite **User Details** die folgenden Schritte aus:
   
   ![Benutzerdetails](./media/active-directory-saas-bime-tutorial/IC775563.png "Benutzerdetails")   
  1. Geben Sie den "Vornamen", den "Nachnamen", den "Anmeldenamen" und die "E-Mail-Adresse" eines gültigen AAD-Benutzerkontos ein, das Sie bereitstellen möchten.
  2. Klicken Sie auf Speichern.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Bime-Benutzerkonten oder mithilfe der von Bime bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Bime folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Bime** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-bime-tutorial/IC775564.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-bime-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


