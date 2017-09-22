---
title: "Konfigurieren der Azure Active Directory-Authentifizierung für die App Services-Anwendung"
description: "Erfahren Sie, wie Sie die Azure Active Directory-Authentifizierung für die App Services-Anwendung konfigurieren."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 6179c4a3ba54260e9bd80156ec84e1d9aa3a0561
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>So konfigurieren Sie Ihre App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

In diesem Thema wird veranschaulicht, wie Sie Azure App Services zur Verwendung von Azure Active Directory als Authentifizierungsanbieter konfigurieren.

## <a name="express"> </a>Konfigurieren von Azure Active Directory mit Express-Einstellungen
1. Navigieren Sie im [Azure-Portal]zu Ihrer Anwendung. Klicken Sie auf **Einstellungen** und anschließend auf **Authentifizierung/Autorisierung**.
2. Falls das Authentifizierungs-/Autorisierungsfeature nicht aktiviert ist, aktivieren Sie es über die Option **Ein**.
3. Klicken Sie auf **Azure Active Directory** und dann unter **Verwaltungsmodus** auf **Express**.
4. Klicken Sie auf **OK** , um die Anwendung in Azure Active Directory zu registrieren. Dadurch wird eine neue Registrierung erstellt. Wenn Sie stattdessen eine vorhandene Registrierung auswählen möchten, klicken Sie auf **Vorhandene App auswählen** , und suchen Sie nach dem Namen einer zuvor erstellten Registrierung des Mandanten.
   Klicken Sie auf die Registrierung, um sie auszuwählen, und klicken Sie auf **OK**. Klicken Sie dann auf dem Blatt „Azure Active Directory-Einstellungen“ auf **OK** .
   
   ![][0]
   
   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
5. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Azure Active Directory umgeleitet.
6. Klicken Sie auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App verwenden.

## <a name="advanced"> </a>(Alternative Methode:) Manuelles Konfigurieren von Azure Active Directory mit erweiterten Einstellungen
Sie können Konfigurationseinstellungen auch manuell bereitstellen Dies ist die bevorzugte Lösung, falls der zu verwendende AAD-Mandant sich von dem Mandanten unterscheidet, mit dem die Anmeldung in Azure erfolgt. Um die Konfiguration abzuschließen, müssen Sie zunächst eine Registrierung in Azure Active Directory erstellen, und App Service dann einige Registrierungsdetails liefern.

### <a name="register"> </a>Registrieren Ihrer Anwendung bei Azure Active Directory
1. Melden Sie sich im [Azure-Portal]an, und navigieren Sie zu Ihrer Anwendung. Kopieren Sie die **URL**. Diese dient zum Konfigurieren Ihrer Azure Active Directory-App.
2. Melden Sie sich im [Klassischen Azure-Portal] an, und navigieren Sie zu **Active Directory**.
   
    ![][2]
3. Wählen Sie Ihr Verzeichnis und anschließend die Registerkarte **Anwendungen** im oberen Bereich aus. Klicken Sie im unteren Bereich auf **HINZUFÜGEN** , um eine neue App-Registrierung zu erstellen.
4. Klicken Sie auf **Add an application my organization is developing**.
5. Geben Sie im Assistenten zum Hinzufügen von Anwendungen einen **Namen** für Ihre Anwendung ein, und klicken Sie auf den Typ **Webanwendung und/oder Web-API**. Klicken Sie dann auf Continue.
6. Fügen Sie in das Feld **Anmelde-URL** die zuvor kopierte URL der Anwendung ein. Geben Sie dieselbe URL in das Feld **App-ID-URI** ein. Klicken Sie dann auf "Weiter".
7. Nachdem die Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Legen Sie unter **Einmaliges Anmelden** die **Antwort-URL** auf die URL Ihrer Anwendung fest, und hängen Sie den Pfad */.auth/login/aad/callback* an. Beispiel: `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Stellen Sie sicher, dass Sie das HTTPS-Schema verwenden.
   
    ![][3]
8. Klicken Sie auf **Speichern**. Kopieren Sie dann die **Client-ID** für die App. Ihre Anwendung wird später anhand dieser Angabe konfiguriert.
9. Klicken Sie auf der Befehlsleiste unten auf **Endpunkte anzeigen**, kopieren Sie dann die URL des **Verbundmetadaten-Dokuments**, und laden Sie dieses herunter, oder öffnen Sie es im Browser.
10. Innerhalb des **EntityDescriptor**-Stammelements muss ein **entityID**-Attribut gemäß `https://sts.windows.net/` gefolgt von einer GUID vorhanden sein, die speziell für Ihren Mandanten gültig ist (Mandanten-ID). Kopieren Sie diese ID, die als **Aussteller-URL**dient. Ihre Anwendung wird später anhand dieser Angabe konfiguriert.

