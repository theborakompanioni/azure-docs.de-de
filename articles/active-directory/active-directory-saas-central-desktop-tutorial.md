---
title: 'Tutorial: Azure Active Directory-Integration mit Central Desktop | Microsoft Docs'
description: "Erfahren Sie, wie Sie Central Desktop mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: b805d485-93db-49b4-807a-18d446c7090e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 325d92e493f6e011367d2c85b52c92838327101e
ms.openlocfilehash: fe32c1d68040ceb9d9de2ad6c4a6dc9ea93f5aef
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Tutorial: Azure Active Directory-Integration mit Central Desktop
In diesem Tutorial wird die Integration von Azure und Central Desktop erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein Abonnement für Central Desktop mit aktiviertem einmaligen Anmelden und einen Central Desktop-Mandanten

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

* Aktivieren der Anwendungsintegration für Central Desktop
* Konfigurieren der einmaligen Anmeldung (SSO)
* Konfigurieren der Benutzerbereitstellung
* Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Szenario")

## <a name="enable-the-application-integration-for-central-desktop"></a>Aktivieren der Anwendungsintegration für Central Desktop
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Central Desktop aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für Central Desktop die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** als Suchbegriff **Central Desktop** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **Central Desktop** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![Central Desktop](./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Central Desktop")
   
## <a name="configure-single-sign-on"></a>Einmaliges Anmelden konfigurieren

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Central Desktop zu authentifizieren.

Im Rahmen dieses Verfahrens müssen Sie eine Base64-codierte Zertifikatsdatei in Ihren Central Desktop-Mandanten hochladen.  
Falls Sie nicht mit diesem Verfahren vertraut sind, finden Sie unter [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o)(Konvertieren eines binären Zertifikats in eine Textdatei; in englischer Sprache) weitere Informationen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Central Desktop** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Central Desktop anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Einmaliges Anmelden konfigurieren")
3. Führen Sie auf der Seite **App-URL konfigurieren** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**: 
   
   1. Geben Sie im Textfeld **Central Desktop-Anmelde-URL** die URL Ihres Central Desktop-Mandanten ein (z.B. *http://contoso.centraldesktop.com*).
   2. Geben Sie im Textfeld „Central Desktop-Antwort-URL“ Ihre Central Desktop AssertionConsumerService-URL ein (z.B. https://contoso.centraldesktop.com/saml2-assertion.php).
   
   >[!NOTE]
   >Sie finden den Wert in den Central Desktop-Metadaten (z.B. *http://contoso.centraldesktop.com*).
   >  
   
   ![App-URL konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC769561.png "App-URL konfigurieren")
4. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden bei Central Desktop konfigurieren** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.
   
  ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Einmaliges Anmelden konfigurieren")
5. Melden Sie sich bei Ihrem **Central Desktop** -Mandanten an.
6. Wechseln Sie zu **Einstellungen**, klicken Sie auf **Erweitert**, und klicken Sie dann auf **Einmaliges Anmelden**.
   
  ![Setup – Erweitert](./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Setup – Erweitert")
7. Führen Sie im Dialogfeld **Single Sign On Settings** die folgenden Schritte aus:
   
  ![Einstellungen für einmaliges Anmelden](./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Einstellungen für einmaliges Anmelden")
   
  1. Aktivieren Sie **Enable SAML v2 Single Sign On**.
  2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Aussteller-URL**, und fügen Sie ihn in das Textfeld **SSO-URL** ein.
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **SSO-Anmelde-URL** ein.
  4. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Central Desktop** den Wert für **Dienst-URL für einmaliges Abmelden**, und fügen Sie ihn in das Textfeld **SSO-Abmelde-URL** ein.
8. Führen Sie im Abschnitt **Message Signature Verification Method** die folgenden Schritte aus:
   
   ![Methode zur Nachrichtensignaturüberprüfung](./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Methode zur Nachrichtensignaturüberprüfung")
   
  1. Wählen Sie **Certificate**aus.
  2. Wählen Sie in der Liste **SSO-Zertifikat** die Option **RSH SHA256** aus.
  3. Erstellen Sie eine Textdatei aus dem heruntergeladenen Zertifikat, kopieren Sie den Inhalt der Textdatei, und fügen Sie ihn in das Feld **SSO Certificate** ein.  
     >[!TIP]
     >Weitere Informationen finden Sie unter [Konvertieren eines binären Zertifikats in eine Textdatei](http://youtu.be/PlgrzUZ-Y1o)
      >  
   4. Aktivieren Sie **Display a link to your SAMLv2 login page**.
9. Klicken Sie auf **Aktualisieren**.
10. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
    
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Einmaliges Anmelden konfigurieren")
    
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich AAD-Benutzer anmelden können, müssen sie in der Central Desktop-Anwendung bereitgestellt werden. In diesem Abschnitt wird beschrieben, wie in Central Desktop AAD-Benutzerkonten erstellt werden.

**So stellen Sie Benutzerkonten für Central Desktop bereit**
1. Melden Sie sich bei Ihrem Central Desktop-Mandanten an.
2. Wechseln Sie zu **Personen \> Interne Mitglieder**.
3. Klicken Sie auf **Add Internal Members**.
   
  ![Personen](./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Personen")
4. Geben Sie im Textfeld **E-Mail-Adresse des neuen Mitglieds** ein AAD-Konto ein, das Sie bereitstellen möchten, und klicken Sie dann auf **Weiter**.
   
  ![E-Mail-Adressen neuer Mitglieder](./media/active-directory-saas-central-desktop-tutorial/IC781052.png "E-Mail-Adressen neuer Mitglieder")
5. Klicken Sie auf **Add Internal member(s)**.
   
  ![Interne Mitglieder hinzufügen](./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Interne Mitglieder hinzufügen")
   
   >[!NOTE]
   >Die Benutzer, die Sie hinzugefügt haben, erhalten eine E-Mail mit einem Link zur Bestätigung, auf den sie zur Bestätigung klicken müssen.
   > 

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Central Desktop-Benutzerkonten oder mithilfe der von Central Desktop bereitgestellten APIs erstellen.
>  

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern in Central Desktop folgende Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **Central Desktop** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


