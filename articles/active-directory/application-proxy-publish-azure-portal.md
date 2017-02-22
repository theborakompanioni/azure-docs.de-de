---
title: "Veröffentlichen von Apps mit Azure AD-Anwendungsproxy | Microsoft Docs"
description: "Es wird beschrieben, wie Sie lokale Anwendungen im Azure-Portal mit dem Azure AD-Anwendungsproxy in der Cloud veröffentlichen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 1fcec43ad50b9c78443ada116b9ca444605c4730
ms.openlocfilehash: dc30e18b92e492137e016e7a7372bfdf3285e5fe

---


# <a name="publish-applications-using-azure-ad-application-proxy---public-preview"></a>Veröffentlichen von Anwendungen mit dem Azure AD-Anwendungsproxy – Public Preview

> [!div class="op_single_selector"]
> * [Azure-Portal](application-proxy-publish-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-application-proxy-publish.md)

Mit dem Azure Active Directory-Anwendungsproxy (AD) können Sie Remotemitarbeiter unterstützen, indem Sie lokale Anwendungen so veröffentlichen, dass über das Internet auf sie zugegriffen werden kann. Über das Azure-Portal können Sie Anwendungen veröffentlichen, die in Ihrem lokalen Netzwerk ausgeführt werden, und sicheren Remotezugriff von außerhalb Ihres Netzwerks ermöglichen.

Dieser Artikel führt Sie durch die Schritte zum Veröffentlichen einer lokalen App mit dem Anwendungsproxy. Nach dem Durcharbeiten dieses Artikels können Sie die Anwendung mit einmaliger Anmeldung, personalisierten Informationen oder Sicherheitsanforderungen konfigurieren.

Wenn Sie mit dem Anwendungsproxy noch nicht vertraut sind, erfahren Sie im Artikel [Bereitstellen von sicherem Remotezugriff auf lokale Anwendungen](active-directory-application-proxy-get-started.md) mehr über diese Funktion.


## <a name="publish-an-on-premises-app-for-remote-access"></a>Veröffentlichen einer lokalen App für Remotezugriff


> [!TIP]
> Wenn Sie den Anwendungsproxy zum ersten Mal verwenden, wählen Sie eine Anwendung aus, die bereits für kennwortbasierte Authentifizierung eingerichtet ist. Der Anwendungsproxy unterstützt andere Arten von Authentifizierung, aber mit kennwortbasierten Apps ist der Einstieg am einfachsten und schnellen. 