### <a name="secrets"> </a>Hinzufügen von Azure Active Directory-Informationen zu Ihrer Anwendung
1. Navigieren Sie im [Azure-Portal]wieder zu Ihrer Anwendung. Klicken Sie auf **Einstellungen** und anschließend auf **Authentifizierung/Autorisierung**.
2. Falls das Authentifizierungs-/Autorisierungsfeature nicht aktiviert ist, aktivieren Sie es über die Option **Ein**.
3. Klicken Sie auf **Azure Active Directory** und dann unter **Verwaltungsmodus** auf **Erweitert**. Fügen Sie die Werte für Client-ID und Aussteller-URL ein, die Sie zuvor abgerufen haben. Klicken Sie dann auf **OK**.
   
   ![][1]
   
   Standardmäßig erfolgt die Authentifizierung über App Service, wobei jedoch der Zugriff auf die Inhalte Ihrer Website und APIs nicht autorisiert wird. Sie müssen die Benutzer in Ihrem App-Code autorisieren.
4. (Optional) Um den Zugriff auf Ihre Website ausschließlich auf Benutzer zu beschränken, die von Azure Active Directory authentifiziert werden, legen Sie **Die auszuführende Aktion, wenn die Anforderung nicht authentifiziert ist** auf **Mit Azure Active Directory anmelden** fest. Dies erfordert, dass alle Anforderungen authentifiziert werden müssen. Alle nicht authentifizierten Anforderungen werden zur Authentifizierung an Azure Active Directory umgeleitet.
5. Klicken Sie auf **Speichern**.

Sie können nun Azure Active Directory für die Authentifizierung in Ihrer App verwenden.

## <a name="optional-configure-a-native-client-application"></a>(Optional) Konfigurieren Sie eine systemeigene Clientanwendung
Mit Azure Active Directory können Sie auch systemeigene Clients registrieren. So haben Sie noch mehr Kontrolle bei der Zuordnung von Berechtigungen. Dies ist erforderlich, wenn Sie mit einer Bibliothek, etwa der **Active Directory-Authentifizierungsbibliothek**, Anmeldungen durchführen möchten.

1. Navigieren Sie im **klassischen Azure-Verwaltungsportal** zu [Active Directory].
2. Wählen Sie Ihr Verzeichnis und anschließend die Registerkarte **Anwendungen** im oberen Bereich aus. Klicken Sie im unteren Bereich auf **HINZUFÜGEN** , um eine neue App-Registrierung zu erstellen.
3. Klicken Sie auf **Add an application my organization is developing**.
4. Geben Sie im Assistenten zum Hinzufügen von Anwendungen einen **Namen** für Ihre Anwendung ein, und klicken Sie auf den Typ **Systemeigene Clientanwendung**. Klicken Sie dann auf Continue.
5. Geben Sie im Feld **Umleitungs-URI** den */.auth/login/done*-Endpunkt Ihrer Website unter Verwendung des HTTPS-Schemas ein. Dieser Wert sollte so ähnlich lauten wie *https://contoso.azurewebsites.net/.auth/login/done*. Wenn Sie eine Windows-Anwendung erstellen, verwenden Sie stattdessen die [Paket-SID](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) als URI.
6. Nachdem die systemeigene Anwendung hinzugefügt wurde, klicken Sie auf die Registerkarte **Konfigurieren** . Suchen Sie die **Client-ID** und notieren Sie sich den Wert.
7. Führen Sie auf der Seite einen Bildlauf nach unten zum Abschnitt **Berechtigungen für andere Anwendungen** durch, und klicken Sie auf **Anwendung hinzufügen**.
8. Suchen Sie nach der zuvor registrierten Web-App, und klicken Sie auf das Pluszeichen. Klicken Sie dann auf das Häkchen, um das Dialogfeld zu schließen. Wenn die Webanwendung nicht gefunden werden kann, navigieren Sie zu ihrer Registrierung, und fügen Sie eine neue Antwort-URL (beispielsweise die HTTP-Version der aktuellen URL) hinzu. Klicken Sie anschließend auf „Speichern“, und wiederholen Sie diese Schritte. Die Anwendung sollte nun in der Liste angezeigt werden.
9. Öffnen Sie für den neu erstellten Eintrag das Dropdownmenü **Delegierte Berechtigungen**, und wählen Sie **Access (appName)** (Zugriff (App-Name) aus. Klicken Sie anschließend auf **Save**.

Sie haben nun eine systemeigene Clientanwendung mit Zugriff auf Ihre App Service-Anwendung konfiguriert.

## <a name="related-content"> </a>Verwandte Inhalte
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure-Portal]: https://portal.azure.com/
[Klassisches Azure-Portal]: https://manage.windowsazure.com/
[alternative method]:#advanced

