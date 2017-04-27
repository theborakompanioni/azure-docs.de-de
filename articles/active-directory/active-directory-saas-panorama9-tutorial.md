---
title: 'Tutorial: Azure Active Directory-Integration mit Panorama9 | Microsoft Docs'
description: "Erfahren Sie, wie Sie Panorama9 mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 66d68ef805a28552c5d161b4761e2aa3052c118c
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Tutorial: Azure Active Directory-Integration mit Panorama9
In diesem Tutorial wird die Integration von Azure und Panorama9 erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Panorama9-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die Panorama9 zugewiesenen Azure AD-Benutzer mittels einmaliger Anmeldung auf Ihrer Panorama9-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Panorama9
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-panorama9-tutorial/IC790016.png "Szenario")

## <a name="enable-the-application-integration-for-panorama9"></a>Aktivieren der Anwendungsintegration für Panorama9
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Panorama9 aktivieren.

**Um die Anwendungsintegration für Panorama9 zu aktivieren, führen Sie die folgenden Schritte durch:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-panorama9-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-panorama9-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-panorama9-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-panorama9-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **Panorama9** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-panorama9-tutorial/IC790017.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Panorama9** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Panorama9](./media/active-directory-saas-panorama9-tutorial/IC790018.png "Panorama9")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Panorama9 zu authentifizieren.  

Zum Konfigurieren des einmaligen Anmeldens (SSO) für Panorama9 müssen Sie einen Fingerabdruckwert aus einem Zertifikat abrufen.  

Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI)weitere Informationen.

**Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Panorama9** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-panorama9-tutorial/IC790019.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **How would you like users to sign on to Panorama9?** (Wie sollen sich Benutzer bei Panorama9 anmelden?) die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-panorama9-tutorial/IC790020.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Panorama9-Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Panorama9 verwendete URL ein (z.B. *https://dashboard.panorama9.com/saml/access/3262*), und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-panorama9-tutorial/IC790021.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Configure single sign-on at Panorama9** (Einmaliges Anmelden bei Panorama9 konfigurieren) auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat lokal auf Ihrem Computer.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-panorama9-tutorial/IC790022.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich in einem anderen Webbrowserfenster bei der Panorama9-Unternehmenswebsite als Administrator an.
6. Klicken Sie in der Symbolleiste oben auf **Verwalten** und dann auf **Erweiterungen**.
   
   ![Erweiterungen](./media/active-directory-saas-panorama9-tutorial/IC790023.png "Erweiterungen")
7. Klicken Sie im Dialogfeld **Erweiterungen** auf **Einmaliges Anmelden**.
   
   ![Einmaliges Anmelden](./media/active-directory-saas-panorama9-tutorial/IC790024.png "des einmaligen Anmeldens")
8. Führen Sie im Abschnitt **Einstellungen** die folgenden Schritte aus:
   
   ![Einstellungen](./media/active-directory-saas-panorama9-tutorial/IC790025.png "Einstellungen")
   
   1. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Configure single sign-on at Panorama9** den Wert von **Single Sign-On Service URL** (Dienst-URL für einmaliges Anmelden), und fügen Sie ihn in das Textfeld **Identity provider URL** (URL des Identitätsanbieters) ein.
   2. Kopieren Sie den **Fingerabdruckwert** aus dem exportierten Zertifikat, und fügen Sie ihn in das Textfeld **Certificate fingerprint** (Fingerabdruck des Zertifikats) ein.    
   
      >[!TIP]
      >Weitere Informationen finden Sie unter [Abrufen des Fingerabdruckwerts eines Zertifikats](http://youtu.be/YKQF266SAxI).
      > 
      
   3. Klicken Sie auf **Speichern**.
9. Wählen Sie im klassischen Azure AD-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-panorama9-tutorial/IC790026.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei Panorama9 anmelden können, müssen sie in Panorama9 bereitgestellt werden.  

* Im Fall von Panorama9 ist die Bereitstellung eine manuelle Aufgabe.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**
1. Melden Sie sich bei der **Panorama9** -Unternehmenswebsite als Administrator an.
2. Klicken Sie im oberen Menü auf **Verwalten** und dann auf **Benutzer**.
   
  ![Benutzer](./media/active-directory-saas-panorama9-tutorial/IC790027.png "Benutzer")
3. Klicken Sie unten auf der Seite auf **+**.
4. Führen Sie im Abschnitt mit den Benutzerdaten die folgenden Schritte aus:
   
  ![Benutzer](./media/active-directory-saas-panorama9-tutorial/IC790028.png "Benutzer")

  1. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse eines gültigen Azure Active Directory-Benutzers ein, den Sie bereitstellen möchten.
  2. Klicken Sie auf **Speichern**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Panorama9-Benutzerkonten oder mithilfe der von Panorama9 bereitgestellten APIs erstellen.
>
>

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Um Panorama9 Benutzer zuzuweisen, führen Sie die folgenden Schritte durch:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Panorama9** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-panorama9-tutorial/IC790029.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-panorama9-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


