---
title: 'Tutorial: Azure Active Directory-Integration mit ScreenSteps | Microsoft Docs'
description: "Erfahren Sie, wie Sie ScreenSteps mit Azure Active Directory verwenden können, um einmaliges Anmelden, automatisierte Bereitstellung und vieles mehr zu ermöglichen."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 7fbb8874367bda9be618332947ffa2c9942fe4ec
ms.lasthandoff: 04/12/2017


---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Tutorial: Azure Active Directory-Integration mit ScreenSteps
In diesem Tutorial wird die Integration von Azure und ScreenSteps erläutert.
Das in diesem Tutorial verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

* Ein gültiges Azure-Abonnement
* Ein ScreenSteps-Mandant

Nach Abschluss dieses Tutorials können sich die ScreenSteps zugewiesenen Azure AD-Benutzer mittels einmaligen Anmeldens (SSO) auf Ihrer ScreenSteps-Unternehmenswebsite bei der Anwendung anmelden (durch den Dienstanbieter initiierte Anmeldung). Alternativ können sie den Zugriffsbereich nutzen (siehe [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md)).

Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. Aktivieren der Anwendungsintegration für ScreenSteps
2. Konfigurieren des einmaligen Anmeldens (SSO) 
3. Konfigurieren der Benutzerbereitstellung
4. Zuweisen von Benutzern

![Szenario](./media/active-directory-saas-screensteps-tutorial/IC778516.png "Szenario")

## <a name="enable-the-application-integration-for-screensteps"></a>Aktivieren der Anwendungsintegration für ScreenSteps
In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für ScreenSteps aktivieren.

**Führen Sie zum Aktivieren der Anwendungsintegration für ScreenSteps die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

    ![Active Directory](./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .

    ![Anwendungen](./media/active-directory-saas-screensteps-tutorial/IC700994.png "Anwendungen")
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .

    ![Anwendung hinzufügen](./media/active-directory-saas-screensteps-tutorial/IC749321.png "Anwendung hinzufügen")
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

    ![Anwendung aus dem Katalog hinzufügen](./media/active-directory-saas-screensteps-tutorial/IC749322.png "Anwendung aus dem Katalog hinzufügen")
6. Geben Sie im **Suchfeld** den Suchbegriff **ScreenSteps** ein.

    ![Anwendungskatalog](./media/active-directory-saas-screensteps-tutorial/IC778517.png "Anwendungskatalog")
7. Wählen Sie im Ergebnisbereich die Option **ScreenSteps** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

    ![ScreenSteps](./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
   
## <a name="configure-single-sign-on"></a>Configure single sign-on
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei ScreenSteps zu authentifizieren.

**So konfigurieren Sie einmaliges Anmelden**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **ScreenSteps** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778519.png "Einmaliges Anmelden konfigurieren")
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei ScreenSteps anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778520.png "Einmaliges Anmelden konfigurieren")

3. Melden Sie sich in einem anderen Webbrowserfenster bei der ScreenSteps-Unternehmenswebsite als Administrator an.

4. Klicken Sie auf **Kontoverwaltung**.

    ![Kontoverwaltung](./media/active-directory-saas-screensteps-tutorial/IC778523.png "Kontoverwaltung")

5. Klicken Sie auf **Einmaliges Anmelden**.

    ![Remoteauthentifizierung](./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remoteauthentifizierung")

6. Klicken Sie auf **Create Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden erstellen).

    ![Remoteauthentifizierung](./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remoteauthentifizierung")

7. Führen Sie im Abschnitt **Create Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden erstellen) die folgenden Schritte aus:

    ![Erstellen eines Authentifizierungsendpunkts](./media/active-directory-saas-screensteps-tutorial/IC778526.png "Erstellen eines Authentifizierungsendpunkts")

    1. Geben Sie in das Textfeld **Titel** einen Titel ein.
    2. Wählen Sie in der Liste **Modus** die Option **SAML** aus.
    3. Klicken Sie auf **Erstellen**.

8. Bearbeiten Sie den neuen Endpunkt.

    ![Endpunkt bearbeiten](./media/active-directory-saas-screensteps-tutorial/IC778528.png "Endpunkt bearbeiten")

9. Führen Sie im Abschnitt **Edit Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden bearbeiten) die folgenden Schritte aus:

    ![Remote Authentication Endpoint](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote Authentication Endpoint")

    1. Kopieren Sie die **SAML-Consumer-URL** in die Zwischenablage.

10. Fügen Sie die **SAML-Consumer-URL** im klassischen Azure Portal auf der Seite **App-URL konfigurieren** im Textfeld **ScreenSteps-Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.

    ![App-URL konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778521.png "App-URL konfigurieren")

11. Klicken Sie zum Herunterladen des Zertifikats auf der Seite **Einmaliges Anmelden konfigurieren für ScreenSteps** auf **Zertifikat herunterladen**, und speichern Sie das Zertifikat auf Ihrem Computer.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778522.png "Einmaliges Anmelden konfigurieren")


12. Wechseln Sie zurück zum Abschnitt **Edit Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden bearbeiten), und führen Sie die folgenden Schritte aus:

    ![Remote Authentication Endpoint](./media/active-directory-saas-screensteps-tutorial/IC778527.png "Remote Authentication Endpoint")

    1. Klicken Sie auf **Upload new SAML Certificate file** (Neue SAML-Zertifikatsdatei hochladen), und laden Sie dann das heruntergeladene Zertifikat hoch.
    2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ScreenSteps** den Wert für **Remoteanmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteanmelde-URL** ein.
    3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite **Einmaliges Anmelden konfigurieren für ScreenSteps** den Wert für **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Remoteabmelde-URL** ein.
    4. Wählen Sie eine **Gruppe** zum Zuweisen von Benutzern bei deren Bereitstellung aus.
    5. Klicken Sie auf **Aktualisieren**.
    6. Wechseln Sie zurück zu **Edit Single Sign-on Endpoint** (Endpunkt für einmaliges Anmelden bearbeiten).
    7. Klicken Sie auf die Schaltfläche **Make default for account** (Als Standard für Konto festlegen), um diesen Endpunkt für alle Benutzer zu verwenden, die sich bei ScreenSteps anmelden. Alternativ können Sie auf die Schaltfläche **Add to Site** (Zu Standort hinzufügen) klicken, um diesen Endpunkt für bestimmte Websites in **ScreenSteps** zu verwenden.


12. Bestätigen Sie im klassischen Azure-Portal die Konfiguration der einmaligen Anmeldung, und klicken Sie dann auf **Abschließen**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu schließen.

    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-screensteps-tutorial/IC778542.png "Einmaliges Anmelden konfigurieren")

## <a name="configuring-user-provisioning"></a>Konfigurieren der Benutzerbereitstellung



## <a name="assign-users"></a>Benutzer zuweisen
Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

**Führen Sie zum Zuweisen von Benutzern zu ScreenSteps die folgenden Schritte aus:**

1. Erstellen Sie im klassischen Azure-Portal ein Testkonto.
2. Klicken Sie auf der Anwendungsintegrationsseite für **ScreenSteps** auf **Benutzer zuweisen**.

    ![Zuweisen von Benutzern](./media/active-directory-saas-screensteps-tutorial/IC778548.png "Zuweisen von Benutzern")
3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.


Wenn Sie die SSO-Einstellungen testen möchten, öffnen Sie den Zugriffsbereich. Weitere Informationen zum Zugriffsbereich finden Sie unter [Einführung in den Zugriffsbereich](active-directory-saas-access-panel-introduction.md).