1. Melden Sie sich als Administrator beim [Azure-Portal](https://portal.azure.com/) an.
2. Wählen Sie **Azure Active Directory** > **Unternehmensanwendungen** > **Hinzufügen** aus.

  ![Hinzufügen einer Unternehmensanwendung](./media/application-proxy-publish-azure-portal/add-app.png)

3. Wählen Sie auf der Seite „Kategorien“ die Option **oder fügen Sie eine eigene hinzu** aus.  

  ![Hinzufügen einer eigenen Anwendung](./media/application-proxy-publish-azure-portal/add-your-own.png)

4. Wählen Sie aus dem Dropdownmenü **Vorhandene Anwendung bereitstellen** aus.
5. Geben Sie einen Namen für Ihre App an, und wählen Sie dann **Hinzufügen** aus. Ein Ladevorgang wird durchgeführt. Nachdem Ihre App hinzugefügt wurde, wird das Blatt „Schnellstart“ geöffnet.
6. Wählen Sie auf dem Blatt „Schnellstart“ die Option **Remotezugriff für Ihre lokale Anwendung aktivieren** aus.

  ![Aktivieren von Remotezugriff für Ihre lokale Anwendung](./media/application-proxy-publish-azure-portal/enable-remote-access.png)

7. Geben Sie die folgenden Informationen zur Anwendung an:

   - **Interne URL**: Die Adresse, die vom Anwendungsproxy-Connector verwendet wird, um aus dem privaten Netzwerk auf die Anwendung zuzugreifen. Sie können einen bestimmten Pfad auf dem Back-End-Server für die Veröffentlichung angeben, während der Rest des Servers nicht veröffentlicht wird. Auf diese Weise können Sie unterschiedliche Websites auf demselben Server als unterschiedliche Apps veröffentlichen und jeweils einen eigenen Namen und Zugriffsregeln vergeben.

     > [!TIP]
     > Stellen Sie beim Veröffentlichen eines Pfads sicher, dass er alle erforderlichen Bilder, Skripts und Stylesheets für Ihre Anwendung enthält. Wenn sich die App beispielsweise unter „https://yourapp/app“ befindet und Bilder unter „https://yourapp/media“ genutzt werden, sollten Sie https://yourapp/ als Pfad veröffentlichen.

   - **Externe URL**: Die Adresse, die Ihre Benutzer aufrufen, um auf die App von außerhalb Ihres Netzwerks zuzugreifen.
   - **Vorauthentifizierung**: Gibt das Verfahren an, wie der Anwendungsproxy Benutzer überprüft, bevor diese Zugriff auf Ihre Anwendung erhalten. 

     - Azure Active Directory: Der Anwendungsproxy leitet Benutzer an die Anmeldung mit Azure AD um. Hierbei werden deren Berechtigungen für das Verzeichnis und die Anwendung authentifiziert. Es wird empfohlen, diese Option standardmäßig aktiviert zu lassen.
     - Passthrough: Benutzer müssen sich nicht bei Azure Active Directory authentifizieren, um Zugriff auf die Anwendung zu erhalten. Sie können weiterhin Authentifizierungsanforderungen auf dem Back-End einrichten.
   - **URL in Headern übersetzen**: Wählen Sie aus, ob die URL in den Headern übersetzt werden soll. 
   - **Connectorgruppe**: Connectors verarbeiten den Remotezugriff auf Ihre Anwendung, und Connectorgruppen helfen Ihnen, Connectors und Apps nach Region, Netzwerk oder Zweck zu organisieren. Wenn Sie noch keine Connectorgruppen erstellt haben, wird Ihre App **Standard** zugewiesen, und eine Warnmeldung wird angezeigt, die Sie auffordert, [eine Connectorgruppe zu erstellen](active-directory-application-proxy-connectors-azure-portal.md).

   ![Konfigurieren der Anwendung](./media/application-proxy-publish-azure-portal/configure-app.png)

8. Wählen Sie **Speichern**aus.

   ![Speichern der Einstellungen des Anwendungsproxys](./media/application-proxy-publish-azure-portal/save-app-proxy.png)

## <a name="add-a-test-user"></a>Hinzufügen eines Testbenutzers 

Um zu testen, ob die Anwendung ordnungsgemäß veröffentlicht wurde, fügen Sie ein Benutzerkonto hinzu, auf das Sie zugreifen können. 

1. Wählen Sie auf dem Blatt „Schnellstart“ die Option **Benutzer zu Testzwecken zuweisen** aus.

  ![Zuweisen eines Benutzers zu Testzwecken](./media/application-proxy-publish-azure-portal/assign-user.png)

2. Wählen Sie auf dem Blatt „Benutzer und Gruppen“ die Option **Hinzufügen** aus.

  ![Hinzufügen eines Benutzers oder einer Gruppe](./media/application-proxy-publish-azure-portal/add-user.png)

3. Wählen Sie auf dem Blatt „Zuweisung hinzufügen“ die Option **Benutzer und Gruppen** aus, und wählen Sie dann das Konto aus, das Sie hinzufügen möchten. 
4. Wählen Sie **Zuweisen** aus.

## <a name="test-your-published-app"></a>Testen der veröffentlichten App

Navigieren Sie in Ihrem Browser zur externen URL, die Sie beim Veröffentlichen konfiguriert haben. Der Startbildschirm sollte angezeigt werden, und Sie sollten in der Lage sein, sich mit dem Testkonto anzumelden, das Sie eingerichtet haben.

![Testen der veröffentlichten App](./media/application-proxy-publish-azure-portal/test-app.png)


## <a name="next-steps"></a>Nächste Schritte
- [Laden Sie Connectors herunter](active-directory-application-proxy-enable.md), und [erstellen Sie Connectorgruppen](active-directory-application-proxy-connectors-azure-portal.md), um Anwendungen in getrennten Netzwerken und Standorten zu veröffentlichen.

- [Richten Sie einmaliges Anmelden](application-proxy-sso-azure-portal.md) für Ihre neu veröffentlichte App ein.



<!--HONumber=Jan17_HO2-->


