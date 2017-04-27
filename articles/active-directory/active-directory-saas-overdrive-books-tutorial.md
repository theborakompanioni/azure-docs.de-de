---
title: 'Tutorial: Azure Active Directory-Integration mit Overdrive Books | Microsoft Docs'
description: "Hier erfahren Sie, wie Sie Overdrive Books mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: e68cede7-1130-4813-bd55-22a9a6fc6bf5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 5d6f44f868010d50b36a9171b7db63aeee7e8bc0
ms.lasthandoff: 04/03/2017


---
# <a name="tutorial-azure-active-directory-integration-with-overdrive-books"></a>Tutorial: Azure Active Directory-Integration mit Overdrive Books
In diesem Tutorial wird die Integration von Azure und OverDrive erläutert.  

Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein OverDrive-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

Nach Abschluss dieses Lernprogramms können sich die Azure AD-Benutzer, die Sie OverDrive zugewiesen haben, mittels einmaliger Anmeldung (Single Sign-On, SSO) auf Ihrer OverDrive-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Tutorial beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für OverDrive
2. Konfigurieren des einmaligen Anmeldens (SSO)
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-overdrive-books-tutorial/IC784462.png "Szenario")

## <a name="enable-the-application-integration-for-overdrive"></a>Aktivieren der Anwendungsintegration für OverDrive
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für OverDrive aktivieren.

**Zum Aktivieren der Anwendungsintegration für OverDrive führen Sie die folgenden Schritte durch:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-overdrive-books-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
   ![Anwendungen](./media/active-directory-saas-overdrive-books-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
   ![Anwendung hinzufügen](./media/active-directory-saas-overdrive-books-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
   ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-overdrive-books-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** das Wort **OverDrive** ein.
   
   ![Anwendungskatalog](./media/active-directory-saas-overdrive-books-tutorial/IC784463.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich **OverDrive** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
   ![OverDrive](./media/active-directory-saas-overdrive-books-tutorial/IC799950.png "OverDrive")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei OverDrive zu authentifizieren.

**Führen Sie zum Konfigurieren von SSO die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **OverDrive** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
   ![Einmaliges Anmelden aktivieren](./media/active-directory-saas-overdrive-books-tutorial/IC784465.png "Einmaliges Anmelden aktivieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei OverDrive anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-overdrive-books-tutorial/IC784466.png "Einmaliges Anmelden konfigurieren")
3. Geben Sie auf der Seite **App-URL konfigurieren** im Textfeld für die **OverDrive-Anmelde-URL** Ihre URL im Format „*http://mslibrarytest.libraryreserve.com*“ ein, und klicken Sie dann auf **Weiter**.
   
   ![App-URL konfigurieren](./media/active-directory-saas-overdrive-books-tutorial/IC784467.png "App-URL konfigurieren")
4. Laden Sie auf der Seite **Einmaliges Anmelden konfigurieren für OverDrive** die Metadatendatei herunter, und senden Sie sie an das Supportteam von OverDrive.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-overdrive-books-tutorial/IC784468.png "Einmaliges Anmelden konfigurieren")
   
   >[!NOTE]
   >Das Supportteam von OverDrive konfiguriert einmaliges Anmelden für Sie. Sie erhalten eine Benachrichtigung, wenn die Konfiguration abgeschlossen ist.
   > 
   > 
5. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.
   
   ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-overdrive-books-tutorial/IC784469.png "Einmaliges Anmelden konfigurieren")
   
## <a name="configure-user-provisioning"></a>Benutzerbereitstellung konfigurieren

Für das Konfigurieren der Benutzerbereitstellung in OverDrive steht kein Aktionselement zur Verfügung.  

Wenn ein zugewiesener Benutzer versucht, sich bei OverDrive anzumelden, wird ggf. automatisch ein OverDrive-Konto erstellt.

>[!NOTE]
>Sie können AAD-Benutzerkonten auch mithilfe anderer Tools zum Erstellen von OverDrive-Benutzerkonten oder mithilfe der von OverDrive bereitgestellten APIs erstellen.
>
>
 
## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Um OverDrive Benutzer hinzuzufügen, führen Sie die folgenden Schritte durch:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **OverDrive** auf **Benutzer zuweisen**.
   
   ![Zuweisen von Benutzern](./media/active-directory-saas-overdrive-books-tutorial/IC784470.png "Zuweisen von Benutzern")
   
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.
   
   ![Ja](./media/active-directory-saas-overdrive-books-tutorial/IC767830.png "Ja")

Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

