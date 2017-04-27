---
title: 'Tutorial: Azure Active Directory-Integration mit RightAnswers | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie RightAnswers mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 7f09e25a-a716-41e1-8ca3-fd00e3d1b8cc
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 62aa62c41e7ac59212fb9f06a1a104984db90621
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Tutorial: Azure Active Directory-Integration mit RightAnswers
In diesem Tutorial wird die Integration von Azure und RightAnswers erläutert. Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein RightAnswers-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

Nach Abschluss dieses Tutorials können sich die RightAnswers zugewiesenen Azure AD-Benutzer wie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)beschrieben mittels einmaliger Anmeldung bei der Anwendung anmelden.

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für RightAnswers
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Szenario")

## <a name="enable-the-application-integration-for-rightanswers"></a>Aktivieren der Anwendungsintegration für RightAnswers
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für RightAnswers aktivieren.

**Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für RightAnswers zu aktivieren:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen](./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendung hinzufügen](./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **RightAnswers** ein.
   
    ![Anwendungskatalog](./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **RightAnswers** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei RightAnswers zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **RightAnswers** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer RightAnswers anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-Einstellungen konfigurieren** im Textfeld für die **Anmelde-URL** die von Ihren Benutzern für die Anmeldung bei Ihrer RightAnswers-Anwendung verwendete URL (z.B. *https://fortify.rightanswers.com/portal/ss/*) ein, und klicken Sie dann auf **Weiter**.
   
    ![App-Einstellungen konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802929.png "App-Einstellungen konfigurieren")
4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für RightAnswers** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei lokal auf Ihrem Computer.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Einmaliges Anmelden konfigurieren")
5. Senden Sie die heruntergeladene Metadatendatei an das Supportteam von RightAnswers.
   
    >[!NOTE]
    >Die eigentliche SSO-Konfiguration muss vom Supportteam von RightAnswers durchgeführt werden.
    >Sie erhalten eine Benachrichtigung, wenn SSO für Ihr Abonnement aktiviert wurde.
    > 
    > 

6. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Damit sich Azure AD-Benutzer bei RightAnswers anmelden können, müssen sie in RightAnswers bereitgestellt werden. Im Fall von RightAnswers ist die Bereitstellung eine automatisierte Aufgabe, daher müssen Sie keine Aktion ausführen.

Benutzer werden beim Versuch des einmaligen Anmeldens bei Bedarf automatisch erstellt.

>[!NOTE]
>Sie können RightAnswers-Benutzerkonten auch mithilfe von anderen Tools zum Erstellen von AAD-Benutzerkonten oder mithilfe der von RightAnswers bereitgestellten APIs erstellen.
> 
> 

## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie die folgenden Schritte aus, um RightAnswers Benutzer zuzuweisen:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.

2. Klicken Sie auf der Anwendungsintegrationsseite für **RightAnswers** auf **Benutzer zuweisen**.
   
    ![Zuweisen von Benutzern](./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
    ![Ja](./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


