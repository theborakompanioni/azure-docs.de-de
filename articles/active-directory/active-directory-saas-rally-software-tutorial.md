---
title: 'Tutorial: Azure Active Directory-Integration mit Rally Software | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Rally Software mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 8d913c024fce9945cfd5eaf336deed259dfbef50
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Tutorial: Azure Active Directory-Integration mit Rally Software
In diesem Tutorial wird die Integration von Azure und Rally Software erläutert.  

Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Einen Rally Software-Mandanten

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für Rally Software
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-rally-software-tutorial/IC769525.png "Szenario")

## <a name="enable-the-application-integration-for-rally-software"></a>Aktivieren der Anwendungsintegration für Rally Software
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für Rally Software aktivieren.

**Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für Rally Software zu aktivieren:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-rally-software-tutorial/IC700994.png "Anwendungen")

4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-rally-software-tutorial/IC749321.png "Anwendung hinzufügen")

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-rally-software-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")

6. Geben Sie im **Suchfeld** das Wort **rally** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-rally-software-tutorial/IC769526.png "Anwendungskatalog")

7. Wählen Sie im Ergebnisbereich **Rally Software** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Rally Software](./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally Software")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei Rally Software zu authentifizieren.

Im Rahmen dieses Verfahrens müssen Sie eine Base-64-codierte Zertifikatsdatei erstellen.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **Rally Software** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rally-software-tutorial/IC749323.png "Einmaliges Anmelden konfigurieren")

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Rally anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Microsoft Azure AD – einmaliges Anmelden](./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD – einmaliges Anmelden")

3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **Rally Software-Mandanten-URL** die URL im Format „*https://\<Mandantenname\>.rally.com*“ ein, und klicken Sie dann auf **Weiter**.
   
    ![App-URL konfigurieren](./media/active-directory-saas-rally-software-tutorial/IC769529.png "App-URL konfigurieren")

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Rally** auf „Metadaten herunterladen“, und speichern Sie die Daten auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rally-software-tutorial/IC769530.png "Einmaliges Anmelden konfigurieren")

5. Melden Sie sich bei Ihrem **Rally Software** -Mandanten an.

6. Klicken Sie in der Symbolleiste oben auf **Einrichtung** und dann auf **Abonnement**.
   
    ![Abonnement](./media/active-directory-saas-rally-software-tutorial/IC769531.png "Abonnement")

7. Klicken Sie oben rechts auf der Symbolleiste auf die Schaltfläche **Aktion**, und wählen Sie dann **Abonnement bearbeiten**.

8. Führen Sie auf der Dialogfeldseite **Abonnement** die folgenden Schritte aus, und klicken Sie dann auf **Speichern und schließen**:
   
    ![Authentifizierung](./media/active-directory-saas-rally-software-tutorial/IC769542.png "Authentifizierung")
   
  1. Wählen Sie im Dropdownmenü für die Authentifizierung die Option **Rally- oder SSO-Authentifizierung** aus.
  2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Rally Software** den Wert der **Identitätsanbieter-ID**, und fügen Sie ihn in das Textfeld **Identitätsanbieter-URL** ein.
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für Rally Software** den Wert für **Remoteabmelde-URL**.

9. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rally-software-tutorial/IC769547.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich AAD-Benutzer anmelden können, müssen sie in der Rally Software-Anwendung unter Verwendung ihrer Azure Active Directory-Benutzernamen bereitgestellt werden.

**Um die Benutzerbereitstellung zu konfigurieren, führen Sie die folgenden Schritte durch:**

1. Melden Sie sich bei Ihrem Rally Software-Mandanten an.

2. Navigieren Sie zu **Einrichtung \> BENUTZER**, und klicken Sie auf **+ Neuen Benutzer hinzufügen**.
   
    ![Benutzer](./media/active-directory-saas-rally-software-tutorial/IC781039.png "Benutzer")

3. Geben Sie den Namen in das Textfeld für den neuen Benutzer ein, und klicken Sie dann auf **Mit Details hinzufügen**.

4. Führen Sie im Abschnitt **Benutzer erstellen** die folgenden Schritte aus:
   
    ![Benutzer erstellen](./media/active-directory-saas-rally-software-tutorial/IC781040.png "Benutzer erstellen")
   
   1. Geben Sie im Textfeld **Benutzername** den Namen des Azure AD-Benutzers ein, den Sie bereitstellen möchten.
   2. Geben Sie im Textfeld **E-Mail-Adresse** die E-Mail-Adresse des Azure AD-Benutzers ein, den Sie bereitstellen möchten.
   3. Klicken Sie auf **Speichern und schließen**.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von Rally Software-Benutzerkonten oder mithilfe der von Rally Software bereitgestellten APIs erstellen.
> 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie die folgenden Schritte aus, um Rally Software Benutzer zuzuweisen:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **Rally Software** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-rally-software-tutorial/IC769548.png "Zuweisen von Benutzern")

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-rally-software-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


